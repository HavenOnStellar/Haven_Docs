# Getting Started

This guide helps contributors run the Haven frontend and smart contracts locally.

## Prerequisites

### Frontend

- Node.js 18 or later
- npm 9 or later

### Smart contracts

- Rust 1.84 or later
- Stellar CLI
- Wasm target for Soroban contracts

Install the Wasm target:

```bash
rustup target add wasm32v1-none
```

## Clone the repositories

```bash
git clone https://github.com/HavenOnStellar/Haven_Frontend.git
git clone https://github.com/HavenOnStellar/Haven_Contracts.git
git clone https://github.com/HavenOnStellar/Haven_Docs.git
```

## Run the frontend

```bash
cd Haven_Frontend
npm install
npm run dev
```

Open http://localhost:3000.

## Check the frontend

```bash
npm run lint
npm run build
```

## Run the contracts

```bash
cd Haven_Contracts
cargo check
cargo test
```

## Build the contract Wasm

```bash
stellar contract build
```

## Testnet contract

The current documented testnet contract ID is:

```text
CAT2TDBXGW6GETW52MQB725PLWN2CBVO3TXJ7PRJ73YSKLHRA7SRN6FC
```

Use this for frontend experiments until a newer deployment is published.
