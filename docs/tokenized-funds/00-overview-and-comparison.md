# Tokenized Mutual Funds — Comparative Analysis

Research pass: September 2026. Purpose: understand how existing tokenized/on-chain funds are
structured — legal wrapper, regulatory regime, and the transfer agent / record keeper / fund
accountant / fund administrator stack — as groundwork for evaluating what's required to launch a
**digital transfer agent** for tokenized mutual funds.

Funds covered:

| Doc | Fund | Sponsor |
|---|---|---|
| [01](./01-benji-franklin-templeton.md) | BENJI / Franklin OnChain U.S. Government Money Fund (FOBXX) | Franklin Templeton |
| [02](./02-buidl-blackrock.md) | BUIDL / BlackRock USD Institutional Digital Liquidity Fund | BlackRock (tokenized by Securitize) |
| [03](./03-superstate-ustb-uscc.md) | USTB (short-duration Treasuries) &amp; USCC (crypto carry) | Superstate → Invesco / Bitwise |
| [04](./04-ousg-ondo-and-wisdomtree.md) | OUSG (Ondo Short-Term US Government Treasuries) + WTGXX (WisdomTree) | Ondo Finance / WisdomTree |
| [05](./05-transfer-agent-deep-dive.md) | Deep dive: the traditional (non-tokenized) transfer agent — registration, the Rule 17Ad- series, annual SEC filings, core activities, and real enforcement-backed risks | N/A — regulatory baseline |
| [06](./06-dtcc-nscc-fund-serv-and-networking.md) | Deep dive: DTCC/NSCC infrastructure — Fund/SERV, Networking, ACATS-Fund/SERV, the broader DTCC mutual-fund tool catalog, and where DTC's tokenization pilot does (and doesn't) intersect with tokenized mutual funds | N/A — regulatory baseline |
| [07](./07-mutual-fund-ecosystem-entities.md) | The full mutual fund ecosystem — sponsor/adviser, board, custodian, fund admin/accounting, underwriter, auditor, pricing services, securities lending, proxy voting vs. solicitation, rating agencies, printers, fidelity bonds, index providers, class-action recovery, CCO — what each entity does and its legal basis | N/A — regulatory baseline |
| [08](./08-shareholder-activity-and-balance-files.md) | How a TA's shareholder activity file and daily balance file interlink (the Rule 17Ad-9 roll-forward), plus a full taxonomy of shareholder activity — trading, distributions, retirement events, fees, corrections, corporate actions, and non-financial maintenance activity | N/A — regulatory baseline |
| [09](./09-ta-data-flows-and-message-sequencing.md) | Direct-to-TA data flows for a domestic corporate treasury client investing in a domestic money market fund via an investor portal. Covers legal-entity KYC (FinCEN beneficial ownership), intraday MMF NAV striking, ACH/NACHA/Fedwire, and 17 scenarios spanning subscriptions/redemptions, dividend & capital gain distributions (cash and reinvest, incl. the daily-accrual tax calculation and state government-obligations exclusion), tax certification, Blue Sky/24f-2, wash sales, escheatment, non-financial maintenance, shares pledged as collateral (UCC Article 8 control), and TA/fund compliance reporting (Form TA-2, Rule 17Ad-13/17Ad-11, SAR/OFAC, Form N-CEN/N-MFP/N-CSR) | N/A — regulatory baseline |

Each fund doc preserves the sourcing and explicitly flags anything the underlying research could
not verify against a primary source — treat unflagged facts as sourced, flagged items as
directional only. Full source links are in each per-fund doc.

---

## 1. Comparison matrix

