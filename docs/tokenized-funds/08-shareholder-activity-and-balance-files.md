# Shareholder Activity Files & Daily Balance Files — How They Interlink

How a mutual fund transfer agent's **shareholder activity file** (every event that happened to an
account today) and **daily balance file** (each account's closing position today) relate to each
other, plus a full taxonomy of what counts as "shareholder activity" — far more than just
subscriptions and redemptions. Companion to `05-transfer-agent-deep-dive.md` and
`06-dtcc-nscc-fund-serv-and-networking.md`.

---

## ⚡ TL;DR

- **The core relationship is a roll-forward**: `Prior day's balance + today's net activity =
  today's balance`. This isn't just an operational convention — it's the literal structure the SEC
  regulates under **Rule 17Ad-9**, which formally defines the **master securityholder file**
  (the balance file), **subsidiary file** (unposted activity awaiting posting), **credits/debits**
  (the individual activity entries), and **control book** (the fund-level total the sum of all
  balances must tie to).
- When the roll-forward breaks — the balance file doesn't tie to activity plus prior balance, or
  to the control book — that's a **"record difference"** under 17Ad-9(g); one unresolved for
  **>30 calendar days** becomes an **"aged record difference"** reportable under **Rule 17Ad-11**
  (already covered in `05`, §2).
- **DTCC's own Networking service draws this exact line**: the **Activity Report** ("any
  financial transaction... that causes a change in an account balance") vs. **Account Maintenance
  & Reconciliation** ("non-financial, detailed customer account information"). This
  financial/non-financial split is DTCC's own operative distinction, not something imposed on
  their model from outside.
- **Shareholder activity is far more than subscriptions/redemptions.** §2 below organizes ~25
  distinct financial transaction types (across trading, income distributions, retirement-account
  events, fees, corrections, and corporate actions) plus ~7 non-financial maintenance activity
  types, each mapped to its IRS tax-reporting code where one exists.
- The specific NSCC record carrying an activity event is the **F55 Activity record**, tagged with
  a **Transaction Type** code (e.g., Type 22 or 35 for a share-class conversion); the underlying
  trade itself typically arrives via a Fund/SERV **001 Order** or **015 Exchange** record (both
  covered in `06`).
- The same activity-file/balance-file pairing recurs **one level up** at the omnibus level, via
  DTCC's **Omni/SERV** (intermediary-to-fund Omnibus Activity and Position files) — so this isn't
  a shareholder-account-only pattern, it's the general shape of TA recordkeeping at every level of
  aggregation.

---

## 1. The core relationship: activity file → balance file, formally defined

### 1.1 The regulatory vocabulary — Rule 17Ad-9

**17 CFR 240.17Ad-9** defines the exact terms this relationship runs on:

- **Master securityholder file** — "the official list of individual securityholder accounts."
  This is the regulatory name for what this doc calls the **daily balance file**: the system of
  record for each account's current position.
- **Subsidiary file** — "any list or record of accounts, securityholders, or certificates that
  evidences debits or credits **that have not been posted** to the master securityholder file."
  This is the regulatory concept behind an **activity file**: entries in transit, not yet reflected
  in the balance.
- **Control book** — "the record or other document that shows the total number of shares... or the
  principal dollar amount... authorized and issued by the issuer." The fund-level total that the
  *sum* of every shareholder's balance must tie to.
- **Credit** — "an addition of appropriate certificate detail to the master securityholder file."
- **Debit** — "a cancellation of appropriate certificate detail from the master securityholder
  file."
- **Record difference** — occurs when either (1) the master securityholder file's total doesn't
  equal the control book's total, or (2) transferred/redeemed certificate detail differs from what's
  currently on the master file, and the difference "cannot be immediately resolved."

[17 CFR 240.17Ad-9, Cornell LII](https://www.law.cornell.edu/cfr/text/17/240.17Ad-9)

Put plainly: the **activity file is the stream of credits/debits** (subsidiary-file entries) that
get **posted** to the **balance file** (master securityholder file), and the aggregate of the
balance file must reconcile to the **control book** (the fund's authorized/issued-shares total).
A break anywhere in that chain is a **record difference**.

### 1.2 The forcing function — Rule 17Ad-11 aged record differences

An **aged record difference** is a record difference that has existed for more than **30 calendar
days**. [17 CFR 240.17Ad-11](https://www.ecfr.gov/current/title-17/chapter-II/part-240/subpart-A/subject-group-ECFR1d63caaeb4b148e/section-240.17Ad-11)
(already covered in `05-transfer-agent-deep-dive.md`, §2, as one of the core Rule 17Ad- series
items). This is the regulatory backstop that forces the roll-forward to actually get reconciled
within a bounded window — an unresolved break between "prior balance + activity = current balance"
becomes a formally reportable problem after 30 days, with quarterly reporting obligations to the
SEC/ARA.

### 1.3 How DTCC's Networking service implements this in practice

DTCC's own **Networking** factsheet draws almost exactly this line, in almost exactly these
words:

- **Activity Report**: "Reports any **financial transaction** occurring directly at the fund or
  other comparable investment product that causes a change in an account balance. Includes
  **closing share balances** for Fund/SERV-initiated activity. Reports year-end tax information on
  reallocated distributions." Note that the Activity Report itself carries closing balances — the
  roll-forward is built into the record design, not just checked downstream.
- **Position Files**: "Allows for the file transmission of **closing share balances** for each
  sub-account at the fund to broker/dealers and other distribution firms." This is the balance
  file proper.
- **Account Maintenance & Reconciliation**: explicitly described as sending **"non-financial,
  detailed customer account information"** — DTCC's own materials use the exact financial/
  non-financial split this doc is organized around.

[DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf)
(already the primary source for `06`, §2 — this doc pulls the activity/balance-specific detail out
of the same document).

The individual activity-level record is the **F55 Activity record**, carrying a **Transaction
Type** code identifying the event (e.g., Type 22 = "non–tax reportable share class exchange," Type
35 = "direct voluntary share class exchange" — confirmed via an ICI industry paper on share-class
conversions). [ICI — Mutual Fund Share Class Conversions: A Matrix of Possibilities (2020)](https://www.ici.org/system/files/attachments/20_ppr_share_class_exchanges.pdf),
p.13. The underlying trade typically arrives via a Fund/SERV **001 Order** (buy/sell, with an
optional Asset Type Indicator — e.g. "G" for a share-class conversion via DCC&S) or **015
Exchange** (a paired buy/sell in one record) — both already covered in `06`, §1.

**Not independently verified**: DTCC's full "Networking Transaction Description Codes" and "SDR
Transaction Codes" documents exist and are structurally referenced (e.g., "01 = Direct Purchase,"
code "BP" for a 529-to-Roth-IRA distribution) but sit behind a DTCC Learning Center member login —
the complete code table couldn't be retrieved. Treat the existence and general shape of the
taxonomy in §2 below as confirmed; treat exact DTCC internal code numbers (where not directly
cited) as illustrative, not authoritative.

### 1.4 A companion file: the Share-Aging File

Not a balance file or an activity file, but a third file type that matters for the same
reconciliation problem: when an account transfers between intermediaries or migrates into/out of
an omnibus arrangement, a **Share-Aging File** carries the holding-period/tenure data needed to
determine **redemption fees, 12b-1 trail commission eligibility, and money-market
commissionability** — data that would otherwise be lost if only current balance transferred
without its history. [DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf)

### 1.5 The same pattern, one level up

This activity-file/balance-file pairing isn't unique to the fund-to-shareholder relationship —
it recurs at the **omnibus intermediary-to-fund** level via DTCC's **Omni/SERV**: "a central
location for participating intermediaries to transmit **Omnibus Activity and Position files** to
fund companies." Same shape, one level of aggregation up. [DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf)

### 1.6 A worked operational example

An SEC-filed Transfer Agency and Service Agreement describes exactly this roll-forward as a daily
deliverable — a **"Daily Fund Activity Summary Report"** containing: **beginning balance, dealer
transactions, shareholder transactions, reinvested dividends, exchanges, adjustments, and ending
balance.** [SEC EDGAR Transfer Agency and Service Agreement, Schedule B](https://www.sec.gov/Archives/edgar/data/824036/000119312508022854/dex99h2.htm).
The same agreement requires a daily **control book reconciliation** ("Super Sheet") delivered to
the fund each morning — the practical, contractual implementation of Rule 17Ad-9's control-book
concept.

In arithmetic terms, for a single account:

```
Beginning share balance (from yesterday's balance file)
  + Purchases (today's activity)
  + Dividend/cap-gain reinvestment shares (today's activity)
  − Redemptions (today's activity)
  − Exchanges out (today's activity)
  + Exchanges in (today's activity)
  ± Corrections/adjustments (today's activity)
  = Ending share balance (today's balance file)
```

And at the fund level, the sum of every account's ending balance must tie to the **control book**
— the fund's total shares authorized/issued. If it doesn't, that's a record difference (17Ad-9(g)),
and if it's not resolved within 30 days, an aged record difference (17Ad-11) requiring SEC/ARA
reporting.

---

## 2. Taxonomy of shareholder activity — financial transactions

Organized by category, each mapped to its IRS tax-reporting treatment where one exists (tax
reporting is itself downstream consumer of the activity file — see `05`, §3, on 1099-DIV/1099-B
and cost-basis reporting).

### 2.1 Core trading activity

| Transaction type | What it is | Notes / tax mapping |
|---|---|---|
| **Purchase** (initial / subsequent) | New money into an account | Fund/SERV **001 Order** (buy side) |
| **Redemption** (full / partial) | Cash out of an account | Fund/SERV **001 Order** (sell side) |
| **Exchange between funds in the same family** | Redemption in one fund + purchase in another, in one transaction | Fund/SERV **015 Exchange** (Transaction Types "C"/"D" — into new vs. existing account) |
| **Share class conversion** (e.g., B→A after CDSC expiry) | Same economic position, different share class | NSCC F55 Activity record **Transaction Type 22** (non-tax-reportable) or **35** (direct voluntary exchange); mechanics via Fund/SERV 001 Order (Asset Type Indicator "G") or 015 Exchange |

### 2.2 Income & distribution activity

| Transaction type | What it is | Tax mapping |
|---|---|---|
| **Dividend distribution (cash)** | Ordinary income paid out in cash | 1099-DIV **Box 1a** (Total Ordinary Dividends) |
| **Dividend reinvestment (shares)** | Same income, used to buy new shares instead | Same 1099-DIV Box 1a; new shares get a new cost-basis lot |
| **Capital gain distribution (cash)** | Fund-level realized gains passed through | 1099-DIV **Box 2a** (Total Capital Gain Distributions — **always reported as long-term**, regardless of the shareholder's own holding period; short-term fund gains instead flow into Box 1a ordinary dividends), with sub-boxes 2b/2c/2d/2e/2f for §1250, §1202, 28%-rate collectibles, and §897 FIRPTA amounts |
| **Capital gain reinvestment (shares)** | Same gain, reinvested into new shares | Same 1099-DIV boxes; new cost-basis lot created |

### 2.3 Retirement-account-specific activity

| Transaction type | What it is | Tax mapping |
|---|---|---|
| **Payroll deduction / automatic investment** (401(k) etc.) | Recurring contribution from a retirement plan | Processed via NSCC **Defined Contribution Clearance & Settlement (DCC&S)**, using a dedicated "Defined Contribution Activity Type Codes" set tied to Record Type 002 |
| **Loan** (401(k) plan loan) | Participant borrows against vested balance | 1099-R **Code L** ("Loan treated as distribution") if defaulted |
| **Loan repayment** | Participant repays a plan loan | 1099-R **Code M** ("Qualified plan loan offset") in offset scenarios |
| **Required Minimum Distribution (RMD)** | Mandatory retirement-account withdrawal | 1099-R, generally **Code 7** (normal) or **Code 1/2** (early, with/without known exception) |
| **IRA rollover / trustee-to-trustee transfer** | Moving retirement assets between custodians | 1099-R **Code G** (direct rollover to eligible plan) or **Code H** (Roth-to-Roth direct rollover) |
| **IRA conversion (Traditional → Roth)** | Converting account type | Reported with a distribution code appropriate to the conversion |
| **Recharacterization** | Undoing/re-classifying a contribution | 1099-R **Code N** (same-year) or **Code R** (prior-year) |
| **Excess contribution removal** | Correcting an over-contribution | 1099-R **Code 8** (and variants 8/1, P for timing) |
| **Death distribution / transfer on death (TOD)** | Beneficiary receives account assets | 1099-R **Code 4** |

### 2.4 Fees, corrections, and adjustments

| Transaction type | What it is | Notes |
|---|---|---|
| **Fee/expense debit** (account maintenance fee, redemption fee) | Direct charge against the account | Standard TA-administered debit |
| **Contingent Deferred Sales Charge (CDSC)** | Back-end load charged on early redemption of certain share classes | "The transfer agent will withhold all contingent deferred sales charges properly payable by holders... and shall pay such amounts over as promptly as possible after the settlement date" — confirmed via SEC-filed fund agreements and the ICI share-class paper |
| **Cost basis adjustment** (wash sale, corporate action) | Adjusting a lot's basis after the fact | Governed by **IRC §6045/§6045A** — "brokers will adjust basis to reflect wash sales... and for events such as wash sales, corporate actions, returns of capital"; reported on 1099-B **Box 1g** (wash sale disallowed loss); money-market-specific wash-sale guidance in **IRS Notice 2013-48** |
| **Correction / cancel-rebill entry** | Reversing and re-entering an erroneous transaction | General securities-industry practice; a TA-specific primary source wasn't independently located — flagged as lower-confidence terminology |
| **NSF / rejected payment reversal** | Reversing a purchase after a check/ACH bounces | Standard reversal transaction type |

### 2.5 Corporate actions & involuntary events

| Transaction type | What it is | Notes |
|---|---|---|
| **Stock split / share adjustment** | Non-cash change in share count | Standard corporate-action-driven TA transaction |
| **Fund merger / reorganization conversion** | Shares of an acquired fund converted to the successor fund | Balance-affecting corporate action |
| **Fund liquidation — final distribution** | Zeroing out balances when a fund terminates | Terminal transaction type |
| **Involuntary redemption** | Fund closes out accounts below a stated minimum balance | Standard prospectus/SAI-disclosed provision — cite the specific fund's prospectus for exact thresholds in a real implementation |
| **Escheatment** (abandoned property to state) | Dormant account balance remitted to a state, per Rule 17Ad-17 lost-shareholder process (see `05`, §2/§3) | Governed by state unclaimed-property law (RUUPA framework, NAUPA as the industry reference body — this characterization is sourced to a secondary/industry document, flagged for confirmation) |
| **Certificate issuance / deposit** (physical ↔ book-entry) | Converting between certificated and book-entry form | Directly implicates 17Ad-9's "certificate detail" concept; DTCC's **FAST (Fast Automated Securities Transfer)** program handles TA-held book-entry positions |
| **Gift transfer** | Re-registering shares to a new owner as a gift | Standard non-taxable-event re-registration |

---

## 3. Taxonomy of shareholder activity — non-financial (maintenance) activity

DTCC's Networking factsheet labels this category explicitly: **"Account Maintenance &
Reconciliation — Enables funds to send non-financial, detailed customer account information to
participating firms,"** further detailing that scheduled reporting includes "withholding
information, branch number, account representative number, and distribution option information."
[DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf), p.2.

These don't move a share balance, but they're still tracked as "activity" — often in the same or a
companion feed to the financial activity file, since account-level context (address, bank
instructions, tax certification) is what makes the financial transactions processable:

- **Address change, name change**
- **Beneficiary designation change**
- **Bank/ACH instruction change**
- **Dividend/distribution option change** (cash vs. reinvest election) — explicitly named
  ("distribution option information") in DTCC's own factsheet
- **Tax certification update** (W-9/W-8BEN) — implied by "withholding information" in the
  factsheet
- **Cost basis method election** (FIFO / average cost / specific lot) — tied to the fund's §6045
  broker-reporting obligations, though not independently found in a DTCC-specific citation
- **Systematic plan setup / change / termination** — the *setup* of a SIP/SWP is itself a
  maintenance event, distinct from each periodic purchase/redemption the plan subsequently
  generates (which are financial activity, §2.1)

**Caveat**: the granular non-financial transaction-code list (comparable in detail to the
financial Transaction Type codes) wasn't independently retrievable — DTCC's detailed "Networking
Transaction Description Codes" document is login-gated. The *existence* of this financial/
non-financial split, however, is confirmed directly from DTCC's own public factsheet language, not
inferred.

---

## 4. Flagged gaps / not independently verified

- Full DTCC "Networking Transaction Description Codes" and "SDR Transaction Codes" tables — exist
  and are structurally referenced, but sit behind a DTCC Learning Center login; not retrieved in
  full.
- DST TA2000-specific or Broadridge-specific transaction code tables — not found publicly; these
  appear to be proprietary, client-facing documentation not indexed by search.
- "Cancel and rebill" as formal TA terminology — only found generic securities-industry usage
  (not a TA-specific primary source).
- NAUPA/RUUPA's precise role vis-à-vis transfer agents specifically — sourced from a secondary
  (state-association comment letter) source, not a NAUPA or RUUPA primary text; `05`'s own
  escheatment coverage should be treated as the more load-bearing reference for that topic.
- Involuntary redemption, stock split/share adjustment, gift transfer, NSF reversal, and
  certificate deposit as *named, standardized* TA transaction types — corroborated by general
  prospectus/SAI/TA-agreement boilerplate language, but no DTCC or vendor code list was found
  confirming exact standardized labels for each across the industry.

## Sources

- [17 CFR 240.17Ad-9, Cornell LII](https://www.law.cornell.edu/cfr/text/17/240.17Ad-9)
- [17 CFR 240.17Ad-11, eCFR](https://www.ecfr.gov/current/title-17/chapter-II/part-240/subpart-A/subject-group-ECFR1d63caaeb4b148e/section-240.17Ad-11)
- [DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf)
- [DTCC Networking / Codes pages, dtcclearning.com](https://dtcclearning.com/products-and-services/mutual-fund-services/networking.html) · [Codes index](https://dtcclearning.com/products-and-services/mutual-fund-services/codes.html)
- [DTCC — Standardized Data Reporting (SDR)](https://dtcclearning.com/products-and-services/mutual-fund-services/networking/standardized-data-reporting-sdr.html)
- [DTCC FAST program](https://www.dtcc.com/asset-services/agent-services/fast)
- [ICI — Mutual Fund Share Class Conversions: A Matrix of Possibilities (2020)](https://www.ici.org/system/files/attachments/20_ppr_share_class_exchanges.pdf)
- [SEC EDGAR — Transfer Agency and Service Agreement, Schedule B](https://www.sec.gov/Archives/edgar/data/824036/000119312508022854/dex99h2.htm)
- [IRS Form 1099-DIV instructions](https://www.irs.gov/instructions/i1099div)
- [IRS Form 1099-R instructions (distribution codes)](https://www.irs.gov/instructions/i1099r)
- [IRS Form 1099-B instructions](https://www.irs.gov/instructions/i1099b)
- [26 U.S.C. §6045, Cornell LII](https://www.law.cornell.edu/uscode/text/26/6045)
- [IRS Notice 2013-48 — wash sales, money market funds](https://www.irs.gov/pub/irs-drop/n-13-48.pdf)
