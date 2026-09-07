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
- **Blue Sky reporting is real but federally preempted for most of what it used to cover**: since
  the fund's shares are a registered '40 Act security, **NSMIA (1996)** preempts states from
  imposing their own registration/merit review — but states still get to require a **notice
  filing plus a fee**, and there's a **separate federal annual filing (Form 24F-2, Rule 24f-2)**
  that runs on the same underlying data: the fund's aggregate net share sales for the fiscal year,
  sourced directly from the TA's own records.
- **Wash sale rules technically apply to this fund but almost never actually trigger**: IRC §1091
  applies to corporations exactly like individuals (no exemption), but a stable-$1.00-NAV money
  market fund produces **$0 gain/loss on essentially every redemption** (proceeds = basis), so
  there's nothing to disallow. It's floating-NAV money market funds where this genuinely matters —
  and where the IRS created a specific de minimis exception (Notice 2013-48) for exactly that
  reason.
- **Escheatment is a real TA obligation but, like wash sales, an edge case for this specific
  client profile**: a corporate treasury cash-sweep account transacts too often to plausibly go
  dormant for the years it takes to trigger unclaimed-property rules. It's included anyway because
  a corporate holder raises a genuinely open question — RUUPA repealed the old "business-to-
  business" exemption that used to exempt/defer property owed between businesses, but whether
  that exemption (where it still exists in non-RUUPA states) has ever applied to mutual fund share
  holdings specifically isn't settled by anything found in this research pass.
- **Ten scenarios in §7**: subscription, redemption, dividend distribution (cash), dividend
  reinvestment, capital gain distribution (cash), capital gain reinvestment, the corporate
  tax-certification scenario, the annual Blue Sky/Rule 24f-2 notice filing cycle, wash sale
  determination on a redemption, and escheatment of a dormant account.

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
| **Fund Counsel / Compliance** | Prepares and files the fund's federal and state securities-law notice filings (§7.8) — a periodic, fiscal-year-driven flow rather than a per-transaction one. |
| **SEC / State Securities Regulators** | Recipients of the annual federal and state notice filings that keep the fund's shares eligible for continued sale (§7.8). |

The TA talks directly to the client's portal on one side and to Fund Accounting/Custodian/Payment
rails on the other. Fund Counsel/Compliance sits one step removed from the daily transaction flow
— it draws periodically on the TA's aggregated sales data rather than participating in individual
orders.

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

### 7.8 Scenario: Blue Sky / annual state notice filing and Rule 24f-2

**Trigger**: this one isn't per-transaction — it's a **fiscal-year-end cycle**, but it depends on
the same TA-sourced sales data as every other scenario in this doc, and it's what keeps the fund
legally eligible to keep accepting orders like the corporate treasury client's in §7.1 in every
state where it has investors. Two related but distinct filings run off this same data: a federal
one (Rule 24f-2) and a state one ("Blue Sky").

**Background, briefly**: because the fund's shares are registered under the '40 Act, the
**National Securities Markets Improvement Act of 1996 (NSMIA)** preempts states from imposing
their own registration or merit-review requirements on it — states can no longer make the fund
qualify its offering state-by-state the way a non-covered security would. What NSMIA leaves in
place is narrower: states can still require a **notice filing plus a fee** for the privilege of
being sold in-state. Separately, at the federal level, **Rule 24f-2** under the Investment Company
Act lets an open-end fund register an *indefinite* number of shares up front, in exchange for
filing an **annual notice (Form 24F-2)** with the SEC and paying a registration fee calculated
from the fiscal year's **aggregate net sales** (shares sold minus shares redeemed) — not a
per-order fee.

