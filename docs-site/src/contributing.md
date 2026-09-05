# Contributing

This page is a starting point across all three repositories. Each
repository's own `CONTRIBUTING.md` remains authoritative for its
repository-specific process — this page does not duplicate them, only
orients you toward the right one.

## Three contribution paths

### Core engine — `stellar-drive-stellar-drive-protocol-canary`

Where to work: `crates/canary-*` (the CLI, project detection, fixture
loading, the XDR/RPC/Soroban runners, policy evaluation, reporting).

What belongs here: changes to how compatibility checks are executed or
reported — a new surface capability, a new report format, a bug in
project detection, a new configuration option. Protocol-specific
*assertions* belong in `ProtocolCanary-Fixtures` instead, not here.

How to test:

```bash
cargo fmt --all --check
cargo check --workspace --all-targets
cargo test --workspace
cargo clippy --workspace --all-targets -- -D warnings
```

All four must pass. Unit tests must not require network access; anything
that talks to a real RPC endpoint belongs in `tests/integration` and must
be explicitly opt-in.

How to submit: a pull request against
[`Stellar-drive/stellar-drive-stellar-drive-protocol-canary`](https://github.com/Stellar-drive/stellar-drive-stellar-drive-protocol-canary),
following its [`CONTRIBUTING.md`](https://github.com/Stellar-drive/stellar-drive-stellar-drive-protocol-canary/blob/main/CONTRIBUTING.md)
(workspace structure, coding standards, and commit style).

### Fixtures — `ProtocolCanary-Fixtures`

Where to work: a protocol-pack directory (e.g. `protocol-28/`), organized
by surface and CAP.

What belongs here: a new compatibility assertion for a real, verifiable
piece of protocol behavior — never a fixture added just to raise a
coverage number. See [Authoring a Fixture](./fixture-authoring.md) for
the full workflow.

How to test:

```bash
python3 tools/validate/validate.py
python3 -m unittest discover tests
```

No build system is required — the validator uses only the Python 3.11+
standard library.

How to submit: a pull request against
[`Stellar-drive/ProtocolCanary-Fixtures`](https://github.com/Stellar-drive/ProtocolCanary-Fixtures),
following its [`CONTRIBUTING.md`](https://github.com/Stellar-drive/ProtocolCanary-Fixtures/blob/main/CONTRIBUTING.md).

### GitHub Action — `ProtocolCanary-Action`

Where to work: `src/` (TypeScript), bundled to the committed `dist/index.js`.

What belongs here: changes to how CI consumes Canary's report — the
summary, annotations, artifact handling, installation/pinning logic.
Compatibility logic itself belongs in `stellar-drive-stellar-drive-protocol-canary`, not here.

How to test:

```bash
npm run typecheck
npm run lint
npm test
npm run build
```

Unit and integration tests never require network access or a real
`stellar-canary` binary — they run against a mock CLI
(`tests/fixtures/mock-canary.cjs`) that simulates every documented result
state via the `MOCK_CANARY_SCENARIO` environment variable. The repository's
own `integration.yml` workflow is the only place it talks to a real
Canary build and a real Testnet endpoint, and it never gates a pull
request.

How to submit: a pull request against
[`Stellar-drive/ProtocolCanary-Action`](https://github.com/Stellar-drive/ProtocolCanary-Action),
following its [`CONTRIBUTING.md`](https://github.com/Stellar-drive/ProtocolCanary-Action/blob/main/CONTRIBUTING.md).

## Contributing to this documentation site

The site itself lives at `docs-site/` in the `stellar-drive-stellar-drive-protocol-canary` repository.
See [`docs-site/README.md`](https://github.com/Stellar-drive/stellar-drive-stellar-drive-protocol-canary/blob/main/docs-site/README.md)
for how to build and preview it locally with mdBook. Keep implementation
detail in each repository's own `docs/`/`CONTRIBUTING.md` and link to it
from here rather than duplicating it — this site is the developer
journey, not a second source of truth.
