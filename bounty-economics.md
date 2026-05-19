# Bounty Economics

Haven's recovery bounty is designed to make returning a stolen device more profitable than reselling it into the informal market.

## Core incentive model

In many emerging markets, a stolen phone may only sell to a fence for about **$15-$40**. The thief or finder takes legal risk, loses time negotiating the sale, and receives only a fraction of the device's retail value.

Haven changes the payoff. When the owner posts a USDC recovery bounty above the expected fence price, the rational choice becomes:

1. report or return the device through Haven;
2. receive the escrowed bounty; and
3. avoid the risk and friction of resale.

The frontend calculator currently communicates this with an average fence-price benchmark of **$35**:

```text
return incentive multiplier = bounty amount / 35
```

A multiplier above `1.0x` means the bounty is higher than the assumed average fence price. Higher multipliers create stronger incentives for fast return.

## Worked examples

| Bounty | Multiplier vs. $35 fence price | Interpretation |
| --- | ---: | --- |
| $50 USDC | 1.4x | A low recovery bounty still beats the average fence price and may be enough for lower-value devices. |
| $100 USDC | 2.9x | A stronger bounty that gives a finder almost three times the expected informal resale payout. |
| $250 USDC | 7.1x | A high-priority bounty for expensive devices where rapid recovery is worth a larger escrow. |

The bounty should be high enough to beat the local fence price, but not so high that it creates unnecessary cost for the owner or insurer.

## Escrow lifecycle

Haven's target lifecycle is:

1. **Deposit** — the owner chooses a bounty amount and authorizes a USDC deposit.
2. **Lock** — the recovery bounty is associated with the hashed device record while the device is marked stolen.
3. **Claim** — a finder or recovery partner follows the public bounty flow and provides the required recovery details.
4. **Release** — after recovery is confirmed, the escrowed funds are released to the approved recipient.

The current contract records the bounty amount when `report_stolen` is called. Full SAC/USDC transfer and escrow support is tracked as a planned contract improvement, so production deployments should treat token movement as part of the escrow roadmap until that implementation is complete.

## Edge cases

### No one claims the bounty

If no finder claims the bounty, the device remains marked as stolen and the bounty remains visible as an incentive. Owners may increase the bounty later if the contract and frontend flow support bounty updates.

### Bounty expiry

A future expiry policy can prevent stale bounties from staying open forever. At expiry, the expected behavior is to close the public bounty and return any locked funds to the owner or insurer, minus any protocol-defined fees.

### Dispute resolution

Disputes can happen if several people claim recovery, if the device condition is contested, or if an insurer owns the claim after payout. Haven should resolve these cases by requiring the authorized device owner or insurer to confirm recovery before funds are released. For higher-value devices, a multisig or third-party recovery partner can be added before release.

## Why USDC

USDC provides a stable reward unit that is easier to understand than volatile tokens. It also makes bounty amounts comparable across countries and devices, which is important when the incentive depends on beating a local cash resale price.
