# Insurance Claims

Haven provides cryptographic proof-of-loss for insurance workflows.

## Goal

Give insurers a verifiable on-chain record that a device has been reported and claimed.

## Current contract function

```rust
file_insurance_claim(owner, hashed_imei, insurer)
```

## Current flow

1. Owner registers a device.
2. Owner files an insurance claim with an insurer address.
3. The contract verifies ownership.
4. The contract assigns the insurer to the device record.

## Current limitations

- Only owner authorization is currently required.
- Claim metadata is not stored yet.
- No cooldown period is enforced.
- Bounty handling during insurance claim is not finalized.

## Planned improvements

- Require owner and insurer authorization
- Add claim ID, timestamp, and payout amount
- Add cooldown period between stolen report and insurance claim
- Add salvage actions for insurer-owned devices
- Emit InsuranceClaimed event
