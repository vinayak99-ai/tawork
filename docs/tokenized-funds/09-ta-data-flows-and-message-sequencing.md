# Transfer Agent Data Flows & Message Sequencing — End-to-End Deep Dive

How data actually moves between the investor/order-entry portal, the transfer agent (TA), fund
accounting, the payment/disbursement agent, the custodian, NSCC, and shareholder reporting — with
the real messaging standards involved (not invented ones) and the step-by-step sequence for a
subscription, a redemption, and a dividend distribution. Companion to `05`, `06`, and `08`.

**Interpretation note**: this doc reads "TA to HCC" from the request as **TA to NSCC** (the DTCC
clearing subsidiary covered in `06`) — this session's voice transcription has consistently
mangled DTCC/NSCC/SEC elsewhere (DDCC, ACC), and NSCC fits the data-flow context. Flag if a
different entity was meant.

**Accuracy discipline**: several specific message codes and formats researched for this doc turned
out **not** to exist or not to apply the way initially assumed (see §7). Every code cited below was
independently verified against a primary or authoritative source; anything not verified is stated
as such, not guessed at. This matters more here than in prior docs, because a wrong message code in
a technical reference is actively misleading, not just incomplete.

---

## ⚡ TL;DR

- **Order intake** runs on two largely parallel systems depending on market/channel: in the **US**,
  broker-dealer/distributor orders reach the TA almost entirely via **NSCC's Fund/SERV** (Record
  Type 001 Order, 015 Exchange — `06`, §1). In **Europe/global cross-border** fund distribution,
  the equivalent messages are **SWIFT ISO 20022 `setr.*`** messages (verified codes in §1). DTCC is
  reportedly layering an optional ISO 20022 interface onto Fund/SERV itself, but a firm timeline for
  that convergence wasn't confirmed.
- **TA ↔ Fund Accounting** is the one leg of this whole diagram with **no named industry-standard
  message format** — NAV transmission and the reverse net-flow report are proprietary/vendor file
  feeds or API calls, often not even crossing a company boundary (the same firm, e.g. State Street
  or BNY Mellon, frequently runs both books on one platform).
- **TA → payment agent/bank**: ACH debits/credits use standard NACHA entry classes (**PPD** for
  standing-authorization consumer debits, **WEB** for online-initiated) — **there is no
  mutual-fund-specific NACHA code**, contrary to what you might expect. Large-dollar wires run over
  **Fedwire**, which itself migrated to **ISO 20022** messaging in July 2025. Custodian daily cash
  statements typically use **MT940/camt.053** (end-of-day) or **MT942/camt.052** (intraday).
- **The forward-pricing rule (Rule 22c-1)** is what forces the NAV-then-price sequencing: an order
  must be priced at the **next NAV computed after receipt**, not any earlier price — this is the
  hard dependency that makes "TA waits for fund accounting's NAV before it can finalize any order"
  non-negotiable, every single business day.
- **Redemption proceeds must be paid within 7 calendar days** (ICA §22(e)) — the one narrow,
  SEC-sanctioned exception being a temporary hold when elder/vulnerable-adult financial
  exploitation is suspected (2018 no-action letter).
- **Full step-by-step sequences** for a subscription, a redemption, and a dividend distribution are
  in §6 — this is the actual "who sends what to whom, in what order" walkthrough.

---

## 0. The map, at a glance

```
                         ┌─────────────────────────┐
   Investor / Advisor    │   Order-entry portal /   │
   ───────────────────►  │   distributor / broker   │
                         └────────────┬─────────────┘
                                      │  Fund/SERV 001/015 (US)
                                      │  or setr.010/004/013 (cross-border)
                                      ▼
   ┌───────────────────────────────────────────────────────────────────┐
   │                         TRANSFER AGENT                            │
   │      (master securityholder file — see doc 08 for internals)      │
   └───┬───────────┬───────────────┬──────────────┬─────────────┬──────┘
       │           │               │              │             │
       │ NAV req   │ ACH/Fedwire   │ Fund/SERV    │ Trade instr/│ Statements,
       │ / net     │ debit/credit  │ confirm,     │ cash confirm│ confirms,
       │ flows     │               │ Networking   │             │ 1099s
       ▼           ▼               ▼              ▼             ▼
   ┌────────┐  ┌─────────┐   ┌──────────┐   ┌───────────┐  ┌────────────┐
   │  Fund  │  │ Payment │   │   NSCC   │   │ Custodian │  │Shareholder │
   │Accounting│ │ agent/  │   │(Fund/SERV,│  │   bank    │  │ reporting/ │
   │(NAV strike)│  bank   │   │Networking)│   │           │  │  tax forms │
   └────────┘  └─────────┘   └──────────┘   └───────────┘  └────────────┘
```

