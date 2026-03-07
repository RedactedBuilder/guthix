# GUTHIX Protocol: Compliance Principles

> **Document Version:** 1.0.0  
> **Last Updated:** March 2026  
> **Status:** 🚧 Experimental / Public Reference  
> **Scope:** Worldwide deployment principles for sgxUSD vault token  
> **Audience:** Users, contributors, partners, regulators, community auditors  

---

## ⚠️ Critical Disclaimer

*This document outlines GUTHIX Protocol's compliance principles and design philosophy. It does not constitute legal advice, regulatory approval, or a guarantee of compliance in any jurisdiction. GUTHIX Protocol is experimental software provided "as-is." Participants use the protocol at their own risk. Cryptocurrency regulations are evolving rapidly; this framework may be updated quarterly or upon material regulatory change. Consult qualified legal counsel in your jurisdiction before participating.*

---

## 🎯 Mission & Philosophy

GUTHIX Protocol is designed as an **experimental, non-custodial liquidity hub** on Solana. Our approach to compliance is rooted in **minimalism, transparency, and user empowerment**.

We believe that decentralized protocols should:
- **Reduce Counterparty Risk:** Users retain custody of assets; protocol smart contracts are non-custodial vaults.
- **Maximize Transparency:** All collateral, NAV calculations, and protocol actions are verifiable on-chain.
- **Prioritize Safety:** Security audits, bug bounties, and emergency controls are foundational, not optional.
- **Respect Jurisdictional Boundaries:** Users are responsible for complying with local laws; the protocol provides tools for informed participation.

sgxUSD is framed as an **experimental vault token**, not a stablecoin, security, or payment instrument. Yield is expressed as passive NAV appreciation derived from external market sources, not protocol-generated profits.

---

## 🛡️ Core Compliance Principles

### 1. Non-Custodial Design
GUTHIX does not hold user funds in traditional custodial wallets. Collateral is locked in audited smart contracts. Users interact via self-custodied wallets (e.g., Phantom, Solflare). This design reduces regulatory hooks associated with custodial financial services.

### 2. No Profit Guarantees
The protocol makes no promises of yield, returns, or NAV stability. sgxUSD value floats based on market performance of underlying assets. All communications avoid language implying guaranteed profits or fixed returns.

### 3. Structural Diversification
Yield is sourced from multiple uncorrelated mechanisms (e.g., funding rates, credit yield, RWA yield, trading fees). This reduces reliance on any single protocol or asset class, enhancing systemic resilience.

### 4. Minimalist Architecture
The core smart contract surface is minimized (7 instructions) to reduce audit complexity and attack vectors. Fewer features mean fewer risks and clearer regulatory characterization.

### 5. Iterative Engagement
We monitor regulatory developments globally and adapt our principles responsibly. This document is a living framework that evolves with the landscape.

---

## 🔍 Transparency Commitments

GUTHIX commits to the following public disclosures to enable user verification and regulatory clarity:

| Disclosure | Frequency | Location |
|------------|-----------|----------|
| **Smart Contract Code** | Permanent | GitHub (`guthix-core`) |
| **Audit Reports** | Pre-launch + Major Updates | GitHub `/audits` |
| **Real-Time NAV** | Continuous | On-chain + Dashboard |
| **Collateral Composition** | Continuous | On-chain + Dashboard |
| **Keeper Activity Logs** | Continuous | On-chain Explorer |
| **Emergency Actions** | As Occurring | Governance Forum / Twitter |
| **Compliance Principles** | Quarterly Review | This Document |

**Dashboard Access:** Users can verify collateral backing and NAV calculations at [dashboard.guthix.finance](https://dashboard.guthix.finance).

---

## 🙋 User Responsibilities

Participation in GUTHIX Protocol is permissionless, but **legal compliance is the user's responsibility**. By interacting with the protocol, users acknowledge:

1. **Jurisdictional Compliance:** You are responsible for ensuring participation is legal in your country, state, or region.
2. **Tax Obligations:** You are responsible for reporting income, capital gains, or other tax liabilities arising from protocol usage.
3. **Risk Acceptance:** You understand and accept risks related to smart contracts, market volatility, liquidity, and regulatory uncertainty.
4. **No Restricted Use:** You confirm you are not accessing the protocol from sanctioned jurisdictions or for illicit purposes.

**Access Restrictions:** The protocol interface may implement geo-fencing or access restrictions to comply with applicable laws. Smart contracts remain permissionless, but frontend access is managed responsibly.

---

## ⚠️ Risk Acknowledgments

Users should understand the following key risk categories before participating:

| Risk Category | Description |
|---------------|-------------|
| **Smart Contract Risk** | Code may contain bugs or vulnerabilities despite audits. Loss of funds is possible. |
| **Market Risk** | sgxUSD NAV floats and may decrease. Yield sources are volatile and not guaranteed. |
| **Counterparty Risk** | Yield depends on external protocols (e.g., Ethena, Maple, Ondo). Their failure impacts sgxUSD. |
| **Liquidity Risk** | Exits occur via secondary markets. Slippage may occur during high volatility. No redemption queue exists. |
| **Regulatory Risk** | Laws may change, potentially affecting access, tax treatment, or protocol viability. |
| **Cross-Chain Risk** | Bridging (e.g., Wormhole) introduces additional smart contract and finality risks. |

*See [User-Facing Risk Disclosure](./docs/RISK_DISCLOSURE.md) for detailed acknowledgments required before swap transactions.*

---

## 📜 Version History

| Version | Date | Changes |
|---------|------|---------|
| `1.0.0` | Mar 2026 | Initial public framework for experimental deployment |

---

## 📄 Related Documents

- [Litepaper](./litepaper.pdf) — Technical and economic overview
- [Risk Disclosure](./docs/RISK_DISCLOSURE.md) — User-facing transaction warnings
- [Security Policy](./SECURITY.md) — Bug bounty and vulnerability reporting
- [Terms of Service](./docs/TERMS.md) — Frontend interface usage terms

---

*© 2026 Guthix Protocol. This document is licensed under MIT License.  
Built on Solana. Secured by minimalism. Compliant by design.*
