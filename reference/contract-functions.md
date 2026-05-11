# Contract Functions

This page summarizes the public functions exposed by `HavenRegistry`.

## initialize

```rust
initialize(admin)
```

Initializes the contract with an administrator address. This should be called once after deployment.

## register_device

```rust
register_device(owner, hashed_imei, device_model) -> DeviceState
```

Registers a device using a SHA-256 hashed IMEI.

## get_device

```rust
get_device(hashed_imei) -> DeviceState
```

Returns the stored device state. The current implementation panics if the device is not found. A future improvement may return an optional result.

## report_stolen

```rust
report_stolen(owner, hashed_imei, bounty_amount, recovery_contact)
```

Marks a device as stolen and records bounty information.

## get_bounty

```rust
get_bounty(hashed_imei) -> i128
```

Returns the current bounty amount for a device, or zero if none exists.

## confirm_recovery

```rust
confirm_recovery(owner, hashed_imei, finder)
```

Marks a stolen device as recovered and clears the bounty record.

## file_insurance_claim

```rust
file_insurance_claim(owner, hashed_imei, insurer)
```

Assigns an insurer to the device record as proof-of-loss.
