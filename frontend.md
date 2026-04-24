# Frontend

The Haven frontend is a Next.js application that introduces the protocol and will eventually provide user and vendor workflows.

## Repository

https://github.com/HavenOnStellar/Haven_Frontend

## Stack

- Next.js
- React
- TypeScript
- Tailwind CSS
- Stellar SDK
- Freighter API

## Current structure

```text
src/app/
├── globals.css
├── layout.tsx
├── page.tsx
└── lib/
    └── havenClient.ts
```

## Current features

- Landing page
- Design system tokens
- SEO metadata
- Stellar/Soroban client stub
- IMEI hashing helper

## Planned features

- Mobile navigation
- Vendor verification page
- User dashboard
- Device registration form
- Stolen device report flow
- Recovery confirmation flow
- Freighter wallet connection
- Environment-based Stellar network config

## Local commands

```bash
npm install
npm run dev
npm run lint
npm run build
```

## CI

The frontend repository has a GitHub Actions workflow that runs on pull requests to `main`:

- install dependencies with `npm ci`
- run `npm run lint`
- run `npm run build`

The `main` branch ruleset requires this check to pass before merging.
