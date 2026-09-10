# DTCC / NSCC Infrastructure for the Mutual Fund Industry

Scope: **Fund/SERV** and **Networking** in technical depth, plus a catalog of the other DTCC/NSCC
tools used across mutual funds, retirement plans, alternatives, and insurance products — and where
DTCC's own tokenization work (DTC's pilot, Smart NAV) does and doesn't intersect with a
tokenized-fund transfer agent business. Companion to `05-transfer-agent-deep-dive.md`.

**Legend**: **[PRIMARY]** = DTCC/NSCC's own factsheets/user guides/fee schedules, or a primary
regulatory filing. **[SECONDARY]** = trade press, law-firm summary, or other third-party source,
used only where a primary source wasn't reachable — flagged inline every time.

---

## ⚡ TL;DR

- **Fund/SERV** (NSCC, launched 1986) is the central hub for mutual fund **order entry,
  confirmation, and net settlement** between fund companies and distributors — 940+ fund users,
  350+ firm users, 1.1M+ orders/day. Settlement is netted into **one payment per day per
  counterparty, in Fed funds at NSCC**, on a same-day/T+1/up-to-10-day cycle.
- **Networking** (also NSCC) is a *different, complementary* service: **non-trade account-level
  reconciliation** — keeping the broker-dealer's/intermediary's records of an omnibus account
  identical to the transfer agent's. It also carries dividend/capital-gain processing and Rule
  22c-2 shareholder-data reporting.
- **Correction to prior research**: the commonly-referenced "Networking Level 1/2/3" scheme does
  **not** match DTCC's current official documentation, which defines **Levels 0, 3, and 4**
  (see §2.2). Treat any "Level 1/2" reference you encounter elsewhere as unconfirmed against
  current primary sources.
- **ACATS-Fund/SERV** is the specific interface that lets a mutual fund position transfer between
  firms through the broader ACATS account-transfer system — settles free of value (no incentive
  charge), typically within 2 business days of validation.
- DTCC runs a much broader mutual-fund-services suite than Fund/SERV and Networking alone:
  **Commission Settlement / DTCC Payment aXis**, **Mutual Fund Profile Service (MFPS)**,
  **Defined Contribution Clearance & Settlement (DCC&S)**, **Alternative Investment Products
  (AIP)**, and an **Insurance & Retirement Services** suite — all cataloged in §3.
- **Key finding for a tokenized-TA business**: DTC's own SEC-sanctioned tokenization pilot
  (no-action letter, Dec 11, 2025) tokenizes **security entitlements** for Russell 1000 equities,
  U.S. Treasuries, and major-index ETFs — **mutual funds are explicitly not in scope**. DTC's
  approach also keeps registered ownership with **Cede & Co.** and tokenizes one layer above the
  fund's own shareholder register — structurally different from, and not competitive with, what
  Superstate/Securitize/Franklin Templeton/WisdomTree are doing at the fund level. See §4.

---

## 1. Fund/SERV — deep technical dive

### 1.1 What it is

