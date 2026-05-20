# Bounty Economics

Haven's recovery bounty model is designed to make returning a stolen device more rational than selling it into a low-trust resale channel.

In many emerging markets, a stolen phone can be sold quickly to a fence for roughly $15 to $40. That price is low because the fence accepts legal risk, device-lock risk, and resale friction. Haven changes the incentive by letting the owner attach a visible USDC bounty that is higher than the expected fence price.

When the bounty is greater than the resale value, a finder or intermediary has a simple economic reason to return the device instead of hiding it.

## Core incentive model

The model compares two options:

1. Sell the stolen device to a fence.
2. Return the device through Haven and claim the bounty.

The return path is economically stronger when:

```text
bounty_amount > expected_fence_price + return_friction_cost
```

Where:

- `bounty_amount` is the USDC reward posted by the owner.
- `expected_fence_price` is the likely resale offer for the stolen device.
- `return_friction_cost` covers the time, transport, and trust cost of returning the device.

The frontend calculator can express this as a bounty multiplier:

```text
bounty_multiplier = bounty_amount / expected_fence_price
```

A multiplier above `1.0x` means the bounty beats the estimated fence price. A multiplier above `2.0x` is stronger because it leaves room for time, transport, and uncertainty.

## Worked examples

The examples below assume a midpoint fence price of `$30` for a locked or risky stolen phone.

| Bounty | Fence price | Multiplier | Incentive effect |
| --- | ---: | ---: | --- |
| $50 | $30 | 1.67x | Better than a typical fence sale, but still sensitive to return friction. |
| $100 | $30 | 3.33x | Strong return incentive; the finder can earn materially more by cooperating. |
| $250 | $30 | 8.33x | Very strong incentive; the bounty can motivate multiple intermediaries to route the device back. |

### Example 1: $50 bounty

A $50 bounty beats a $30 fence offer by $20.

```text
50 / 30 = 1.67x
```

This can work for a nearby finder or a vendor who wants a clean transaction, but it may be too low if recovery requires travel, communication, or negotiation.

### Example 2: $100 bounty

A $100 bounty is more than three times the assumed fence price.

```text
100 / 30 = 3.33x
```

At this level, returning the device is usually the better financial choice even after accounting for time and effort.

### Example 3: $250 bounty

A $250 bounty is more than eight times the assumed fence price.

```text
250 / 30 = 8.33x
```

This is useful for high-value devices or urgent recovery. The bounty is large enough to make cooperation attractive even when the device passes through more than one person.

## USDC escrow lifecycle

Haven uses USDC because a stable-dollar bounty is easier to understand than a volatile token-denominated reward.

The intended escrow lifecycle is:

```text
Deposit -> Lock -> Claim -> Release
```

1. **Deposit**: the owner funds the bounty in USDC.
2. **Lock**: the smart contract records the stolen-device report and locks the bounty against that device record.
3. **Claim**: a finder or recovery participant starts the recovery flow and provides the required proof or recovery handoff.
4. **Release**: once recovery is confirmed, the smart contract releases the bounty to the approved recipient.

The current documentation notes that the contract records bounty amounts today, while token escrow transfer is planned as part of the contract roadmap. This page describes the target protocol design so contributors, users, and reviewers understand the economic model before that implementation is completed.

## Edge cases

### No one claims the bounty

If no finder claims the bounty before expiry, the bounty should remain locked until the owner cancels it or the expiry rule allows refund. The owner should be able to recover unused funds after the claim window closes.

### Bounty expiry

Expiry prevents funds from being locked forever. A clear expiry window also gives finders urgency: the reward is available only while the recovery campaign is active.

A typical flow is:

```text
active bounty -> expiry reached -> claim disabled -> owner refund enabled
```

### Dispute resolution

Disputes can happen when multiple parties claim involvement, the device is returned damaged, or the owner disputes whether the recovery conditions were met.

A dispute flow should define:

- who can open a dispute;
- what evidence is required;
- whether the bounty remains locked during review;
- who can approve, reject, or split the payout;
- what events are emitted for indexers and audit tools.

Until the dispute mechanism is fully specified, high-value bounties should use conservative confirmation steps and clear off-chain communication.

## Design implications

The bounty amount is not only a reward. It is a market signal.

A good bounty should be:

- high enough to beat the local fence price;
- stable enough to be trusted by finders and vendors;
- locked in escrow so the finder believes payment will happen;
- visible enough that secondary-market participants know the device is worth returning;
- reversible only under clear expiry or dispute rules.

That is the central game-theoretic promise of Haven: make honest recovery more profitable than dishonest resale.
