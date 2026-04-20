# System Architecture

Haven has three main layers:

1. User-facing frontend
2. Soroban smart contract registry
3. Future integrations for wallets, fiat rails, and indexers

## User layer

The frontend is a Next.js application. Today it includes a landing page and a typed client stub. Future pages will include:

- Device registration dashboard
- Vendor verification portal
- Stolen device report flow
- Recovery confirmation flow
- Insurance provider dashboard

## Contract layer

The main contract is `HavenRegistry`. It manages:

- Device registration
- Stolen status
- Recovery bounty records
- Insurance claim authority

The core modules are:

- `device.rs`
- `killswitch.rs`
- `recovery.rs`
- `insurance.rs`

## Data flow

1. The user enters a device IMEI.
2. The IMEI is hashed off-chain using SHA-256.
3. The hash is sent to the Soroban contract.
4. The contract stores a `DeviceState` keyed by hashed IMEI.
5. Vendors and users can query the device status.
6. If stolen, the owner can attach a bounty.
7. If recovered, the owner confirms and the finder receives the bounty.
8. If insurance is involved, the claim transfers authority to the insurer.

## Privacy model

The raw IMEI should never be stored on-chain. The frontend currently includes a development helper for hashing, but production deployments should prefer server-side or privacy-preserving hashing flows.

## Future integrations

- Freighter for B2B wallet flows
- Passkey Kit for biometric onboarding
- Launchtube for fee abstraction
- SEP-24 anchors for fiat-to-USDC bounty funding
- Indexers for device status and event history
