# Superstate — USTB (short-duration Treasuries) & USCC (crypto carry)

Entities involved: Superstate Inc. (San Francisco), Superstate Advisers LLC (SEC-registered
investment adviser), **Superstate Services LLC** (SEC-registered transfer agent), Superstate Trust
(Delaware statutory trust housing both funds as series).

This fund pair is the most directly relevant precedent for a **digital-transfer-agent-as-a-service**
model — see §1 and the 2026 pivot below.

## 1. Fund structure

- **USTB** is a series of **Superstate Trust**, a Delaware statutory trust — currently a **private
  fund exempt under ICA §3(c)(7)**, not '40-Act-registered. CEO Robert Leshner has described it as
  sitting "in parallel to exchange-traded products" as "a private Section 3c7 fund."
- Superstate has a **pending Form N-1A** to convert USTB into a fully **registered money market
  fund under §2(a)(7)** — draft name "Superstate USTB Money Market Fund," a series of Superstate
  Trust. As of the June 2025 SEC comment letter this was still preliminary. So: **today** USTB
  matches the "private/3(c)(7) fund with a registered TA, not a 2a-7 money fund" model — but it is
  actively converting toward full 2a-7 registration.
- **USCC** is a series of a Delaware statutory trust, offered as a private fund to Qualified
  Purchasers under Securities Act §4(a)(2)/Reg D Rule 506(c) (Form D filed) — a Reg D private
  placement, not a '40-Act fund.
- **Major 2026 structural pivot**: Superstate is exiting direct fund management.
  - **Invesco** becomes investment manager of USTB (day-to-day portfolio management via its Global
    Liquidity team); on completion (targeted Q2 2026) the fund is renamed **"Invesco Short Duration
    US Government Securities Fund"**, keeping the USTB ticker/smart contracts. Superstate retains
    the on-chain infrastructure/transfer-agent role.
  - **Bitwise** takes over investment management of USCC (targeted completion June 1, 2026),
    renaming it **"Bitwise Crypto Carry Fund"** (same ticker/contracts) — Superstate again keeps the
    transfer-agent/on-chain-infrastructure role.
  - This reflects Superstate's stated strategic shift toward being a pure tokenization/
    infrastructure ("FundOS") provider rather than an asset manager.

## 2. Regulatory regime

- Current exemption: ICA **§3(c)(7)** (USTB) and Securities Act **§4(a)(2)/Reg D Rule 506(c)**
  (USCC) — both restricting investors to sophisticated/institutional classes.
- Eligible investors: U.S. **Qualified Purchasers** (generally ≥$5M investable assets for
  individuals, ≥$25M for institutions) plus non-U.S. investors meeting equivalent standards.
- Filings: Form D (USCC); a pending **Form N-1A** for USTB's conversion to a registered money
  market fund (CIK 1982577, "Superstate Trust"). No evidence found of a separate Form 10
  Exchange Act reporting-company registration — the "reporting" apparatus is centered on the
  transfer-agent registration plus the pending N-1A, not a Form 10.
- Superstate's public position (SEC comment letter) is that this model is already compliant with
  existing securities law — Exchange Act §17A (transfer agents), UCC Article 8 (uncertificated
  securities) — without needing new exemptive relief.

## 3. Service providers

| Role | Entity | Confidence |
|---|---|---|
| Investment adviser | Superstate Advisers LLC (SEC-registered) | Primary (N-1A) |
| **Transfer agent (both funds)** | **Superstate Services LLC** — SEC-registered digital transfer agent (registration finalized ~March 2025) | Primary |
| Legal counsel | Dechert LLP | Primary (N-1A) |
| NAV calculation | NAV Consulting | Secondary, unconfirmed on a primary Superstate page |
| Custodian (USTB, crypto/digital) | Anchorage Digital Bank, N.A. and/or BitGo Trust Company (investor choice) | Secondary |
| Custodian (USCC) | Anchorage Digital Bank, N.A. | Secondary, unconfirmed |
| Auditor (USCC) | Ernst & Young LLP | Secondary, unconfirmed |
| Auditor / administrator / underlying-Treasury custodian (USTB) | **Not confirmed.** Secondary aggregators named UMB Bank (custodian) and Federated Hermes (sub-adviser) — treat skeptically, especially the Federated Hermes claim given Invesco's 2026 takeover | Unconfirmed / low confidence |
| New investment manager, USTB | Invesco Advisers, Inc. (Global Liquidity team; PMs Laurie Brignac, Marques Mercier) | Primary |
| New investment manager, USCC | Bitwise Investments | Primary |

## 4. Technology / blockchain

