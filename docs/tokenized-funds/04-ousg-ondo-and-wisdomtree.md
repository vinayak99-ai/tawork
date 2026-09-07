# OUSG (Ondo Finance) & WisdomTree Digital Funds (WTGXX)

## Part A — OUSG (Ondo Short-Term US Government Treasuries)

### 1. Fund structure

- Issuing entity: **Ondo I LP**, a **Delaware** limited partnership (confirmed via SEC Form D
  filings, CIK 0001957431, filed 2023-01-11, amended annually through 2026-01-20). Investors become
  limited partners; OUSG tokens represent unitized LP interests. General partner: Ondo I GP LLC
  (Delaware LLC, wholly owned by Ondo Finance Inc.); investment manager: Ondo Capital Management
  LLC (Delaware LLC, also wholly owned by Ondo Finance Inc.).
- **⚠️ Conflicting secondary claim**: several secondary/AI-aggregated sources describe the wrapper
  as a *Cayman Islands* LP. This directly conflicts with the primary SEC Form D filing (Delaware).
  No primary source (prospectus/PPM or SEC filing) confirms a Cayman entity — treat "Delaware" as
  the reliably sourced fact and the Cayman claim as unconfirmed.
- Underlying holdings: since a 2024 restructuring, >99% of assets are invested in **BlackRock's
  BUIDL**. Ondo's own docs describe a broader "Qualified Access" portfolio construction potentially
  spanning money-market/short-duration funds from BlackRock, Fidelity, Franklin Templeton, and
  WisdomTree, plus USDC/bank deposits for liquidity — current published allocation still shows
  BUIDL as dominant. Originally (2023) backed by short-term Treasury ETFs (primarily iShares SHV)
  before the 2024 BUIDL migration.

### 2. Regulatory regime

- **ICA §3(c)(7)** private-fund exemption (confirmed via SEC Form D item 3C.7).
- **Reg D Rule 506(c)** securities offering exemption (private placement, general-solicitation
  permitted, verified accredited investors).
- Eligible investors: **Qualified Purchasers** and verified **Accredited Investors**, subject to
  KYC/AML/sanctions screening; non-US investors must meet comparable home-jurisdiction standards.
- Not registered under the '40 Act — the key structural contrast with WisdomTree (Part B).

### 3. Service providers

| Function | Provider | Confidence |
|---|---|---|
| Fund administrator | **NAV Consulting** — daily read-only access to fund accounts, calculates NAV, prepares reports | Named directly by Ondo |
| Independent daily verification/attestation agent | **Ankura Trust Company** — confirms reserve assets exist, are in custody, and match token supply outstanding | Named directly by Ondo |
| Custodian (traditional/cash) | Clear Street LLC (secondary sources); one source also names BNY Mellon for the BUIDL position | Secondary only, not on Ondo's own docs |
| Crypto custodian/broker | Coinbase Custody / Coinbase Prime | Referenced generically in Ondo docs |
| **Transfer agent** | **Not disclosed by name anywhere found.** RWA.xyz explicitly shows "Transfer Agent: not listed"; Ondo's own docs don't name one — Ondo appears to manage the on-chain allowlist/compliance function itself rather than naming a discrete registered TA (unlike WisdomTree) | Explicitly unconfirmed |
| Auditor | Not reliably confirmed (one low-quality result conflated NAV Consulting with an audit role) | Unconfirmed |

**Bottom line**: Ankura Trust's documented role is independent daily reserve verification/
attestation, not transfer agency; Clear Street's documented role (where named at all) is custody/
prime brokerage. **No source names a discrete transfer agent for OUSG** — a notable gap versus
every other fund in this comparison set.

### 4. Technology / blockchain

- Token issued on **Ethereum, Polygon, Solana, and XRP Ledger** (RWA.xyz); one 2026 secondary
  source lists **Mantle** instead of/in addition to XRPL — chain support has evidently expanded
  over time and sources don't fully agree on the current live list.
- **Securitize** is referenced as the platform routing subscriptions into BlackRock's BUIDL primary
  market.
- Ondo is separately building its own L1, **"Ondo Chain,"** for broader RWA settlement/
  interoperability (Ethereum, Solana, XRPL, Mantle, Arbitrum, Aptos, Sui, Cosmos ecosystem) — this
  is separate infrastructure, not confirmed as OUSG's current settlement chain.

### 5. Investor access

- Minimum investment: Ondo's own docs state **$5,000** for instant subscriptions/redemptions
  (with $100K/$50K minimums for non-instant paths); other trackers cite a flat $100,000 — likely
  reflects different investment "modes" documented inconsistently; treat Ondo's own $5,000 figure
  as more authoritative.
- Eligible investors: Qualified Purchasers and Accredited Investors after Ondo's KYC onboarding —
  no retail access.