1. TA: over the fiscal year, every order in §7.1–§7.2 (and every other fund investor's activity)
   accumulates in the TA's activity file/balance file records (`08`, §1) — this is the same
   roll-forward data already described throughout this doc, now aggregated across the whole fiscal
   year instead of read one transaction at a time.
2. TA → Fund Counsel/Compliance: at fiscal year-end, supplies the fund's **aggregate net sales**
   for the year (and, where a specific state's fee depends on in-state sales rather than a flat
   asset-tier fee, sales attributable to investors domiciled in that state — which could include
   the state where this corporate treasury client is headquartered, if that state isn't already
   covered by an existing notice filing).
3. Fund Counsel/Compliance → SEC: files **Form 24F-2** within **90 days of fiscal year-end**,
   paying the federal registration fee computed from that net-sales figure. Interest accrues if
   the fee is paid late.
4. Fund Counsel/Compliance → State Securities Regulators: separately files a **state notice
   filing** in each state where the fund is sold — many states use a standardized form for this
   (one real example: Alabama requires **Form NF**, executed by the fund, with a consent to
   service of process, and a **fee tiered by the fund's total net assets** — $350 up to $25M,
   rising to $2,000 at $250M+). **This varies meaningfully by state** — fee structure, required
   form, and even whether a notice filing is required at all differ from state to state, and some
   states have reduced or eliminated these requirements over time. Don't assume Alabama's specific
   numbers apply elsewhere; treat it as one illustrative example, not a universal figure.
5. State Securities Regulator → Fund Counsel/Compliance: the notice filing is typically effective
   for a fixed period (Alabama: **12 months** from receipt or SEC effectiveness, whichever is
   later), with a **renewal window before expiration** (Alabama: within 60 days prior).
6. Fund Counsel/Compliance: tracks each state's renewal deadline going forward — this is a
   recurring compliance calendar, not a one-time filing, for as long as the fund keeps selling
   shares in that state.

[National Securities Markets Improvement Act background, PipelineRoad](https://pipelineroad.com/glossary/blue-sky-laws) ·
[Alabama Securities Commission — Notice Filings for Mutual Funds](https://asc.alabama.gov/statute/notice-filings-for-mutual-funds/) (illustrative single-state example, not a universal standard) ·
[17 CFR 270.24f-2, eCFR](https://www.ecfr.gov/current/title-17/chapter-II/part-270/section-270.24f-2) ·
[SEC Form 24F-2](https://www.sec.gov/files/form24f-2.pdf) ·
[DFIN — What is SEC Form 24F-2](https://www.dfinsolutions.com/knowledge-hub/thought-leadership/knowledge-resources/form-24f-2)

### 7.9 Scenario: Wash sale determination on a redemption

**Trigger**: corporate treasury client redeems shares at a tax loss, and either already holds or
subsequently acquires "substantially identical" shares within the surrounding window. **Important
caveat up front, the same way §7.5/§7.6 flagged capital gains as unusual for this fund type**:
for the specific stable-$1.00-NAV government/retail money market fund modeled throughout this doc,
this scenario is a **near-non-event** — see why in step 1 below. It's included to show the
mechanism and to flag the one real case where it actually matters.

**Background**: **IRC §1091** disallows a loss deduction on a securities sale if the taxpayer
buys "substantially identical" securities within **30 days before or after** the loss sale (a
61-day window total). **This applies to corporations the same as individuals — there's no
corporate exemption here**, unlike the backup-withholding/1099-DIV exemption in §7.7. Wash sale
and the corporate tax-reporting exemption are two unrelated rules; don't conflate them.

1. TA: at every redemption, calculates gain/loss by comparing proceeds to that specific lot's cost
   basis (`08`, §2.4). **For this fund specifically**: because it maintains a stable **$1.00 NAV**
   (amortized cost/penny rounding, §4), a shareholder buys at $1.00/share and redeems at
   $1.00/share — **proceeds equal basis, so realized gain/loss is $0 on essentially every
   redemption**. With no loss, there's nothing for §1091 to disallow. This is the fund-type-specific
   reason wash sale is a non-issue here, not a general statement that money market funds are
   exempt from wash sale rules.
2. **Where this actually matters instead**: a *floating*-NAV money market fund (institutional
   prime/municipal funds under the SEC's 2014 MMF reform) — since a floating-NAV fund's share
   price isn't fixed at $1.00, a shareholder redeeming can realize a small gain or loss, and given
   how frequently cash-management investors transact (the same buy/redeem pattern as §7.1/§7.2,
   potentially multiple times a day per §4), nearly every redemption could technically trigger a
   wash sale question without some relief.
3. **The relief that exists for that case**: **IRS Notice 2013-48** provides that a redemption
   loss on floating-NAV MMF shares is **not treated as subject to wash sale disallowance at all**
   if the loss is **≤0.5% (50 basis points) of the shareholder's basis** in those shares — a
   de minimis exception specifically designed to avoid applying lot-by-lot wash sale accounting to
   routine, frequent cash-management activity.
4. If a wash sale *were* triggered (a floating-NAV fund, a loss exceeding the 0.5% de minimis
   threshold, and a repurchase within the 61-day window): TA/cost-basis reporting system disallows
   the loss and **adds the disallowed amount to the basis of the repurchased shares** — the loss
   isn't lost permanently, it's deferred into the new lot's basis.
5. TA → Fund Accounting/Cost Basis Reporting: adjusts the repurchased lot's basis accordingly.
6. TA → IRS Reporting (year-end): a triggered wash sale is reported on **Form 1099-B, Box 1g**
   (Wash Sale Loss Disallowed) — though whether this specific reporting line follows the same
   corporate-exemption pattern discussed for 1099-DIV in §7.7 is **not independently verified**
   (see §8's gaps) — flagged rather than assumed either way.
7. TA → Shareholder Reporting: reflects the adjusted basis on the client's cost-basis statement.

[26 U.S.C. §1091, Cornell LII](https://www.law.cornell.edu/uscode/text/26/1091) ·
[IRS Notice 2013-48 — Application of Wash Sale Rules to Money Market Fund Shares](https://www.irs.gov/pub/irs-drop/n-13-48.pdf) ·
[Federal Register — Method of Accounting for Gains and Losses on Shares in Certain Money Market Funds](https://www.federalregister.gov/documents/2014/07/28/2014-17689/method-of-accounting-for-gains-and-losses-on-shares-in-certain-money-market-funds-broker-returns) ·
[Tax Notes — IRS Issues Guidance on Wash Sale Rules for Money Market Funds](https://www.taxnotes.com/research/federal/irs-guidance/revenue-procedures/irs-issues-guidance-on-wash-sale-rules-for-money-market/dpnk)

### 7.10 Scenario: Escheatment of a dormant account

**Trigger**: the corporate treasury client's account goes dormant — correspondence returned
undeliverable, no transactions, no response to outreach — for long enough to cross a state's
unclaimed-property dormancy threshold. **Same caveat pattern as §7.5/§7.6/§7.9**: for the specific
client profile modeled throughout this doc — an actively managed corporate cash-sweep vehicle
transacting frequently, potentially multiple times a day (§4) — genuine multi-year dormancy is an
edge case, not the expected path. It's included because it's a real TA obligation that could apply
to *any* account type, and because a corporate holder introduces a genuine complication worth
flagging rather than glossing over.

1. TA: per **Rule 17Ad-17** (`05`, §2), runs a database search for the "lost" account 3–12 months
   after correspondence first comes back undeliverable, then a second search 6–12 months after
   the first — at no charge to the holder.
2. TA: if the account remains unresponsive and the state's dormancy period elapses (the RUUPA model
   default is **3 years** of owner inactivity, though actual periods vary by state, per `05` §3),
   the account becomes reportable as unclaimed/abandoned property.
3. **Corporate-holder complication, worth flagging explicitly**: many states historically applied
   a **"business-to-business" (B2B) exemption** — property owed between businesses in the ordinary
   course of business was either fully exempt from escheatment or had reporting **deferred until
   the business relationship ends**. **RUUPA (2016) repealed this exemption** in states that
   adopted it verbatim, meaning a corporate-owned account is escheatable there on the same terms
   as an individual's. In states that kept an older-style unclaimed property statute, some form of
   B2B exemption may still apply. **Not independently confirmed**: whether a B2B exemption of this
   kind has ever been applied specifically to mutual fund share holdings (as opposed to its more
   commonly discussed context — unpaid vendor invoices, rebates, accounts receivable/payable) — the
   research for this pass found the B2B exemption's general shape but not a source confirming or
   ruling out its application to securities/investment-fund holdings specifically. Treat this as a
   genuinely open question for a real corporate treasury account, not a settled "yes, it applies"
   or "no, it doesn't."
4. TA: if reportable, compiles the required data (last known address, account value, holder
   identification) into the **NAUPA standard electronic file format** (`05`, §3).
5. TA → State (holder's last-known-address state): remits the property — for a mutual fund
   account, typically the shares are redeemed at the applicable NAV and the **cash proceeds** are
   what's actually escheated (consistent with how most states expect securities-related unclaimed
   property to be reported), though some states have their own specific handling for in-kind
   security transfers.
6. TA → Fund Accounting: reports the account closure and associated redemption, same as any other
   full redemption (§7.2) in terms of fund-level cash effect.
7. TA → Shareholder Reporting: the account is closed on the TA's books; the corporation's only
   path to recover the funds afterward is directly through the state's unclaimed-property claim
   process, not through the fund/TA.

[26 U.S.C./RUUPA background, `05` §2–3](./05-transfer-agent-deep-dive.md) ·
[Jones Day — Unclaimed Property Auditors Target B2B Property](https://www.jonesday.com/en/insights/2019/02/illinois-unclaimed-property-auditors-target) ·
[Baker Tilly — Unraveling the Business-to-Business Exemption](https://www.bakertilly.com/insights/unclaimed-property-unraveling-the-business-to-business-exemption) ·
[UPCR — ULC approves RUUPA](https://www.upcr-llc.com/ulc-approves-the-revised-uniform-unclaimed-property-act-ruupa/)

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
- **Blue Sky notice-filing requirements vary by state, and this doc verified only one state's
  specific numbers** (Alabama's Form NF, fee tiers, and 12-month renewal cycle) as an illustrative
  example — whether every state still requires a notice filing at all, what each charges, and
  which form each uses were not independently checked state-by-state. Some states have reduced or
  eliminated notice-filing requirements for '40 Act funds over time; treat the specific figures in
  §7.8 as one data point, not a 50-state standard.
- Whether a state's notice-filing fee is calculated from statewide asset tiers (as in Alabama) or
  from in-state sales volume specifically (which would require the TA to report state-attributed
  sales, not just an aggregate) — likely varies by state; not independently confirmed either way
  beyond the single Alabama example.
- Whether Form 1099-B's wash sale disallowed-loss reporting (Box 1g, §7.9) follows the same
  corporate-exemption pattern as 1099-DIV — not independently verified; flagged in §7.9 itself and
  restated here since it's the same open question as the first bullet above, just for a different
  box on the same form.
- **Whether a business-to-business unclaimed-property exemption has ever been applied to mutual
  fund share holdings specifically** (§7.10) — the B2B exemption's existence and RUUPA's repeal of
  it are confirmed; its application to securities/fund holdings (versus its more commonly
  discussed context of unpaid invoices/rebates) is not.
- Exact state-by-state handling of escheated mutual fund shares — whether states uniformly expect
  cash proceeds from a forced redemption (as assumed in §7.10) or sometimes expect an in-kind
  transfer of the shares themselves — not independently verified across states.

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
- [PipelineRoad — Blue Sky Laws glossary (NSMIA/covered securities background)](https://pipelineroad.com/glossary/blue-sky-laws) · [Alabama Securities Commission — Notice Filings for Mutual Funds](https://asc.alabama.gov/statute/notice-filings-for-mutual-funds/) · [17 CFR 270.24f-2, eCFR](https://www.ecfr.gov/current/title-17/chapter-II/part-270/section-270.24f-2) · [SEC Form 24F-2](https://www.sec.gov/files/form24f-2.pdf) · [DFIN — What is SEC Form 24F-2](https://www.dfinsolutions.com/knowledge-hub/thought-leadership/knowledge-resources/form-24f-2)
- [26 U.S.C. §1091, Cornell LII](https://www.law.cornell.edu/uscode/text/26/1091) · [IRS Notice 2013-48 — Application of Wash Sale Rules to Money Market Fund Shares](https://www.irs.gov/pub/irs-drop/n-13-48.pdf) · [Federal Register — Method of Accounting for Gains and Losses on Shares in Certain Money Market Funds](https://www.federalregister.gov/documents/2014/07/28/2014-17689/method-of-accounting-for-gains-and-losses-on-shares-in-certain-money-market-funds-broker-returns) · [Tax Notes — IRS Issues Guidance on Wash Sale Rules for Money Market Funds](https://www.taxnotes.com/research/federal/irs-guidance/revenue-procedures/irs-issues-guidance-on-wash-sale-rules-for-money-market/dpnk)
- [Jones Day — Unclaimed Property Auditors Target B2B Property](https://www.jonesday.com/en/insights/2019/02/illinois-unclaimed-property-auditors-target) · [Baker Tilly — Unraveling the Business-to-Business Exemption](https://www.bakertilly.com/insights/unclaimed-property-unraveling-the-business-to-business-exemption) · [UPCR — ULC approves RUUPA](https://www.upcr-llc.com/ulc-approves-the-revised-uniform-unclaimed-property-act-ruupa/) · [LegalClarity — Escheatment Laws by State](https://legalclarity.org/escheatment-laws-by-state-what-businesses-need-to-know/)