Every arrow in this diagram is covered in detail below, with what's actually verified about the
format used on it.

---

## 1. Portal / distributor → TA — order intake

### 1.1 US domestic: NSCC Fund/SERV

Already covered in depth in `06`, §1 — summarized here for sequencing: a broker-dealer/distributor
submits a **Fund/SERV 001 Order** (purchase/redemption) or **015 Exchange** record; the TA
confirms back through the same channel. This is DTCC's own description of Fund/SERV as **"the U.S.
industry standard"** for this exact function. [DTCC Fund/SERV](https://dtcclearning.com/products-and-services/mutual-fund-services/fund-serv.html)

### 1.2 Cross-border / global: SWIFT ISO 20022 `setr.*` messages

**Verified message list** (cross-checked against SWIFT's own Standards MX Funds Message Definition
Report and independent corroborating sources — this corrects an initial assumption that had wrong
code numbers):

| Code | Message | Direction |
|---|---|---|
| **setr.010.001** | Subscription Order | Instructing party (distributor/investment manager) → executing party (TA) |
| **setr.012.001** | Subscription Order Confirmation | Executing party (TA) → instructing party |
| **setr.004.001** | Redemption Order | Instructing party → executing party |
| **setr.006.001** | Redemption Order Confirmation | Executing party → instructing party |
| **setr.013.001** | Switch Order (fund-to-fund exchange) | Instructing party → executing party |
| **setr.015.001** | Switch Order Confirmation | Executing party → instructing party |
| setr.014.001 | Switch Order Cancellation Request | — |
| setr.005/011.001 | Redemption/Subscription Order Cancellation Request | — |
| setr.065.001 | Investment Fund Order Cancellation Request (general, cross-type) | — |
| setr.066.001 | Investment Fund Cancellation Advice | — |
| setr.016/017/018 | Order Instruction Status Report / Order Cancellation Status Report / Request For Order Status Report | — |
| setr.001/003/007/009 | Redemption/Subscription **Bulk** Order + Bulk Order Confirmation (batched) | — |
| setr.059–062, 064 | Alternative Funds Subscription/Redemption Order + Confirmations, Status Report — for hedge funds/PE, **not** '40 Act mutual funds | — |

