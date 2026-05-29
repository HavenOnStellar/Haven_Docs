# Bounty Economics

Haven uses recovery bounties to change the expected payoff for a stolen device. In many emerging markets, a stolen phone can be sold quickly to an informal buyer or fence for roughly $15-$40. That resale price creates a simple incentive problem: if returning the phone is slower, riskier, or less profitable than reselling it, rational actors will choose resale.

The Haven bounty system makes return the better economic choice. When the owner reports a device as stolen, they attach a USDC-denominated recovery bounty. If the bounty is higher than the local fence price, a finder, repair shop, or secondary-market buyer can earn more by returning the device than by reselling it.

## Incentive Model

The frontend calculator currently models the return incentive with a $35 reference fence price:

```text
return_multiplier = bounty_amount / 35
```

This is not a universal price oracle. It is a default planning assumption that sits inside the $15-$40 range used by the product copy. Teams deploying Haven in a specific city or market should tune the reference value using local recovery data, device category, and resale conditions.

The practical target is:

```text
bounty_amount > expected_resale_value + return_friction
```

`return_friction` includes time, transport, verification, perceived risk, and any hassle involved in coordinating recovery. A bounty that only matches the fence price may not be enough. A bounty that clearly exceeds the resale value gives intermediaries a reason to check Haven before buying or moving a device.

## Worked Examples

| Bounty | Multiplier vs. $35 fence price | Interpretation |
| --- | ---: | --- |
| $50 USDC | 1.4x | Return is modestly better than resale. This may work for lower-value devices or markets where recovery friction is low. |
| $100 USDC | 2.9x | Return is clearly more profitable than resale. This is a stronger default for common midrange or flagship devices. |
| $250 USDC | 7.1x | Return dominates resale economics. This level is appropriate for high-value devices, insurance-backed recovery, or urgent recovery scenarios. |

Example: if a used phone would likely sell to a fence for $35 and a finder sees a $100 USDC bounty, returning it pays about 2.9 times more than selling it. The rational path shifts from concealment to recovery.

## Escrow Lifecycle

The intended USDC escrow lifecycle is:

1. **Deposit** - The owner chooses a bounty amount while reporting a device as stolen.
2. **Lock** - The contract or escrow module locks the bounty so the owner cannot withdraw it after finders begin acting on the incentive.
3. **Claim** - A finder or authorized recovery participant presents the recovered device through the recovery flow.
4. **Release** - After the owner, insurer, or dispute process confirms recovery, escrowed USDC is released to the finder.

In the current contract documentation, `report_stolen()` records the bounty amount and `confirm_recovery()` clears the bounty record. Token escrow transfer and payout are planned follow-up work for the SAC/USDC integration. Until that integration lands, documentation and UI should distinguish between the recorded bounty amount and the final escrowed-token implementation.

## Edge Cases

### No one claims the bounty

If no finder claims the bounty before the recovery window expires, the protocol should let the owner or insurer close the bounty and recover locked funds. A production implementation should define the expiry period at bounty creation time and emit an event when funds become refundable.

### Bounty expiry

Expiry prevents stale incentives from remaining open forever. The device can still remain marked stolen, but the payment obligation should either be renewed by the owner or released back to the funder after the deadline.

### Dispute resolution

Disputes can occur when a finder claims recovery but the owner does not confirm, or when multiple parties claim the same device. The protocol should support a defined reviewer role, insurer authority, or multi-signature confirmation flow. Events should preserve enough history to show the device status, bounty amount, claimant, timestamps, and final resolution.

### Underfunded bounties

A bounty below the expected resale value is mostly symbolic. The UI should warn owners when the selected amount is below the local reference value or below the protocol minimum.

### Overfunded bounties

Very high bounties can attract abuse. Production flows should include device ownership checks, recovery proof, cooldowns, and optional insurer review before release.

## Why This Matters

Haven does not rely only on morality or law enforcement. It uses a transparent on-chain reward to make the honest action economically superior. When vendors and finders know that recovery pays better than resale, the stolen-device market becomes less liquid and less attractive.
