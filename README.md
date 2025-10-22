# Hop-Corr Workspace

Hop-Corr is a Rust workspace that explores companion-matrix driven streams,
action pipelines, and relay tooling for verifiable ledger-style channels. The
workspace is split into focused crates so that core algebra, persistence, CLI
operations, and relay endpoints can evolve independently while sharing the same
finite-field primitives.

## Crate Overview

- `hop-core` – Finite-field arithmetic, Shamir VSS helpers, companion-matrix
  utilities, and stream generators that expose the algebraic backbone.
- `hop-actions` – Action definitions, polynomial evaluation routines, and
  centralizer checks used to validate per-step transformations.
- `hop-channels` – Channel state machine with pluggable persistence layers,
  subscribers, and optional torsor masking helpers.
- `hop-consensus` – Minimal consensus interface plus a simple queueing
  implementation for batching actions and emitting receipts.
- `hop-mix` – Prototype batch mixer for aggregating multiple contributor
  actions into a single composite action.
- `hop-relay` – Relay library (and optional HTTP server via the `net` feature)
  that handles persistence, bundle ingestion, VRF endpoints, and vault APIs.
- `fulbody-cli` – Command-line client for local experimentation, HTTP interaction
  with a relay, vault management, and receipt verification.
- `hop-accel` – Placeholder crate reserved for future optimized kernels.
- `xtask` – Shell crate for future workspace automation or code generation.

Each crate is designed to compile independently, but they also compose through
workspace features such as optional torsor masking.

## Building

```bash
cargo build
```

The workspace currently targets the 2021 Rust edition and relies only on the
stable toolchain.

## Testing

```bash
cargo test
```

Feature-specific test suites (for example torsor masking) can be invoked with
standard Cargo feature flags.

## Running The Relay

The relay exposes HTTP endpoints behind the `net` feature flag:

```bash
cargo run -p hop-relay --features net -- --listen 127.0.0.1:8787
```

For production-style deployments, ensure the desired policy files, certificate
material, and storage directories exist before launching the relay.

## CLI Quick Start

```bash
cargo run -p fulbody-cli -- create --coeffs 4,3,2
cargo run -p fulbody-cli -- send-bundle --coeffs 4,3,2 --batch "1,2;0,1" \
  --id 000000... --dir ./data
```

The CLI supports JSON output (`--json`), vault operations, transcript replay,
and receipt verification.

## Workspace Conventions

- All crates share a single `Cargo.lock` and use the workspace resolver (v2).
- Integration tests for persistence create data inside `.testdata/` under the
  crate directory and clean it up automatically.
- Torsor masking helpers and VRF APIs are gated behind optional features.

## Licensing

See the `LICENSE` file for details. Commercial licensing is available—contact
lexluger.dev@proton.me.
# hop-corr
