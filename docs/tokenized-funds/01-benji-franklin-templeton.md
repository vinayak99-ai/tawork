# BENJI / Franklin OnChain U.S. Government Money Fund (FOBXX)

Sponsor: Franklin Templeton. Research based primarily on the Fund's **SEC-filed Statement of
Additional Information (SAI), dated August 1, 2026** and the **SEC no-action incoming letter dated
August 12, 2026** — both read in full as primary sources. Secondary sources noted separately.

## 1. Fund structure

- Series of **Franklin Templeton Trust**, a Delaware statutory trust (organized Aug 2, 2019),
  registered as a **diversified, open-end management investment company** under the **Investment
  Company Act of 1940**.
- Classified as a **government money market fund under Rule 2a-7** — ≥99.5% of assets in U.S.
  government securities, cash, and fully government/cash-collateralized repo; targets a stable
  $1.00 NAV via amortized-cost/penny-rounding.
- **"BENJI" is not a separate legal security or token contract.** It's the branded on-chain
  representation of the Fund's single share class (ticker FOBXX), recorded on the transfer agent's
  blockchain-integrated system. Per the SAI: shares recorded there are "under the unilateral
  control of the Fund's transfer agent" — a **permissioned system** using smart-contract whitelists
  (or token configuration, on Stellar). Legal ownership rests on the TA's official master
  securityholder file, not on wallet possession alone: a wrongly-transferred wallet holder "would
  have no legal claim" to the shares.
- Does not invest in cryptocurrencies or other native digital assets.

## 2. Regulatory regime

- Registered '40 Act fund, subject to full **Rule 2a-7** requirements (liquidity minimums, WAM/WAL
  caps, stress testing, board oversight).
- No dedicated exemptive order for the tokenized structure itself — the blockchain-integrated
  recordkeeping operates within existing Rule 2a-7 / general '40 Act custody and transfer-agent
  rules, developed after multi-year discussions with SEC staff (Division of Investment Management,
  Trading and Markets, Corporation Finance) starting **February 8, 2022**.
- **August 12, 2026 SEC no-action letter** (the most significant recent development): Franklin
  requested and received staff assurance it would not recommend enforcement under **Section 17(f)
  and Rule 17f-2(b)/(e)/(f)** of the '40 Act if *other* Franklin registered funds hold OnChain
  Fund/BENJI shares for cash management or securities-lending collateral without complying with
  17f-2's physical-custody/vault/examination provisions (which don't fit uncertificated,
  blockchain-recorded shares). Modeled on a 1992 no-action letter for an affiliated
  book-entry master-feeder arrangement. Conditioned on 12 representations: TA's unilateral
  administrative/private-key control, segregated per-fund wallets, transaction confirmations, daily
  reconciliation, board approval/annual review, and **≥3 independent-accountant verifications per
  fiscal year (≥2 unannounced)**. Fact-specific relief, not a blanket rule.

## 3. Service providers

| Function | Entity |
|---|---|
| Investment manager | Franklin Advisers, Inc. (0.15% mgmt fee) |
| Sub-advisor | Western Asset Management Company, LLC |
| **Transfer agent / dividend / shareholder servicing** | **Franklin Templeton Investor Services, LLC ("FTIS")** — operates the blockchain-integrated recordkeeping ("Integrated System") |
| Administrator | Franklin Templeton Services, LLC (FT Services) |
| Sub-administrator | JPMorgan Chase Bank, N.A. |
| Custodian | JPMorgan Chase Bank (traditional custody of portfolio securities; distinct from the blockchain share-recordkeeping, which FTIS controls as TA) |
| Auditor | PricewaterhouseCoopers LLP |
| Principal underwriter/distributor | Franklin Distributors, LLC |
| Fund legal counsel | Stradley Ronon Stevens & Young, LLP |

**Mechanism**: FTIS runs a system combining (1) a traditional internal book-entry database with
private shareholder PII, and (2) one or more public blockchains recording anonymized transactional
data (purchases, redemptions, NAV, dividends), joined by referential linkage into the official
master securityholder file. FTIS retains unilateral administrative control — whitelisting wallets,
freezing/correcting/migrating records, and holding custody of private keys for TA-hosted wallets
(investors may alternatively self-host, subject to FTIS approval). Explicitly distinguished from
"permissionless" tokens like BTC/ETH.

## 4. Technology / blockchain

- Chains (per Aug 2026 SAI): **Stellar** (primary/original, launched 2021, retail focus),
  **Polygon, Aptos, Avalanche, Arbitrum, Ethereum, Solana, Base**, plus **BNB Smart Chain**.