**`setr.020` was an incorrect assumption going into this research and does not appear to exist** in
this message catalogue — don't use it. [Redemption Order (setr.004.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-004-001-Redemption-Order.html) ·
[Redemption Order Confirmation (setr.006.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-006-001-Redemption-Order-Confirmation.html) ·
[Subscription Order (setr.010.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-010-001-Subscription-Order.html) ·
[Subscription Order Confirmation (setr.012.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-012-001-Subscription-Order-Confirmation.html) ·
[Switch Order (setr.013.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-013-001-Switch-Order.html) ·
[full setr business-area catalogue](https://www.iotafinance.com/en/SWIFT-ISO20022-Business-area-setr-Securities-Trade.html) ·
[SWIFT Standards MX Funds MDR](https://www2.swift.com/knowledgecentre/rest/v1/publications/stdsmx_funds_mdrs/8.0/SR2022_MX_Funds_MDR1_Standards.pdf)

The message definitions themselves confirm the **TA is the typical "executing party"** on these
messages — e.g., the Redemption Order message is "sent by an instructing party... to the executing
party... for example, a transfer agent." This is the same functional role Fund/SERV plays
domestically — **setr and Fund/SERV are largely separate, parallel rails serving the same purpose
in different markets**, not competing standards in the same market.

**Convergence, flagged as directional only**: DTCC's own Settlement Transformation materials
describe building "an ISO 20022 interface to NSCC's Fund/SERV, Networking, Mutual Fund Profile, and
DTCC Payment aXis services" — i.e., DTCC is layering optional ISO 20022 connectivity onto Fund/SERV.
**No firm timeline for this was confirmed against a primary DTCC page with dates** — treat as
directional, not completed. [DTCC Settlement Transformation FAQ](https://www.dtcc.com/initiatives/Content/2-sett_trans_faqs/2_sett_trans_faq.htm) ·
[ISO 20022 adoption initiatives report](https://www.iso20022.org/sites/default/files/2020-03/ISOInitiatives_July2018.pdf)

**Legacy ISO 15022 MT codes**: not independently confirmed for investment-fund-specific orders.
MT 5xx (Category 5) is confirmed as "Securities Markets" broadly, but which specific MT number(s)
within it were the legacy dedicated fund-order messages wasn't pinned down — and **MT600 is
Commodity Trade Confirmation, not funds**, despite superficially plausible naming. Describe the
legacy layer generically ("SWIFT's older ISO 15022 FIN messages, now superseded by setr") rather
than citing a specific MT number. [SWIFT Category 5 MDR](https://www2.swift.com/knowledgecentre/rest/v1/publications/us5md_20210723/2.0/us5md_20210723.pdf)

---

## 2. TA ↔ Fund Accounting — the NAV cycle (no standard message format)

### 2.1 Why this leg exists — Rule 22c-1 forward pricing

**Rule 22c-1** (17 CFR 270.22c-1) requires a fund to sell/redeem shares "at a price based on the
current net asset value... **next computed after receipt** of an order" — orders can't be priced at
yesterday's NAV or an estimate; they must wait for the actual next-struck NAV.
[17 CFR 270.22c-1, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.22c-1) ·
[SEC 2003 Amendments to Rules Governing Pricing of Mutual Fund Shares](https://www.sec.gov/rules-regulations/2003/12/amendments-rules-governing-pricing-mutual-fund-shares)

**Rule 2a-4** (17 CFR 270.2a-4) defines "current net asset value": portfolio securities with
readily available market quotations are valued at current market value; others at fair value
determined in good faith by the board (this is where `07`'s Rule 2a-5 pricing-service coverage
connects); changes in holdings must be reflected no later than the first calculation on the first
business day following trade date. [17 CFR 270.2a-4, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.2a-4)

Operationally: pricing begins at NYSE close (4:00pm ET); third-party pricing services (`07`, §8)
feed security prices to fund accounting, which strikes NAV per share and applies control
procedures. [ICI — FAQs: Mutual Fund Share Pricing](https://www.ici.org/faqs/faq/mfs/faqs_navs)

### 2.2 NAV transmission — fund accounting → TA (no named standard)

**This is the one leg of the entire diagram with no confirmed industry-standard message format.**
Vendor material (e.g., Milestone Group's NAV/Unit Pricing platform) describes NAV-striking systems
that "upload, merge and validate data from multiple sources, including accounting systems, registry
and transfer agencies" — confirming the NAV strike is typically a **proprietary file feed or API
call**, and very often not even a cross-company transmission at all, since the same servicer (State
Street, BNY Mellon, JPMorgan — `07`, §4/§5) frequently runs both the fund accounting books and the
TA platform. [Milestone Group NAV Unit Pricing](https://www.milestonegroup.com/solutions/fund-processing/nav-unit-pricing)

**Don't present this as a named standard in any downstream use of this document** — it's
proprietary/vendor-specific, confirmed by absence of any published spec, not by a source
affirmatively saying "there is no standard."

### 2.3 The reverse flow — TA → fund accounting, aggregate net flows for cash management

Directionally confirmed as a real, necessary process: fund accounting/the portfolio manager needs
the **day's aggregate net subscription/redemption total** (not per-shareholder detail) to know
whether to raise cash (sell securities to fund net redemptions) or invest incoming cash (net
subscriptions) — this is the mechanical link between TA processing and portfolio-level liquidity
management, and it's also where **Rule 22e-4** liquidity-risk-management obligations get their
operational trigger. **No specific named message/file standard was found for this flow either** —
same caveat as §2.2.

The **ICI's "Mutual Fund Operations Planning Guide for an Early Market Close"** looked like the
closest primary source to a documented end-to-end sequence covering this exact TA/fund-accounting/
NSCC interaction, but its content wasn't extractable via automated fetch in this research pass
(PDF rendered as binary/image content). **Worth reading directly if this level of process detail
matters for implementation work**: [ICI PDF](https://www.ici.org/system/files/attachments/pdf/19_ppr_marketclose.pdf)

---

## 3. TA → payment agent / bank — cash disbursement

### 3.1 ACH — no fund-specific NACHA code exists

**Correcting an assumption**: NACHA Standard Entry Class (SEC) codes are chosen by the *nature of
the receiver and authorization channel*, not by transaction purpose. There is **no NACHA code
specific to "mutual fund transaction."** A TA debiting a shareholder's bank account for a purchase,
or crediting redemption proceeds, uses:

- **PPD** (Prearranged Payment and Deposit) — for consumer accounts with a standing/recurring
  authorization (e.g., a systematic investment plan, `08` §2.3).
- **WEB** — for consumer debits initiated online/by phone (e.g., a one-off purchase via the
  investor portal).
- **CCD** (Corporate Credit or Debit) — for business-to-business movements (e.g., a retirement-plan
  sponsor's contribution).

[Nacha — Company Entry Descriptions](https://www.nacha.org/rules/risk-management-topics-company-entry-descriptions) ·
[Modern Treasury — SEC codes](https://www.moderntreasury.com/learn/sec-codes) ·
[Increase — ACH Standard Entry Class Codes](https://increase.com/documentation/ach-standard-entry-class-codes)

**Worth noting for a 2026-era document**: Nacha Risk Management Rule amendments add two new
standardized Company Entry Descriptions, **"PAYROLL"** and **"PURCHASE,"** effective March 20,
2026 — "PURCHASE" could plausibly apply to a fund-purchase debit, but no source was found
explicitly tying it to mutual fund transactions specifically. **Flag as an unverified potential
application**, not a confirmed one.

### 3.2 Fedwire — large-dollar wires, now on ISO 20022

Confirmed as the mechanism for large-dollar wire disbursements and for the underlying settlement
banking layer beneath NSCC's own net settlement (`06`, §1.3/§3.2, "Fed funds at NSCC" / National
Settlement Service). **Current development worth flagging**: the Federal Reserve's Fedwire Funds
Service **migrated to ISO 20022 messaging in July 2025**, for both domestic and cross-border
transfers. [JPMorgan — ISO 20022 migration](https://www.jpmorgan.com/insights/payments/fx-cross-border/iso-20022-migration)

### 3.3 SWIFT interbank messages (MT202/MT103, pacs.008/pacs.009) — likely not used domestically

Your instinct that SWIFT interbank messaging would be over-engineering for ordinary US domestic
fund settlement is well-founded: no evidence was found of MT202/MT103 or their ISO 20022
successors (pacs.008/pacs.009) being used in day-to-day US TA cash operations, which run on
Fedwire, ACH, and NSCC's National Settlement Service instead. SWIFT interbank messaging is for
cross-border wires, atypical for a US-only fund. **This is an absence-of-evidence inference, not a
sourced negative statement** — don't present it as a confirmed "US TAs never use SWIFT," just as
"no evidence found of routine use."

### 3.4 Custodian cash statements — MT940/942 → camt.053/052

Confirmed and directly applicable to custodian-to-TA/fund cash position reporting:

- **MT940 / camt.053** — end-of-day statement with final booked balances (the reconciliation
  format — this is what a TA would use to confirm the prior day's cash movements actually settled).
- **MT942 / camt.052** — intraday statement updates (useful for same-day visibility before
  end-of-day finality).

camt.053 is the ISO 20022 successor to MT940. [camt.052 vs 053 vs 054 explainer](https://validatefin.com/en/blog/camt-052-vs-053-vs-054) ·
[MT940 to ISO 20022 migration](https://treasuryxl.com/blog/the-future-of-financial-messaging-migrating-from-mt940-to-iso-20022/) ·
[PaymentBrief camt overview](https://paymentbrief.com/articles/camt-052-053-054-account-reporting-reference/)

**Caveat**: the formats' general banking application is well documented; a source specifically
naming a *US mutual fund custodian* using MT940/camt.053 with a TA by name wasn't found — this is
an analogy from general banking practice, not a directly cited fund-specific instance.

### 3.5 DTCC Payment aXis — narrower than shareholder disbursement

Worth an explicit correction here: **DTCC Payment aXis (`06`, §3.2) settles commissions and
mutual fund fees** (12b-1 trails, retirement-plan fees) between fund companies/TAs and
broker-dealers via NSCC net settlement — **it is not the mechanism for paying redemption proceeds
to individual shareholders.** Don't conflate the two. [DTCC Payment aXis](https://www.dtcc.com/wealth-management-services/mutual-fund-services/dtcc-payment-axis)

---

## 4. TA ↔ NSCC — order confirmation, position reconciliation, fee settlement

Already covered in full technical depth in `06` — cross-referenced here for sequencing purposes
only:

- **Fund/SERV** (§1.1 above): order entry/confirmation/net settlement.
- **Networking** (`06`, §2): Activity Report (financial) and Account Maintenance & Reconciliation
  (non-financial) — the exact split this doc's companion, `08`, is built around.
- **ACATS-Fund/SERV** (`06`, §1.5): account-level position transfers between firms.
- **DTCC Payment aXis / Commission Settlement** (`06`, §3.2, and §3.5 above): fee/commission
  settlement, distinct from shareholder cash movement.

---

## 5. TA → custodian — trade instructions and position reconciliation

The custodian (`07`, §3) holds the fund's actual cash and securities. The TA's role here is
narrower than with fund accounting: the TA reports the day's **aggregate cash need** (net
subscriptions/redemptions, per §2.3) so the custodian can fund the corresponding wire/ACH
disbursements, and receives back the cash-position confirmations described in §3.4
(MT940/942 → camt.053/052). No fund-specific standardized message format for the TA-to-custodian
instruction leg itself was independently verified in this research pass — treat this leg the same
way as §2 (proprietary/vendor file feed, likely intra-firm when custodian and TA share a servicer).

---

## 6. Shareholder reporting — TA → investor / IRS

- **Trade confirmations**: sent to the shareholder (and, per §1, back through Fund/SERV or setr to
  the distributor) after each transaction.
- **Periodic account statements**: monthly/quarterly summaries reflecting the balance-file state
  (`08`, §1).
- **Tax forms**, confirmed via IRS instructions:
  - **1099-DIV** — dividends/distributions, issued for ≥$10 of taxable income (`08`, §2.2).
  - **1099-B** — share sale/redemption proceeds, with cost-basis reporting under §6045 (`05`, §3;
    `08`, §2.4).
  - **1099-R** — retirement-account distributions (`08`, §2.3).
  - **5498** — IRA contribution reporting.
  [IRS Form 1099-B Instructions](https://www.irs.gov/instructions/i1099b) ·
  [IRS Forms 1099-R and 5498 Instructions](https://www.irs.gov/instructions/i1099r) ·
  [IRS 1099-DIV FAQ](https://www.irs.gov/faqs/interest-dividends-other-types-of-income/1099-div-dividend-income/1099-div-dividend-income)

---

## 7. Other TA data flows

- **AML/CIP** (`05`, §3): Section 326 of the USA PATRIOT Act required the joint SEC/FinCEN rule
  imposing Customer Identification Program obligations on mutual funds, including OFAC screening
  and SAR-filing procedures. No specific message-format standard was found for a TA's SAR/CTR
  filing to FinCEN beyond FinCEN's own e-filing system — flagged as a gap, not researched to
  format-level detail here. [SEC — Customer Identification Programs for Mutual Funds](https://sec.gov/rules/2003/04/customer-identification-programs-mutual-funds) ·
  [SEC AML Source Tool for Mutual Funds](https://www.sec.gov/about/divisions-offices/division-examinations/amlmfsourcetool)
- **Escheatment** (`05`, §3; `06`, §5.4): NAUPA publishes the **NAUPA II / NAUPA III standard
  electronic file format**, used by holders (including TAs) to report unclaimed mutual fund
  property to state administrators, with a dedicated field for Fund Family Name.
  [NAUPA Standard Electronic File Format](https://unclaimed.org/wp-content/uploads/NAUPAStandardElectronicFileFormat-11.20.19.pdf) ·
  [NAUPA III File Format Draft](https://unclaimed.org/wp-content/uploads/NAUPA-III-File-Format-Review-Draft-1.4.pdf)
- **Board/administrator reporting**: not substantively researched in this pass — flagged as an open
  gap. Likely centers on ICA §15(c) board-oversight obligations (`07`, §2) and the "Super Sheet"
  control-book reconciliation described in `08`, §1.6, but wasn't verified to message-format detail
  here.

---

## 8. Full step-by-step sequences

### 8.1 A subscription (purchase) order, start to finish

1. Investor places an order via the distributor's platform/advisor, or a direct fund portal.
2. Order reaches the TA — via **Fund/SERV 001 Order** (US domestic, §1.1) or **setr.010.001
   Subscription Order** (cross-border, §1.2).
3. TA validates the order (KYC/AML per §7, account status, minimum investment, blue-sky
   eligibility per `07`, §17) — but **cannot finalize pricing yet**.
4. TA waits for fund accounting to strike the day's NAV after market close (Rule 22c-1, §2.1) —
   via the proprietary/unstandardized feed described in §2.2.
5. TA prices the order at that NAV, calculates shares issued, and posts the transaction — this is
   simultaneously an **activity file entry (credit)** and a **balance file update**, per `08`, §1.
6. TA sends confirmation back through the same channel it came in on (**Fund/SERV** confirmation or
   **setr.012.001 Subscription Order Confirmation**).
7. TA reports the day's aggregate net inflow to fund accounting (§2.3) so incoming cash gets
   invested appropriately.
8. TA (or, per the actual cash rail, the fund's payment processor) debits the investor's bank
   account via **ACH (PPD or WEB, §3.1)** or processes the incoming wire via **Fedwire (§3.2)**.
9. Custodian confirms the cash receipt via an end-of-day **MT940/camt.053** statement (§3.4).
10. TA generates the shareholder's trade confirmation statement (§6).

### 8.2 A redemption order, start to finish

1. Investor requests redemption via portal/advisor.
2. Order reaches TA via **Fund/SERV 001 Order** (sell side) or **setr.004.001 Redemption Order**.
3. TA validates the order and waits for the day's NAV (same Rule 22c-1 dependency as §8.1).
4. TA prices the redemption, debits the shareholder's share balance (activity file/balance file,
   `08`, §1), and confirms via **Fund/SERV** or **setr.006.001 Redemption Order Confirmation**.
5. TA reports the day's aggregate net outflow to fund accounting (§2.3) — this may trigger a
   security sale if the fund's cash buffer is insufficient (Rule 22e-4 liquidity management).
6. Custodian funds the disbursement; TA/payment agent disburses proceeds via **ACH credit** or
   **wire (Fedwire)**, per §3.1/§3.2.
7. **Hard deadline**: proceeds must be paid within **7 calendar days** of tender, per **ICA
   §22(e)** — confirmed directly, including the "unreasonable, undisclosed and unforeseen delays"
   rationale in the statute's legislative purpose. The one narrow, SEC-sanctioned exception: a
   **June 1, 2018 SEC no-action letter** permits a TA to temporarily delay disbursement when
   financial exploitation of a "Specified Adult" (an elder or vulnerable-adult investor) is
   suspected — an exception to, not a repeal of, the 7-day rule.
   [SEC No-Action Letter, ICI, June 1 2018](https://www.sec.gov/divisions/investment/noaction/2018/investment-company-institute-060118-22e.htm) ·
   [SEC Committee of Annuity Insurers §22(e) materials](https://www.sec.gov/investment/cai-22e-041124)
8. TA generates the redemption confirmation and, at year-end, the **1099-B** cost-basis report
   (§6).

### 8.3 A dividend/capital-gain distribution cycle

1. Fund accounting/the board declares a dividend rate (record date, ex-date, payable date), per
   `07`, §2.
2. Fund accounting transmits the dividend rate to the TA — same unstandardized proprietary feed as
   §2.2.
3. TA calculates each shareholder's dividend amount based on shares held as of the record date, and
   applies each account's cash-vs-reinvest election (a non-financial maintenance attribute, `08`,
   §3).
4. **Reinvest accounts**: TA creates a new purchase transaction (shares issued from the
   distribution) — no cash leaves the fund; this posts as a credit to both the activity file and
   balance file (`08`, §1).
5. **Cash accounts**: TA instructs disbursement via **ACH (PPD)** or check, per §3.1.
6. TA reports the total cash-paid vs. reinvested split back to fund accounting so the fund's NAV
   and cash position reflect the distribution correctly.
7. At year-end, TA generates **1099-DIV** for all shareholders who received ≥$10 in distributions
   (§6), correctly splitting ordinary dividends (Box 1a) from capital gain distributions (Box 2a —
   always long-term regardless of the fund's actual holding period, per `08`, §2.2).

---

## 9. Flagged gaps / not independently verified

Given how much of this document depends on getting specific message codes right, this list is
longer and more load-bearing than in prior docs — **do not fill these gaps with assumptions**:

1. **Any specific legacy ISO 15022 MT code** dedicated to investment fund orders — not confirmed;
   describe the legacy layer generically only.
2. **`setr.020`** — does not appear to exist in the current setr catalogue; don't use it.
3. **A NACHA SEC code specific to "mutual fund transactions"** — does not exist; use PPD/WEB/CCD
   per the receiver/channel, not a fund-specific code. The new 2026 "PURCHASE" Company Entry
   Description is plausible but not confirmed for fund use.
4. **A named standard file format for NAV transmission** (fund accounting → TA) — proprietary/
   vendor-specific (e.g., Milestone Group), not a published industry standard.
5. **A named standard format for TA → fund-accounting aggregate net-flow reporting** —
   directionally confirmed as a real process, format unconfirmed.
6. **Whether US TAs affirmatively never use SWIFT MT103/pacs.008** — absence-of-evidence
   inference, not a sourced negative.
7. **Content of the ICI market-close operations PDF and DTCC Fund/SERV/Networking lifecycle
   diagrams** — located but not text-extracted in this research pass; read them directly before
   citing specifics beyond what's in `06`.
8. **DTCC's timeline/scope for an ISO 20022 interface layered onto Fund/SERV** — mentioned in
   secondary summaries, not confirmed against a primary DTCC page with dates.
9. **Board/administrator reporting formats** — not researched to message-format detail.
10. **Custodian use of MT940/camt.053 specifically in a mutual fund context** — the formats'
    general banking application is well sourced; fund-custody-specific usage is an analogy, not a
    directly cited instance.
11. **TA → FinCEN SAR/CTR filing format** — not researched beyond confirming FinCEN's own e-filing
    system exists.

## Sources

- [Redemption Order (setr.004.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-004-001-Redemption-Order.html) · [Redemption Order Confirmation (setr.006.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-006-001-Redemption-Order-Confirmation.html) · [Subscription Order (setr.010.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-010-001-Subscription-Order.html) · [Subscription Order Confirmation (setr.012.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-012-001-Subscription-Order-Confirmation.html) · [Switch Order (setr.013.001)](https://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-013-001-Switch-Order.html) · [full setr catalogue](https://www.iotafinance.com/en/SWIFT-ISO20022-Business-area-setr-Securities-Trade.html) · [setr.065.001](http://www.iotafinance.com/en/SWIFT-ISO20022-Message-setr-065-001-Investment-Fund-Order-Cancellation-Request.html)
- [SWIFT Standards MX Funds Message Definition Report](https://www2.swift.com/knowledgecentre/rest/v1/publications/stdsmx_funds_mdrs/8.0/SR2022_MX_Funds_MDR1_Standards.pdf) · [SWIFT Investment Funds ISO 20022 training](https://www.swift.com/myswift/services/training/swift-training-catalogue/browse-swift-training-catalogue/investment-funds-iso-20022-messages) · [SWIFT Category 5 MDR](https://www2.swift.com/knowledgecentre/rest/v1/publications/us5md_20210723/2.0/us5md_20210723.pdf)
- [DTCC Fund/SERV](https://dtcclearning.com/products-and-services/mutual-fund-services/fund-serv.html) · [DTCC Settlement Transformation FAQ](https://www.dtcc.com/initiatives/Content/2-sett_trans_faqs/2_sett_trans_faq.htm) · [ISO 20022 adoption initiatives report](https://www.iso20022.org/sites/default/files/2020-03/ISOInitiatives_July2018.pdf) · [DTCC Payment aXis](https://www.dtcc.com/wealth-management-services/mutual-fund-services/dtcc-payment-axis)
- [17 CFR 270.22c-1, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.22c-1) · [SEC 2003 Amendments to Pricing Rules](https://www.sec.gov/rules-regulations/2003/12/amendments-rules-governing-pricing-mutual-fund-shares) · [17 CFR 270.2a-4, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.2a-4) · [ICI FAQs: Mutual Fund Share Pricing](https://www.ici.org/faqs/faq/mfs/faqs_navs)
- [Milestone Group NAV Unit Pricing](https://www.milestonegroup.com/solutions/fund-processing/nav-unit-pricing) · [ICI — Mutual Fund Operations Planning Guide for an Early Market Close](https://www.ici.org/system/files/attachments/pdf/19_ppr_marketclose.pdf)
- [Nacha — Company Entry Descriptions](https://www.nacha.org/rules/risk-management-topics-company-entry-descriptions) · [Modern Treasury — SEC codes](https://www.moderntreasury.com/learn/sec-codes) · [Increase — ACH SEC Codes](https://increase.com/documentation/ach-standard-entry-class-codes)
- [JPMorgan — Fedwire ISO 20022 migration](https://www.jpmorgan.com/insights/payments/fx-cross-border/iso-20022-migration)
- [camt.052 vs 053 vs 054 explainer](https://validatefin.com/en/blog/camt-052-vs-053-vs-054) · [MT940 to ISO 20022 migration](https://treasuryxl.com/blog/the-future-of-financial-messaging-migrating-from-mt940-to-iso-20022/) · [PaymentBrief camt overview](https://paymentbrief.com/articles/camt-052-053-054-account-reporting-reference/)
- [SEC No-Action Letter, ICI, June 1 2018 (ICA §22(e))](https://www.sec.gov/divisions/investment/noaction/2018/investment-company-institute-060118-22e.htm) · [SEC Committee of Annuity Insurers §22(e) materials](https://www.sec.gov/investment/cai-22e-041124)
- [SEC — Customer Identification Programs for Mutual Funds](https://sec.gov/rules/2003/04/customer-identification-programs-mutual-funds) · [SEC AML Source Tool for Mutual Funds](https://www.sec.gov/about/divisions-offices/division-examinations/amlmfsourcetool)
- [NAUPA Standard Electronic File Format](https://unclaimed.org/wp-content/uploads/NAUPAStandardElectronicFileFormat-11.20.19.pdf) · [NAUPA III File Format Draft](https://unclaimed.org/wp-content/uploads/NAUPA-III-File-Format-Review-Draft-1.4.pdf)
- [IRS Form 1099-B Instructions](https://www.irs.gov/instructions/i1099b) · [IRS Forms 1099-R and 5498 Instructions](https://www.irs.gov/instructions/i1099r) · [IRS 1099-DIV FAQ](https://www.irs.gov/faqs/interest-dividends-other-types-of-income/1099-div-dividend-income/1099-div-dividend-income)
