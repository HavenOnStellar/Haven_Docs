# Bounty Economics

Haven's bounty system is designed around a simple idea: returning a stolen device should be worth more than selling it through informal resale channels.

In many emerging markets, a stolen phone may sell to a fence for roughly $15 to $40. If the owner offers a recovery bounty above that expected resale price, returning the device becomes the rational economic choice.

## Core incentive model

The bounty should exceed the expected informal resale value:

    recovery_bounty > expected_fence_price

The frontend calculator can express this with a multiplier:

    recommended_bounty = estimated_fence_price * bounty_multiplier

For example, if the expected fence price is $25 and the multiplier is 2:

    $25 * 2 = $50

## Worked examples

### $50 bounty

    Expected fence price: $25
    Recovery bounty:      $50
    Finder advantage:     $25

A $50 bounty can be enough when the likely resale value is low.

### $100 bounty

    Expected fence price: $40
    Recovery bounty:      $100
    Finder advantage:     $60

A $100 bounty creates a stronger incentive for mid-range devices or faster recovery.

### $250 bounty

    Expected fence price: $40
    Recovery bounty:      $250
    Finder advantage:     $210

A higher bounty may make sense for high-value devices, business devices, or phones tied to important accounts or insurance processes.

## Escrow lifecycle

Haven uses USDC escrow so the bounty is visible, funded, and released through the recovery flow.

    deposit -> lock -> recovery request -> release

1. Deposit: the owner funds the bounty in USDC.
2. Lock: the smart contract records the bounty against the stolen device.
3. Recovery request: the recovery party uses the device recovery flow.
4. Release: once recovery is confirmed, the locked bounty is released.

This reduces ambiguity for both sides. The recovery party can see that funds exist, and the owner does not need to pay outside the protocol before recovery is confirmed.

## Edge cases

### No recovery request

If no one starts a recovery flow, the device remains marked as stolen and the bounty remains tied to the recovery record until expiry or cancellation rules apply.

### Bounty expiry

A bounty may include an expiry period. After expiry, the protocol can allow the owner to renew, reclaim, or redirect the locked funds depending on final contract rules.

### Dispute resolution

Disputes may occur if multiple parties report recovery, if device condition is unclear, or if ownership must be verified. Future protocol rules should use contract events, device status, owner confirmation, and dispute-resolution logic to resolve these cases.

## Why this works

The model changes the payoff around stolen devices. Instead of relying only on enforcement or blacklisting, Haven creates a direct economic reason to return the device.

A bounty above the expected fence price turns recovery into the better deal.
