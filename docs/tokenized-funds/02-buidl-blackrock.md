# BUIDL / BlackRock USD Institutional Digital Liquidity Fund

Sourcing note: primary/official sources (Securitize materials, SEC filings, Circle, BNB
Chain/press releases) prioritized; secondary aggregators used to corroborate or fill gaps, marked
as such.

## 1. Fund structure

- **Not** a registered '40 Act fund. Private fund organized as **"BlackRock USD Institutional
  Digital Liquidity Fund Ltd."**, domiciled in the **British Virgin Islands (BVI)**.
- Investment manager: BlackRock Financial Management, Inc.
- Portfolio: 100% in cash, U.S. Treasury bills, and repurchase agreements.
- Targets a stable **$1.00 per token** NAV; yield accrues daily and is **distributed monthly as
  new BUIDL tokens airdropped to holders' wallets** (rebase-by-airdrop, not price appreciation).

## 2. Regulatory regime

- Offered under **Rule 506(c) of Regulation D**; relies on the **Section 3(c)(7)** exclusion under
  the Investment Company Act of 1940 (sold only to "qualified purchasers").
- Private placement — interests are not SEC-registered securities.
- **Form D** filed ~March 14, 2024 (ahead of March 20 launch); at least one Form D/A amendment on
  file (received July 18, 2025 per OTC Markets).
- BVI-domiciled; U.S. distribution under Reg D. Non-U.S. offering mechanics not confirmed.

## 3. Service providers

| Role | Provider |
|---|---|
| Investment manager | BlackRock Financial Management, Inc. |
| **Transfer agent & tokenization platform** | **Securitize, LLC** (SEC-registered transfer agent; maintains the official share register linking wallet addresses to verified investor identity) |
| Placement agent / broker-dealer, ATS | Securitize Markets, LLC |
| Custodian (cash/Treasuries) & fund administrator | **BNY Mellon** |
| Auditor | **PricewaterhouseCoopers LLP** |
| Digital-asset custody partners (investor-selected) | Anchorage Digital Bank NA, BitGo, Coinbase, Fireblocks (named at launch as "initial ecosystem partners") |
| Secondary-market USDC conversion | **Circle**, via a dedicated smart contract (launched April 2024) |

**Securitize's role is vertically integrated**: SEC-registered transfer agent of record (official
ownership ledger) *and* the tokenization/technology platform *and*, via Securitize Markets LLC,
placement agent/broker-dealer/ATS. Per Securitize's own SEC comment letter, this bundles
transfer-agent + broker-dealer/ATS + RIA + fund-administrator-adjacent functions in one corporate
group — a notably different model from a traditional mutual fund's separated TA/distributor/admin
roles.

## 4. Technology / blockchain

- Launched on **Ethereum mainnet** (ERC-20), March 20, 2024.
- Expanded to **Aptos, Arbitrum, Avalanche, Optimism, Polygon** (Nov 13, 2024) as additional share
  classes; then **Solana** (~March 2025, 7th chain); then **BNB Chain** (accepted as collateral for
  trading on Binance). Current full chain count beyond these eight not fully confirmed as of
  September 2026 — some secondary sources reference a "9th chain" without naming it consistently.
- Cross-chain interoperability reportedly via **Wormhole** — secondary-source only, not verified
  against a primary source.
- Securitize (as TA) maintains the authoritative legal share register; the blockchain(s) serve as
  the operational transfer record, described in regulatory materials as "the primary ledger to
  record asset ownership and transactions" under Securitize's transfer-agent function.
- Every wallet is onboarded through Securitize's KYC/AML process and qualified-purchaser
  attestation before being added to a smart-contract **allow-list** (Securitize's "DS Protocol");
  transfers are restricted on-chain to whitelisted wallets only.

## 5. Investor access

- Minimum investment: **$5,000,000**.
- Eligible investors: **Qualified Purchasers** (individuals ≥$5M investments, institutions ≥$25M),
  consistent with the §3(c)(7) exemption.
- Subscription via Securitize's platform after KYC/AML and QP verification; once whitelisted,
  tokens move 24/7/365 among other whitelisted wallets.
- Redemption/liquidity: (1) direct fund-level redemption via Securitize/BlackRock (exact timing not
  confirmed), or (2) **Circle's smart contract** (Apr 2024) letting eligible holders convert to
  USDC near-instantly, 24/7, as a secondary off-ramp.
- **No source found indicating DTCC/NSCC plays any role** in subscriptions, redemptions, or
  settlement — as an unregistered private-placement security, BUIDL is not DTCC/NSCC-eligible in
  the way a registered '40 Act fund's shares would be. (Inference, not directly sourced.) DTCC has
  separate, unrelated tokenization initiatives (Canton Network pilot, planned Stellar integration)
  not connected to BUIDL.

## 6. Basic facts

- Launch: **March 20, 2024**.
- AUM (secondary-source, approximate, volatile — cross-check a live tracker like
  [rwa.xyz/assets/BUIDL](https://app.rwa.xyz/assets/BUIDL)): ~$500M (Jul 2024) → ~$1.7B (~Mar 2025)
  → ~$2B (Mar 2026) → ~$2.4–2.9B range through mid-2026 (sources disagree). Widely reported as the
  **largest tokenized U.S. Treasury/RWA fund** (~34–40% market share depending on source/date).
- Fees: reported as chain-dependent — ~0.50% (Ethereum, Arbitrum, Optimism share classes) vs.
  ~0.20% (Aptos, Avalanche, Polygon share classes), per secondary-source reporting only — not
  verified against a primary fee schedule.

## Flagged gaps

- Exact current (Sept 2026) full chain list and precise AUM.
- Redemption timing/mechanics for direct (non-Circle) redemptions.
- Wormhole's specific role, from a primary source.
- Non-U.S. offering mechanics given the BVI wrapper.
- Primary-source fee schedule confirming the chain-dependent fee structure.

## Sources

- [StockTitan/PR — BlackRock launches BUIDL](https://www.stocktitan.net/news/BLK/black-rock-launches-its-first-tokenized-fund-buidl-on-the-ethereum-u1gqppflov4h.html)
- [Crane Data summary](https://www.cranedata.com/archives/all-articles/10236/)
- [OTC Markets Form D/A filing](https://www.otcmarkets.com/filing/html?id=18621246&guid=Zyt-knONFgGAB3h)
- [SEC CFTC written input, Carlos Domingo/Securitize](https://www.sec.gov/files/ctf-written-input-carlos-domingo-securitize-100325.pdf)
- [Circle press release — USDC smart contract for BUIDL](https://www.circle.com/pressroom/circle-announces-usdc-smart-contract-for-transfers-by-blackrocks-buidl-fund-investors)
- [The Block — multi-chain expansion](https://www.theblock.co/post/326288/blackrock-buidl-aptos-arbitrum-avalanche-optimism-polygon)
- [PR Newswire — new BUIDL share classes](https://www.prnewswire.com/news-releases/blackrock-launches-new-buidl-share-classes-across-multiple-blockchains-to-expand-access-and-potential-of-buidl-ecosystem-302304035.html)
- [CoinMarketCap — Solana expansion](https://coinmarketcap.com/academy/article/blackrock-expands-buidl-to-solana-as-tokenized-fund-surpasses-dollar17-billion)
- [BNB Chain blog — BUIDL on BNB Chain](https://www.bnbchain.org/en/blog/blackrocks-buidl-fund-launches-bnb-chain-tokenized-by-securitize-and-accepted-as-collateral-on-binance)
- [Zerocap — BUIDL overview](https://zerocap.com/insights/snippets/what-is-buidl-blackrock/)
