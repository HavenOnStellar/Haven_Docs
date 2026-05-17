# Bounty Economics

Haven uses recovery bounties to make returning a stolen phone more valuable than reselling it. The protocol does not need to track a device's location. Instead, it changes the payoff for anyone who finds or handles a stolen device.

## Core incentive model

In many resale markets, a stolen phone may only sell to a fence for about **$15-$40** because the buyer takes legal, resale, and unlock risk. Haven's frontend uses **$35** as the default average fence value for comparison.

The bounty multiplier is:

```text
return multiplier = posted bounty / average fence value
```

Using the current frontend assumption:

```text
return multiplier = posted bounty / 35
```

A bounty is economically stronger when the multiplier is greater than `1.0x`. In practice, a bounty above the top of the expected fence range gives the finder a clearer reason to return the device instead of reselling it.

## Worked examples

| Posted bounty | Compared with $35 fence value | Incentive outcome |
| --- | ---: | --- |
| $50 USDC | 1.4x more valuable to return | Clears the typical fence price and creates a small but direct premium for return. |
| $100 USDC | 2.9x more valuable to return | Makes return the clearly rational choice for most finders and informal buyers. |
| $250 USDC | 7.1x more valuable to return | Strong incentive for high-value devices or markets where recovery needs to compete with organized resale. |

These examples are not fixed protocol limits. They are practical presets that help owners choose a bounty that exceeds the expected off-market resale value.

## USDC escrow lifecycle

The target lifecycle for a Haven recovery bounty is:

1. **Deposit** — the owner chooses a bounty amount, approves the USDC transfer, and calls the stolen-device reporting flow.
2. **Lock** — the smart contract records the stolen status and locks the bounty against the device's hashed identifier. The raw IMEI should never be stored on-chain.
3. **Claim** — a finder or recovery participant follows the return flow and provides the required recovery information.
4. **Release** — after the owner confirms recovery, the smart contract releases the escrowed USDC to the finder address.

This keeps the owner's promise credible: the return reward is not just an informal offer, but a pre-funded amount held for the recovery path.

## Why escrow matters

Without escrow, a finder has to trust that the owner will pay after the phone is returned. With escrow:

- the reward amount is visible before the finder acts;
- the owner cannot quietly withdraw the promised reward during an active recovery flow;
- settlement can happen through the contract once recovery is confirmed;
- marketplaces and vendors can treat the stolen status and reward as verifiable signals.

## Edge cases

### No one claims the bounty

If no finder claims the bounty, the device remains marked as stolen and the bounty stays associated with the recovery record until an expiry or cancellation policy applies. A future expiry flow should let the owner close an inactive bounty after a defined period while preserving an audit trail.

### Bounty expiry

Expiry should be explicit rather than automatic by surprise. A safe expiry design includes:

- a visible expiry timestamp or block height;
- a grace period for in-progress recovery attempts;
- an event emitted when the bounty is cancelled or reclaimed;
- no deletion of the original stolen-device report.

### Dispute resolution

Disputes can happen if a finder starts a return but the owner does not confirm recovery, or if recovery evidence is incomplete. A practical dispute process should keep funds locked while the case is reviewed and should rely on auditable evidence such as recovery contact records, owner confirmation, vendor verification, and device status history.

For higher-value bounties, Haven can add a multi-signature or designated-arbiter flow so neither side can unilaterally force a release while a dispute is unresolved.

## Design principle

The recovery bounty should be high enough to beat the local resale value, simple enough for a finder to understand, and credible enough that payment is expected when the device is returned. That combination is what turns the device from a resale target into a recovery opportunity.
