# GUTHIX Protocol Litepaper

> **The Yield-Bearing Liquidity Hub**
> Version 3.3.0 — March 2026

---

## Abstract

GUTHIX is a minimalist decentralised liquidity protocol built around a single token: **sgxUSD**.

sgxUSD is a vault token. It starts at $1.00 and floats upward as protocol revenue accrues. It is not a stablecoin and makes no peg guarantee. All revenue flows to sgxUSD holders via NAV appreciation — no emissions, no governance tokens, no distributions. Hold it. That's it.

Yield comes from four structurally uncorrelated sources simultaneously:

- **Ethena funding rates** via sUSDe collateral
- **Maple credit yield** via syrupUSDC collateral
- **Ondo RWA yield** via USDY collateral
- **Bridge arbitrage fees** from Wormhole-bridged USDC variants and LayerZero USDT0

No single source dominates. When one compresses, the others carry.

The protocol is a single immutable Anchor program with seven instructions. There is no foundation, no company, no controlling entity of any kind. The contracts are deployed as-is under the MIT licence and cannot be upgraded by anyone.

---

## The Problem

**Single-source yield concentration.** sUSDe gives you Ethena funding rates. syrupUSDC gives you Maple credit. USDY gives you Ondo RWA yield. Each is a single point of failure. When their source compresses, you feel all of it.

**Stranded bridge capital.** Wormhole-bridged USDC variants from Ethereum, BNB Chain, and Polygon sit on Solana with thin liquidity, high slippage, and no yield. Same for LayerZero USDT0. They are productive capital going to waste.

**Emission-driven APY.** Most yield protocols subsidise returns with inflationary token emissions. That is not yield. It is dilution rebranded.

**Yield leakage.** Protocols that harvest yield-bearing collateral and convert it to USDC destroy compounding at every cycle. The conversion step is the tax.

---

## The Solution

GUTHIX holds yield-bearing assets as collateral and pairs sgxUSD directly against them. Both sides of each pool compound natively. No harvesting. No conversion. No leakage.

Bridge liquidity pools capture constant arbitrage flow from users unwinding Wormhole and LayerZero positions. Those fees route to the vault. Every bridge unwind is a yield event for sgxUSD holders.

The primary entry and exit pool is `sgxUSD / USDT` — the highest volume stablecoin pair on Solana. A secondary `sgxUSD / USDC` pool serves Jupiter routing and USDC holders.

One token. Five yield sources. Passive appreciation.

---

## Architecture

### The Program

`guthix-core` is a single Anchor program with exactly seven instructions:

| Instruction | Authority | Description |
|---|---|---|
| `Initialize` | Guardian | Setup vault, token mint, guardian multisig |
| `Deposit` | Public | Lock collateral → mint sgxUSD at current NAV |
| `Withdraw` | Public | Burn sgxUSD → withdraw collateral at current NAV |
| `WithdrawCollateral` | Keeper (PDA-signed) | Unlock collateral for POL deployment; epoch limits enforced on-chain |
| `UpdateNAV` | Keeper (PDA-signed) | Update exchange rate; circuit breaker enforced on-chain |
| `UpdateConfig` | Guardian (3-of-5) | Adjust parameters, pause, rotate Keeper |
| `Pause` | Guardian (3-of-5) | Emergency halt |

No staking logic. No governance voting. No emission schedules. No synthetic minting. **Immutable on deployment.**

### Immutability

Once deployed, the contracts cannot be upgraded, patched, or altered by any party — including the original developers. This eliminates upgrade-key risk entirely. The audit result is permanent. The tradeoff: a discovered vulnerability cannot be patched in-place. The Guardian pause and Keeper rotation are the only available responses.

### The Keeper

The Keeper is an off-chain process that handles POL rebalancing, NAV updates, fee routing, and pool monitoring. The reference implementation is open source. Any party may run one. The Guardian multisig authorises which Keeper address the program accepts and may rotate it at any time.

The Keeper operates within strict on-chain constraints:

- **Per-epoch withdrawal cap** — cannot drain the vault even if compromised
- **NAV circuit breaker** — maximum NAV change per epoch enforced in the program
- **PDA signing** — all vault operations are program-signed; the Keeper cannot move funds outside program rules
- **Guardian override** — the 3-of-5 multisig can pause or replace the Keeper at any time

### Frontends

There is no official frontend. Any party may build one. Individual frontend operators are solely responsible for their own compliance obligations, access controls, and terms of service. The protocol enforces no geographic restrictions at the contract level.

