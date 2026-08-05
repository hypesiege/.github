# Desktop application allocation

Verified **2026-08-05**.

Hypesiege uses the paired native desktop application standard:

- Rust: [`hypesiege/hypesiege-desktop.rs`](https://github.com/hypesiege/hypesiege-desktop.rs) — **planned**, not yet verified as a published repository.
- Flutter: [`hypesiege/hypesiege-flutter-app`](https://github.com/hypesiege/hypesiege-flutter-app) — **live**.

The Rust URL is an allocation target, not proof that the remote exists. Do not mark it live until the repository, native targets, tests, packaging, and supported-platform matrix are verified.

The live Flutter repository records the companion contract in [`COMPANION_DESKTOP.md`](https://github.com/hypesiege/hypesiege-flutter-app/blob/main/COMPANION_DESKTOP.md), merged through [PR #11](https://github.com/hypesiege/hypesiege-flutter-app/pull/11).

## Product boundary

Both implementations should support semantic parity for media composition, asset import/export, drafts, approvals, scheduling, account and channel selection, publishing state, retries, notifications, offline recovery, and auditability.

The Rust and Flutter implementations remain independently buildable, testable, releasable applications. Shared schemas, clients, fixtures, sample campaigns, publishing-state models, and conformance tests should be versioned deliberately.

## Feature-delivery rule

Every desktop-facing change must inspect both implementations, define shared acceptance criteria, update both or record an explicit no-change rationale, and report Rust and Flutter status separately.

## Project routing

- GitHub Project: [`hypesiege-project` — Project 1](https://github.com/orgs/hypesiege/projects/1)
- Linear project: `github.com/hypesiege`
- Central registry: [`approved-private-registry`](private-registry://canonical/registry/desktop-applications.json)
- Portfolio rollout: [`DEN-2469`](https://linear.app/denman/issue/DEN-2469/roll-out-paired-rust-flutter-desktop-repositories-across-the-portfolio)

Repository creation, renames, transfers, archival, or platform-status changes must update this document, Linear, the central registry, and both companion repositories together.
