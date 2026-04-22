# Smart Contracts

The Haven smart contracts live in the `Haven_Contracts` repository.

Repository: https://github.com/HavenOnStellar/Haven_Contracts

## Contract

The main contract is `HavenRegistry`.

## Modules

```text
contracts/haven_registry/src/
├── lib.rs
├── device.rs
├── killswitch.rs
├── recovery.rs
├── insurance.rs
└── test.rs
```

## Device registration

`register_device()` stores a device using a SHA-256 hashed IMEI. The raw IMEI should never be submitted to the contract.

## Killswitch

`report_stolen()` marks a device as stolen, stores recovery contact information, and records a bounty amount. Token escrow transfer is planned as a follow-up implementation.

## Recovery

`confirm_recovery()` marks the device as recovered and clears the bounty record. Actual token payout to the finder is planned.

## Insurance

`file_insurance_claim()` assigns an insurer to the device record and represents proof-of-loss. Future improvements include multi-signature claims, claim metadata, cooldowns, and salvage flows.

## Local commands

```bash
cargo check
cargo test
stellar contract build
```

## CI

The contracts repository has a GitHub Actions workflow that runs on pull requests to `main`:

- `cargo check --workspace --all-targets`
- `cargo test --workspace`

The `main` branch ruleset requires this check to pass before merging.