- Both funds issue tokens on **Ethereum, Solana, and Plume**, or traditional book-entry form
  (investor's choice). USTB launched Ethereum-only (Feb 2024); Solana/Plume added later.
- Token standard: ERC-20 equivalent. USTB Ethereum contract:
  `0x43415eB6ff9DB7E26A15b704e7A3eDCe97d31C4e`.
- **On-chain transfer agent mechanics** (from Superstate's June 2025 SEC filing): Superstate
  Services LLC maintains a hybrid on-chain/off-chain "master securityholder file" — token
  balances/ownership on-chain, PII off-chain, unified into one file. Ownership enforced by a
  smart-contract **"Allowlist"**: transfers to non-allowlisted addresses fail at the contract level.
  Superstate runs an **"On-Chain Regulatory and Resiliency Process" (ORRP)** to vet which
  blockchains/DeFi protocols are permissioned. Lost-access recovery mirrors traditional
  lost-certificate reissuance (burn old tokens, mint new ones after identity re-verification).
- KYC/verification performed by a "verification agent" — Superstate itself or a third party
  (broker/custodian/wallet provider). One secondary source names Parallel Markets as a KYC provider
  for USTB — not independently confirmed.

## 5. Investor access

- Minimum initial investment: **$100,000**, waivable at the fund's discretion (both funds; also
  stated in the SEC N-1A draft).
- Eligible investors: U.S. Qualified Purchasers (and non-U.S. equivalents) — no retail access given
  the §3(c)(7)/Reg D structure.
- Subscriptions: USD wire or USDC (Ethereum/Solana/Plume); USDC settles near-instantly, USD wire
  same-day if received before ~5pm ET (cutoffs vary slightly by fund/source).
- Redemptions: USD or USDC; USDC near-immediate, USD wire same-day if requested before ~1pm ET
  (USTB) — some variation reported across fund pages.

## 6. Basic facts

- Founder/CEO: **Robert Leshner** (co-founder, Compound Labs). Superstate founded April 19, 2023.
- Funding: $4M seed (Jun 2023) → $14M Series A (Nov 2023, Distributed Global/CoinFund) → **$82.5M
  Series B** (Jan 22, 2026, led by Bain Capital Crypto and Distributed Global; Haun Ventures,
  Brevan Howard Digital, Galaxy Digital, Bullish, ParaFi, others). Total disclosed funding >$100M.
- USTB launch: **February 1, 2024** (Superstate Trust formed as a Delaware statutory trust Oct 26,
  2023, per a secondary source not independently verified against a primary filing).
- USCC launch: **July 22, 2024**.
- AUM (2026 sources, varying by date): USTB ~$836M–$967M (~150 institutional investors); USCC
  ~$225M–$278M at time of the Bitwise takeover announcement (May 2026). Combined AUM ~$850M cited
  in Superstate's June 2025 SEC letter.
- Fees: USTB management fee **0.15%**; USCC management fee **0.75%** (flat).

## Flagged gaps

- USTB's underlying-Treasury custodian and any sub-adviser role — unreliable secondary sourcing
  (Federated Hermes claim looks inconsistent with the Invesco takeover and should be treated
  skeptically).
- USTB auditor and formal fund administrator/accountant — not named in the preliminary N-1A.
- USCC's auditor (E&Y) — single secondary source.
- Parallel Markets as USTB's KYC provider — single secondary source.
- Exact Delaware trust formation date (Oct 26, 2023) — not cross-checked against EDGAR.
- Whether any Form 10 reporting-company registration exists — no evidence found either way.

## Sources

- [American Banker — Invesco to manage Superstate's USTB](https://www.americanbanker.com/news/invesco-to-manage-superstates-tokenized-ustb-fund)
- [SEC N-1A filing](https://www.sec.gov/Archives/edgar/data/1982577/000110465925042142/tm2513524d1_n1a.htm)
- [Superstate SEC Crypto Task Force comment letter, Jun 17, 2025](https://www.sec.gov/files/ctf-superstate-letter-061725.pdf)
- [Gate Learn — USCC overview](https://www.gate.com/learn/articles/overview-of-uscc-superstate-crypto-carry-fund/6275)
- [The Defiant — USCC launch](https://thedefiant.io/news/defi/superstate-launches-crypto-carry-fund-uscc)
- [PR Newswire — Invesco/Superstate USTB partnership](https://www.prnewswire.com/news-releases/invesco-and-superstate-advance-institutional-tokenization-through-ustb-partnership-302722437.html)
- [CoinDesk — Bitwise takeover of USCC](https://www.coindesk.com/markets/2026/05/07/bitwise-expands-into-tokenized-funds-with-planned-takeover-of-superstate-s-uscc-fund)
- [The Defiant — Bitwise/USCC](https://thedefiant.io/news/defi/bitwise-to-take-over-superstate-s-usd267m-tokenized-crypto-carry-fund)
- [eco.com — USTB deep dive](https://eco.com/support/en/articles/15254017-ustb-deep-dive-2026-superstate-s-short-treasury-fund)
- [CoinDesk — Superstate registers transfer agent with SEC](https://www.coindesk.com/business/2025/03/06/tokenized-asset-manager-superstate-registers-transfer-agent-with-sec)
- [The Block — USTB Ethereum launch](https://www.theblock.co/post/275554/robert-leshner-superstate-ethereum-tokenized-fund-ustb)
- [Etherscan — USTB token contract](https://etherscan.io/token/0x43415eB6ff9DB7E26A15b704e7A3eDCe97d31C4e)
- [The Block — Superstate Series B](https://www.theblock.co/news/deals/2026-01-22-superstate-raises-82-5-million-usd-series-b-funding-round-386690)
- [Superstate newsroom — Series B](https://superstate.com/newsroom/superstate-raises-82.5m-series-b-financing)
- [Superstate newsroom — USCC launch](https://superstate.com/newsroom/introducing-the-superstate-crypto-carry-fund-uscc)
- [Fortune — Superstate founding / USCC launch](https://fortune.com/crypto/2023/11/15/compound-founders-new-firm-superstate-14-million-modernize-investing-crypto-style-tools/)