| | **BENJI (FOBXX)** | **BUIDL** | **USTB** | **USCC** | **OUSG** | **WTGXX** |
|---|---|---|---|---|---|---|
| Sponsor | Franklin Templeton | BlackRock | Superstate → **Invesco** (2026) | Superstate → **Bitwise** (2026) | Ondo Finance | WisdomTree |
| Legal wrapper | Series of Franklin Templeton Trust (Delaware statutory trust) | "BlackRock USD Institutional Digital Liquidity Fund Ltd." (BVI) | Series of Superstate Trust (Delaware statutory trust) | Series of a Delaware statutory trust | Ondo I LP (Delaware LP; some secondary sources claim Cayman — unresolved conflict) | Series of a Delaware statutory trust |
| **'40 Act status** | **Registered** open-end investment company, **Rule 2a-7 government money market fund** | **Not registered** — private fund | **Not registered today**; Form N-1A pending to convert to a registered **2a-7 money market fund** | **Not registered** — private fund | **Not registered** — private fund | **Registered** open-end investment company under the '40 Act |
| Offering exemption (if private) | N/A (registered) | Reg D Rule 506(c) + ICA §3(c)(7) | ICA §3(c)(7) (pending N-1A conversion) | Reg D Rule 506(c) + ICA §3(c)(7) | Reg D Rule 506(c) + ICA §3(c)(7) | N/A (registered) |
| Eligible investors | Direct retail + institutional (no intermediaries/retirement plans) | Qualified Purchasers only | Qualified Purchasers only | Qualified Purchasers only | Qualified Purchasers + accredited investors | Retail — ordinary mutual fund investor |
| Minimum investment | Varies by chain (reported $20–$5M, not fully verified) | **$5,000,000** | **$100,000** (waivable) | **$100,000** (waivable) | ~$5,000 (instant path) / $100,000 (cited elsewhere) | **$1.00** |
| **Transfer agent** | **Franklin Templeton Investor Services, LLC (FTIS)** — in-house affiliate, blockchain-integrated system | **Securitize, LLC** — SEC-registered TA, also runs tokenization platform + (via affiliate) broker-dealer/ATS | **Superstate Services LLC** — SEC-registered digital TA (own subsidiary) | **Superstate Services LLC** (same) | **Not named in any source** — appears unspecified/undisclosed | **WisdomTree Transfers, Inc.** — in-house affiliate |
| Fund administrator | FT Services, LLC (sub-admin: JPMorgan Chase Bank) | BNY Mellon | Not confirmed from primary source (secondary sources cite NAV Consulting) | Not confirmed | NAV Consulting (per Ondo docs) | Not confirmed |
| Custodian (traditional assets) | JPMorgan Chase Bank | BNY Mellon | Not confirmed (secondary: UMB Bank — unverified, questionable) | Not confirmed | Secondary sources: Clear Street / BNY Mellon (unconfirmed on Ondo's own docs) | Not confirmed |
| Digital-asset custody options | Not detailed | Anchorage, BitGo, Coinbase, Fireblocks | Anchorage Digital Bank and/or BitGo (investor choice) | Anchorage Digital Bank (secondary source) | Coinbase Custody / Coinbase Prime | Not confirmed |
| Auditor | **PricewaterhouseCoopers** | **PricewaterhouseCoopers** | Not confirmed | Ernst &amp; Young (secondary source) | Not confirmed | Not confirmed |
| Blockchains | Stellar (primary), Polygon, Aptos, Avalanche, Arbitrum, Ethereum, Solana, Base, BNB Chain | Ethereum (launch), + Aptos, Arbitrum, Avalanche, Optimism, Polygon, Solana, BNB Chain | Ethereum (launch), + Solana, Plume | Ethereum, Solana, Plume | Ethereum, Polygon, Solana, XRP Ledger (+Mantle per some sources) | Stellar (primary "Secondary Record"), Ethereum |
| Token/legal-title model | Token = permissioned representation of book-entry ownership; TA's off-chain master file is authoritative | Token via ERC-20, whitelisted; Securitize's DS Protocol enforces allowlist; blockchain treated as "primary ledger" by TA function | Hybrid on-chain/off-chain master securityholder file; allowlist enforced at smart-contract level | Same TA architecture as USTB | Permissioned multi-chain token; TA function not disclosed | Blockchain is an explicitly **secondary, non-authoritative** record; book-entry stays authoritative |
| Redemption / settlement | ACH, same-day cutoff; **no DTCC/NSCC involvement found** | Direct fund redemption + Circle USDC smart-contract conversion (24/7); no DTCC/NSCC found | USD wire or USDC/Solana/Plume, same-day/near-instant; no DTCC/NSCC found | Same as USTB | USDC/PYUSD, instant 24/7 mint/redeem riding on BUIDL primary market | Not detailed; registered-fund structure, dealer-principal liquidity model marketed for 24/7 trading |
| Launch | Apr 2021 (Stellar); TA blockchain-of-record cutover Feb 2022 | Mar 20, 2024 | Feb 1, 2024 | Jul 22, 2024 | Jan 26, 2023 | Oct 2023 |
| AUM (2026, approximate — sources vary) | ~$726M–$828M | ~$2.5B–$3B (largest tokenized Treasury fund) | ~$836M–$967M | ~$225M–$278M | ~$400M–$625M | Not found |
| Management fee | 0.15% | ~0.20–0.50% (varies by chain, secondary source) | 0.15% | 0.75% | 0.15% (waived through Jan 2027) | Not found |

## 2. The core structural spectrum

Every fund above sits somewhere on one spectrum: **how much of the traditional mutual-fund
regulatory apparatus does the tokenized structure keep, versus how much does it route around by
using a private-fund exemption?**

- **Fully registered, retail-eligible, blockchain-as-secondary-record**: WisdomTree (WTGXX) and
  Franklin Templeton (BENJI/FOBXX) both chose to stay inside the '40 Act (WTGXX is not even a
  money market fund's Rule 2a-7 subtype restriction issue the same way, while FOBXX specifically
  *is* a 2a-7 government MMF). Both treat the blockchain token as a representation reconciled
  against an authoritative, transfer-agent-controlled off-chain book-entry record — the blockchain
  is not itself dispositive of legal ownership. This preserves retail access ($1–low minimums) and
  full '40 Act investor protections, at the cost of Rule 2a-7 / '40 Act compliance overhead and the
  need to resolve novel custody questions (see Franklin's 2022–2026 SEC no-action process below).

- **Private fund, QP-only, TA is the main innovation**: BUIDL, USTB (pre-conversion), USCC, and
  OUSG all rely on ICA §3(c)(7) plus Reg D 506(c) — sidestepping '40 Act registration entirely by
  restricting the investor base to Qualified Purchasers. This is architecturally simpler and faster
  to launch (no SEC fund registration process) but caps the addressable market to institutional /
  high-net-worth investors. **A registered transfer agent is not legally required for this
  structure** — Exchange Act §17A(c)(1) only mandates TA registration for a "qualifying security"
  (one registered under §12, i.e., exchange-listed/reporting-company securities), and a 3(c)(7)
  fund's Reg D interests don't meet that definition; this is exactly why most traditional private
  funds/hedge funds just use an unregistered fund administrator to track their investor register.
  Every fund in this comparison **chose** to register one anyway — a deliberate, voluntary move
  (confirmed directly from Superstate's own registration reporting) driven by blockchain-specific
  trust needs (public, transferable tokens benefit from a regulated authoritative record-keeper in
  a way an informal administrator arrangement doesn't provide) and proactive regulatory alignment
  (SEC Crypto Task Force commentary has flagged transfer agents as central to how tokenized
  securities should fit into existing law) — not a compliance mandate.

- **Qualified Purchaser vs. Accredited Investor — two different laws, and QP is the harder bar.**
  These come from separate statutes and serve different purposes, and it's easy to conflate them.
  **Accredited Investor (AI)** is a Securities Act concept (Reg D **Rule 501(a)**) that gates who
  can be sold *any* unregistered security in a private placement: individual income >$200k/>$300k
  joint (2 prior years + expectation of the same this year) or net worth >$1M excluding primary
  residence; entities generally >$5M in assets; plus, since 2020, Series 7/65/82 license holders,
  "knowledgeable employees" of the specific private fund, and family offices with >$5M AUM.
  **Qualified Purchaser (QP)** is an Investment Company Act concept (**§2(a)(51)**) that gates who
  can invest in a fund relying on **§3(c)(7)** specifically — nothing else in securities law calls
  for QP status: individuals need ≥**$5M in investments** (a narrower, technically-defined term
  under Rule 2a51-1 — securities, investment-purpose real estate, cash/cash-equivalents, commodity
  interests; explicitly excludes a primary residence or business-use real estate), entities need
  ≥**$25M in investments**. **In practice QP is a strict subset of AI** — $5M in investments alone
  clears the AI net-worth bar easily, but plenty of AIs (the $200k-income path, a Series 65 license,
  a knowledgeable-employee designation) aren't anywhere near QP. This is exactly why every fund in
  this comparison checks **both**: §3(c)(7) requires QP status to preserve the ICA exemption, and
  the Reg D 506(c) offering *itself* separately requires verifying AI status — two different
  regulators' tests, stacked on top of each other, even though QP status happens to economically
  imply AI status.
  [SEC — Accredited Investor Definition, 2020 amendments](https://www.sec.gov/files/rules/final/2020/33-10824.pdf)

- **Superstate's 2026 pivot is the most instructive data point for your specific question.**
  Superstate started as an asset manager that also built its own transfer-agent infrastructure
  (Superstate Services LLC, SEC-registered digital TA, registered ~March 2025). In 2026 it handed
  *investment management* of both its funds to established managers (Invesco for USTB, Bitwise for
  USCC) while **keeping the transfer-agent/on-chain infrastructure role for itself**. That's
  effectively "digital transfer agent as a service" being spun out as the durable, valuable layer —
  separable from running the fund itself. It's also mid-conversion of USTB from a 3(c)(7) private
  fund into a fully registered 2a-7 money market fund, i.e., moving from the "private fund" model
  toward the "fully registered" model over time.

## 3. Where tokenization actually changes the operational stack

Across every fund researched, tokenization is concentrated almost entirely in the
**transfer-agency / shareholder-recordkeeping layer**. It does *not* change:

- **Underlying asset custody** — still traditional bank custodians (BNY Mellon, JPMorgan) holding
  T-bills/repo/cash, exactly as in a conventional money market fund.
- **Fund administration/accounting** — still traditional administrators (BNY Mellon, JPMorgan
  sub-admin, NAV Consulting) computing NAV off the traditional portfolio.
- **Audit** — still Big 4 (PwC, EY) doing conventional fund audits, sometimes with added
  reserve/reconciliation attestation procedures.

What tokenization changes:

- **The shareholder register** — instead of (or alongside) a database-only book-entry system, a
  permissioned blockchain token becomes part of, or a mirror of, the record of who owns what.
- **Distribution/settlement rails** — none of the six funds researched showed any DTCC/NSCC
  involvement. Subscriptions/redemptions run on ACH/wire or stablecoin (USDC/PYUSD) transfer,
  frequently with same-day or 24/7 near-instant settlement — a materially different (and faster)
  operational model than NSCC Fund/SERV-cleared mutual fund transactions.
- **Investor access mechanics** — wallets replace (or supplement) traditional accounts; KYC/AML
  happens once per wallet via an allowlist rather than per-trade via an intermediary.

## 3.5 Taxation — four genuinely different regimes across the comparison set

This is one of the most consequential structural differences in this whole comparison, and it has
nothing to do with tokenization directly — it's a function of each fund's underlying legal-entity
choice, with tokenization only adding a couple of genuinely novel wrinkles on top.

- **Franklin Templeton BENJI/FOBXX — Regulated Investment Company (RIC)**, same as any traditional
  mutual fund. Registered under Subchapter M (IRC §851), reports via **Form 1099-DIV**,
  capital-gain distributions get long-term treatment under §852(b)(3). This is the world `09`
  already covers in depth (daily accrual, declared dividends, corporate exemption logic). **The
  on-chain wrapper adds zero novel tax consequences here.**
- **Ondo OUSG and Superstate's USTB/USCC — partnerships, K-1 pass-through.** Confirmed directly
  from Superstate's own documentation for USTB/USCC: *"The Fund is classified as a partnership for
  U.S. Federal income tax purposes and is not subject to Federal income tax liability. Shareholders
  are responsible for reporting their distributive share of the Fund's taxable income or loss on
  their tax returns."* Both are series of a Delaware statutory trust (multi-owner DSTs default to
  partnership taxation absent an affirmative corporate election); OUSG is structurally the same
  (per expert third-party analysis, though OUSG's own public documentation doesn't state its
  federal tax classification expressly). **The key practical difference from the RIC model**: a
  partnership allocates its distributive share of income to every investor annually **whether or
  not cash is actually distributed** — real "phantom income" exposure that doesn't exist in the
  declare-and-pay dividend world `09` covers. Income retains its character flowing through
  (interest stays interest, gains stay gains) with no fund-level re-characterization.
- **BlackRock BUIDL — PFIC (Passive Foreign Investment Company), a materially worse default
  position.** BUIDL is a BVI limited company; under the check-the-box regulations
  (Treas. Reg. §301.7701-3), a foreign entity like this defaults to **foreign-corporation** tax
  status absent an affirmative Form 8832 election, and since its income is almost entirely passive
  (Treasury/repo interest), it lands in **PFIC** classification under IRC §1297. Confirmed directly
  from BlackRock's own materials: *"U.S. taxable investors will also receive PFIC statements."*
  Absent a **§1295 QEF election**, US holders face **§1291 excess-distribution treatment** — an
  annual interest charge on amounts deemed deferred from prior years. A **§1296 mark-to-market
  election is likely unavailable**, since it requires the stock be "regularly traded" on a
  qualified exchange, which BUIDL's restricted-transferability Allowlist structure doesn't satisfy.
- **Circle/Hashnote USYC — not a US tax question at all.** A Cayman Islands mutual fund restricted
  to non-US Persons under Regulation S; US investors aren't meant to hold it directly. Any US
  exposure would run through a separate Reg D feeder, whose own tax classification (likely
  partnership) would govern instead.

**Two tokenization-specific tax wrinkles, both genuinely novel — not present in a traditional
fund's tax analysis:**

- **IRC §7704 Publicly-Traded-Partnership risk**, specific to the partnership-taxed funds (OUSG,
  USTB, USCC): if a partnership's interests become too freely/publicly tradable, §7704(a)
  reclassifies the whole entity as a **corporation** — a serious structural risk that's real
  precisely *because* these interests exist as transferable on-chain tokens rather than illiquid
  LP interests. Three defenses exist: the Allowlist itself (preventing "readily tradable" secondary
  markets), the §1.7704-1(j) 2%-annual-transfer de minimis safe harbor, or the §1.7704-1(h)
  private-placement safe harbor (unregistered offering + ≤100 partners). Worth noting: this makes
  the Allowlist restriction not just a securities-law control, but load-bearing for the tax
  analysis too.
- **Distribution-mechanics-driven timing effects**: NAV-up tokens (OUSG-style — value accrues in
  token price, no periodic distribution) create no separate inclusion event; partnership holders
  are still taxed annually on their distributive share regardless. Daily in-kind distributions
  (BUIDL-style) trigger daily §1291(b) PFIC excess-distribution testing. A hypothetical rebasing
  token (share count itself increases to reflect yield) would be treated as **ordinary income on
  receipt under §61**, at the fair market value of the new tokens — by analogy to Rev. Rul. 2023-14
  (staking rewards).

[Astraea Counsel — Tokenized Treasury Funds in 2026: A Securities, Tax, and Custody Compliance Framework](https://astraea.law/insights/tokenized-treasury-funds-securities-compliance-2026) (BUIDL/BENJI/OUSG/USYC tax analysis) · Superstate documentation (USTB/USCC partnership classification, sourced via search snippet — the live legal-structure page returned a 404 on direct navigation at the time of this research, consistent with ongoing docs-site reorganization following the Invesco/Bitwise handoffs)

**Flagged gap**: USTB/USCC's partnership classification is confirmed via a search-engine excerpt
of Superstate's own documentation, not a directly-loaded primary page (the specific
`/legal-structure` URL 404'd on direct fetch) — worth re-verifying against a live page if this
distinction becomes load-bearing for anything. OUSG's classification is explicitly flagged by the
source itself as inferred/conventional-understanding rather than issuer-confirmed.

## 4. What building a digital transfer agent appears to require

Synthesizing the patterns above (most concretely documented in Franklin Templeton's SAI + August
2026 SEC no-action letter, and Superstate's June 2025 SEC Crypto Task Force comment letter):

1. **SEC transfer agent registration under Exchange Act §17A** (Rule 17Ad-1 et seq.). This is the
   legal predicate every model relies on — Securitize, Superstate Services LLC, WisdomTree
   Transfers Inc., and Franklin Templeton Investor Services LLC are all registered transfer agents.
   A blockchain doesn't substitute for this; it operates underneath it.

2. **A hybrid on-chain/off-chain "master securityholder file."** The legally authoritative record
   of ownership is controlled by the transfer agent, not by whichever wallet holds a token. Design
   pattern: off-chain database holds PII and is the book-entry system of record; on-chain
   transactions are anonymized (wallet addresses only) and reconciled into the same master file in
   real time or daily. Franklin's SAI is explicit that a wrongly-transferred wallet holder "would
   have no legal claim" to shares — the TA's records control, not raw possession of tokens.

3. **A permissioned-chain / allowlist architecture**, not a permissionless bearer-token model.
   Every fund enforces KYC/AML/OFAC screening before a wallet is whitelisted to hold or receive
   tokens; transfers to non-whitelisted addresses fail at the smart-contract level (Superstate's
   "Allowlist," Securitize's "DS Protocol"). This is what makes it a *security recorded on a
   blockchain*, not a cryptocurrency.

4. **Retained administrative control over the smart contract / private keys.** The TA (not
   investors, not the chain itself) holds unilateral ability to whitelist, freeze, correct, and
   migrate records — via multi-sig/MPC key architecture split across hot/cold environments.
   Investors may self-custody token balances, but the TA's administrative layer sits above that.

5. **A formal blockchain-network suitability framework.** Before adding any chain, define minimum
   standards: node/validator redundancy, uptime (Franklin uses ≥99.9%), block finality time,
   multi-sig support, independent smart-contract security audits. This is a control an examiner
   will expect to see documented, not improvised per-chain.

6. **Reconciliation and independent verification controls.** Daily reconciliation between on-chain
   and off-chain records; periodic independent-accountant verification of the reconciliation
   (Franklin's no-action letter conditions required at least three verifications per fiscal year,
   at least two unannounced); fund board approval and annual review of the whole arrangement.

7. **A considered choice on regulatory wrapper**, made explicit, not left implicit:
   - Registered '40 Act fund (retail-eligible, lower minimums, full disclosure regime, Rule 2a-7
     constraints if a money market fund) — Franklin/WisdomTree's path.
   - Private fund under §3(c)(7) + Reg D 506(c) (QP-only, faster to launch, no fund-level SEC
     registration, but capped distribution) — BUIDL/OUSG/USCC's path.
   - Superstate's USTB shows funds can migrate from the private path to the registered path once
     the on-chain TA infrastructure and operational track record are established.

8. **Willingness to proactively engage SEC staff on novel custody questions rather than assume
   existing rules cleanly apply.** Franklin's blockchain-integrated system took from February 2022
   to August 2026 to get formal no-action assurance specifically on how Rule 17f-2's
   physical-custody/examination provisions apply to uncertificated, blockchain-recorded shares held
   by *other* Franklin funds. That is a multi-year regulatory engagement, not a one-time filing —
   budget for it as a genuine program, not a checkbox.

9. **Decide whether to vertically integrate or specialize.** Securitize (BUIDL) bundles transfer
   agent + tokenization platform + broker-dealer/ATS + investment adviser under one roof. Franklin
   and WisdomTree keep the TA function in-house as a captive affiliate. Superstate is pivoting
   toward being *only* the TA/infrastructure layer and letting established managers (Invesco,
   Bitwise) run the actual funds. Each is a viable model; the "digital transfer agent" business
   itself (Superstate's endpoint) is the one most directly analogous to what you described wanting
   to build.

10. **Underlying fund operations (custody, administration, accounting, audit) don't need to be
    reinvented.** They can stay with conventional providers (bank custodians, fund administrators,
    Big 4 auditors) — the tokenization/digital-TA layer is what's novel and where the real product
    and regulatory work concentrates.

## 5. Open items worth resolving before going deeper

These came up as genuinely unresolved across the research (conflicting or unavailable sources —
not something to treat as settled fact):

- OUSG's transfer agent is not named anywhere in Ondo's own materials or third-party trackers —
  worth a direct inquiry if OUSG is a structural comparator you care about.
- OUSG's domicile (Delaware per SEC Form D vs. Cayman per multiple secondary sources) is
  unresolved.
- Several funds' auditors and fund administrators for the *digital* funds specifically (USTB, USCC,
  WTGXX) were not confirmed against primary sources.
- Current, live AUM figures should be pulled from a live tracker (e.g., rwa.xyz) rather than the
  point-in-time figures here, which vary noticeably by source and date even within 2026.

See each per-fund doc for full citations and additional flagged gaps.
