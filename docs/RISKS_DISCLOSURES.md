# GUTHIX Protocol: Risk Disclosures

> **Document Version:** 1.0.0  
> **Last Updated:** March 2026  
> **Status:** 🚧 Experimental / Public Disclosure  
> **Applies To:** sgxUSD vault token, GUTHIX Protocol smart contracts, all protocol interfaces  
> **Legal Effect:** All users must acknowledge these risks before participating  

---

## ⚠️ Critical Warning

**GUTHIX Protocol is experimental software provided "AS-IS" and "AS-AVAILABLE." By accessing or using the protocol, you acknowledge and accept ALL risks described below. If you do not understand or accept these risks, DO NOT use this protocol.**

**sgxUSD is NOT:**
- ❌ A stablecoin (value floats; not pegged to any asset)
- ❌ Insured (no FDIC, SIPC, or government backing)
- ❌ A bank deposit or savings account
- ❌ Guaranteed to maintain or increase in value
- ❌ Redeemable at a fixed price
- ❌ Regulated as traditional financial product in most jurisdictions

**You may lose some or all of your investment. Past performance does not indicate future results.**

---

## 📋 Table of Contents

1. [Protocol Overview](#protocol-overview)
2. [General Risk Acknowledgments](#general-risk-acknowledgments)
3. [Specific Risk Categories](#specific-risk-categories)
4. [Jurisdictional Risks](#jurisdictional-risks)
5. [Tax Implications](#tax-implications)
6. [No Warranties or Guarantees](#no-warranties-or-guarantees)
7. [Limitation of Liability](#limitation-of-liability)
8. [User Responsibilities](#user-responsibilities)
9. [Updates to This Document](#updates-to-this-document)
10. [Contact & Questions](#contact--questions)

---

## 📖 Protocol Overview

### What is GUTHIX Protocol?

GUTHIX Protocol is a decentralized, non-custodial liquidity hub on Solana that issues **sgxUSD**, a vault token representing a pro-rata claim on a diversified basket of yield-bearing digital assets.

### How Does It Work?

- Users swap USDC for sgxUSD via automated smart contracts
- Collateral is deployed across yield-generating strategies (e.g., Ethena, Maple, Ondo, liquidity pools)
- Yield accrues as NAV (Net Asset Value) appreciation
- Users exit by swapping sgxUSD back to USDC on secondary markets
- No redemption queue; no guaranteed exit price

### Key Characteristics

| Feature | Description |
|---------|-------------|
| **Non-Custodial** | Users retain wallet custody; protocol smart contracts hold collateral |
| **Permissionless** | Anyone can interact (subject to legal restrictions) |
| **Transparent** | All transactions, collateral, and NAV visible on-chain |
| **Experimental** | Research-stage software; not production-grade financial infrastructure |
| **Decentralized** | No central operator; governed by smart contracts and limited multisig |

---

## ⚠️ General Risk Acknowledgments

By using GUTHIX Protocol, you expressly acknowledge and accept that:

1. **Cryptocurrency investments are highly speculative and volatile**
2. **You are solely responsible for your investment decisions**
3. **You have the financial sophistication to understand these risks**
4. **You can afford to lose all funds you commit to the protocol**
5. **No one associated with GUTHIX is responsible for your losses**
6. **Regulatory environments are uncertain and may change adversely**
7. **This is not financial, legal, or tax advice**

---

## 🎯 Specific Risk Categories

### 1. Smart Contract Risk

**Description:** GUTHIX operates via smart contracts on Solana. Despite audits and testing, code may contain bugs, vulnerabilities, or exploits.

**Potential Impacts:**
- Total or partial loss of funds
- Protocol malfunction or shutdown
- Unauthorized access or theft
- Unintended behavior from upgrades or interactions

**Mitigations (Not Guarantees):**
- Professional audits (OtterSec, Neodyme)
- Bug bounty program via Immunefi
- Minimalist code design (7 instructions)
- Emergency pause mechanisms

**Residual Risk:** **HIGH** — Audits reduce but do not eliminate smart contract risk. New vulnerabilities may be discovered at any time.

---

### 2. Market & Volatility Risk

**Description:** sgxUSD NAV floats based on the performance of underlying yield-bearing assets. Values can increase or decrease significantly.

**Potential Impacts:**
- NAV depreciation below your entry price
- Yield source compression or failure
- Correlation convergence during market stress
- Permanent capital loss

**Yield Sources & Risks:**

| Source | Risk Profile |
|--------|-------------|
| **Ethena (sUSDe)** | Funding rate volatility; delta-hedging failure |
| **Maple (syrupUSDC)** | Credit defaults; borrower insolvency |
| **Ondo (USDY)** | RWA valuation uncertainty; regulatory action |
| **Trading Fees** | Volume dependency; impermanent loss |
| **Bridge Arbitrage** | Cross-chain volatility; bridge failures |

**Residual Risk:** **HIGH** — Yield is not guaranteed; NAV may decrease.

---

### 3. Counterparty & Protocol Risk

**Description:** GUTHIX depends on external protocols (Ethena, Maple, Ondo, Wormhole, Meteora). Their failure directly impacts sgxUSD.

**Potential Impacts:**
- Collateral asset depegging or insolvency
- Smart contract exploits in integrated protocols
- Governance attacks on partner protocols
- Regulatory shutdown of yield sources

**Specific Dependencies:**

| Protocol | Role | Risk if Fails |
|----------|------|--------------|
| **Ethena** | Funding rate yield | ~33% of yield basket affected |
| **Maple Finance** | Credit yield | ~33% of yield basket affected |
| **Ondo Finance** | RWA yield | ~33% of yield basket affected |
| **Wormhole** | Cross-chain bridging | Bridge liquidity & cross-chain access affected |
| **Meteora** | Liquidity pools | Trading functionality impaired |

**Residual Risk:** **HIGH** — Diversification reduces but does not eliminate counterparty risk.

---

### 4. Liquidity Risk

**Description:** Exits occur via secondary market swaps, not direct redemption. Liquidity may be insufficient during stress.

**Potential Impacts:**
- High slippage on exits (5-50%+ during volatility)
- Inability to exit at fair value
- Protocol-Owned Liquidity (POL) depletion
- Cascading liquidations during market panic

**No Redemption Guarantee:**
- ❌ No redemption queue
- ❌ No guaranteed exit price
- ❌ No liquidity backstop beyond POL
- ❌ No lender of last resort

**Residual Risk:** **MEDIUM-HIGH** — POL depth helps but cannot guarantee liquidity in extreme scenarios.

---

### 5. Oracle & Price Feed Risk

**Description:** NAV calculations depend on oracle price feeds (Pyth, Switchboard). Oracle failures or manipulation affect protocol operations.

**Potential Impacts:**
- Incorrect NAV pricing
- Exploitable arbitrage opportunities
- Collateral misvaluation
- Protocol insolvency from bad data

**Mitigations:**
- Dual-oracle design (Pyth + Switchboard)
- Time-weighted average prices (TWAP)
- Deviation circuit breakers
- Guardian pause authority

**Residual Risk:** **MEDIUM** — Redundancy helps but oracle manipulation remains possible.

---

### 6. Cross-Chain & Bridge Risk

**Description:** sgxUSD bridges to Base via Wormhole NTT. Bridge transactions carry additional risks beyond native Solana operations.

**Potential Impacts:**
- Bridge smart contract exploits
- Transaction finality delays or failures
- Asset loss during bridge transfer
- Regulatory complications across chains

**Historical Context:**
- Wormhole suffered a $320M exploit in February 2022 (since patched)
- Bridge exploits remain a top DeFi vulnerability category
- Cross-chain regulatory compliance is uncertain

**Residual Risk:** **MEDIUM-HIGH** — Bridges are complex and historically vulnerable.

---

### 7. Regulatory & Legal Risk

**Description:** Cryptocurrency regulations vary by jurisdiction and change frequently. Regulatory actions may affect access, taxation, or protocol viability.

**Potential Impacts:**
- Access restrictions or geo-blocking
- Token classification as security or regulated instrument
- Tax treatment changes (income vs. capital gains)
- Enforcement actions against protocol or users
- Mandatory registration or licensing requirements

**Jurisdictional Uncertainties:**

| Jurisdiction | Status |
|--------------|--------|
| **United States** | SEC/CFTC jurisdiction unclear; state-level variations |
| **European Union** | MiCA framework evolving; "other crypto-asset" likely |
| **Singapore** | PSA Digital Payment Token classification |
| **Other** | Varies widely; many jurisdictions lack clear guidance |

**Residual Risk:** **HIGH** — Regulatory landscape is fluid and enforcement is unpredictable.

---

### 8. Operational & Keeper Risk

**Description:** Protocol functions depend on off-chain Keeper services for NAV updates, rebalancing, and fee routing.

**Potential Impacts:**
- Keeper downtime or failure
- Delayed NAV updates
- Suboptimal rebalancing
- Centralization concerns

**Mitigations:**
- PDA-signed withdrawals with limits
- Guardian multisig oversight
- Keeper decentralization roadmap
- Monitoring and alerting systems

**Residual Risk:** **LOW-MEDIUM** — Operational risks are managed but not eliminated.

---

### 9. Governance & Multisig Risk

**Description:** Guardian Squads (3-of-5 multisig) holds limited emergency powers (pause, config updates).

**Potential Impacts:**
- Multisig key compromise
- Coordinated malicious action by signers
- Censorship or selective pausing
- Governance centralization concerns

**Safeguards:**
- Limited scope (pause/config only; no fund withdrawal)
- Trusted signers with public identities
- Transparency of all actions
- Decentralization roadmap

**Residual Risk:** **LOW-MEDIUM** — Powers are limited but centralization exists.

---

### 10. Technology & Infrastructure Risk

**Description:** Protocol depends on underlying infrastructure (Solana blockchain, IPFS, RPC providers, frontend hosting).

**Potential Impacts:**
- Solana network outages or congestion
- RPC provider failures
- Frontend downtime or censorship
- IPFS pinning failures
- DNS hijacking or phishing

**Residual Risk:** **MEDIUM** — Infrastructure risks are external and largely uncontrollable.

---

### 11. Tax & Accounting Risk

**Description:** Tax treatment of sgxUSD transactions is uncertain and varies by jurisdiction.

**Potential Impacts:**
- Unexpected tax liabilities
- Complex reporting requirements
- Capital gains vs. ordinary income classification uncertainty
- Cross-chain transaction tracking complexity
- Audit risk from tax authorities

**Common Considerations:**
- Swaps (USDC → sgxUSD) may be taxable events
- NAV appreciation may be taxed as income or capital gains
- Bridge transactions may trigger additional reporting
- Yield accrual timing (realized vs. unrealized) is unclear

**Residual Risk:** **HIGH** — Tax treatment is jurisdiction-specific and evolving.

---

### 12. Experimental & Development Risk

**Description:** GUTHIX is experimental software in active development. Features, parameters, and design may change.

**Potential Impacts:**
- Protocol changes affecting your position
- Undiscovered bugs in new code
- Parameter adjustments (fees, allocations, limits)
- Feature deprecation or removal
- Complete protocol shutdown

**Residual Risk:** **MEDIUM-HIGH** — Experimental status means higher uncertainty.

---

## 🌍 Jurisdictional Risks

### General Warning

**Cryptocurrency regulations vary significantly by jurisdiction. You are solely responsible for understanding and complying with laws applicable to your location.**

### High-Risk Jurisdictions

| Jurisdiction | Specific Concerns |
|--------------|------------------|
| **United States** | SEC securities enforcement; state money transmitter laws; tax reporting complexity |
| **New York** | BitLicense requirements; strict virtual currency regulations |
| **Texas** | Active securities enforcement; MSB licensing requirements |
| **China** | Cryptocurrency transactions prohibited |
| **Other** | Many jurisdictions lack clear guidance; assume risk if unclear |

### Sanctions & Restricted Parties

**You may NOT use GUTHIX Protocol if you are:**
- Located in a sanctioned jurisdiction (OFAC, UN, EU lists)
- A sanctioned individual or entity
- Acting on behalf of a sanctioned party
- Using the protocol for illicit purposes

**Compliance Measures:**
- Geo-fencing may be implemented at interface layer
- Blockchain screening for sanctioned addresses
- Transaction monitoring for suspicious activity

---

## 💰 Tax Implications

### General Guidance (Not Tax Advice)

**Tax treatment of GUTHIX Protocol participation is complex and uncertain. Consult a qualified tax advisor in your jurisdiction.**

### Common Tax Considerations

| Event | Potential Tax Treatment |
|-------|----------------------|
| **USDC → sgxUSD swap** | Taxable exchange (capital gain/loss on USDC) |
| **sgxUSD NAV appreciation** | Unrealized gain (taxed when sold?) or income (as it accrues?) |
| **sgxUSD → USDC swap** | Taxable event (capital gain/loss on sgxUSD) |
| **Bridge transactions** | Potentially taxable; jurisdiction-dependent |
| **Trading fees earned** | Ordinary income (if applicable) |

### Reporting Obligations

You may be required to report:
- All cryptocurrency transactions
- Capital gains and losses
- Foreign asset holdings (FBAR, FATCA in US)
- Cross-chain transfers
- Yield or income accruals

**Failure to comply with tax obligations may result in penalties, interest, or legal action.**

---

## 🚫 No Warranties or Guarantees

### Express Disclaimers

**GUTHIX Protocol is provided "AS-IS" and "AS-AVAILABLE" without warranties of any kind, either express or implied, including but not limited to:**

- ❌ **No warranty of merchantability**
- ❌ **No warranty of fitness for a particular purpose**
- ❌ **No warranty of non-infringement**
- ❌ **No warranty of accuracy or reliability**
- ❌ **No warranty of uninterrupted or error-free operation**
- ❌ **No warranty of security or protection from loss**

### No Guarantees

**The following are expressly NOT guaranteed:**

- NAV stability or appreciation
- Yield generation or APY
- Liquidity availability
- Smart contract security
- Regulatory compliance
- Tax treatment
- Protocol longevity
- Feature availability
- Interface accessibility
- Support or maintenance

---

## ⚖️ Limitation of Liability

### To the Maximum Extent Permitted by Law

**In no event shall GUTHIX Protocol, its contributors, developers, auditors, partners, or any affiliated parties be liable for:**

1. **Any direct, indirect, incidental, special, consequential, or exemplary damages**
2. **Loss of profits, data, goodwill, or other intangible losses**
3. **Costs of procurement of substitute services or products**
4. **Damages arising from protocol use or inability to use**
5. **Damages from unauthorized access or alteration of transactions**
6. **Damages from third-party protocol failures or exploits**
7. **Damages from regulatory actions or legal proceedings**
8. **Any other damages arising from or related to protocol participation**

### Liability Cap

**If liability cannot be excluded or limited under applicable law, the total aggregate liability shall not exceed the amount of fees paid by you to the protocol (if any) or $100 USD, whichever is greater.**

### Indemnification

**You agree to indemnify, defend, and hold harmless GUTHIX Protocol and all affiliated parties from any claims, liabilities, damages, losses, or expenses (including legal fees) arising from:**
- Your use of the protocol
- Your violation of these terms
- Your violation of applicable laws or third-party rights

---

## 🙋 User Responsibilities

By using GUTHIX Protocol, you agree to:

### 1. Legal Compliance
- Comply with all applicable laws and regulations
- Verify participation is legal in your jurisdiction
- Not access from sanctioned jurisdictions or for illicit purposes
- Report and pay all applicable taxes

### 2. Due Diligence
- Read and understand all documentation
- Assess your risk tolerance and financial capacity
- Consult legal, tax, and financial advisors as needed
- Monitor protocol updates and announcements

### 3. Security Practices
- Secure your private keys and seed phrases
- Use hardware wallets for significant amounts
- Verify contract addresses before transacting
- Beware of phishing and social engineering
- Keep software and wallets updated

### 4. Risk Acceptance
- Accept all risks described in this document
- Understand you may lose some or all funds
- Acknowledge no guarantees or warranties exist
- Accept that protocol changes may affect your position

### 5. Prohibited Activities
- Market manipulation or exploit abuse
- Money laundering or terrorist financing
- Sanctions evasion
- Illegal gambling or gaming
- Violation of intellectual property rights
- Interference with protocol operations

---

## 🔄 Updates to This Document

### Version Control

This document is versioned and may be updated periodically. Changes are categorized as:

| Change Type | Description | User Action Required |
|------------|-------------|---------------------|
| **MAJOR** | Material risk changes or new disclosures | Re-acknowledgment required |
| **MINOR** | Clarifications or non-material updates | Notice on next session |
| **PATCH** | Typographical or formatting fixes | No action required |

### Notification of Changes

- **Major changes:** Prominent notice on interface; re-acknowledgment required before further use
- **Minor changes:** Notice in changelog; effective on next session
- **Patch changes:** No notice; updated version available on GitHub

### Continued Use

**Your continued use of GUTHIX Protocol after changes constitute acceptance of the updated terms. If you do not agree, discontinue use immediately.**

### Version History

| Version | Date | Changes |
|---------|------|---------|
| `1.0.0` | Mar 2026 | Initial public risk disclosure |

---

## 📞 Contact & Questions

### General Inquiries

- **Email:** `legal@guthix.finance`
- **Response Time:** 5-7 business days
- **PGP Encryption:** Available upon request

### Security Issues

- **Email:** `security@guthix.finance`
- **Bug Bounty:** [Immunefi Program](#)
- **Response:** Immediate for critical issues

### Compliance Questions

- **Email:** `compliance@guthix.finance`
- **Jurisdictional Access:** Include your location and specific question
- **Response Time:** 5-7 business days

### Important Notes

- **Not Legal Advice:** Communications do not constitute legal, tax, or financial advice
- **No Confidentiality:** Do not share sensitive information; communications are not confidential
- **Public Record:** Communications may be retained for compliance purposes

---

## 📄 Related Documents

- [Litepaper](./litepaper.pdf) — Technical and economic overview
- [Compliance Principles](./docs/COMPLIANCE_PRINCIPLES.md) — Protocol compliance framework
- [Terms of Service](./docs/TERMS.md) — Interface usage terms
- [Security Policy](./SECURITY.md) — Vulnerability reporting
- [Smart Contracts](./contracts/) — Audited source code

---

## ✅ Acknowledgment Requirement

**Before using GUTHIX Protocol, you must:**

1. Read this entire Risk Disclosure document
2. Understand the risks described herein
3. Acknowledge acceptance via interface prompt
4. Consult advisors as needed for your situation
5. Confirm participation is legal in your jurisdiction

**If you cannot honestly complete these steps, DO NOT use this protocol.**

---

*© 2026 Guthix Protocol. This document is licensed under MIT License.  
Last Updated: March 2026 | Version: 1.0.0  
Built on Solana. Secured by minimalism. Use at your own risk.*
