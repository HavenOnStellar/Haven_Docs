# Confirming Recovery

The recovery flow marks a stolen device as recovered and releases the bounty to the finder.

## Goal

Reward the finder and restore the device's non-stolen status.

## Current contract function

```rust
confirm_recovery(owner, hashed_imei, finder)
```

## Flow

1. A finder or vendor contacts the owner using the recovery contact.
2. The owner verifies that the device has been recovered.
3. The owner submits the finder's Stellar address.
4. The contract verifies owner authorization.
5. The contract marks the device as no longer stolen.
6. The bounty record is cleared.

## Current limitation

The current contract clears the bounty record but does not yet transfer escrowed tokens to the finder.

## Planned improvements

- Actual token payout
- DeviceRecovered event
- Time-lock against bounty gaming
- Recovery proof or dual-signature flow
- Recovery history for insurance audits
