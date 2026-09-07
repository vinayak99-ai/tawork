# Transfer Agent Data Flows — Direct-to-TA Institutional Money Market Fund

How data moves when a **domestic US corporate treasury client** invests directly in a **domestic
(Rule 2a-7) money market fund** through an **investor portal that connects straight to the
transfer agent**. This is a **direct-at-fund** distribution model. Companion to `05`, `07`, and
`08`.

**Real-world precedent for this pattern**: this isn't a hypothetical simplification — it's how
some funds actually operate. Franklin Templeton's OnChain Fund (`01`, §5) sells directly to
investors, distributing only via its own Benji App (individuals) or **Institutional Web Portal**
(institutions) — the same direct-at-fund shape modeled here, just without the blockchain layer.

---

## ⚡ TL;DR

- **The actors, direct-at-fund**: Corporate Treasury Client → Investor Portal → Transfer Agent,
  with the TA coordinating with **Fund Accounting** (NAV), a **Custodian Bank** (fund assets), a
  **Payment rail** (Fedwire/ACH), and **Shareholder/IRS Reporting** — a short, direct chain end to
  end.
- **Money market fund NAV mechanics matter here**: a Rule 2a-7 government/retail money fund
  targets a stable **$1.00 NAV** via amortized cost/penny rounding, and — specifically to serve
  institutional cash-management clients like a corporate treasury — many such funds **strike NAV
  multiple times a day** (commonly 2–3 times: morning, midday, afternoon) so the client can invest
  or redeem more than once in a single business day.
- **Payment rail skews toward Fedwire, not ACH**, for this client type: corporate treasury cash
  management moves large sums same-day, which is Fedwire's whole purpose (real-time gross
  settlement, immediate and final) — ACH's batch/scheduled-window settlement is a secondary option
  here, not the primary one.
- **KYC/AML looks different for a legal-entity customer**: opening the account requires FinCEN's
  **Beneficial Ownership Rule** (31 CFR 1010.230) — identifying the natural person(s) who own
  **≥25% equity** and the one person with significant management control, not just screening an
  individual's own identity.
- **Tax treatment changes materially for a domestic corporation**: NRA withholding never applies
  (the investor is domestic), and — with a properly filed Form W-9 claiming corporate exempt-payee
  status — **backup withholding doesn't apply either, and the fund generally isn't even required to
  issue a 1099-DIV at all**. That exemption evaporates the moment backup withholding actually gets
  triggered (e.g., no valid W-9 on file) — then the normal reporting obligations kick back in.
- **Seven scenarios in §7**: subscription, redemption, dividend distribution (cash), dividend
  reinvestment, capital gain distribution (cash), capital gain reinvestment, and the corporate
  tax-certification scenario.

---

## 1. The actors in this model

| Actor | Role |
|---|---|
| **Corporate Treasury Client** | The investor — a domestic US corporation managing its own cash reserves, not an individual retail investor. |
| **Investor Portal** | A direct channel into the TA's own order-processing system — proprietary web session, secure file/SFTP batch, or API. |
| **Transfer Agent (TA)** | Same core function as `05` — maintains the master securityholder file, processes orders, applies KYC/AML, generates statements and tax forms. |
| **Fund Accounting** | Strikes the fund's NAV (potentially multiple times a day for this fund type — §3). |
| **Custodian Bank** | Holds the fund's actual portfolio securities and cash (`07`, §3). |
| **Payment System** | Fedwire (primary, given treasury-scale same-day movements) or ACH (secondary) — §4. |
| **Shareholder/IRS Reporting** | Account statements, trade confirmations, and (where applicable) 1099-DIV — §6. |

The TA talks directly to the client's portal on one side and to Fund Accounting/Custodian/Payment
rails on the other.

---

## 2. Account opening & KYC/AML for a legal-entity customer

Two things are specific to a corporate customer, versus the individual-investor case already
covered in `05`, §3:

- **FinCEN's Beneficial Ownership Rule (31 CFR 1010.230)**: when the TA opens a new account for a
  legal entity, it must identify and verify **(1) each individual who owns ≥25% of the entity's
  equity**, and **(2) one individual with significant responsibility to control/manage/direct the
  entity** (an executive officer, senior manager, or similar). This is on top of, not instead of,
  the entity's own identification (CIP under the fund's AML program, `05` §3).
- **When re-verification is required**: historically this had to be reassessed at various trigger
  points; a **FinCEN exceptive-relief order dated February 13, 2026** now limits mandatory
  identification/verification of beneficial owners to **(1) initial account opening, (2) whenever
  the institution has reason to doubt previously obtained beneficial-ownership information, and
  (3) other risk-based triggers under the institution's own CDD procedures** — not a rigid
  recurring schedule.
- **Form W-9 at onboarding**: this is also where the corporation claims its **exempt payee**
  status for backup-withholding purposes (relevant to the tax scenario in §7.7) — the exemption
  isn't automatic just because the investor is a corporation; it has to be properly certified on
  the W-9.

[31 CFR 1010.230, eCFR](https://www.ecfr.gov/current/title-31/subtitle-B/chapter-X/part-1010/subpart-B/section-1010.230) ·
[FinCEN — CDD Rule FAQs](https://www.fincen.gov/resources/statutes-and-regulations/cdd-rule-faqs) ·
[FinCEN Order — Exceptive Relief, Feb 13 2026](https://www.fincen.gov/system/files/2026-02/FinCEN-Order-CCDExceptiveRelief.pdf)

---

## 3. Order intake — investor portal directly to the TA

The flow is simply:

1. Corporate treasury client logs into the investor portal (a direct channel into the TA's system
   — the same "institutional web portal" pattern documented for BENJI/FOBXX in `01`, §5).
2. The order (subscription, redemption, etc.) is submitted straight into the TA's order-processing
   and recordkeeping system.
3. The TA validates and — once NAV is available (§4) — prices and posts it directly.

This is deliberately the simplest possible order-intake path in this doc set: one direct
connection.

---

## 4. TA ↔ Fund Accounting — the NAV cycle for a money market fund

The **Rule 22c-1** forward-pricing dependency and **Rule 2a-4** current-NAV mechanics apply here —
an order still has to wait for the **next NAV computed after receipt**. What's specific to a money
market fund and a treasury-cash-management client:

- A **Rule 2a-7 government/retail money market fund** targets a stable **$1.00 NAV** using
  amortized-cost/penny-rounding valuation (`01`, §1, covers this exact mechanic for FOBXX).
- **Multiple intraday NAV strikes**: money market funds serving institutional cash-management
  clients commonly strike NAV **more than once a day — typically 2–3 times (morning, midday,
  afternoon before close)** — specifically so a corporate treasury client can invest or redeem
  more than once in the same business day rather than waiting for a single end-of-day NAV. Intraday
  NAV striking requires the TA to apply each distinct NAV to the correct transactions/account
  balances at the right time of day, with corresponding effects on settlement timing.
  [ICI — Intraday Processing for Floating NAV Money Market Funds Working Group](https://www.ici.org/ops_mmf_reform/intraday)

The fund-accounting-to-TA NAV feed itself remains **proprietary/vendor-specific** — no named
industry-standard format.

---

## 5. TA → payment / custodian — cash movement

### 5.1 Fedwire vs. ACH for a corporate treasury client

Both rails remain available, but the emphasis shifts for this client type: a corporate treasury
client moving substantial cash same-day is exactly Fedwire's use case (real-time gross settlement
— one transfer at a time, immediate and final). ACH's batch/scheduled-window model (§5.2) is more
suited to smaller or recurring movements. See the **ACH/NACHA/Fedwire primer** below for what each
actually is.

### 5.2 Primer — what ACH, NACHA, and Fedwire actually are

- **ACH (Automated Clearing House)** is the **network itself** — the US electronic payment system
  that moves money between bank accounts. It's **batch-based, not real-time**: transactions are
  collected and settled in scheduled windows (Same Day ACH narrows the window but isn't
  instant/real-time the way Fedwire is).
- **NACHA is not the network — it's the rule-maker.** NACHA (National Automated Clearing House
  Association) is a private, nonprofit association that **writes and enforces the ACH Network's
  operating rules** — including the SEC (Standard Entry Class) codes below. NACHA itself never
  touches or moves any money.
- **Who actually clears and settles ACH transactions**: two **"ACH Operators"** do the real work
  under NACHA's rules — **FedACH** (Federal Reserve Banks, the only public-sector operator) and
  **EPN (Electronic Payments Network)** (The Clearing House, owned by ~25 large banks, the only
  private-sector operator). The two are fully interoperable and exchange files multiple times a
  day.
- **Fedwire (Fedwire Funds Service)** is a **completely separate Federal Reserve system** — not
  part of the ACH Network, not governed by NACHA. It's **real-time gross settlement (RTGS)**: each
  wire settles individually, immediately, and irrevocably.

| | ACH | Fedwire |
|---|---|---|
| **Governed by** | NACHA (private rules body) | The Federal Reserve directly |
| **Operated by** | FedACH (the Fed) + EPN (The Clearing House) | Federal Reserve Banks only |
| **Settlement style** | Batched, net-settled, scheduled windows | Real-time gross settlement — immediate and final |
| **Fit for this scenario** | Smaller/recurring treasury movements | **Primary rail** — large, same-day treasury cash management |

[Modern Treasury — A Complete Primer to ACH](https://www.moderntreasury.com/journal/a-complete-primer-to-ach-understanding-the-four-key-players) ·
[Federal Reserve History — Automated Clearing House Payments](https://www.federalreservehistory.org/essays/automated-clearing-house) ·
[Wikipedia — FedACH](https://en.wikipedia.org/wiki/FedACH) ·
[Wikipedia — Electronic Payments Network](https://en.wikipedia.org/wiki/Electronic_Payments_Network) ·
[JPMorgan — Fedwire ISO 20022 migration](https://www.jpmorgan.com/insights/payments/fx-cross-border/iso-20022-migration)

### 5.3 NACHA entry classes — still no fund-specific code

NACHA Standard Entry Class codes are chosen by **receiver type and authorization channel**, not
transaction purpose — there's no NACHA code specific to "mutual fund transaction." A
corporate-to-corporate ACH movement (where used) would typically be **CCD** (Corporate Credit or
Debit); a consumer-style debit would be PPD/WEB, but those are less likely for a corporate
treasury account specifically.
[Nacha — Company Entry Descriptions](https://www.nacha.org/rules/risk-management-topics-company-entry-descriptions)

### 5.4 Custodian cash confirmation

The custodian confirms cash movements via end-of-day **MT940/camt.053** (final booked balances) or
intraday **MT942/camt.052** statements.

---

## 6. Shareholder reporting / IRS reporting

Trade confirmations and account statements work the same as the general case (`08`, §1). The tax
reporting mechanics are where a domestic corporate investor genuinely differs — covered in full in
§7.7 below, but the headline: **the fund is generally not required to issue a 1099-DIV to a
corporation at all**, regardless of distribution amount, unless backup withholding was actually
triggered on that payment.

---

## 7. Scenarios — trigger and steps

Same format as before: **what starts it**, then a simple `Entity → Entity: what happens` list.

### 7.1 Scenario: Subscription (purchase) order

**Trigger**: corporate treasury client wants to invest cash into the fund.

1. Corporate Treasury Client → Investor Portal: submits a purchase order.
2. Investor Portal → TA: order arrives directly (§3).
3. TA: validates the order (KYC/AML per §2, account status) — **cannot price it yet**.
4. Fund Accounting → TA: sends the applicable NAV once struck — possibly one of several intraday
   strikes for this fund type (§4).
5. TA: prices the order at that NAV, calculates shares issued, posts the transaction — an
   **activity file credit** and a **balance file update** simultaneously (`08`, §1).
6. TA → Investor Portal: sends confirmation directly back to the client.
7. TA → Fund Accounting: reports the net inflow so incoming cash gets invested appropriately.
8. TA/Payment System → Client's bank: receives the cash via **Fedwire** (primary, §5.1) or ACH
   (secondary, §5.3).
9. Custodian → TA: confirms the cash receipt via **MT940/camt.053** (§5.4).
10. TA → Client: sends the trade confirmation statement (§6).

### 7.2 Scenario: Redemption order

**Trigger**: corporate treasury client needs cash back from the fund.

1. Corporate Treasury Client → Investor Portal: submits a redemption request.
2. Investor Portal → TA: order arrives directly.
3. TA: validates the order, waits for the applicable NAV (same dependency as §7.1).
4. TA: prices the redemption, debits the client's share balance (`08`, §1).
5. TA → Fund Accounting: reports the net outflow — may trigger a security sale if the fund's cash
   buffer is insufficient (Rule 22e-4 liquidity management).
6. Custodian → TA/Payment System: funds the disbursement.
7. TA/Payment System → Client's bank: pays proceeds, typically via **Fedwire** given the treasury
   context and likely same-day expectation. **Hard deadline: within 7 calendar days of tender (ICA
   §22(e))** — in practice, same-day or next-day for a treasury cash-management fund, well inside
   that statutory ceiling.
   [SEC No-Action Letter, ICI, June 1 2018](https://www.sec.gov/divisions/investment/noaction/2018/investment-company-institute-060118-22e.htm)
8. TA → Client: generates the redemption confirmation now; a 1099-B is generally **not** required
   for a corporate holder (see the general corporate-exemption logic in §7.7 — 1099-B follows a
   similar exempt-recipient pattern to 1099-DIV for corporations, though this doc's primary
   research focused on the dividend/distribution side).

### 7.3 Scenario: Dividend distribution (cash)

**Trigger**: fund declares a dividend; the corporate account is elected for cash payout.

1. Fund Board/Accounting → TA: declares the dividend rate and record/ex/payable dates (`07`, §2).
2. TA: calculates the client's dividend amount (shares held as of record date × rate); the
   account's election is **Cash**.
3. TA → Payment System: instructs disbursement via **Fedwire** (typical for a treasury account) or
   ACH.
4. Payment System → Client's bank: pays the cash amount.
5. TA → Fund Accounting: reports the total cash paid out.
6. TA → Shareholder Reporting: posts the payment to the activity file/balance file and the
   client's account statement (`08`, §1).
7. TA → IRS Reporting (year-end): **only if** backup withholding applied on this payment (see
   §7.7) — otherwise, per the corporate exemption, **no 1099-DIV is required for this
   distribution at all**.

### 7.4 Scenario: Dividend reinvestment

**Trigger**: same dividend declaration as §7.3, account elected to reinvest — the more common
default for a treasury cash-sweep arrangement, where the point is to keep idle cash working rather
than pull it out.

1. Fund Board/Accounting → TA: same dividend declaration as §7.3, step 1.
2. TA: calculates the client's dividend amount; the account's election is **Reinvest**.
3. TA: uses the dividend amount to buy new shares at the current NAV — **no cash leaves the
   fund**; posts as a credit to both the activity file and balance file (`08`, §1), with a new
   cost-basis lot for the new shares.
4. TA → Fund Accounting: reports the total dividends reinvested (shares issued, no net cash
   impact).
5. TA → Shareholder Reporting: updates the account's share balance and statement.
6. TA → IRS Reporting (year-end): same exemption logic as §7.3 — reinvesting doesn't change
   whether a 1099-DIV is required; that still turns on backup-withholding status (§7.7), not on
   the cash-vs-reinvest election.

### 7.5 Scenario: Capital gain distribution (cash)

**Trigger**: fund declares a capital gain distribution — **worth flagging as unusual for this
specific fund type**: a stable-NAV, Rule 2a-7 government/retail money market fund using
amortized-cost valuation rarely realizes meaningful capital gains, since it generally holds
short-term instruments to maturity rather than trading them for gains. This scenario is included
for completeness (and would be far more routine for a floating-NAV or longer-duration fund), but
don't expect it to be a regular event for the specific fund modeled in this doc.

1–6. Same steps as §7.3 (substitute "capital gain distribution" for "dividend").
7. TA → IRS Reporting (year-end): if required at all (same corporate-exemption logic as §7.3),
   reported on **Form 1099-DIV, Box 2a** (Total Capital Gain Distributions — always long-term
   regardless of the fund's actual holding period, per `08`, §2.2).

### 7.6 Scenario: Capital gain reinvestment

**Trigger**: same rare capital gain declaration as §7.5, account elected to reinvest. Mechanically
identical to §7.4 — new shares purchased at current NAV, new cost-basis lot, no cash leaves the
fund — with the same "unusual for this fund type" caveat as §7.5.

### 7.7 Scenario: Tax certification for a domestic corporate investor

**Trigger**: a distribution is payable to the corporate treasury client, and the TA needs to
determine what (if any) withholding and reporting applies. This is the scenario that runs through
Account Opening → Accounting → Payments → Shareholder Reporting → IRS Reporting — reworked here
for a domestic corporation rather than the general case.

**Branch A — valid Form W-9 on file, exempt payee status properly claimed** (the expected case for
a properly onboarded corporate treasury account):

1. TA (at account opening, §2): confirms the corporation furnished a valid **Form W-9** with the
   correct exempt-payee code for interest/dividend-type payments.
2. TA (at each distribution): confirms this certification is still current — no backup withholding
   applies.
3. TA → Fund Accounting: reports the **gross distribution** — gross and net are the same amount
   here, since nothing is withheld.
4. TA/Payment System → Client's bank: pays the **full gross amount**.
5. TA → Shareholder Reporting: reflects the full distribution on the account statement.
6. TA → IRS Reporting (year-end): **the fund is not required to file Form 1099-DIV for dividends
   paid to a C corporation, S corporation, or most tax-exempt organizations, regardless of the
   amount** — this exemption is unconditional on amount (unlike the individual-investor $10
   threshold) but is **overridden the moment backup withholding is actually applied** on a
   payment, or if liquidation proceeds reach ≥$600 (a narrower trigger not relevant to routine
   dividend/cap-gain distributions).

**Branch B — no valid W-9 on file** (e.g., certification lapsed, TIN mismatch — the exemption
requires an affirmative, valid certification; it isn't automatic just because the payee happens to
be a corporation):

1. TA: detects the missing/invalid certification at distribution-calculation time.
2. TA: applies standard **backup withholding at 24%** of the gross distribution (IRC §3406) — the
   same mechanism that would apply to any payee, corporate or not, lacking a valid TIN
   certification.
3. TA → Fund Accounting: reports gross distribution, amount withheld, and net payable.
4. TA/Payment System → Client's bank: pays only the **net (after-withholding)** amount.
5. TA/Payment System → IRS: remits the withheld amount via the **Electronic Federal Tax Payment
   System (EFTPS)** — a periodic deposit, not a per-payment remittance.
6. TA → Shareholder Reporting: reflects both the gross distribution and the withheld amount.
7. TA → IRS Reporting (year-end): **Form 1099-DIV is now required** despite the corporate
   exemption in Branch A, specifically because backup withholding occurred — gross distribution in
   the income box, withheld amount in the federal income tax withheld box, plus the payer's own
   **Form 945** filing for total backup withholding remitted across all payees.

Note: **NRA (nonresident alien) withholding and Form 1042-S never apply in this scenario** — the
investor is explicitly a domestic US corporation, not a foreign person, so Chapter 3 withholding
is out of scope entirely here (it would apply to a foreign corporate or individual investor
instead, a different scenario this doc doesn't model).

[Wikipedia — Backup Withholding](https://en.wikipedia.org/wiki/Backup_withholding) ·
[IRS Instructions for the Requester of Form W-9](https://www.irs.gov/instructions/iw9) ·
[BoomTax/LegalClarity summaries of 1099-DIV corporate exemption](https://legalclarity.org/do-corporations-get-1099-forms/)
(secondary sources summarizing IRS Form 1099-DIV instructions — corporate exemption itself is
standard IRS guidance, cross-checked across multiple summaries agreeing on the same rule)

---

## 8. Flagged gaps / not independently verified

- Whether 1099-B for redemption proceeds follows the exact same corporate-exemption pattern as
  1099-DIV — the research for this pass focused on the dividend/distribution side; §7.2's note on
  this is a reasonable inference from the general "exempt recipient" concept in IRS reporting
  rules, not independently confirmed against the 1099-B instructions specifically.
  See `05`, §3 and `08`, §2.4 for the general 1099-B/cost-basis framework this would sit within.
- Exact mechanics of a corporate treasury client's investor-portal technology (whether TA-hosted,
  fund-complex-hosted, or integrated via a separate treasury management system) — described
  generically here by analogy to the documented BENJI/FOBXX Institutional Web Portal model (`01`,
  §5), not independently verified for money market funds generally.
- Whether *all* Rule 2a-7 government/retail money market funds offering direct institutional
  access strike multiple intraday NAVs, or only some — the ICI source describes this as common
  practice for funds serving cash-management clients, not a universal requirement.

## Sources

- [31 CFR 1010.230, eCFR](https://www.ecfr.gov/current/title-31/subtitle-B/chapter-X/part-1010/subpart-B/section-1010.230) · [Cornell LII mirror](https://www.law.cornell.edu/cfr/text/31/1010.230)
- [FinCEN — CDD Rule FAQs](https://www.fincen.gov/resources/statutes-and-regulations/cdd-rule-faqs)
- [FinCEN Order — Exceptive Relief from Repeat Beneficial Ownership Verification, Feb 13 2026](https://www.fincen.gov/system/files/2026-02/FinCEN-Order-CCDExceptiveRelief.pdf)
- [17 CFR 270.22c-1, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.22c-1) · [17 CFR 270.2a-4, Cornell LII](https://www.law.cornell.edu/cfr/text/17/270.2a-4)
- [ICI — Intraday Processing for Floating NAV Money Market Funds Working Group](https://www.ici.org/ops_mmf_reform/intraday)
- [Modern Treasury — A Complete Primer to ACH: Understanding the Four Key Players](https://www.moderntreasury.com/journal/a-complete-primer-to-ach-understanding-the-four-key-players) · [Federal Reserve History — Automated Clearing House Payments](https://www.federalreservehistory.org/essays/automated-clearing-house) · [Wikipedia — FedACH](https://en.wikipedia.org/wiki/FedACH) · [Wikipedia — Electronic Payments Network](https://en.wikipedia.org/wiki/Electronic_Payments_Network)
- [Nacha — Company Entry Descriptions](https://www.nacha.org/rules/risk-management-topics-company-entry-descriptions)
- [JPMorgan — Fedwire ISO 20022 migration](https://www.jpmorgan.com/insights/payments/fx-cross-border/iso-20022-migration)
- [camt.052 vs 053 vs 054 explainer](https://validatefin.com/en/blog/camt-052-vs-053-vs-054) · [MT940 to ISO 20022 migration](https://treasuryxl.com/blog/the-future-of-financial-messaging-migrating-from-mt940-to-iso-20022/)
- [SEC No-Action Letter, ICI, June 1 2018 (ICA §22(e))](https://www.sec.gov/divisions/investment/noaction/2018/investment-company-institute-060118-22e.htm)
- [Wikipedia — Backup Withholding](https://en.wikipedia.org/wiki/Backup_withholding) · [IRS Instructions for the Requester of Form W-9](https://www.irs.gov/instructions/iw9) · [LegalClarity — Do Corporations Get a 1099?](https://legalclarity.org/do-corporations-get-1099-forms/) · [BoomTax — 1099-DIV Filing Threshold](https://boomtax.com/tax-forms/what-is-1099-div-reporting-threshold)