---

## Pool Architecture

GUTHIX deploys Protocol-Owned Liquidity across three tiers.

### Tier 1 — Yield Engines

sgxUSD is paired directly against yield-bearing stablecoins. Both sides appreciate natively. No harvesting required — yield is always on.

| Pool | Yield Source | Swap Fee |
|---|---|---|
| sgxUSD / sUSDe | Ethena funding rates | 0.10% |
| sgxUSD / syrupUSDC | Maple credit yield | 0.10% |
| sgxUSD / USDY | Ondo RWA yield | 0.10% |

### Tier 2 — Bridge Liquidity Hub

Captures arbitrage fees from users unwinding Wormhole and LayerZero bridge positions. Neither side appreciates natively — yield is purely trading fees from constant bridge arb flow.

| Pool | Bridge | Swap Fee |
|---|---|---|
| sgxUSD / whUSDC.e | Wormhole (Ethereum) | 0.20% |
| sgxUSD / whUSDC.bnb | Wormhole (BNB Chain) | 0.20% |
| sgxUSD / whUSDC.poly | Wormhole (Polygon) | 0.20% |
| sgxUSD / USDT0 | LayerZero OFT (30+ chains) | 0.20% |

Higher fees on Tier 2 reflect bridge risk and active Keeper monitoring requirements.

### Tier 3 — Entry / Exit Ramp

| Pool | Role | Swap Fee |
|---|---|---|
| sgxUSD / USDT | Primary ramp — highest volume stablecoin pair | 0.05% |
| sgxUSD / USDC | Secondary ramp — Jupiter routing and USDC access | 0.05% |

### The LayerZero Connection

USDT0 is LayerZero's omnichain OFT USDT, bridgeable across 30+ chains. The Tier 2 `sgxUSD / USDT0` pool and the Tier 3 `sgxUSD / USDT` pool together form a complete routing path: a user on any LayerZero-connected chain bridges USDT0 to Solana, swaps to sgxUSD via the USDT pool, and holds yield-bearing exposure — without touching USDC or Wormhole. GUTHIX has two independent bridge ecosystems feeding inflow. This is intentional.

### Capital Allocation

| Tier | Allocation |
|---|---|
| Yield Engines (Tier 1) | 50% |
| Bridge Hub (Tier 2) | 35% |
| Entry / Exit Ramp (Tier 3) | 15% |

---

## NAV Mechanics

```
sgxUSD NAV appreciation =
  Blended Tier 1 collateral yield (sUSDe + syrupUSDC + USDY, weighted)
  + Tier 2 bridge arb fees (whUSDC.e + whUSDC.bnb + whUSDC.poly + USDT0)
  + Trading fees from all pools, routed to vault by Keeper each epoch
```

### Swap-to-Grow

When users buy sgxUSD on secondary markets, USDC or USDT accumulates in the Tier 3 pool. The Keeper routes the excess into the vault as additional collateral without minting new sgxUSD. Collateral per token increases. NAV rises. Every secondary market buy is a yield event for all holders.

### Silent Rebalance

When users sell sgxUSD, POL absorbs the flow. No redemption queues. No withdrawal delays. Large coordinated exits widen the spread naturally, creating an arbitrage opportunity that incentivises re-entry.

---

## Cross-Chain

sgxUSD bridges canonically to Base via Wormhole NTT. On Base it is usable on EVM DEXs and as collateral in EVM lending protocols.

| Direction | Mechanism | Result |
|---|---|---|
| EVM → Solana | whUSDC / USDT0 absorbed by Tier 2 pools | Bridge arb fees → sgxUSD NAV |
| Solana → Base | Wormhole NTT lock / mint | sgxUSD live on Base; yield continues |
| Base → Solana | Wormhole NTT burn / unlock | Exit via Solana secondary market |

---

## Risk Disclosure

sgxUSD holders bear protocol-level risk directly. NAV may compress. The following are the primary risk categories:

**Collateral depeg.** sUSDe, syrupUSDC, or USDY depegs from expected value. Mitigated by three uncorrelated sources, dual-oracle monitoring (Pyth + Switchboard), and Guardian pause authority.

**Bridge asset depeg.** whUSDC variants or USDT0 depeg independently. Mitigated by four independent bridge pairs, higher swap fees as buffer, and block-level Keeper monitoring.

**Wormhole / LayerZero exploit.** Both are core dependencies that cannot be fully mitigated. Size exposure accordingly.

