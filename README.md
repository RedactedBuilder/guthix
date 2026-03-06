# GUTHIX Protocol

> **The Yield-Bearing Liquidity Hub**
> **Version:** 3.0.0
> **Status:** 🚧 In Development
> **Network:** Solana (Primary) | Base (Bridge)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Solana](https://img.shields.io/badge/Solana-Anchor-9945FF.svg?logo=solana)](https://solana.com)
[![Base](https://img.shields.io/badge/Base-EVM-0052FF.svg?logo=base)](https://base.org)
[![Wormhole](https://img.shields.io/badge/Wormhole-NTT-000000.svg?logo=wormhole)](https://wormhole.com)

---

## 📖 Overview

**GUTHIX** is a minimalist decentralized liquidity protocol and the first yield-bearing liquidity hub for the Solana stablecoin ecosystem. It is built around a single token: **sgxUSD**.

sgxUSD generates yield from five structurally uncorrelated sources simultaneously — Ethena funding rates, Maple credit yield, Ondo RWA yield, and trading fees from bridge arbitrage across Wormhole-bridged stablecoin pairs. No single source dominates. If one yield source compresses, the others carry the basket.

Users swap in, hold, and appreciate. That is the entire user experience.

There are no governance tokens. No token emissions. No staking UI. No redemption queues. No synthetic units. One token, one action, passive yield.

### 🔑 Key Features

- **Yield-Bearing Foundation:** sgxUSD is paired against yield-bearing assets from day one. Native collateral yield compounds directly into sgxUSD NAV — no harvesting, no conversion, no yield leakage.
- **Bridge Liquidity Hub:** Deep StableSwap pools for Wormhole-bridged USDC variants from Ethereum, BNB Chain, and Polygon — capturing bridge arb fees as a structurally distinct yield source.
- **Diversified Yield:** Five uncorrelated yield sources in a single token. Funding rates, credit yield, RWA yield, and bridge arb fees compound simultaneously.
- **Pure Real Yield:** 100% of all revenue flows to sgxUSD holders via NAV appreciation. Zero emissions. Zero dilution.
- **Minimalist Security:** Single custom Anchor program (`guthix-core`). Governance via Squads Multisig. Bridging via Wormhole NTT.
- **Cross-Chain:** sgxUSD bridges canonically to Base via Wormhole NTT — accessible to EVM users without manual bridging.

---

## 🏗 Architecture

### System Diagram

```mermaid
graph TD
    User[User] -->|Swap USDC| Interface[UI / Jupiter]
    Interface -->|IXs| Core[guthix-core Program]

    Core -->|Lock Collateral| Vault[Collateral Vault]
    Core -->|Mint / Burn| Token[SPL Token: sgxUSD]

    Guardian[Guardian Squads] -->|Admin| Core
    Keeper[Off-Chain Keeper] -->|Monitor & Rebalance| Core
    Keeper -->|Deploy POL| Meteora[Meteora StableSwap Pools]
    Keeper -->|Route Fees + Excess USDC| Vault

    Wormhole[Wormhole NTT] -->|Bridge| Base[Base Hub]

    style Core fill:#f9f9f9,stroke:#333,stroke-width:4px
    style Keeper fill:#e1f5fe,stroke:#0277bd,stroke-width:2px
    style Guardian fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
```

### Component Breakdown

| Component | Implementation | Responsibility |
| :--- | :--- | :--- |
| **Core Logic** | `guthix-core` (Anchor) | Collateral locking, NAV calculation, sgxUSD minting/burning, config |
| **Governance** | Squads Protocol (Multisig) | Parameter updates, emergency pauses, Keeper authorization |
| **Token** | SPL Token | sgxUSD vault token |
| **Bridging** | Wormhole NTT | Canonical lock/mint across Solana ↔ Base |
| **Liquidity** | Meteora StableSwap | Protocol-Owned Liquidity across all pool tiers |
| **Maintenance** | Off-Chain Keeper (Rust/TS) | POL rebalancing, NAV updates, Swap-to-Grow routing, pool monitoring |

### Three-Tier Pool Architecture

| Tier | Pool | Role | Swap Fee |
| :--- | :--- | :--- | :--- |
| **Yield** | sgxUSD / sUSDe | Ethena funding rate yield | 0.10% |
| **Yield** | sgxUSD / syrupUSDC | Maple credit yield | 0.10% |
| **Yield** | sgxUSD / USDY | Ondo RWA yield | 0.10% |
| **Bridge** | sgxUSD / whUSDC.e | Ethereum bridge arb fees | 0.20% |
| **Bridge** | sgxUSD / whUSDC.bnb | BNB Chain bridge arb fees | 0.20% |
| **Bridge** | sgxUSD / whUSDC.poly | Polygon bridge arb fees | 0.20% |
| **Bridge** | sgxUSD / USDT0 | Cross-chain USDT arb fees | 0.20% |
| **Ramp** | sgxUSD / USDC | Entry / exit only | 0.05% |

---

## 🚀 Getting Started

### Prerequisites

- **Rust** (Latest stable version)
- **Solana Tool Suite** (v1.16+)
- **Anchor Framework** (v0.29+)
- **Node.js** (v18+)
- **Yarn** or **npm**

### Installation

1. **Clone the Repository**
    ```bash
    git clone https://github.com/guthix-protocol/guthix-core.git
    cd guthix-core
    ```

2. **Install Dependencies**
    ```bash
    # Solana/Anchor
    cargo build-bpf

    # Frontend/SDK
    cd client
    yarn install
    ```

3. **Local Development**
    ```bash
    # Start local validator
    solana-test-validator

    # Deploy programs (Devnet)
    anchor deploy --provider.cluster devnet
    ```

4. **Run Tests**
    ```bash
    anchor test
    ```

---

## 📂 Repository Structure

```text
guthix-core/
├── programs/
│   └── guthix-core/       # Single custom program (Vault + Config + NAV)
├── clients/
│   ├── sdk/               # TypeScript SDK for interaction
│   └── keeper/            # Rust/TS bot for POL rebalancing and NAV updates
├── scripts/
│   ├── deploy-squads.ts   # Setup Guardian Multisig
│   ├── deploy-ntt.ts      # Configure Wormhole NTT
│   └── init-core.ts       # Initialize Vault Program
├── tests/
│   └── guthix-core.test.ts
├── Anchor.toml
├── Cargo.toml
└── README.md
```

---

## 🛠 Smart Contract Interface

### Program Instructions

```rust
// guthix-core program instructions
pub enum Instruction {
    Initialize,           // Setup vault, token mint, guardian
    Deposit,              // Lock collateral → Mint sgxUSD at current NAV
    Withdraw,             // Burn sgxUSD → Withdraw collateral at current NAV
    WithdrawCollateral,   // Keeper-only: Unlock collateral for POL deployment
    UpdateNAV,            // Keeper-only: Update sgxUSD exchange rate
    UpdateConfig,         // Guardian-only: Adjust params, pause, keeper address
    Pause,                // Guardian-only: Emergency halt
}
```

✅ **Only 7 instructions.** No staking logic. No governance voting. No emission schedules. No synthetic token minting.

### Account Structure

| Account | Type | Authority | Description |
| :--- | :--- | :--- | :--- |
| `Vault` | PDA | Program | Holds all collateral (sUSDe, syrupUSDC, USDY, whUSDC variants, USDC) |
| `Config` | PDA | Guardian | Protocol parameters (fees, limits, keeper address, pool thresholds) |
| `State` | PDA | Program | Global state (TVL, sgxUSD supply, NAV, paused status) |
| `Keeper` | PDA | Guardian | Authorized keeper for NAV updates and rebalancing operations |

### Example: Depositing for sgxUSD (TypeScript SDK)

```typescript
import { GuthixSDK } from '@guthix-protocol/sdk';

const sdk = new GuthixSDK({ cluster: 'mainnet-beta' });

// Deposit USDC → Receive sgxUSD at current NAV
const tx = await sdk.deposit({
  collateralMint: USDC_MINT,
  collateralAmount: 1000_000_000, // 1000 USDC (6 decimals)
  minSgxUsdOut: 995_000_000,      // Slippage protection
  owner: wallet.publicKey,
});

await sdk.sendTransaction(tx);
// sgxUSD appreciates automatically across five yield sources from this point
```

### Example: Exiting via Secondary Market

```typescript
// Exit by swapping sgxUSD → USDC on Jupiter
// POL absorbs the flow via Silent Rebalance — no redemption instruction needed
const jupiter = new Jupiter({ cluster: 'mainnet-beta' });

const route = await jupiter.computeRoutes({
  inputMint: SGXUSD_MINT,
  outputMint: USDC_MINT,
  amount: 1000_000_000,
});

await jupiter.exchange({ routeInfo: route.routesInfos[0] });
```

### Example: Bridging to Base

```typescript
// Bridge sgxUSD from Solana to Base via Wormhole NTT
import { WormholeNTT } from '@wormhole-foundation/sdk';

const ntt = new WormholeNTT({ sourceChain: 'solana', targetChain: 'base' });

const tx = await ntt.transfer({
  tokenMint: SGXUSD_MINT,
  amount: 1000_000_000,
  recipient: evmWalletAddress,
  owner: wallet.publicKey,
});

await sdk.sendTransaction(tx);
// sgxUSD minted canonically on Base — yield continues to accrue
```

---

## 🤖 Keeper Bot

The off-chain Keeper manages POL deployment across all three tiers, NAV updates, Swap-to-Grow routing, and bridge pool monitoring. All vault operations are PDA-signed with per-epoch withdrawal limits — the Keeper holds no privileged mint authority.

### Setup

```bash
cd clients/keeper
cargo build --release
```

### Configuration

```toml
# keeper/config.toml
[keeper]
cluster = "mainnet-beta"
keeper_keypair = "~/.config/solana/keeper.json"
guthix_core_program = "GUTHIX_PROGRAM_ID"

[strategy]
# Swap-to-Grow: route excess USDC to vault if USDC > 55% of sgxUSD/USDC pool
swap_to_grow_threshold = 0.55
# Silent Rebalance: rebalance POL if sgxUSD > 60% of any pool
rebalance_threshold = 0.60
# Max POL allocation per yield pool (% of TVL)
max_yield_pool_allocation_pct = 20
# Max POL allocation per bridge pool (% of TVL)
max_bridge_pool_allocation_pct = 10
# Bridge pool depeg alert threshold
bridge_depeg_threshold = 0.995
```

### Running the Keeper

```bash
# Start keeper bot
cargo run --release -- --config config.toml

# Run with monitoring dashboard
cargo run --release -- --config config.toml --monitor
```

### Keeper Responsibilities

| Task | Frequency | Trigger |
| :--- | :--- | :--- |
| **Update sgxUSD NAV** | Every epoch | Collateral yield accrual + fee collection |
| **Swap-to-Grow** | As needed | Excess USDC in sgxUSD/USDC pool > threshold |
| **Silent Rebalance** | As needed | sgxUSD sell pressure exceeds POL depth |
| **Bridge Fee Collection** | Every epoch | Tier 2 pool trading fee accrual |
| **Bridge Pool Monitoring** | Every block | whUSDC variant depeg detection |
| **Vault Solvency Check** | Every block | NAV vs. total sgxUSD supply |

### Swap-to-Grow Logic

When users buy sgxUSD on secondary markets (USDC → sgxUSD), USDC accumulates in the Tier 3 POL pool. The Keeper routes it to the vault:

1. **Detect** excess USDC in sgxUSD/USDC pool above threshold
2. **Withdraw** excess USDC from POL position
3. **Route** USDC to vault as additional collateral
4. **Update NAV** — collateral backing per sgxUSD increases; no new sgxUSD minted

**Result:** Every secondary market buy raises sgxUSD NAV for all holders instantly.

---

## 🧪 Testing

### Unit Tests

```bash
# Run all tests
anchor test

# Run specific test suite
anchor test --skip-deploy --skip-local-validator -- tests/guthix-core.test.ts
```

### Test Coverage

```bash
# Generate coverage report
cargo llvm-cov --lcov --output-path lcov.info
```

### Devnet Deployment

```bash
# Deploy to devnet
anchor deploy --provider.cluster devnet

# Initialize protocol
yarn ts-node scripts/init-core.ts --cluster devnet
```

---

## 📊 Monitoring & Analytics

### Key Metrics Dashboard

| Metric | Endpoint | Description |
| :--- | :--- | :--- |
| **NAV per sgxUSD** | `/api/nav` | Current net asset value per token |
| **Total Collateral** | `/api/collateral` | Sum of all locked collateral (USD) |
| **Yield Basket** | `/api/basket` | Per-asset allocation across all tiers |
| **Bridge Volume** | `/api/bridge` | 24h/7d volume per Tier 2 pool |
| **Protocol Revenue** | `/api/revenue` | 24h/7d/30d fee + collateral yield by source |
| **Keeper Activity** | `/api/keeper` | Last rebalance, Swap-to-Grow, NAV update |

### On-Chain Verification

All protocol state is publicly verifiable on Solana:

```bash
# View vault collateral
solana account <VAULT_PDA> --output json

# View protocol config
solana account <CONFIG_PDA> --output json

# View sgxUSD NAV
solana account <STATE_PDA> --output json
```

---

## 🛡 Security

### Defense-in-Depth Strategy

| Layer | Implementation | Purpose |
| :--- | :--- | :--- |
| **Minimal Code** | Single custom program (`guthix-core`); 7 instructions; single token | Smallest possible audit surface |
| **Standard Dependencies** | Squads, Wormhole, SPL Token, Meteora | Battle-tested, audited infrastructure only |
| **Off-Chain Keeper** | Logic upgradable without redeployment; PDA-signed withdrawals | Isolate complexity; enable rapid iteration |
| **Guardian Multisig** | 3-of-5 trusted signers; emergency pause per pool | Human oversight for black-swan events |
| **Transparency** | Real-time NAV, collateral proofs, Keeper activity on-chain | Community verification; reduced information asymmetry |

### Audit Status

| Audit Firm | Status | Report |
| :--- | :--- | :--- |
| **Community Review** | ✅ Complete | [GitHub Issues](https://github.com/guthix-protocol/guthix-core/issues) |
| **OtterSec** | 🚧 In Progress | Expected Q3 2026 |
| **Immunefi Bounty** | 🚧 Planned | Post-mainnet launch |

### Reporting a Vulnerability

Please do not open public issues for security vulnerabilities. Report them directly to **security@guthix.finance**.

---

## 🤝 Contributing

We welcome contributions from the community!

1. **Fork the Repo**
2. **Create a Feature Branch** (`git checkout -b feature/amazing-feature`)
3. **Commit Changes** (`git commit -m 'Add amazing feature'`)
4. **Push to Branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

### Contribution Guidelines

- **Code Style:** Follow Rust/Anchor best practices. Run `cargo fmt` before committing.
- **Tests:** All new features must include unit and integration tests.
- **Documentation:** Update README and inline docs for any new instructions or accounts.
- **Security:** No PRs without security consideration. Tag `@guthix-security` for review.

Please read our [Contributing Guidelines](./CONTRIBUTING.md) for details on our code of conduct and development process.

---

## 🗓 Roadmap

| Phase | Timeline | Milestones |
| :--- | :--- | :--- |
| **Phase 1: Core Foundation** | Q2 2026 | • `guthix-core` devnet deployment <br> • SPL Token integration <br> • Keeper bot MVP <br> • All eight pools on devnet <br> • Community audit |
| **Phase 2: Mainnet Launch** | Q3 2026 | • Professional audit complete <br> • Collateral partner POL seeding (Ethena / Maple / Ondo) <br> • All eight pools live on mainnet <br> • Jupiter aggregator integration |
| **Phase 3: Multichain** | Q4 2026 | • Wormhole NTT (Solana ↔ Base) <br> • sgxUSD on Base DEXs <br> • Lending protocol integrations (Kamino / MarginFi) <br> • Public analytics dashboard |
| **Phase 4: Decentralization** | Q1 2027 | • Guardian Council activation <br> • Safety Fund >5% TVL <br> • Keeper bonding (optional) <br> • Additional collateral assets (community vote) |

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

---

## ⚠️ Disclaimer

*This repository is for informational purposes only and does not constitute financial advice, investment recommendations, or an offer to sell or solicitation of an offer to buy any securities. GUTHIX is a decentralized protocol. sgxUSD is a vault token whose value floats and is not guaranteed to appreciate. Collateral assets including sUSDe, syrupUSDC, USDY, and Wormhole-bridged stablecoin variants carry their own independent risks including depeg risk. Participants acknowledge that they are using the software at their own risk. Cryptocurrency investments are volatile and high-risk. Please consult a qualified financial advisor before making any investment decisions.*

---

## 📞 Contact

- **Website:** [www.guthix.finance](https://www.guthix.finance)
- **Twitter:** [@GuthixProtocol](https://twitter.com/GuthixProtocol)
- **Email:** [research@guthix.finance](mailto:research@guthix.finance)
- **Partnerships:** [partnerships@guthix.finance](mailto:partnerships@guthix.finance)
- **Security:** [security@guthix.finance](mailto:security@guthix.finance)

---

*© 2026 Guthix Protocol Foundation. All rights reserved.*
*Built on Solana. Secured by minimalism.*
*One token. Five yield sources. Liquidity that builds itself.*
