# Reporting a Stolen Device

The stolen-device report flow is Haven's killswitch.

## Goal

Mark a registered device as stolen and attach a recovery bounty.

## Current contract function

```rust
report_stolen(owner, hashed_imei, bounty_amount, recovery_contact)
```

## Flow

1. The owner selects a registered device.
2. The owner enters a bounty amount and recovery contact.
3. The owner signs the transaction.
4. The contract verifies ownership.
5. The contract marks the device as stolen.
6. The bounty amount is recorded.

## Current limitation

The current contract records the bounty amount but does not yet transfer SAC/USDC tokens into escrow. Token transfer support is part of the contract roadmap.

## Planned improvements

- Minimum bounty validation
- USDC escrow transfer
- DeviceStolen event emission
- Ability to increase bounty after report
