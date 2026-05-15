# Development Workflow

This page explains the recommended workflow for contributors.

## Frontend workflow

```bash
cd Haven_Frontend
npm install
npm run dev
npm run lint
npm run build
```

Before opening a frontend PR, make sure lint and build pass locally.

## Contracts workflow

```bash
cd Haven_Contracts
cargo check
cargo test
```

Before opening a contracts PR, make sure the contract compiles and tests pass.

## CI checks

Both repositories use GitHub Actions on pull requests to `main`.

### Frontend CI

- `npm ci`
- `npm run lint`
- `npm run build`

### Contracts CI

- `cargo check --workspace --all-targets`
- `cargo test --workspace`

## Branch protection

The `main` branches are protected with rulesets. Pull requests require:

- at least one approving review
- resolved review conversations
- passing CI checks

## Maintainer review

Maintainers review PRs for correctness, scope, security, and maintainability. Small focused PRs are easier to review and merge.