**No upgrades for bugs.** Immutable contracts mean a discovered vulnerability cannot be patched in-place. The Guardian pause is the only available response. The pre-deployment audit is therefore permanent and critical.

**Keeper compromise or abandonment.** On-chain epoch limits constrain damage. Guardian can rotate Keeper to any community operator. The open-source reference implementation means any party can step in.

**Development abandonment.** The original developers may cease activity at any time. The on-chain contracts operate indefinitely regardless. The open-source Keeper and SDK allow the community to sustain all off-chain operations independently.

**No controlling entity.** There is no foundation, company, or DAO to respond to regulatory action, compensate users for losses, or make representations on behalf of the protocol.

---

## Security

| Layer | Implementation |
|---|---|
| Immutable contracts | Non-upgradeable post-deployment; eliminates upgrade-key risk |
| Minimal surface | Single program; 7 instructions; one token |
| Standard dependencies | Squads, Wormhole, SPL Token, Meteora — all audited |
| On-chain Keeper limits | Per-epoch withdrawal cap + NAV circuit breaker in program |
| Guardian multisig | 3-of-5; emergency pause; Keeper rotation |
| Dual-oracle pricing | Pyth + Switchboard; TWAP; deviation circuit breakers |
| Open-source Keeper | MIT licence; any party may run it |

### Audit Plan

| Phase | Activity |
|---|---|
| Pre-deployment | Community review + formal verification of `guthix-core` |
| Pre-deployment | Professional audit — OtterSec / Neodyme (prerequisite for mainnet) |
| Post-deployment | Immunefi bug bounty, active from launch |

The protocol will not deploy to mainnet before the professional audit is complete. Because contracts are immutable, the audit result is final and permanent.

---

## Roadmap

| Phase | Timeline | Key Milestones |
|---|---|---|
| **Phase 1 — Core Foundation** | Q2 2026 | `guthix-core` devnet · Keeper MVP (open source) · All pools on devnet · Community audit |
| **Phase 2 — Mainnet Launch** | Q3 2026 | Professional audit complete · Immutable contracts deployed · POL seeding · Jupiter integration · Keeper reference published |
| **Phase 3 — Multichain** | Q4 2026 | Wormhole NTT live · sgxUSD on Base · LayerZero USDT0 routing live · Analytics dashboard |
| **Phase 4 — Community Maintenance** | Q1 2027+ | 3+ independent Keeper operators · No single-operator dependency · Original developers no longer required |

This roadmap reflects current intentions of the development contributors. It is not a commitment. Development may slow, pause, or cease. After mainnet deployment, community contributors may continue any aspect of it independently.

---

## Ecosystem

### Collateral Partners

| Partner | Pool |
|---|---|
| Ethena | sgxUSD / sUSDe |
| Maple | sgxUSD / syrupUSDC |
| Ondo | sgxUSD / USDY |

### Infrastructure

| Partner | Role |
|---|---|
| Meteora | StableSwap pools |
| Wormhole | NTT canonical bridge (Solana ↔ Base) + Tier 2 whUSDC assets |
| LayerZero | USDT0 OFT routing — Tier 2 and Tier 3 pools |
| Jupiter | Aggregator routing for all sgxUSD pairs |
| Pyth / Switchboard | Dual-oracle NAV and collateral pricing |
| Squads | Guardian multisig |

---

## Legal

The GUTHIX smart contracts are immutable, permissionless software deployed on public blockchains and published under the MIT licence. They are provided as-is with no warranty of any kind. No person or entity — including the original developers — owns, controls, or is responsible for the protocol.

This document does not constitute financial advice, an offer to sell, or a solicitation to buy any financial instrument. sgxUSD is a vault token whose value floats and is not guaranteed to appreciate. Collateral assets carry independent risks including depeg and bridge protocol risk. Past performance is not indicative of future results. Participants may lose some or all of the value they deposit.

Each participant is solely responsible for determining the legality of interacting with this protocol in their jurisdiction.

---

## Get Involved

**Developers**
```bash
git clone https://github.com/RedactedBuilder/guthix
cd guthix
anchor test
```

- Contribute to `guthix-core` or the Keeper bot
- Build a frontend — no permission needed
- Report vulnerabilities: security@guthix.finance

**Collateral & Infrastructure Partners**
partnerships@guthix.finance

**Community**
[@GuthixFinance](https://twitter.com/GuthixFinance)

---

*MIT Licence. No rights reserved by any entity.*
*Built on Solana. Secured by minimalism. One token. Five yield sources.*
