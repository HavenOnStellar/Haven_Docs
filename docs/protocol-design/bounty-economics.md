---
title: Bounty Economics Explainer
description: Game-theoretic rationale and escrow mechanics behind Haven's bounty system
---

# Bounty Economics

Haven's bounty system is designed around a simple game-theoretic insight: **in emerging markets, a stolen phone typically sells to a fence for $15-$40**. By setting the bounty *above* this black-market price, returning the device becomes the rational economic choice for anyone who possesses it.

## Core Incentive Model

| Outcome | Typical Value | Rational Choice |
|---------|--------------|-----------------|
| Sell to fence | $15-$40 | Low, risky, illegal |
| Return for bounty | ≥ $50 | Higher, safe, legal |

When the bounty exceeds the fence price by a meaningful margin, the finder maximizes expected value by returning the device through Haven rather than liquidating it illicitly.

## Bounty Multiplier Formula

The frontend calculator uses the following formula to suggest a bounty based on device value and risk tier:

```
bounty = base_bounty × multiplier + floor
```

| Variable | Description | Default |
|----------|-------------|--------|
| `base_bounty` | User-selected percentage of device resale value | 10% |
| `multiplier` | Risk-tier coefficient (Low=1.0, Med=1.5, High=2.0) | 1.5 |
| `floor` | Minimum bounty to exceed fence price | $50 |

> **Note:** The floor ensures that even low-value devices have a bounty above the typical fence price in target markets.

## Worked Examples

### Example 1: $50 Bounty (Floor Level)
- **Device resale value:** $200
- **Risk tier:** Low (multiplier 1.0)
- **Calculation:** `$200 × 10% × 1.0 + $50 = $70` → clamped to floor-adjusted minimum of **$50** if user overrides downward
- **Rationale:** Exceeds the $15-$40 fence range; sufficient for older or low-demand devices

### Example 2: $100 Bounty (Standard)
- **Device resale value:** $400
- **Risk tier:** Medium (multiplier 1.5)
- **Calculation:** `$400 × 10% × 1.5 + $50 = $110` → suggested **$100** after rounding
- **Rationale:** Comfortably above fence price for mid-range smartphones; strong incentive without overpaying

### Example 3: $250 Bounty (High-Value Device)
- **Device resale value:** $900
- **Risk tier:** High (multiplier 2.0)
- **Calculation:** `$900 × 10% × 2.0 + $50 = $230` → rounded up to **$250**
- **Rationale:** Premium devices attract sophisticated thieves; higher bounty offsets greater black-market demand and ensures return remains optimal

## USDC Escrow Mechanism

All bounties are held in a Stellar-based USDC smart contract to ensure trustless execution.

### Escrow Lifecycle

```
Deposit → Lock → Claim → Release
```

1. **Deposit:** Owner sends USDC to the Haven escrow contract when creating a bounty listing.
2. **Lock:** Funds are locked in the contract with a time-bound claim window. No party can withdraw unilaterally.
3. **Claim:** Finder submits proof-of-return (e.g., verified handoff at partner location). Contract validates claim against predefined conditions.
4. **Release:** Upon valid claim, USDC is transferred to the finder's wallet. If no valid claim occurs before expiry, funds return to the owner.

> All state transitions are on-chain and auditable via Stellar Horizon.

## Edge Cases

### No One Claims the Bounty
If the claim window expires without a valid submission, the escrow contract automatically releases funds back to the original depositor. There is no penalty for unclaimed bounties.

### Bounty Expiry
Bounties have a configurable TTL (default: 30 days). Owners may extend before expiry by depositing additional USDC. Expired bounties are delisted from the active registry.

### Dispute Resolution
Disputes (e.g., contested handoffs, fraudulent claims) are handled through a multi-sig arbitration panel composed of trusted community validators. The panel reviews evidence and issues a binding decision within 72 hours. Arbitration outcomes are recorded on-chain for transparency.

- If the finder is validated, funds release normally.
- If the claim is rejected, funds return to the owner.
- Repeated fraudulent claims result in wallet blacklisting from the protocol.
