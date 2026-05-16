# Haven Protocol

Haven is a decentralized device registry built on Stellar and Soroban. It is designed to make smartphone theft economically unviable by combining on-chain device registration, trustless recovery bounties, and cryptographic proof-of-loss for insurers.

## What Haven does

Haven lets a device owner register a privacy-preserving hash of their device IMEI on-chain. If the device is stolen, the owner can activate a killswitch, mark the device as stolen, and attach a recovery bounty. Vendors, finders, and secondary-market participants can verify a device before buying it and are incentivized to return stolen devices rather than resell them.

## Core ideas

- Raw IMEIs should never be stored on-chain.
- Device status should be publicly verifiable.
- Recovery should be economically better than resale.
- Insurance claims should have immutable proof-of-loss.
- The user experience should eventually support wallets, passkeys, fiat on-ramps, and gas abstraction.

## Repositories

- Frontend: https://github.com/HavenOnStellar/Haven_Frontend
- Smart contracts: https://github.com/HavenOnStellar/Haven_Contracts
- Documentation: https://github.com/HavenOnStellar/Haven_Docs

## Current status

Haven is currently in an early open-source preparation phase. The smart contract contains the core registry, killswitch, recovery, and insurance flows. The frontend contains the landing page and a typed Stellar/Soroban client stub that will be expanded into dashboard and verification flows.
