# Architecture Diagrams

This page provides version-controlled Mermaid diagrams for the Haven system architecture, bounty lifecycle, insurance flow, and USDC escrow interactions.

## Repository overview

Haven is split across three repositories:

- `Haven_Contracts`: Soroban smart contracts for device state, theft reports, recovery, insurance, and bounty records.
- `Haven_Frontend`: user interface for registration, stolen-device reporting, wallet signing, and recovery workflows.
- `Haven_Docs`: protocol, user, and contributor documentation.

```mermaid
flowchart LR
    docs[Haven_Docs\nProtocol and user docs]
    frontend[Haven_Frontend\nWeb app and client flows]
    contracts[Haven_Contracts\nSoroban contracts]
    users[Users, finders, vendors, insurers]

    docs -->|explains| frontend
    docs -->|documents| contracts
    users -->|register, report, recover| frontend
    frontend -->|hash IMEI locally| frontend
    frontend -->|build transaction| wallet[Freighter wallet]
    wallet -->|sign transaction| frontend
    frontend -->|submit signed transaction| contracts
    contracts -->|events and state| frontend
    frontend -->|status and recovery views| users
```

## Device registration and stolen report data flow

The frontend should hash the IMEI before any on-chain write. The raw IMEI should stay off-chain.

```mermaid
sequenceDiagram
    actor User
    participant UI as Haven_Frontend
    participant Hash as Local IMEI hashing
    participant Wallet as Freighter
    participant Contract as HavenRegistry on Soroban

    User->>UI: Enter device details
    UI->>Hash: Hash raw IMEI locally
    Hash-->>UI: hashed_imei
    UI->>Wallet: Request signature for register_device
    Wallet-->>UI: Signed transaction
    UI->>Contract: register_device(owner, hashed_imei)
    Contract-->>UI: Device state stored

    User->>UI: Report device stolen
    UI->>Wallet: Request signature for report_stolen
    Wallet-->>UI: Signed transaction
    UI->>Contract: report_stolen(owner, hashed_imei, bounty_amount, recovery_contact)
    Contract-->>UI: Stolen status and bounty record stored
```

## Bounty lifecycle

The bounty lifecycle turns a stolen-device report into a recoverable economic incentive. The target escrow model is deposit, lock, claim, and release.

```mermaid
stateDiagram-v2
    [*] --> Registered: register_device
    Registered --> Stolen: report_stolen
    Stolen --> KillswitchActive: mark stolen and expose recovery contact
    KillswitchActive --> EscrowLocked: owner deposits USDC bounty
    EscrowLocked --> ClaimPending: finder submits recovery claim
    ClaimPending --> Confirmed: owner confirms device recovery
    Confirmed --> Released: smart contract releases USDC
    Released --> Recovered: bounty record cleared
    Recovered --> [*]

    EscrowLocked --> Expired: bounty expiry reached
    Expired --> Refunded: owner withdraws unclaimed bounty
    Refunded --> [*]

    ClaimPending --> Disputed: owner or finder disputes claim
    Disputed --> Confirmed: dispute resolved in finder favor
    Disputed --> EscrowLocked: dispute rejected, bounty remains active
```

## USDC escrow sequence

This sequence describes the intended escrow interactions for a USDC-funded recovery bounty.

```mermaid
sequenceDiagram
    actor Owner
    actor Finder
    participant UI as Haven_Frontend
    participant Wallet as Freighter
    participant Registry as HavenRegistry
    participant USDC as USDC token contract

    Owner->>UI: Choose bounty amount
    UI->>Wallet: Request approval/deposit transaction
    Wallet-->>UI: Signed transaction
    UI->>USDC: approve or transfer bounty amount
    UI->>Registry: lock_bounty(hashed_imei, amount, asset)
    Registry->>USDC: verify/receive escrowed USDC
    Registry-->>UI: Bounty locked

    Finder->>UI: Start recovery claim
    UI->>Registry: claim_bounty(hashed_imei, finder)
    Registry-->>UI: Claim pending

    Owner->>UI: Confirm recovered device
    UI->>Wallet: Request confirmation signature
    Wallet-->>UI: Signed confirmation
    UI->>Registry: confirm_recovery(hashed_imei, finder)
    Registry->>USDC: release escrow to finder
    USDC-->>Finder: USDC payout
    Registry-->>UI: Device recovered, bounty cleared
```

## Insurance claim flow

Insurance flows connect a theft report to proof-of-loss, insurer authority, and possible salvage rights.

```mermaid
flowchart TD
    theft[Device reported stolen]
    proof[Owner submits proof-of-loss]
    insurer[Insurer address assigned]
    review[Insurer reviews claim]
    approved[Claim approved]
    denied[Claim denied]
    salvage[Salvage rights transferred]
    recovery[Device later recovered]
    payout[Insurance payout completed]

    theft --> proof
    proof --> insurer
    insurer --> review
    review -->|valid proof| approved
    review -->|missing or invalid proof| denied
    approved --> payout
    approved --> salvage
    salvage --> recovery
    recovery -->|insurer owns salvage path| insurer
    denied -->|owner may revise evidence| proof
```

## Implementation notes

- Mermaid keeps the diagrams reviewable in Git and editable in future PRs.
- The diagrams describe the target protocol design; current contract pages note where token transfers and escrow release are still planned work.
- The frontend should continue to treat raw IMEI values as local-only data and submit only hashed identifiers to the contract.