- Subscription/redemption: **instant, 24/7/365** minting and redemption using USDC or PYUSD,
  priced off NAV, riding on same-day mint/redeem access to BUIDL's primary market. Not confirmed
  whether Ondo also provides its own balance-sheet liquidity buffer distinct from the BUIDL primary
  market.
- Fees: management fee 0.15% (waived through Jan 1, 2027 per Ondo's docs); RWA.xyz separately shows
  0% fees, likely reflecting the waived state.

### 6. Basic facts

- Launch: **January 26, 2023**.
- AUM: varies notably by source/date — ~$172.8M (Dec 2024) → ~$409.5M (RWA.xyz, date unclear) →
  ~$625M (Q1 2026) — treat the ~$400–625M range as directional for 2026, not a precise figure.

### Flagged gaps

- Jurisdiction (Delaware vs. Cayman) — unresolved conflict between primary and secondary sources.
- Transfer agent — not named anywhere.
- Auditor — not reliably named.
- Custodian — only in secondary sources, not Ondo's own docs.
- AUM and current chain list — meaningfully inconsistent across sources.

---

## Part B — WisdomTree Digital Funds (WTGXX)

Secondary comparison point — chosen because it takes the *opposite* regulatory approach from
OUSG/BUIDL/USCC.

- **Regulatory status**: WTGXX (WisdomTree Government/Treasury Money Market Digital Fund) is a
  genuine **SEC-registered, open-end mutual fund under the '40 Act** (Investment Company Act File
  No. 811-23659) — same regime as any ordinary registered mutual fund, full disclosure/prospectus
  obligations. This is the core contrast with OUSG's 3(c)(7)/Reg D private-fund structure.
- **Transfer agent**: **WisdomTree Transfers, Inc.** — an in-house/affiliated TA maintaining the
  official book-entry share register; blockchain records are a secondary, reconciled
  representation, not the legal record of ownership. (One source names "Securrency Transfers,
  Inc." — Securrency was WisdomTree's original blockchain technology partner/subsidiary before
  being folded into WisdomTree's own TA entity; "WisdomTree Transfers, Inc." appears to be the
  current name.)
- **Blockchain**: Shares digitally represented ("Secondary Record") on the **Stellar** blockchain,
  with **Ethereum** cited as an additional/interoperable network. The blockchain record is
  explicitly **secondary/non-authoritative** — legal ownership stays in the TA's book-entry system,
  reconciled to the blockchain at least daily.
- **Investor access**: much lower barrier than OUSG — **$1.00 minimum investment**, open to
  ordinary mutual fund investors (not restricted to QPs/accredited investors), reflecting its
  retail-eligible '40 Act registration.
- **Launch/innovation**: commenced operations **October 2023**; WisdomTree markets this structure
  as enabling **24/7 trading and instant settlement** for a registered mutual fund's tokenized
  shares — described in its own IR materials as a first for SEC-registered tokenized fund shares
  trading/settling within the US regulatory perimeter via a dealer-principal liquidity model.
- **Custodian**: not confirmed by name in any accessible source (fact sheet PDF returned a 403).

### Flagged gaps

- Custodian — not found in any accessible source.
- Fund administrator/accountant, auditor — not confirmed.

## Sources

- [SEC EDGAR — Ondo I LP Form D/A filings](https://www.sec.gov/Archives/edgar/data/1957431/000195743126000001/0001957431-26-000001-index.htm)
- [eco.com — OUSG Deep Dive 2026](https://eco.com/support/en/articles/15254014-ousg-deep-dive-2026-ondo-s-short-treasury-fund)
- [readi.fi — OUSG fund profile](https://readi.fi/asset/fund-ousg-ondo-short-term-us-government-treasuries-by-ondo-i-lp/)
- [Ondo docs — OUSG Overview](https://docs.ondo.finance/qualified-access-products/ousg/overview)
- [Ondo docs — Trust and Security](https://docs.ondo.finance/trust-and-security)
- [Ondo docs — Trust and Transparency](https://docs.ondo.finance/qualified-access-products/ousg/trust-and-transparency)
- [RWA.xyz — OUSG](https://app.rwa.xyz/assets/OUSG)
- [Ondo Chain FAQ](https://docs.ondo.finance/ondo-chain/faq)
- [SEC EDGAR — WisdomTree Government Money Market Digital Fund filing](https://www.sec.gov/Archives/edgar/data/1859001/000121465923015364/z1117230497.htm)
- [WisdomTree fund fact sheet (WTGXX)](https://www.wisdomtree.com/-/media/us-media-files/documents/resource-library/fund-fact-sheets/digital/wtgxx.pdf)
- [WisdomTree IR — 24/7 trading and instant settlement](https://ir.wisdomtree.com/news-events/press-releases/detail/777/wisdomtree-to-launch-247-trading-and-instant-settlement)