- SAI defines a formal **"Blockchain Network Suitability Framework"** with quantitative minimum
  standards: node redundancy, uptime ≥99.9%, block time <10 sec, multi-sig capability, third-party
  smart-contract audits.
- Security/tech partners cited on Franklin's BENJI site: **Trail of Bits** and **Ancilia**
  (security audits); wallet infrastructure called **"Gêmeo."**
- Private-key/admin-control architecture per the no-action letter: multi-signature and
  multi-party-computation (MPC), geographically/operationally distributed signers, separate
  hot/cold administrative environments, offline recovery.

## 5. Investor access

- Channels: **Benji App** (iOS/Android) for individuals, or an **Institutional Web Portal**.
- Direct sales only — **excludes financial intermediaries and retirement plans**; U.S. residents
  only (narrow territory/military carve-outs); not registered for sale in Canada or the EU/EEA.
- Minimums reportedly vary by network ($20–$5,000,000 cited in one non-primary fetch) — **not
  independently re-verified**, treat as indicative only.
- No secondary market currently (Fund reserves right to establish one); **peer-to-peer
  wallet-to-wallet transfers are permitted** within/between approved networks. No share
  certificates; fractional shares to nearest 1/100.
- **No DTCC/NSCC involvement found anywhere** in the SAI or no-action letter — settlement runs
  through FTIS's proprietary system plus ACH for cash, bypassing the traditional NSCC Fund/SERV
  distribution chain most funds use.
- Redemptions: same-day if received before 1pm Pacific/NYSE close; proceeds via ACH within seven
  days (from an earlier, not fully re-verified prospectus fetch).

## 6. Basic facts

- Launch: **April 2021** on Stellar; TA's blockchain became the official record-of-record starting
  **February 8, 2022** (these may be two distinct milestones — not fully reconciled in sources).
- Management fee: **0.15%** (no separately disclosed net/gross expense ratio found).
- AUM: reported ~**$726M–$828M** as of Q1 2026 across secondary sources (not independently
  confirmed against a live primary figure); Franklin's broader tokenized money-fund family
  (including non-U.S. vehicles) reportedly totals ~$2.6B.
- Notable principal shareholders (SAI, July 1, 2026): Stellar Development Foundation (27.30%),
  Franklin Distributors LLC (15.53%), Franklin Advisors Inc. (15.41%), Ondo I LP (9.45%), Johnson
  Family Trust (7.21%).

## Flagged gaps

- Exact per-network minimum investment amounts.
- Precise current AUM and 7-day yield (Franklin's own page and Morningstar returned errors on
  fetch).
- Net vs. gross expense ratio as a distinct disclosed figure.
- Reconciling the April 2021 vs. February 2022 launch/cutover dates.

## Sources

- [SAI, Franklin OnChain U.S. Government Money Fund, dated Aug 1, 2026](https://www.franklintempleton.com/forms-literature/download-preview/9001-SAI)
- [SEC No-Action Letter incoming request, Aug 12, 2026](https://www.sec.gov/files/investment/no-action/franklin-templeton-no-action-incoming-letter-081226.pdf)
- [SEC No-Action Letters index entry](https://www.sec.gov/rules-regulations/no-action-interpretive-exemptive-letters/division-investment-management-staff-no-action-interpretive-letters/franklin-templeton-081226)
- [Prospectus](https://www.franklintempleton.com/forms-literature/download-preview/9001-PSUM)
- [485BPOS, SEC EDGAR](https://www.sec.gov/Archives/edgar/data/1786958/000174177325000031/c485bpos.htm)
- [Invest with Benji](https://digitalassets.franklintempleton.com/benji/)
- [Stellar case study: Franklin Templeton](https://stellar.org/case-studies/franklin-templeton)
- [The Block: SEC clears Franklin Templeton funds to use onchain BENJI system](https://www.theblock.co/news/defi/2026-08-12-sec-clears-franklin-templeton-funds-use-onchain-benji-system-cash-management-411654)
- [The Block: Franklin Templeton brings Benji to BNB Chain](https://www.theblock.co/post/372036/franklin-templeton-bnb-chain-benji-tokenization)
- [Genfinity: SEC clearance coverage](https://genfinity.io/2026/08/24/franklin-templeton-tokenized-fund-sec-clearance-benji-etfs-mutual-funds/)
- [Avalanche blog: BENJI on Avalanche](https://www.avax.network/about/blog/franklin-templeton-launches-tokenized-money-market-fund-benji-avalanche)
