# Registering a Device

Device registration is the first step in the Haven protocol.

## Goal

Create an on-chain record for a device without exposing the raw IMEI.

## Current contract function

```rust
register_device(owner, hashed_imei, device_model)
```

## Flow

1. The owner provides the device IMEI in the app.
2. The IMEI is hashed off-chain using SHA-256.
3. The owner signs the transaction.
4. The contract stores a `DeviceState` under the hashed IMEI.
5. Anyone can later query the device status using the hash.

## Important privacy note

The raw IMEI should not be sent to the blockchain. Production flows should avoid exposing the raw IMEI in logs, analytics, or browser history.

## Current status

The contract supports registration. The frontend registration form is planned.
