# The Mutual Fund Ecosystem — Who Does What

A map of every distinct third-party entity/role involved in running a U.S. registered open-end
mutual fund ('40 Act fund), beyond the transfer agent (`05-transfer-agent-deep-dive.md`) and
DTCC/NSCC infrastructure (`06-dtcc-nscc-fund-serv-and-networking.md`) already covered in this
folder. Written for both a quick skim and a deep read — the table in §0 is the "very high level"
version; §1–§18 are the "very low level" version with legal basis and named examples.

**Sourcing note**: Investment Company Act sections and SEC rules are cited directly (Cornell
LII/SEC.gov). Named-firm examples come from SEC EDGAR filings where possible, otherwise from
industry/trade sources — flagged inline as such, and again in the gaps section at the end.

---

## 0. Quick-reference table (high level)

| # | Entity | Core job | Legal basis | Named examples |
|---|---|---|---|---|
| 1 | **Fund sponsor / investment adviser** | Organizes the fund, manages the portfolio, often supplies other services via affiliates | ICA §15 (advisory contract approval) | Fidelity, Vanguard, BlackRock, Franklin Templeton, Capital Group |
| 2 | **Board of Trustees/Directors** (incl. independent trustees) | Governance/oversight — approves advisory contract, oversees valuation, CCO, 12b-1 plans | ICA §10 (independence), §15(c), Rule 2a-5, Rule 38a-1 | N/A — individuals, not vendor firms |
| 3 | **Custodian bank** | Safekeeps fund cash/securities (assets, not shareholder records) | ICA §17(f), Rule 17f-2, Rule 17f-4 | State Street, JPMorgan, BNY Mellon |
| 4 | **Fund administrator** | NAV support, financial statements, board reporting, N-CSR/N-PORT/N-CEN filing support | No single ICA mandate — contractual role | State Street, BNY Mellon, U.S. Bancorp Fund Services |
| 5 | **Fund accountant** | Daily NAV calc specifically — pricing, income/expense accruals, corporate actions, cash recon | Underpins Rule 22c-1 forward pricing | Often the same firm as #4 |
| 6 | **Principal underwriter / distributor** | Sells fund shares; **is a FINRA-member broker-dealer** (unlike the TA) | ICA §2(a)(29), Rule 12b-1 | Fidelity Distributors, Vanguard Marketing Corp |
| 7 | **Independent auditor** | Audits annual financial statements | ICA §32(a), Rule 32a-4 | PwC (~41% of funds), EY, Deloitte, KPMG |
| 8 | **Pricing / valuation services** | Evaluated prices feeding NAV, esp. illiquid/fixed income | Rule 2a-5 (fair value, 2020/2021) | ICE Data Services, Bloomberg BVAL, S&P Global |
| 9 | **Securities lending agent** | Lends portfolio securities for fee income, manages collateral | General SEC no-action/exemptive framework | eSecLending, BNY Mellon, State Street |
| 10 | **Proxy voting/research (adviser-facing)** | Research/vote recommendations on *portfolio companies'* proxies | Not ICA-mandated | ISS, Glass Lewis (>90% of market) |
| 10b | **Proxy solicitation (fund-facing)** | Solicits/tabulates votes at the *fund's own* shareholder meetings | Flows from ICA §15/§13 shareholder-vote provisions | Broadridge, Computershare, D.F. King |
| 11 | **Fund rating/data services** | Peer classification, star ratings, benchmarking | Not a regulatory requirement | Morningstar, Lipper (LSEG/Refinitiv) |
| 12 | **Financial printer / EDGAR filing agent** | Typesets/files prospectuses, SAIs, N-CSR/N-PORT/N-CEN | Supports Securities Act/ICA disclosure obligations | Donnelley Financial Solutions (DFIN), Toppan Merrill, Broadridge |
| 13 | **Fidelity bond provider** | Crime/fidelity insurance vs. officer/employee larceny | ICA §17(g), Rule 17g-1, Form N-17G-1 | Not independently confirmed (see gaps) |
| 14 | **Index provider** (index funds only) | Licenses index methodology/data/branding | Commercial licensing, not ICA-mandated | S&P Dow Jones, MSCI, FTSE Russell, CRSP |
| 15 | **Securities class-action recovery service** | Monitors/files claims in class-action settlements on the fund's behalf | Fiduciary best-practice, not ICA-specific | Financial Recovery Technologies, Chicago Clearing Corp |
| 16 | **Chief Compliance Officer (CCO)** | Administers written compliance program; reports directly to the board | Rule 38a-1 | In-house, or outsourced: ACA Group, Vigilant Compliance |
| 17 | **State "Blue Sky" filing service** | Manages state-level notice filings | NSMIA framework (background, not independently verified here) | Not confirmed — see gaps |
| 18 | **Fund/independent counsel, D&O/E&O insurers** | Legal advice to independent directors; liability insurance | Practice-driven, not directly cited here | — |

---

## 1. Fund sponsor / investment adviser / "fund house"

Organizes the fund, manages the portfolio, and typically supplies (directly or via affiliates)
several other services on this list (distribution, sometimes administration). The **"fund
complex"/"fund family"** is the group of funds advised by one adviser (or affiliates) and
principally underwritten by one distributor — distinct from the individual **registrant** (the
trust or corporation that's the actual SEC registrant, often housing many "series"/funds under one
legal entity — e.g., a single "XYZ Investment Trust" holding dozens of funds as series).

**Legal basis**: ICA **§15(a)** makes it unlawful to serve as adviser except under a written
contract approved by a majority vote of shareholders, continuing no more than two years unless
annually renewed by the board or shareholders; **§15(c)** requires the contract and its renewals
be approved by a majority of the disinterested (independent) directors, who must request and
evaluate information reasonably necessary to assess the contract, with the adviser obligated to
furnish it. [15 U.S.C. §80a-15](https://uscode.house.gov/view.xhtml?req=granuleid%3AUSC-prelim-title15-section80a-15&num=0&edition=prelim) · [SEC 2004 release on advisory-contract disclosure](https://www.sec.gov/rules-regulations/2004/06/disclosure-regarding-approval-investment-advisory-contracts-directors-investment-companies)

Named examples: Fidelity, Vanguard, BlackRock, Franklin Templeton, Capital Group (American
Funds), T. Rowe Price.

---

## 2. Board of Trustees/Directors, including independent trustees

Approves the advisory contract, oversees fair-valuation processes, approves 12b-1 plans, oversees
the CCO, and generally acts as shareholders' "independent watchdog."

**Legal basis**:
- **§10(a)** sets a statutory floor: independent ("disinterested") directors must be **≥40%** of
  the board.
- **§10(b)(2)** effectively requires a **majority** independent when the fund's principal
  underwriter is affiliated with the adviser.
- Since the SEC's 2001 fund-governance rule amendments (effective July 1, 2002), reliance on
  numerous key exemptive rules (Rule 12b-1 among them) is conditioned on the board being
  **majority independent** — this is how "majority independent" became near-universal in practice
  even though the bare statutory floor is 40%.
- **§15(c)**: board (specifically disinterested directors) approves advisory contracts (§1 above).
- **Rule 2a-5**: the board (or its valuation designee, typically the adviser, subject to board
  oversight) has "active oversight" responsibility for fair-value determinations, including
  overseeing pricing services (§8 below).
- **Rule 38a-1**: the board (including a majority of independent directors) appoints and approves
  compensation of the CCO, and approves fund compliance policies (§16 below).
- **Rule 12b-1** plans require board approval, including independent-director approval, as a
  condition of the rule's exemptive relief.

[SEC — Role of Independent Directors (2001)](https://www.sec.gov/rules/2001/01/role-independent-directors-investment-companies) ·
[Federal Register version](https://www.govinfo.gov/content/pkg/FR-2001-01-16/pdf/01-536.pdf) ·
[SEC Rule 2a-5 Small Entity Compliance Guide](https://www.sec.gov/resources-small-businesses/small-business-compliance-guides/good-faith-determinations-fair-value-small-entity-compliance-guide) ·
[17 CFR §270.38a-1](https://www.law.cornell.edu/cfr/text/17/270.38a-1)

Not a vendor category — trustees are individuals. The **Independent Directors Council (IDC)**, an
ICI affiliate, is the trade body specifically for independent fund directors.
[IDC](https://www.idc.org/advisory-contract-approval)

---

## 3. Custodian bank

Safekeeps fund **cash and securities** — physically/electronically holding assets separate from
the adviser's control, settling trades, collecting income. **Distinct from the transfer agent**,
which maintains shareholder records/ownership, not fund assets.

**Legal basis**: **§17(f)(1)** requires every registered fund to place and maintain its securities
and other assets in the custody of (A) a qualifying bank, (B) a member of a national securities
exchange per SEC rules, or (C) the fund itself under SEC rules. **Rule 17f-2** governs the narrow,
now-rare case of a fund self-custodying (dual-officer access controls, thrice-yearly independent-
accountant verification, Form N-17F-2). **Rule 17f-4** separately governs custody through a
securities depository (i.e., the DTCC/NSCC-linked custody chain covered in `06`).

[§17(f) discussion, SEC no-action letter](https://www.sec.gov/divisions/investment/noaction/maxim011504-in.pdf) ·
[17 CFR §270.17f-2](https://www.law.cornell.edu/cfr/text/17/270.17f-2) ·
[17 CFR §270.17f-4](https://www.law.cornell.edu/cfr/text/17/270.17f-4)

Named examples, confirmed via SEC-filed custody agreements: **State Street Bank and Trust
Company**, **JPMorgan Chase Bank, N.A.** [State Street example](https://www.sec.gov/Archives/edgar/data/1300087/000119312518062985/d517585dex99g.htm) ·
[JPMorgan example](https://www.sec.gov/Archives/edgar/data/36405/000168386322003916/f12302d2.htm).
**BNY Mellon** and **Northern Trust** are also well-known custodians industry-wide (BNY Mellon
confirmed via §4 below; Northern Trust not independently confirmed via a fetched primary filing
in this research — industry-standard knowledge, flagged accordingly).

---

## 4. Fund administrator

NAV calculation *support/oversight*, financial-statement preparation, expense management, board
reporting, and regulatory filing support (N-CSR, N-PORT, N-CEN). A BNY Mellon fund-administration
agreement describes the administrator as one who "supplies office facilities, data processing
services, clerical, accounting and bookkeeping services, internal auditing and legal services,
administrative services…prepares reports to shareholders, tax returns and SEC filings…[and]
calculates the net asset value of fund shares."
[BNY Mellon Funds Trust SEC filing](https://www.sec.gov/Archives/edgar/data/0001111565/000089968111000342/bny-485apos_092811.htm)

**Legal basis**: no single ICA section requires a fund to hire a third-party administrator — the
function exists to support other statutory obligations (accurate NAV under §22(c)/Rule 22c-1,
financial-statement/filing obligations, Rule 2a-5 valuation support). It's a **contractual**, not
mandated-by-name, role.

**How this differs from "fund accounting" (§5)**: in practice these are sometimes the *same*
entity/contract (as in the BNY Mellon example, which bundles bookkeeping, NAV calculation, and
reporting) and sometimes split. No universal industry rule — treat administration as the broader
"back-office and reporting orchestration" layer, and fund accounting as the narrower "books-and-
NAV" function nested within it. [FundCount comparative analysis](https://fundcount.com/fund-administration-vs-fund-accounting-a-comparative-analysis/)
(industry/vendor source, not regulatory).

Named examples: State Street, BNY Mellon, U.S. Bancorp Fund Services, SS&C (the latter two not
independently confirmed via a fetched primary filing — industry-standard knowledge).

---

## 5. Fund accountant / "fund accounting" function

The narrower, mechanical function embedded within (or alongside) administration: calculating
**daily NAV per share** — pricing portfolio holdings, accruing income and expenses, processing
corporate actions, and reconciling cash/positions against the custodian's records. As one industry
source puts it: "the accountant owns the integrity of the numbers, while the administrator owns
the orchestration of the process around them." [FundCount](https://fundcount.com/fund-administration-vs-fund-accounting-a-comparative-analysis/)
(industry/vendor commentary, not regulatory).

**Legal basis**: not a standalone ICA-named requirement — underpins the daily-pricing obligation
under **Rule 22c-1** (forward pricing) and is where Rule 2a-5's fair-valuation and pricing-service-
oversight obligations get operationally executed.

Named examples: same firm set as administration (State Street, BNY Mellon, JPMorgan) frequently
perform both roles under one servicing agreement.

**The underlying software, not just the servicing firm**: one widely-used real-time fund
accounting/investment administration platform is **FIS InvestOne** — now rebranded **"FIS
Investment Accounting Manager"** (both names in current use; FIS's own current materials and
independent trackers both attach "formerly InvestOne" to the new name). Confirmed directly from
FIS's own brochure: processes **$25+ trillion in assets across 24 countries**, manages **83,000+
portfolios across ~1,000 fund groups**, **75% of clients run it as SaaS/hosted**. Named customers
quoted in FIS's own materials include **Ultimus Fund Solutions** (a fund administrator, §4 above),
**Jackson National Asset Management**, and **Principal Management** (mutual funds) — confirming
this is genuinely in production use, not just marketing copy. Modules include Investment/Multiple
Books of Record (IBOR/MBOR), an Exception Manager, an Expense Calculator, a SWIFT Adaptor, Corporate
Actions Management, and Intraday Valuations explicitly built for multiple-daily-NAV use cases like
money market funds (`09`, §4) — and, notably, **InvestOne bundles its own Transfer Agency module**,
meaning the same vendor can in principle sit on both sides of the TA/fund-accounting relationship
this doc set treats as separate functions.
[FIS — InvestOne brochure](https://www.fisglobal.com/-/media/fisglobal/files/pdf/brochure/fis-investone-brochure.pdf)
· [FIS — Investment Accounting Manager product page](https://www.fisglobal.com/products/fis-investment-accounting-manager)

---

## 6. Principal underwriter / distributor

Distributes/sells fund shares — either purchasing shares from the fund as principal for resale, or
acting as the fund's agent selling to dealers/the public. Compensated via, among other things,
**Rule 12b-1** distribution fees when the fund has adopted a 12b-1 plan.

**Legal basis**: **§2(a)(29)** defines "principal underwriter" for an open-end company as any
underwriter who, as principal, purchases from the fund (or has the contractual right to purchase)
securities for distribution, or who, as the fund's agent, sells or has the right to sell to a
dealer or the public — expressly excluding a dealer who buys through a principal underwriter
acting as the fund's agent. [15 U.S.C. §80a-2](https://www.law.cornell.edu/uscode/text/15/80a-2) ·
[12b-1 fee structure example, SEC filing](https://www.sec.gov/Archives/edgar/data/811030/000089418911001741/rule-12b1.htm)

> **The key contrast with the transfer agent**: a principal underwriter must be registered as a
> broker-dealer under the Exchange Act **and be a FINRA member** in good standing (industry
> practice flowing from Exchange Act §15(a) broker-dealer registration plus FINRA membership
> rules — not independently re-verified against FINRA's own rule text in this research, but
> well-established). The transfer agent, by contrast, registers with the SEC (or a bank regulator)
> under Exchange Act **§17A** and is **not** a FINRA member, since it doesn't effect securities
> transactions. This asymmetry was already noted in `05-transfer-agent-deep-dive.md` §1.3 — this
> is the entity on the other side of that asymmetry.

[ACA Group summary](https://www.acaglobal.com/industry-insights/what-to-know-about-intermediary-channels-for-mutual-fund-distribution/) (industry source, flagged)

Named examples: Fidelity Distributors Company LLC, Vanguard Marketing Corporation, BlackRock
Investments, LLC (standard industry names, not individually sourced to a filing here).

---

## 7. Independent auditor

Audits and reports on the fund's annual financial statements; typically also reviews certain
regulatory reports and the fund's federal tax return.

**Legal basis**: **§32(a)(2)** requires the fund's independent public accountant be selected by,
or submitted to shareholders for ratification/rejection by, majority vote — subject to **Rule
32a-4**, which exempts a fund from the shareholder-ratification requirement if it has an
independent audit committee (composed entirely of independent directors) that assumes oversight
responsibility for accounting/auditing, has a charter, etc.
[17 CFR §270.32a-4](https://www.law.cornell.edu/cfr/text/17/270.32a-4)

Named examples (with approximate market share, trade-press sourced, not official SEC statistics):
**PwC** (~41% of SEC-registered mutual funds by one analysis), **EY** (~17%), **Deloitte**
(~16%), **KPMG** (~12%). [Accounting Today](https://www.accountingtoday.com/news/pwc-dominates-mutual-fund-audits)

---

## 8. Pricing / valuation services

Supplies security prices/evaluated prices feeding into daily NAV, especially for hard-to-price
fixed income and other illiquid instruments. ICE's Fair Value Information Services are marketed
specifically to "assist [fund managers] with fair value requirements."
[ICE Fair Value Information](https://www.ice.com/fixed-income-data-services/data-and-analytics/pricing/fair-value)
Bloomberg's **BVAL** "supplies independent and transparent evaluated pricing daily for over 2.7
million securities," used by asset managers "to calculate the net asset value (NAV) of their bond
investments." [Bloomberg BVAL](https://professional.bloomberg.com/products/data/enterprise-catalog/pricing/evaluated-pricing/)

**Legal basis**: **Rule 2a-5** (adopted December 2020, effective 2021) establishes the "good
faith" fair-value determination framework where no readily-available market quotation exists. It
requires: (1) periodically assessing/managing material valuation risks; (2) establishing/applying
fair-value methodologies; (3) testing those methodologies; and (4) **overseeing and evaluating any
pricing services used** — with the board able to designate the adviser (or another party) to
perform these functions subject to the board's active oversight, and requiring clear
documentation of pricing-service use/methodology.
[SEC Small Entity Compliance Guide](https://www.sec.gov/resources-small-businesses/small-business-compliance-guides/good-faith-determinations-fair-value-small-entity-compliance-guide) ·
[PwC summary](https://www.pwc.com/us/en/industries/financial-services/library/sec-good-faith-determinations-of-fair-value.html)

Named examples: **ICE Data Services** (formerly Interactive Data Corp/IDC, now branded ICE Data
Pricing & Reference Data), **Bloomberg BVAL**, **S&P Global** (formerly IHS Markit), **Refinitiv/
LSEG** — all named as major fixed-income evaluation vendors in industry benchmarking.
[SS&C vendor benchmark study](https://www.ssctech.com/blog/evaluating-vendor-selection-fixed-income-study-2022) (industry source, flagged)

---

## 9. Securities lending agent

Lends portfolio securities to borrowers (e.g., broker-dealers needing shares for short sales) for
fee income, manages and marks-to-market cash/non-cash collateral, and invests cash collateral per
fund guidelines. An eSecLending SEC-filed agency agreement describes the agent as responsible for
"marketing to approved borrowers available securities from the Fund's portfolio," coordinating
loans/collateral with the fund's custodian, and "arranging for the investment of cash collateral
received from borrowers in accordance with the Fund's investment guidelines."
[Prudential Investment Portfolios SEC filing](https://www.sec.gov/Archives/edgar/data/0001067442/000006759018000295/pip16bpos.htm)

**Legal basis**: securities lending by registered funds is generally conducted under SEC no-
action/exemptive relief rather than a single dedicated ICA statutory section — **not
independently pinned down to one primary citation in this research pass; flag for follow-up** if
a precise rule/no-action-letter cite is needed.

Named examples: **eSecLending** (Securities Finance Trust Company) as lending agent under SEC-
filed agency agreements; **BNY Mellon** and **State Street** widely known to double as lending
agents for funds they custody, per BNY Mellon's own product materials.
[BNY Mellon Agency Securities Lending product sheet](https://bk.bnymellon.com/rs/353-HRB-792/images/Agency%20Securities%20Lending%202020_Product%20Sheet_v1.0.PDF)

---

## 10. Proxy voting / research services — two genuinely distinct functions

**(a) Portfolio-company proxy voting (adviser-facing)**: institutional investors, including fund
advisers, pay proxy advisory firms — principally **ISS** and **Glass Lewis** — for research and
vote recommendations on portfolio companies' shareholder meetings. These two firms "collectively
control more than 90% of the U.S. proxy advisory market."
[Harvard Law School Forum on Corporate Governance](https://corpgov.law.harvard.edu/2018/06/14/the-big-thumb-on-the-scale-an-overview-of-the-proxy-advisory-industry/)

**(b) Fund-level shareholder-vote proxy solicitation (fund-facing)**: separately, when the *fund
itself* holds a shareholder meeting (e.g., to approve a new advisory contract or merger), it hires
a proxy solicitor to communicate with and tabulate votes from the fund's own shareholders.
**Broadridge** provides "shareholder communications, vote solicitation, tabulation, and pass
through voting" for mutual funds and ETFs; **Computershare Fund Services** markets itself as "the
preferred proxy solicitor for mutual fund issuers." Fidelity's own materials confirm it retains
"Computershare, Broadridge and D.F. King" as third-party proxy vendors for shareholder
communications. [Broadridge — Mutual Fund & Alternative Investment Proxy](https://www.broadridge.com/capability/governance-and-regulatory-compliance/proxy-services/mutual-fund-alternative-investment-proxy) ·
[Computershare — Mutual Fund Proxy Solicitation](https://www.computershare.com/us/business/investor-engagement/mutual-fund-proxy-solicitation) ·
[Fidelity Proxy Voting FAQ](https://www.fidelity.com/mutual-funds/information/proxy-voting-faq)

**Legal basis**: no direct ICA section mandates use of a proxy advisory firm; adviser proxy-voting
policies and Form N-PX disclosure create the practical need. Fund-level shareholder votes stem
from the Act's general shareholder-approval provisions (§15 for advisory contracts, §13 for
fundamental policy changes) and the Exchange Act's proxy rules (§14) as applied to funds.

---

## 11. Fund rating / data services

**Morningstar** and **Lipper** (now part of LSEG/Refinitiv) classify funds into peer groups/
categories and produce star/leader ratings based on risk-adjusted performance relative to peers.
Morningstar's star rating places only the top 10% of a category at 5 stars and the bottom 10% at
1 star, based on its Category system tied to its global Style Box.
[Britannica summary of Morningstar methodology](https://www.britannica.com/money/mutual-fund-ratings)
Lipper instead ranks by quintile within peer groups across measures including total return,
consistent return, preservation, expenses, and (in the U.S.) tax efficiency.
[Lipper Leaders US Methodology PDF](https://www.lipperleaders.com/documents/LipperLeaders_Methodology_US.pdf)

**Legal basis**: not a regulatory requirement — purely an industry/market-driven service used for
fund marketing, sales-literature disclosure, and investor comparison.

---

## 12. Financial printer / EDGAR filing agent

Typesets, formats (including XBRL tagging where required), and files prospectuses, SAIs,
shareholder reports, and periodic forms (N-CSR, N-PORT, N-CEN) via EDGAR. **Donnelley Financial
Solutions (DFIN)** "assists mutual funds, hedge and alternative investment funds, and insurance
companies in creating, formatting, and filing SEC required registration forms and subsequent
ongoing disclosures," including "full-service EDGAR filing preparation and filing agent
services." [DFIN corporate description](https://www.dfinsolutions.com/about) DFIN is the 2016
spin-off of RR Donnelley & Sons' capital-markets/financial-communications division. **Toppan
Merrill** (the former Merrill Corporation capital-markets business, now under Japan's Toppan)
similarly "provides financial printing services including capital markets transactions, funds,
annuities, financial reporting, and SEC filings." [Toppan Merrill](https://www.toppanmerrill.com/)

**Legal basis**: not itself a distinct statutory role — exists to satisfy the fund's Securities
Act/Exchange Act/ICA disclosure and EDGAR-filing obligations (Form N-1A, N-CSR, N-PORT, N-CEN).

Named examples: Donnelley Financial Solutions (DFIN), Toppan Merrill, Broadridge (which also has
financial-communications/printing capabilities alongside its proxy business).

---

## 13. Fidelity bond providers

Provides fidelity/crime insurance protecting the fund against larceny and embezzlement by officers
and employees with access to fund securities or cash.

**Legal basis**: **§17(g) and Rule 17g-1** require every registered management investment company
to "provide and maintain a bond issued by a reputable fidelity insurance company," covering "each
officer and employee who may have access to the securities or funds of the company," against
larceny/embezzlement — as individual, schedule, or blanket bonds. Rule 17g-1 requires **board
approval** of the bond's amount/terms and periodic filing (**Form N-17G-1**) documenting the bond
and premium allocation. [Example fidelity-bond SEC filing](https://www.sec.gov/Archives/edgar/data/1098482/000119312509043686/d4017ga.htm) ·
[Liftman Insurance — "Rule 17g-1 Fidelity Bonds"](https://www.liftman.com/mutual-fund-insurance/mutual-fund-fidelity-bonds-rule-17g-1-fidelity-bond/) ·
[Example joint-fidelity-bond filing](https://www.sec.gov/Archives/edgar/data/1278752/000119312512170359/d336545d4017g.htm)

**Named carriers not independently confirmed** in this research pass — large fund complexes
typically obtain "joint" blanket bonds (per filing titles like "Filing of Joint Fidelity Bond
Pursuant to Rule 17g-1"), but the underlying insurance carrier names weren't surfaced. Flagged as
a gap.

---

## 14. Index providers (for index funds specifically)

Licenses index methodology, constituent data, and branding to funds tracking that index, and
(typically) calculates/maintains the index itself.

**Legal basis**: not an ICA-mandated role — a commercial licensing relationship, though disclosure
of the benchmark/index appears in fund prospectuses per general disclosure requirements.

Named examples and fee structure: **S&P Dow Jones Indices**, **MSCI**, **FTSE Russell**, and
**CRSP** (Center for Research in Security Prices) are the dominant providers; together with
Nasdaq they "capture in aggregate about 95% of the entire ETF market" and generated "more than
$6.5 billion in revenue in 2023 [with] profit margins in the range of 60-70%." Fees are commonly
described as ranging roughly **0.01%–0.10% of assets annually**, with "S&P Dow Jones has the
lowest licensing fees, CRSP has the second-lowest, and FTSE Russell and MSCI charge the highest."
[Pomegra — Index Licensing Fees](https://pomegra.io/learn/library/track-c-strategies/passive-investing/chapter-07-the-major-index-providers/index-licensing-fees)
(explainer source, fee figures are industry estimates, not provider-disclosed data). Concrete
example of the cost impact: "Vanguard switched a number of its index-tracking mutual funds and
ETFs to CRSP indexes from MSCI benchmarks in 2013, slashing each fund's already low expense ratio
by an additional 1-2 basis points." [CNBC](https://www.cnbc.com/id/100137167)

---

## 15. Securities class-action recovery / litigation monitoring services

Monitors securities (and antitrust) class-action settlements, identifies institutional clients'
(including funds') eligibility, files claims on their behalf, and pursues recovery of settlement
proceeds — a service institutional holders use because unclaimed settlement funds otherwise go
unrecovered.

**Legal basis**: no ICA-specific mandate — a fiduciary/best-practice matter for advisers managing
fund assets, not a named statutory requirement.

Named examples: **Financial Recovery Technologies (FRT)** — "over 2,500 institutional clients
worldwide including...asset managers"; **Chicago Clearing Corporation (CCC)** — founded 1993,
serving "bank trust departments, investment advisors, money managers, mutual funds, pension
funds..." and having "recovered over $3 billion for class members."
[FRT](https://frtservices.com/) · [Chicago Clearing Corporation](https://chicagoclearing.com/)
Broadridge also offers adjacent class-action claims-filing capabilities (not independently
fetched from a primary Broadridge page in this pass — flagged).

---

## 16. Chief Compliance Officer (CCO)

Administers the fund's board-approved written compliance policies and procedures, and reports
**directly to the board — not to the adviser**. Rule 38a-1 explicitly bars any officer/director/
employee of the fund, adviser, or principal underwriter from coercing, manipulating, misleading,
or fraudulently influencing the CCO.

**Legal basis**: **Rule 38a-1** (adopted 2004) requires each fund to adopt and implement written
policies/procedures reasonably designed to prevent federal securities law violations, covering
oversight of key service providers (adviser, principal underwriter, administrator, transfer
agent); appoint a CCO whose designation and compensation are approved by the board, including a
majority of independent directors; and review policies/procedures (including those of key service
providers) at least annually. [17 CFR §270.38a-1](https://www.law.cornell.edu/cfr/text/17/270.38a-1) ·
[SEC 2003 adopting release](https://www.sec.gov/rules-regulations/2003/12/compliance-programs-investment-companies-investment-advisers)

Named examples: the CCO may be an employee of the adviser or an outsourced compliance-consulting
firm. **ACA Group** (which absorbed Foreside's outsourced-CCO practice in 2022) and **Vigilant
Compliance** both market Outsourced CCO services for open-end mutual funds, closed-end funds, and
multi-series trusts — Vigilant explicitly describes oversight responsibilities spanning "the Fund
Adviser, Fund Accountant, Administration, Custodian, Transfer Agent, Distributor, and Principal
Underwriter" (i.e., this one role is meant to watch *every other entity on this list*).
[ACA Group](https://www.acaglobal.com/news-and-announcements/aca-group-unveils-outsourced-chief-compliance-officer-practice/) ·
[Vigilant Compliance](https://vigilantllc.com/solutions/compliance-solutions/mutual-fund/)

---

## 17. State "Blue Sky" filing / compliance services

NASAA operates the **Electronic Filing Depository (EFD)**, launched December 2014, letting
issuers submit state notice filings and pay state fees in one session (confirmed specifically for
Form D/Reg D offerings). Mutual fund shares sold across state lines are generally covered by
**NSMIA** (National Securities Markets Improvement Act of 1996) federal preemption of most state
*registration* requirements for "covered securities" (which includes '40 Act fund shares), but
states retain notice-filing and fee authority.

**Update**: `09`, §7.8 now walks through the actual mechanics with a concrete state example
(Alabama's Form NF, tiered fees, 12-month renewal cycle) and the related federal Rule 24f-2/Form
24F-2 annual filing that runs off the same TA-sourced net-sales data — worth reading alongside
this entry.

**Still not independently confirmed**: a single dominant, specifically-mutual-fund-focused named
third-party blue-sky compliance vendor (as opposed to which entity typically handles the filing —
fund counsel, per `09`, §1). Search results returned mostly private-placement/Reg D blue-sky
service providers, not confirmed mutual-fund-specific filing agents. **This category needs further,
more targeted research** (e.g., ICI's Service Directory, or a specific fund's SAI service-provider
exhibit) before treating any single firm as the standard provider.
[NASAA EFD description](https://www.investnext.com/blog/blue-sky-filing/) (secondary source,
flagged); the NSMIA framework itself is background knowledge here, not independently
re-verified against a primary source in this pass.

---

## 18. Other distinct roles worth knowing about

- **Fund/independent counsel**: independent legal counsel to the independent directors is
  effectively required in practice — many SEC exemptive rules (echoing the fund-governance
  conditions in §2) condition relief on independent directors being advised by counsel who is
  itself independent of the adviser. [SEC — Role of Independent Directors](https://www.sec.gov/rules/2001/01/role-independent-directors-investment-companies)
  (general characterization, not pinned to the exact rule text on independent-counsel conditions
  in this pass).
- **D&O / E&O insurance carriers**: distinct from the §17(g) fidelity bond — funds and their
  trustees separately carry directors & officers / errors & omissions liability insurance, a
  standard commercial-insurance relationship not independently sourced to a regulatory citation
  here.
- **ICI Service Directory**: worth noting as a meta-resource for filling gaps in this doc — ICI
  states its "Investment Company Service Directory offers a convenient, one-stop source of
  valuable information about fund industry products and service providers in more than 120
  categories, ranging from compliance and distribution to legal and marketing."
  [ICI Service Directory](https://www.ici.org/service_directory)

---

## How this maps onto the digital-TA question

Read alongside `05` and `06`: tokenization concentrates almost entirely in the **transfer agent**
layer (§ list above item that isn't numbered here but is covered in `05`) and, per `06`'s finding
on DTC's pilot, potentially the **custody/entitlement** layer (§3) longer-term. Every other entity
on this list — adviser (§1), board (§2), administrator/accountant (§4/§5), underwriter (§6),
auditor (§7), pricing services (§8), lending agent (§9), proxy services (§10), rating agencies
(§11), printer (§12), fidelity bond (§13), index provider (§14), class-action recovery (§15), CCO
(§16) — continues operating essentially unchanged whether the fund's shares are tokenized or not.
That's consistent with the finding in `00-overview-and-comparison.md` §3: tokenization changes the
shareholder-register and settlement-rail layers, not the rest of the fund's operational stack.

## Flagged gaps / not independently verified

1. **§17(g) fidelity bond insurance carriers** — no specific named underwriters confirmed.
2. **Blue Sky filing vendor (§17)** — no mutual-fund-specific named third-party service confirmed;
   needs targeted follow-up.
3. **Securities lending agent's specific statutory/rule basis** — general SEC no-action/exemptive
   framework referenced but not pinned to a specific citation.
4. **Index licensing fee ranges (0.01%–0.10%)** — sourced from an explainer site, not providers'
   own fee schedules or SEC disclosure — treat as approximate.
5. **Principal underwriter's FINRA-membership requirement** — corroborated via industry
   commentary rather than directly fetched FINRA/Exchange Act primary text.
6. **Northern Trust and U.S. Bancorp Fund Services/SS&C as custodian/administrator examples** —
   included as well-known industry names but not independently confirmed via a fetched primary
   SEC filing (unlike State Street, BNY Mellon, and JPMorgan, which were confirmed).
7. **Independent fund counsel as a quasi-required role** — characterized generally but not tied to
   a specific rule citation.

## Sources

- [FIS — InvestOne brochure](https://www.fisglobal.com/-/media/fisglobal/files/pdf/brochure/fis-investone-brochure.pdf) · [FIS — Investment Accounting Manager (formerly InvestOne) product page](https://www.fisglobal.com/products/fis-investment-accounting-manager)
- [15 U.S.C. §80a-15 (ICA §15)](https://uscode.house.gov/view.xhtml?req=granuleid%3AUSC-prelim-title15-section80a-15&num=0&edition=prelim)
- [15 U.S.C. §80a-2 (ICA §2(a)(29))](https://www.law.cornell.edu/uscode/text/15/80a-2)
- [17 CFR §270.17f-2](https://www.law.cornell.edu/cfr/text/17/270.17f-2) · [17 CFR §270.17f-4](https://www.law.cornell.edu/cfr/text/17/270.17f-4)
- [17 CFR §270.32a-4](https://www.law.cornell.edu/cfr/text/17/270.32a-4)
- [17 CFR §270.38a-1](https://www.law.cornell.edu/cfr/text/17/270.38a-1)
- [SEC — Good Faith Determinations of Fair Value (Rule 2a-5) Small Entity Compliance Guide](https://www.sec.gov/resources-small-businesses/small-business-compliance-guides/good-faith-determinations-fair-value-small-entity-compliance-guide)
- [SEC — Role of Independent Directors of Investment Companies (2001)](https://www.sec.gov/rules/2001/01/role-independent-directors-investment-companies)
- [SEC — Compliance Programs of Investment Companies and Investment Advisers (2003)](https://www.sec.gov/rules-regulations/2003/12/compliance-programs-investment-companies-investment-advisers)
- [Accounting Today — PwC dominates mutual fund audits](https://www.accountingtoday.com/news/pwc-dominates-mutual-fund-audits)
- [ICE Fair Value Information Services](https://www.ice.com/fixed-income-data-services/data-and-analytics/pricing/fair-value) · [Bloomberg BVAL](https://professional.bloomberg.com/products/data/enterprise-catalog/pricing/evaluated-pricing/)
- [Harvard Law School Forum — proxy advisory industry](https://corpgov.law.harvard.edu/2018/06/14/the-big-thumb-on-the-scale-an-overview-of-the-proxy-advisory-industry/)
- [Broadridge — Mutual Fund & Alternative Investment Proxy](https://www.broadridge.com/capability/governance-and-regulatory-compliance/proxy-services/mutual-fund-alternative-investment-proxy) · [Computershare — Mutual Fund Proxy Solicitation](https://www.computershare.com/us/business/investor-engagement/mutual-fund-proxy-solicitation)
- [Britannica — Morningstar methodology](https://www.britannica.com/money/mutual-fund-ratings) · [Lipper Leaders US Methodology](https://www.lipperleaders.com/documents/LipperLeaders_Methodology_US.pdf)
- [Donnelley Financial Solutions (DFIN)](https://www.dfinsolutions.com/about) · [Toppan Merrill](https://www.toppanmerrill.com/)
- [Liftman Insurance — Rule 17g-1 Fidelity Bonds](https://www.liftman.com/mutual-fund-insurance/mutual-fund-fidelity-bonds-rule-17g-1-fidelity-bond/)
- [Pomegra — Index Licensing Fees](https://pomegra.io/learn/library/track-c-strategies/passive-investing/chapter-07-the-major-index-providers/index-licensing-fees) · [CNBC — Vanguard CRSP switch](https://www.cnbc.com/id/100137167)
- [Financial Recovery Technologies](https://frtservices.com/) · [Chicago Clearing Corporation](https://chicagoclearing.com/)
- [ACA Group — Outsourced CCO](https://www.acaglobal.com/news-and-announcements/aca-group-unveils-outsourced-chief-compliance-officer-practice/) · [Vigilant Compliance](https://vigilantllc.com/solutions/compliance-solutions/mutual-fund/)
- [ICI Service Directory](https://www.ici.org/service_directory)