"The U.S. industry standard for processing and settling mutual fund, bank collective fund and
other pooled investment product transactions between fund companies and distributors." Launched
**1986** by NSCC with an industry consortium — at launch, 6 clients doing 15 orders/day; today
(per DTCC's factsheet), **940+ fund users and 350+ firm users, averaging 1.1M+ orders/day**.
[DTCC Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**

### 1.2 Trade lifecycle, end to end

Fund and firm (broker-dealer) each connect to the central Fund/SERV hub, exchanging: **Mutual
Fund Order → Order Confirmation → Correction Processing → Settlement Detail → Settlement Summary
→ Registration Information → Registration Confirmation.**

- **Order Processing**: 2:00 a.m.–midnight ET, Monday–Friday (a 22-hour operating day).
- **Fund Account Registration**: firms transmit registration files 2:00 a.m.–midnight;
  money-market fund registrations must be submitted *with* the order.
- **Order Confirmation**: funds confirm / firms retrieve 2:00 a.m.–midnight; money-market purchase
  orders may confirm *after* settlement.
- **Exception Processing**: pre- and post-settlement corrections, reconfirmation, firm/fund exits
  and deletes, cash adjustments.
- **Cash Adjustments**: claims for dividends, capital gains, commission billing/adjustment,
  systematic withdrawal payments; funds can also update CDSC and long/short-term capital-gain
  data through this feature.

[DTCC Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**

### 1.3 Settlement mechanics

"Flexible settlement features support various investment product requirements, such as **'Same
Day' (T)** and **'Next Day' (T+1)** settlement cycles, and **any settlement cycle up to ten
days**. All obligations are settled in **Fed funds at NSCC**." Firms can override to set a
settlement date 1–7 days out ("Settlement Override"); funds can set an "Alternate Settlement" date
for specific firms/securities. Money settlement is streamlined to **one payment (single debit or
credit) per counterparty, via daily net settlement of total mutual fund activity in USD** — this
netting is the core mechanism that limits multi-counterparty settlement risk.
[Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**

The exact Fedwire/National Settlement Service mechanics aren't spelled out in the Fund/SERV
factsheet itself, but *are* explicitly confirmed for the related **Commission Settlement / DTCC
Payment aXis** service (§3.2): settlement occurs via the **Federal Reserve System's National
Settlement Service**, netted with other NSCC settlement activity. Reasonable to infer the same
rail underlies Fund/SERV's "Fed funds at NSCC" language, but that inference isn't directly
confirmed in a Fund/SERV-specific document — **flagged as inferred, not primary-confirmed for
Fund/SERV itself.**

### 1.4 Transaction types supported

Purchases, redemptions, exchanges; **transfers/re-registrations** via ACATS-Fund/SERV (§1.5);
**dividend/capital-gain reinvestment adjustments** and **systematic withdrawal payments** via Cash
Adjustments. Product coverage: '40 Act funds (load, no-load, open-end, money market), wrap
programs, Defined Contribution plans (401(k), via DCC&S — §3.3), non-U.S.-domiciled funds, bank
collective funds, unit investment trusts, and Section 529 plans.
[Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**

### 1.5 ACATS-Fund/SERV — how a mutual fund position actually transfers between firms

**ACATS** (Automated Customer Account Transfer Service) is NSCC's central system, launched 1985,
for transferring customer account assets between firms — mandated for use by FINRA Rule 11870.
**ACATS-Fund/SERV** is the interface linking ACATS to Fund/SERV specifically, established 1989.
[ACATS-Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/ACATS-FundSERV-Fact-Sheet.pdf) **[PRIMARY]**

Mechanics, confirmed from the full public **ACATS User Guide (dated October 17, 2025)**
[dtcclearning.com](https://www.dtcclearning.com/helpfiles/ec/acats/Content/Resources/Attachments/acats-user-help.pdf) **[PRIMARY]**:

- Mutual fund transfer at the account level is a **Position Transfer Fund (PTF)**: "a
  standardized, automated method of processing transfers of mutual fund assets between brokers,
  banks, and mutual fund companies." A member firm (receiver or deliverer) initiates; the mutual
  fund company is the contra-party. **Only one mutual fund asset per PTF.**
- ACATS creates an **ACATS-Fund/SERV Record Type 018** (registration instruction) from the
  combined TI/AT/FR records and sends it to Fund/SERV, which forwards it to the fund; the fund
  must respond with a **Record Type 019 within one business day**, relayed back to ACATS.
  Acknowledgment → PTF ages to **310–Settle Close**; rejection/no-response →
  **600–Reject**. If a fund gets no response window (e.g., a record received Wednesday evening
  has until Friday 11:00 AM ET to respond), Fund/SERV deletes/purges the record.
- Full/partial account transfers containing a mutual fund asset (FUL, PTD, PTR, RCR) route the
  fund leg through this same 018/019 link; the receiver submits a **Fund Registration (FR)
  record** by 4:00 PM ET or ACATS auto-generates a **default registration**.
- **No incentive/settlement charge applies to fund-to-firm transfers** — ACATS only assesses
  incentive charges (100% of market value, to encourage timely delivery) on broker-to-broker
  transfers. **PTFs always settle free of value.**
- **Settlement timing**: a standard full account transfer generally completes in **5 business
  days** (4 if accelerated); nonstandard transfers containing a mutual fund or option asset settle
  **2 business days** after validation (vs. 1 day for other nonstandard asset types) — the extra
  day accounts for the Fund/SERV round-trip.
- Reject codes 15/28 are dedicated to PTF (deliverer/receiver) rejects; mutual-fund-specific field
  definitions (SSN/EIN indicator, Fund/SERV control number, disbursement options for
  accruals/residuals/fractional-share cash-in-lieu, BIN/FIN account-number indicators, systematic
  withdrawal payment indicators) are documented in Section 9 of the ACATS User Guide.

### 1.6 Membership

"Any interested financial organization that meets NSCC qualifications can use the service,"
including non-U.S. entities (directly or via a U.S. affiliate).
[Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**

**Not independently confirmed**: the exact NSCC Rulebook citation for the "Mutual Fund Services
Member" category. Web-search snippets describe a category limited to Fund/SERV, Networking,
Commission Settlement, and MFPS, and describe Mutual Fund Services as a **"non-guaranteed" NSCC
service** (NSCC does not guarantee settlement payments for these transactions, unlike its equities
CNS guarantee) — this is plausible and consistent with everything else found, but the rule text
itself (`dtcc.com/-/media/Files/Downloads/legal/rules/nscc_rules.pdf`) wasn't extractable in this
research pass. **Flagged rather than asserted as confirmed.**

### 1.7 Fees

Confirmed directly from DTCC's **"Mutual Fund Services 2026 Fee Schedule"** (last updated
12/08/25): [dtcc.com fee schedule PDF](https://www.dtcc.com/wealth-management-services/mutual-fund-services/-/media/Files/Downloads/Investment-Product-Services/user-documentation/mutual-fund-services.pdf) **[PRIMARY]**

| Service | Membership | Transaction/activity fee |
|---|---|---|
| Fund/SERV | $50.00/month | $0.06 per transaction |
| Networking | $200.00/month | $0.10 per 100 records (~$0.001/record); Omni/SERV omnibus files: **$1,500.00/month flat** |
| DTCC Payment aXis — Commission and Fee Settlement | $50/mo + $50/mo processing minimum | Tiered $0.10–$0.30 per hundred records by volume |
| DTCC Payment aXis — Invoicing and Fee Settlement | $500/mo | Tiered per-record fees |
| Mutual Fund Profile Service | Phase I only: $325/mo; Phase I+II: $1,250/mo (small families ≤25 Security Issue IDs get a $1,000/mo credit → net $250/mo) | — |
| MF Info Xchange | Tier 1 (>25 Security Issue IDs): $1,500/mo; Tier 2 (≤25): $250/mo | — |
| ACATS (fund-to-broker leg) | — | **No incentive charge** on fund-to-firm transfers |

---

## 2. Networking — deep technical dive

### 2.1 What problem it solves

Networking is "the U.S. industry standard for reconciling accounts and processing dividends and
capital gains for mutual funds and other pooled investments" — the streamlined exchange of
customer account-level information between funds/pooled-investment sponsors and intermediaries
(broker-dealers, banks, trusts, TPAs) so both sides hold **identical customer account data**. This
is the omnibus/street-name reconciliation function: it's a genuinely different job from Fund/SERV
(trade order entry/confirmation/settlement).
[DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf) **[PRIMARY]**

### 2.2 Networking Levels — the actual technical definitions

Prior research on this topic (see `05-transfer-agent-deep-dive.md`) could only find secondary
references to a "Level 1/2/3" scheme, behind member-only guides. This pass found DTCC's current
factsheet defines **"Networking Level Account Control"** with levels **0, 3, and 4** —
**not 1/2/3**:

- **Level 0**: "Used to identify underlying customer accounts that are non-Networked, held
  directly at a fund company, **or** are trust accounts shared by a trust company and a brokerage
  firm" — a dual arrangement where both parties share info but the trust retains fiduciary
  responsibility (a "Trust Networked Account" variant).
- **Level 3**: "Broker/dealers and other intermediaries **maintain full customer account
  control**, handling all orders, customer statements and reporting. Underlying customers have
  **no direct privileges** with the fund company." This is the classic omnibus/street-name
  configuration.
- **Level 4**: "The fund company handles all underlying customer communications. Underlying
  customers have **full shareholder privileges directly with the fund**," or transact through a
  brokerage/distribution firm that processes orders and is informed by the fund company of all
  record changes — networked, but effectively fund-controlled.

[Networking Factsheet, p.3](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf) **[PRIMARY]**

A separate DTCC Learning Center "Matrix Levels" page and a Law Insider contract-clause aggregator
both corroborate a "Level 3 networked accounts" definition consistent with the factsheet, and
**no Level 1 or Level 2 appears anywhere** in either current source. **Conclusion: the commonly
assumed "Level 1/2/3" terminology does not match DTCC's current official scheme (0/3/4).** It's
possible older/legacy or member-gated documentation used different numbering historically — that
couldn't be ruled out (the member-only "Networking Level 3 Accounts User Guide" wasn't
accessible) — so treat this as DTCC's **current** scheme, not necessarily its only historical one.

### 2.3 Data exchanged / capabilities

All confirmed from the Networking Factsheet **[PRIMARY]**:

- **Networking for Direct Accounts**: investor account data held directly at the fund, even where
  an intermediary is also listed on the account.
- **Activity Report**: any financial transaction at the fund causing a balance change, including
  Fund/SERV-initiated activity and year-end tax info on reallocated distributions.
- **Dividend Distribution Reports**: dividend rate, posting/record/payable dates, short/long-term
  capital gains, new share balances (cash vs. reinvest) — with an option to use **NSCC's net
  settlement system** for members servicing cash-dividend accounts. Networking "simplifies the
  settlement process for dividends and capital gains by calculating a single settlement figure
  for each client, every day."
- **Position Files**: closing share balances per sub-account, transmitted to broker/dealers.
- **Account Maintenance & Reconciliation**: funds send non-financial detailed account info to
  firms; a **"scrub file"** lets firms review how accounts are set up at the fund; a **Fund
  Account Response Indicator** lets firms request a full set of fund conversion records;
  scheduled detailed reporting includes withholding info, branch number, rep number, distribution
  options.
- **Share-Aging File**: for accounts migrating between intermediaries/omnibus arrangements —
  passes data needed to determine redemption fees, 12b-1 trail commissions, and money-market
  commissionability.
- **Standardized Data Reporting (SDR)**: supports **SEC Rule 22c-2**, which requires
  intermediaries to respond to fund requests for shareholder-level data in omnibus accounts (to
  monitor market-timing/frequent-trading and impose redemption fees). SDR reportedly operates at
  two granularities — **summary-data level** (for "super-omnibus" accounts composed of multiple
  plans/trusts/investor-omnibus accounts) and **detail-data level** (specific accounts/
  shareholder-trading level). The Rule 22c-2 purpose is **[PRIMARY]**-confirmed; the two-tier
  summary/detail description is from secondary aggregation, **not independently quote-verified**.
- **Omni/SERV**: "a central location for participating intermediaries to transmit Omnibus
  Activity and Position files to fund companies."
- **Retirement Plan Reporting (RPR)**: centralized/standardized transmission of retirement-plan-
  level data among industry participants.
- Complementary/cross-referenced services: Fund/SERV, **Mutual Fund Daily Price and Rate file
  (Profile I)**, **Profile II** (dividend and security-issue database), **DTCC Payment aXis**,
  **MF Info Xchange**.

### 2.4 Networking's specific record types — confirmed via ICI's UMC operations guide

DTCC's public factsheet describes Networking's *functions* (§2.3 above) but not its underlying
record-type labels. ICI's **"Mutual Fund Operations Planning Guide for an Unexpected Market
Close"** (March 2019 — an operational planning document written *with* DTCC/NSCC input, read in
full) names the actual record types **[PRIMARY, cross-industry document]**:

- **B50** — new account.
- **B51** — nonfinancial maintenance update (the record carrying the "non-financial detailed
  account info" described in §2.3's Account Maintenance & Reconciliation bullet).
- **B52** — transfer (this is the "Networking B52 transfers" mechanism used to move shares between
  two accounts held by the same intermediary for one beneficial owner, or to complete a mutual
  fund transfer that failed/rejected in ACATS-Fund/SERV — see §2.5).
- **F55** — activity record (financial transaction causing a balance change). Notably, **an F55
  activity record requires an NAV to be provided even when it's reporting a transfer** — because
  most TA systems require an NAV to complete any share reregistration, even though a transfer
  itself isn't a priced/financial transaction. Funds are advised to use the **last available NAV**
  for transfer processing when the current NAV isn't yet available, since transfers aren't
  price-dependent.
- **F53** — dividend acknowledgment (confirmed in the guide's "Networking Transfer Processing"
  flowchart, generated by the transfer agent in response to a Networking-submitted transfer).

### 2.5 ACATS-Fund/SERV's real acknowledgment mechanic and failure consequences

`06`'s ACATS-Fund/SERV coverage (§1.5) described the 018/019 record handshake; ICI's UMC guide
adds the operational stakes **[PRIMARY, cross-industry document]**:

- A fund **must acknowledge (Record 019) an ACATS-Fund/SERV transfer within two business days of
  receipt** (business days per the NSCC operating calendar). Miss that window and the mutual fund
  portion of the transfer **"fails"** — it doesn't retry automatically.
- **Consequence of a fail**: both the delivering and receiving intermediary **freeze the
  shareholder's position** pending resolution, and the ACATS system runs a **Mutual Fund Cleanup
  (MFC)** process that also unwinds any DTCC collateral tied to the position. The shareholder
  cannot access their (still fully invested) assets during this window — real market risk and
  opportunity cost, not just an administrative delay.
- **Recovery path**: intermediaries resolve a failed/rejected transfer by submitting a **Networking
  B52 record** (the recommended/best practice) or manual instructions directly to the fund.
- **Reject Code 016** ("Trade Date Missing/Invalid") is the specific NSCC code returned when a
  current-date order or exchange is submitted to a fund that's closed that day — the fund rejects
  it back to NSCC, which relays it to the submitting intermediary for resubmission with a valid
  trade date.

### 2.6 MFPS's actual daily cutoff and role as the standard NAV/rate distribution channel

This resolves a gap flagged in `09` (§2.2/§9): while the fund-accounting-to-TA leg of NAV
transmission remains proprietary/vendor-specific, the **TA/fund-to-intermediary** leg has a
genuine named industry standard — confirmed directly from ICI's UMC guide, which describes
**NSCC MFPS I — Price/Rate** as **"the common industry practice"** for this exact purpose:

- Daily NAVs and accrual rates are expected via **MFPS I — Price/Rate cycle 98, with a cutoff of
  10:45 p.m. ET** — funds/intermediaries are advised to hold their final nightly batch-processing
  cycle until after this cycle completes.
- **MFPS I — Price/Rate** supports a spreadsheet/comma-delimited upload path via DTCC's Web Direct
  service as an alternate delivery method when the standard automated feed isn't available —
  explicitly preferred over PDF or other static formats.
- **MFPS II — Distribution Declaration** is the companion service carrying projected and actual
  distribution data: record, reinvestment, and payable dates for dividends and capital gains.
  (This is distinct from MFPS II's **Security Database**, which holds prospectus/operational rules
  data — both were noted in §3.5 below without this level of operational detail.)

### 2.7 Actual published diagrams of this data flow

For anyone who wants a visual rather than the prose description above: **ICI's "Navigating
Intermediary Relationships" (December 2022)** — publicly available, no DTCC login required — is a
64-page report built specifically to explain fund/intermediary/TA data flows, and it includes real
box-and-arrow diagrams (not just narrative) for most of what §1–§2 describe:

- **Figure 4 (Fund/SERV)**: three-lane diagram — Intermediary ↔ Fund/SERV ↔ Fund — showing
  **Orders and Exchanges** flowing intermediary→fund, **Confirmations** and **Trade Settlement**
  flowing both directions.
- **Figures 5–6 (ACATS-Fund/SERV)**: the broker-to-broker account-transfer flow — customer transfer
  request → ACATS → re-registration request relayed through Fund/SERV to the fund → acknowledgment
  back — matching `06`'s own §1.5 description of this same mechanic, drawn out visually.
- **Figure 7 (Networking)**: a single diagram laying out all four Networking sub-services side by
  side — **Original Networking** (account registration/maintenance, share transfers, share aging,
  activity and position files), **Omni/SERV** (activity/position files, 529 plan aggregation),
  **Standardized Data Reporting** (data request/response for Rule 22c-2 compliance), and
  **Retirement Plan Reporting** (recordkeeper-to-broker/dealer) — each shown as its own bidirectional
  lane between Intermediary and Fund (or Retirement Plan Sponsor for RPR).
- **Figure 8**: a clean table of **the three Networking levels — 0, 3, 4** — independently
  corroborating `06`'s own §2.2 finding (and its correction of the initially-assumed "1/2/3"
  scheme) from a second primary source. Confirms Levels 1 and 2 were formally **retired in 2015**.
- **Figures 9–10**: worked examples of **omnibus** (one TA-level account representing many
  underlying investors/plan participants, reconciled via CDS III/Omni/SERV or SDR) vs.
  **individual/Networked Level 3** accounts (TA's books show the broker-dealer's name FBO the
  individual investor) — useful for seeing exactly what "the TA's books" contain in each model.

**Directly relevant to `09`'s modeling choice**: page 39 of this report explicitly names and
defines the **"direct-at-fund" account** — opened via "check and app" or "subscription way"
business, i.e. an application and check (or, by extension, a portal) sent straight to the fund's
transfer agent rather than through a broker-dealer — as a **Level 0 non-Networked account that is
typically not processed through the NSCC at all**, with the fund/TA as the investor's sole point of
contact. That's an independent, named confirmation of the exact account model `09`'s entire
scenario set is built around, from a different primary source than the one `09` itself cites.

**One new mechanism worth flagging for a later pass**: the report also names **Client Data Share
(CDS) I, II, and III** — CDS I lets a fund complex transmit account info back to the broker-dealer
of record for a direct-at-fund/Level 0 account (regulatory books-and-records purposes); CDS II lets
a broker-dealer transmit not-fully-disclosed shareholder identity data to the fund for oversight
purposes; CDS III is the mechanism Omni/SERV operationalizes for omnibus subaccount transparency.
None of these are covered elsewhere in `06` yet — noted here as a gap rather than written up in
full, since this pass focused on locating the diagrams rather than a full CDS deep dive.

[ICI — Navigating Intermediary Relationships, December 2022](https://www.ici.org/system/files/2022-12/22-ppr-navigating-intermediary-relationships.pdf)
— read in full (PDF text extraction failed via standard fetch; read directly via Claude's native
PDF page-rendering instead, per the pattern noted in `09`'s own gaps).

### 2.4 Relationship to Fund/SERV

Explicitly complementary, not overlapping: **Fund/SERV = trade order entry/confirmation/
settlement. Networking = non-trade account-level reconciliation, dividend/capital-gain
processing, and account maintenance.** **[PRIMARY]**

### 2.5 Membership

"Any interested financial organization that meets the qualifications of DTCC's National
Securities Clearing Corporation (NSCC) subsidiary." Access via mainframe/SMART connection or the
MyDTCC Web Portal. **[PRIMARY]**

### 2.6 Recent regulatory movement — worth a follow-up

A Federal Register notice titled *"Self-Regulatory Organizations; National Securities Clearing
Corporation; Notice of Filing and Immediate Effectiveness of a Proposed Rule Change To Facilitate
the Exchange of a Mutual Fund Share to an Exchange-Traded Fund Share, **Shorten the Settlement
Time for Certain Networking Payments**, and Clarify Certain Fees"* was filed **May 14, 2026**.
[Federal Register listing](https://www.federalregister.gov/documents/2026/05/14/2026-09589/) —
title and existence confirmed **[PRIMARY]**, but the full text couldn't be fetched (a redirect the
tools couldn't follow), so the specific mechanics of the mutual-fund-to-ETF-share exchange
facility and the Networking settlement-time change are **unconfirmed beyond the title**. Worth a
direct follow-up pull — it directly touches the fund-to-ETF conversion trend and confirms NSCC is
actively re-engineering Networking payment settlement timing in 2026.

---

## 3. Other DTCC/NSCC tools used in mutual funds & asset management

### 3.1 ACATS (broader context)

NSCC's central system (since 1985) for transferring **all** customer account asset types between
firms — equities, corporate/municipal bonds, UITs, mutual funds, insurance/annuity assets,
options, cash — not fund-specific, but the mutual fund leg routes through ACATS-Fund/SERV (§1.5).
Standard transfer completes in ~4–5 business days; broker-to-broker transfers carry NSCC-assessed
**incentive charges** (100% of market value) to encourage timely delivery, except transfers
involving a bank or a mutual fund company (which settle free of value).
[ACATS User Guide](https://www.dtcclearning.com/helpfiles/ec/acats/Content/Resources/Attachments/acats-user-help.pdf) **[PRIMARY]**

### 3.2 Commission Settlement / DTCC Payment aXis

"A regeneration of the former **Mutual Fund Commission Settlement service, a tried and tested
service since 1992**." Handles 12b-1 fees/trail commissions, CDSC payouts, and other fee types
between fund companies and distributors, for both broker-controlled and retirement-plan accounts;
also supports firm-to-firm commissions for retirement-plan processing.

**Mechanics**: funds/firms submit single-batch files to NSCC Monday–Saturday; NSCC returns two
output files (detail + summary) Monday–Saturday; **settlement occurs the following business day**,
with NSCC notifying both sides of their obligation one day prior; commissions/fees are **netted
with other NSCC net-settlement activity** and settle via the **Federal Reserve System's National
Settlement Service** — the clearest confirmation found of the actual Fed-funds settlement rail
used across DTCC's Mutual Fund Services generally. Two sub-products: **Commission and Fee
Settlement** and **Invoicing and Fee Settlement** (see fee table in §1.7).
[DTCC Payment aXis Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/DTCC-Payment-aXis-Fact-Sheet.pdf) **[PRIMARY]**

### 3.3 Defined Contribution Clearance & Settlement (DCC&S)

Confirmed to exist as "a key feature of Fund/SERV" that "economically leveraged NSCC's existing
technology and infrastructure to allow the defined contribution market to automate and simplify
the processing of 401(k) orders." [Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf) **[PRIMARY]**.
Secondary summary (not independently page-verified): streamlines purchase/redemption/exchange
transactions in DC and other retirement plans for mutual fund/insurance companies, TPAs, plan
trustees/administrators, and broker-dealers, via an "NSCC Defined Contribution Interface" enabling
the TPA/Settling Entity relationship within Fund/SERV and Networking. **[SECONDARY, unverified
beyond existence/purpose]**

### 3.4 Alternative Investment Products (AIP) Service

"A standardized, trading and reporting platform that links the alternative investments industry"
— hedge funds, funds of funds, private equity, non-traded REITs, BDCs. "Based on the successful
model of DTCC's Mutual Fund Services, AIP is poised to similarly transform the business of
investments in alternatives." Functions: creating unique fund identifiers, registering/maintaining
investor accounts, executing purchases/redemptions/capital calls with money settlement,
transmitting electronic documents, reporting investor-level position/activity data. Users: fund
managers, fund administrators, transfer agents, broker-dealers, custodians — noted as "a good
control location" for compliance purposes.
[AIP Fact Sheet, © 2025](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/AIP/AIP-Fact-Sheet.pdf) **[PRIMARY]**.
A trade-press source reports AIP passed **2,500 unique clients** **[SECONDARY, Hedgeweek]**.

### 3.5 Mutual Fund Profile Service (MFPS)

Two phases: **MFPS I / "Profile I"** — Mutual Fund Daily Price and Rate File, launched 1996,
standardized daily price/rate data distribution from funds to intermediaries. **MFPS II /
"Profile II"** — a repository of two databases: (1) **Security Issue Database** (prospectus and
operational rules data, referenced against SEC EDGAR via the web-based Profile Security Database
tool); (2) **Distribution Database** (record/reinvestment/payable dates for dividend, capital-
gain, and commission payments). Existence and structure **[PRIMARY]**-confirmed via DTCC pages and
the fee schedule's explicit "Phase I (Price and Rate) Only" / "Phase I and II" line items; full
page-level detail beyond this summary **not independently quote-verified**.

### 3.6 Insurance & Retirement Services (I&RS)

A suite covering the annuity/life-insurance processing cycle between carriers and distributors
(broker-dealers, banks, brokerage general agencies, IBDs). **[SECONDARY summary, not
independently fetched page-by-page.]** Sub-services identified: **Asset Pricing (AAP)** — daily
transmission of underlying-fund values for variable annuity/life products from carriers to
distributors; **Applications & Premium (APP)** — transmits annuity application/initial-premium
info from distributors to carriers; **Commissions** — automates compensation settlement/
communication between carriers and distributors; **Insurance Processing Service (IPS)** —
automates broker-dealer-of-record changes when clients transfer accounts.

The **ACATS/IPS interface** is directly **[PRIMARY]**-confirmed from the ACATS User Guide
(Section 10): insurance contracts settle via an **Insurance Registration (IR) record** →
converted to an **Inforce Transaction (IFT)** → sent to the carrier, which responds
Acknowledged / Hard Reject / **Soft Reject (20-business-day corrective-action window)**; insurance
assets always settle free of value.

### 3.7 Institutional Trade Processing (ITP) / legacy Omgeo

DTCC ITP (formerly Omgeo) is a wholly-owned DTCC subsidiary providing buy-side/sell-side/custodian
pre-trade matching and settlement (LEI services, TradeSuite/CTM-style matching).
**[SECONDARY — not fetched page-by-page.]** Its relevance to mutual funds/asset management is as
the **institutional trade-matching layer** for an asset manager's broader securities activity
(not fund-share-specific like Fund/SERV) — framed by DTCC as extending its reach into the
institutional-investor client segment, complementing rather than overlapping with Fund/SERV.

---

## 4. DTCC's tokenization initiatives — and why they don't (yet) touch mutual funds

This is the most consequential section for a digital transfer agent business, so it's presented
in full rather than summarized.

### 4.1 Smart NAV pilot (2024)

DTCC + **Chainlink** tested bringing mutual fund **Net Asset Value (NAV) data onto blockchains**,
using Chainlink's CCIP interoperability protocol to standardize disseminating fund NAV across
"virtually any private or public blockchain." Participants: **JPMorgan, Franklin Templeton, BNY
Mellon**. **[SECONDARY — trade press only; DTCC's own digital-assets pages returned HTTP 403 to
automated fetch tools in this research pass, so this couldn't be confirmed against a DTCC primary
source. Worth trying a direct browser fetch of dtcc.com/digital-assets/tokenization if primary
confirmation matters.]** This is the closest DTCC has come to *fund-level* tokenization
infrastructure, but as a NAV-data-distribution pilot, not a shareholder-register/transfer-agent
function.

### 4.2 Collateral AppChain

A DTCC shared infrastructure layer (reported launched 2025) for moving tokenized traditional and
digital assets for collateral purposes; expanded with a **Chainlink partnership (~May 2026)**
targeting **Q4 2026 launch**, using Chainlink's Runtime Environment for eligibility checks,
pricing, margining, collateral optimization, and settlement. **[SECONDARY only]** — collateral/repo-
focused, not mutual-fund-share-specific.

### 4.3 DTC Tokenization Pilot — the important one

On **December 11, 2025**, the SEC's Division of Trading and Markets issued a **no-action letter**
to **The Depository Trust Company (DTC — the settlement-and-custody subsidiary, distinct from
NSCC)** permitting DTC to run a **pilot tokenization service for three years from launch**,
without enforcement action under specified '34 Act provisions. **[Existence confirmed via multiple
law-firm alerts summarizing the primary SEC no-action letter — the letter itself wasn't directly
fetchable in this pass; treat the mechanics below as SECONDARY, sourced from Carlton Fields,
Dechert, and Cadwalader summaries that were consistent with each other.]**

Key mechanics:

- Scope is limited to tokenizing **"security entitlements"** (not the underlying securities
  themselves) in specific highly-liquid asset classes: **Russell 1000 Index equities**, **U.S.
  Treasury bills/bonds/notes**, and **ETFs tracking major indices (S&P 500, Nasdaq-100)**.
- **Mutual funds are explicitly NOT in scope of this pilot** as currently described — only
  equities, Treasuries, and index ETFs.
- Registered ownership stays with **Cede & Co.** (DTC's nominee); participants convert book-entry
  entitlements into tokens held in DTC-approved **"Registered Wallets"** on approved blockchains;
  tokens can transfer peer-to-peer **without DTC involvement**, including outside DTC's normal
  operating hours.
- DTC must provide **quarterly reports** (participant identities, tokenized values, transfer
  volumes, blockchains used); technology must be "compliance aware" (distribution controls,
  transaction reversibility).
- Timeline reported as: MVP targeted 1H 2026; broader industry rollout / launch 2H 2026, as a
  3-year pilot. By May 2026, DTCC reportedly convened 50+ firms in a working group, with limited
  production trades targeted for July 2026 and full launch October 2026 — **these later-stage
  dates are secondary-source only and not independently re-verified in this pass.**
- On **December 17, 2025**, DTCC announced a partnership with **Digital Asset** (the company
  behind the **Canton Network**) to tokenize a subset of DTC-custodied **U.S. Treasury
  securities** on Canton — the first concrete asset class targeted under the pilot. **DTCC will
  co-chair the Canton Foundation alongside Euroclear.** **[SECONDARY — trade press; DTCC's own
  press release wasn't directly fetchable (403).]**

### 4.4 Why this matters for a digital transfer agent business

This is a structural contrast worth stating plainly: unlike Superstate/Securitize/Franklin
Templeton/WisdomTree — which tokenize the **fund share itself**, with a dedicated digital transfer
agent maintaining an on-chain/off-chain hybrid securityholder register (directly confirmed from a
live SEC filing: WisdomTree's 485APOS for the "WisdomTree 500 Digital ETF" states shares are
"recorded on an applicable blockchain and maintained by **WisdomTree Transfers, Inc., the Fund's
digital transfer agent**," with token transfers acting as an information source for the digital
TA's book-entry updates, and conversion between "DTCC Shares" [traditional, held via Cede & Co.,
BNY Mellon as transfer agent] and "Tokenized Shares" available at investor discretion —
**[PRIMARY, SEC EDGAR filing]**) —

**DTC's own tokenization pilot instead tokenizes entitlements to securities that remain custodied
and registered in DTC's existing system, one layer above the fund's own shareholder register, and
its current scope does not include mutual fund shares at all.**

Practical implication: **DTCC/DTC's tokenization work today does not compete with, or provide
infrastructure for, a tokenized-fund transfer-agent business.** It operates at the DTC-custody/
entitlement layer for equities and Treasuries — a different layer from where a digital TA for
mutual funds operates. A digital-TA business building for tokenized *mutual fund shares*
specifically is not (yet) something DTC's pilot displaces or standardizes — that space is still
occupied by the fund-level approaches cataloged in `01`–`04` of this folder.

### 4.5 NSCC's role specifically

No documented **NSCC** (as opposed to DTC) role was found in the tokenization pilot in any source
reviewed — the pilot is consistently described as a **DTC** (custody/settlement subsidiary)
program, not an **NSCC** (clearing subsidiary, and the entity behind Fund/SERV/Networking)
program. Absence of evidence isn't evidence of absence here — flagged, not asserted as a confirmed
negative.

---

## 5. Flagged gaps / not independently verified

- Exact NSCC Rulebook citation for Fund/SERV / "Mutual Fund Services Member" status — the
  rulebook PDF wasn't text-extractable in this pass; a follow-up attempt with better PDF tooling
  is worthwhile if the precise rule number matters.
- Whether a Networking "Level 1/2" designation existed historically before the current 0/3/4
  scheme — not ruled in or out; the member-gated technical guide wasn't accessible.
- Full text of the May 14, 2026 Federal Register NSCC rule filing on mutual-fund-to-ETF-share
  exchange and Networking settlement-timing changes — title confirmed, full text not fetched.
- DTCC's own primary pages on Smart NAV and the Collateral AppChain — dtcc.com/news and
  dtcc.com/digital-assets returned HTTP 403 to automated tools in this pass; all detail above
  relies on trade press.
- ~~MFPS I/II sub-service page-level detail~~ — **partially resolved**: §2.4–2.6 now confirm
  MFPS I's cycle 98/10:45 p.m. ET cutoff, its role as the standard NAV/rate distribution channel,
  and MFPS II's Distribution Declaration function, sourced from ICI's UMC operations guide.
  MFPS II's Security Database and Insurance & Retirement Services remain unverified beyond the
  summaries given.
- DTC tokenization pilot mechanics — sourced from consistent law-firm summaries of the primary SEC
  no-action letter, not the letter itself.
- **Client Data Share (CDS) I, II, and III** (§2.7) — named and their general purpose described in
  the ICI intermediary-relationships report, but not independently deep-dived here: exact data
  fields exchanged, technical delivery mechanism, and whether CDS III is fully synonymous with
  Networking Omni/SERV or a distinct-but-related service were not confirmed beyond that report's
  one-paragraph description.

## Sources

- [DTCC Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/14597-MF-FundServ-Factsheet.pdf)
- [DTCC Networking Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/WMS-Networking-Fact-Sheet.pdf)
- [ACATS-Fund/SERV Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/ACATS-FundSERV-Fact-Sheet.pdf)
- [ACATS User Guide, Oct 17 2025, dtcclearning.com](https://www.dtcclearning.com/helpfiles/ec/acats/Content/Resources/Attachments/acats-user-help.pdf)
- [DTCC Payment aXis Factsheet](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/Funds/DTCC-Payment-aXis-Fact-Sheet.pdf)
- [AIP Fact Sheet, © 2025](https://www.dtcc.com/-/media/Files/Downloads/Investment-Product-Services/Wealth-Management-Services/AIP/AIP-Fact-Sheet.pdf)
- [DTCC Mutual Fund Services 2026 Fee Schedule](https://www.dtcc.com/wealth-management-services/mutual-fund-services/-/media/Files/Downloads/Investment-Product-Services/user-documentation/mutual-fund-services.pdf)
- [Federal Register — NSCC proposed rule change, May 14 2026](https://www.federalregister.gov/documents/2026/05/14/2026-09589/)
- [ICI — Navigating Intermediary Relationships, December 2022](https://www.ici.org/system/files/2022-12/22-ppr-navigating-intermediary-relationships.pdf) — read in full; source of §2.7's diagrams (Figures 1, 4–10) and the CDS I/II/III and "direct-at-fund"/Level 0/"check and app" terminology
- WisdomTree 485APOS (SEC EDGAR) — "WisdomTree 500 Digital ETF" digital transfer agent / DTCC Shares vs. Tokenized Shares language
- [ICI — Mutual Fund Operations Planning Guide for an Unexpected Market Close, March 2019](https://www.ici.org/system/files/attachments/pdf/19_ppr_marketclose.pdf) — read in full; the operational cross-industry document behind §2.4–2.6
