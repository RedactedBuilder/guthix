# GUTHIX Protocol

> **The Synthetic Liquidity Standard**  
> **Version:** 1.0.0 (Protocol-First)  
> **Status:** 🚧 In Development  
> **Network:** Solana (Operations) | Base (Governance Hub)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Solana](https://img.shields.io/badge/Solana-Anchor-9945FF.svg?logo=solana)](https://solana.com)
[![Base](https://img.shields.io/badge/Base-EVM-0052FF.svg?logo=base)](https://base.org)
[![Wormhole](https://img.shields.io/badge/Wormhole-NTT-000000.svg?logo=wormhole)](https://wormhole.com)

---

## 📖 Overview

**GUTHIX** is a decentralized liquidity protocol designed to unify fragmented stablecoin markets. By aggregating diverse stable assets (USDC, sUSDe, syrupUSDC) into a **Unified Liquidity Reserve**, Guthix mints **gxUSD**, a synthetic stablecoin pegged 1:1 to the USD.

Unlike traditional protocols, Guthix operates without an inflationary governance token in v1.0.0. We focus on **Real Yield** for liquidity providers, **Hybrid Liquidity** (AMM + CLOB), and **Multichain Accessibility** (Wormhole + LayerZero routing).

### 🔑 Key Features

*   **Synthetic Stablecoin (gxUSD):** Fully backed by a basket of yield-bearing and standard stablecoins.
*   **Hybrid Liquidity Model:** 
    *   **Meteora StableSwap AMM** for core peg stability (gxUSD/USDC).
    *   **Solana CLOB** for efficient bridge access pairs (gxUSD/USDT0).
*   **Yield Flywheel:** 100% of collateral yield is reinvested to deepen exotic liquidity pools (whUSDC, axlUSDC), reducing slippage and driving volume.
*   **Real Yield Economics:** 100% of swap/trading fees flow directly to Liquidity Providers. No inflationary token emissions.
*   **Multichain Native:** 
    *   **Wormhole NTT (Lock/Unlock):** Canonical supply tracking on Base.
    *   **LayerZero Access:** Via deep gxUSD/USDT0 CLOB liquidity.
*   **Tokenless Governance:** Multi-sig → Guardian Council → DAO (Phase 3).

---

## 🏗 Architecture

### Hub-and-Spoke Model

```mermaid
graph TD
    Base[Base (Hub)] -->|Wormhole NTT Lock/Unlock| Solana[Solana (Spoke)]
    Solana -->|Meteora AMM| CorePools[Core Pools: gxUSD/USDC]
    Solana -->|Meteora AMM | YieldPools[Yield Pools: sUSDe/gxUSD]
    Solana -->|CLOB | BridgePools[Bridge Pools: gxUSD/USDT0]
    YieldPools -->|Yield Reinvestment| BridgePools
    Base -->|Governance| MultiSig[Foundation Multi-sig]
```

### Liquidity Venues

| Venue | Type | Use Case | Configuration |
| :--- | :--- | :--- | :--- |
| **Meteora** | StableSwap AMM | Core Stability & Collateral Arb | Low slippage near peg |
| **Solana CLOB** | Central Limit Order Book | Bridge Access (USDT0, whUSDC) | Directional limit orders |

### Collateral Framework (v1.0.0)

*   **Tier 1 (Core):** USDC (Native)
*   **Tier 1.5 (Yield):** sUSDe (Ethena), syrupUSDC (Syrup Finance)
*   **Tier 2 (Trading Pair Only):** USDT0 (LayerZero), whUSDC (Wormhole)
*   **Reserve Safety Fund:** Funded by 50% of Mint/Redeem fees. Target: 5% of TVL.

---

## 🚀 Getting Started

### Prerequisites

*   **Rust** (Latest stable version)
*   **Solana Tool Suite** (v1.16+)
*   **Anchor Framework** (v0.29+)
*   **Node.js** (v18+)
*   **Yarn** or **npm**

### Installation

1.  **Clone the Repository**
    ```bash
    git clone https://github.com/guthix-protocol/guthix-core.git
    cd guthix-core
    ```

2.  **Install Dependencies**
    ```bash
    # Solana/Anchor
    cargo build-bpf
    
    # Frontend/SDK
    cd client
    yarn install
    ```

3.  **Local Development**
    ```bash
    # Start local validator
    solana-test-validator
    
    # Deploy programs (Devnet)
    anchor deploy --provider.cluster devnet
    ```

4.  **Run Tests**
    ```bash
    anchor test
    ```

---

## 📂 Repository Structure

```text
guthix-core/
├── programs/               # Solana Anchor Programs
│   ├── guthix-vault/       # Collateral management & mint/burn
│   ├── guthix-ntt/         # Wormhole NTT integration
│   └── guthix-governance/  # Multi-sig & DAO logic (Future)
├── client/                 # TypeScript SDK & Frontend
│   ├── src/
│   │   ├── sdk/            # Protocol interaction SDK
│   │   ├── components/     # React components
│   │   └── utils/          # Helpers (Oracle, Fee calc)
│   └── tests/              # E2E Tests
├── docs/                   # Documentation
│   ├── litepaper.pdf       # Latest Litepaper
│   └── api.md              # API Reference
├── scripts/                # Deployment & Maintenance Scripts
├── Anchor.toml             # Anchor Configuration
└── README.md               # This file
```

---

## 🗓 Roadmap

| Phase | Timeline | Milestones |
| :--- | :--- | :--- |
| **Phase 1: Solana Foundation** | Q1 2024 | • Core Vault & Mint/Burn <br> • Meteora AMM Integration <br> • Devnet Audit |
| **Phase 2: Base Hub** | Q2 2024 | • Wormhole NTT (Lock/Unlock) <br> • Base Governance Hub <br> • Mainnet Beta |
| **Phase 3: Multichain** | Q3 2024 | • CLOB Liquidity (USDT0) <br> • Aggregator Integrations (Jupiter, LI.FI) <br> • Yield Reinvestment Automation |
| **Phase 4: Decentralization** | Q4 2024 | • Safety Fund > 5% TVL <br> • DAO Transition <br> • Admin Key Removal |

---

## 🛡 Security

Security is our highest priority. Guthix employs a defense-in-depth strategy.

*   **Audits:** All smart contracts will be audited by recognized firms (e.g., OtterSec, Neodyme) before mainnet launch.
*   **Oracles:** Dual-oracle system (Pyth + Switchboard) for price feeds and peg monitoring.
*   **Circuit Breakers:** On-chain monitors can pause minting if oracle deviations exceed thresholds.
*   **Safety Fund:** 50% of Mint/Redeem fees accrue to an insurance fund to cover potential shortfalls.
*   **Bug Bounty:** A bug bounty program will be launched via Immunefi prior to TGE.

### Reporting a Vulnerability

Please do not open public issues for security vulnerabilities. Report them directly to **security@guthix.finance**.

---

## 🤝 Contributing

We welcome contributions from the community!

1.  **Fork the Repo**
2.  **Create a Feature Branch** (`git checkout -b feature/amazing-feature`)
3.  **Commit Changes** (`git commit -m 'Add amazing feature'`)
4.  **Push to Branch** (`git push origin feature/amazing-feature`)
5.  **Open a Pull Request**

Please read our [Contributing Guidelines](./CONTRIBUTING.md) (Coming Soon) for details on our code of conduct and development process.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

---

## ⚠️ Disclaimer

*This document is for informational purposes only and does not constitute financial advice, investment recommendations, or an offer to sell or solicitation of an offer to buy any securities. GUTHIX is a decentralized protocol. Participants acknowledge that they are using the software at their own risk. Cryptocurrency investments are volatile and high-risk. Please consult a qualified financial advisor before making any investment decisions.*

*© 2026 Guthix Protocol Foundation. All rights reserved.*
```
