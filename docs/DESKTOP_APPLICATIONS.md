# Desktop application allocation

Verified **2026-08-06**.

Hypesiege uses the paired desktop application standard:

- Rust: [`hypesiege/hypesiege-desktop.rs`](https://github.com/hypesiege/hypesiege-desktop.rs) — **planned**, not yet verified as a published repository.
- Flutter, current: [`hypesiege/hypesiege-flutter-app`](https://github.com/hypesiege/hypesiege-flutter-app) — **live**.
- Flutter canonical rename target: `hypesiege/hypesiege-flutter` — planned naming normalization; do not describe as published until renamed and verified.

## Why both Rust and Flutter remain active

The Rust and Flutter applications remain first-class side-by-side implementations so the product can compare performance, OS integration, media workflows, accessibility, developer velocity, mobile reuse, release engineering, user preference, and long-term maintenance using the same real publishing features.

Every desktop-facing change must inspect both implementations, use shared acceptance criteria and fixtures, and normally update both. A one-sided change requires a documented no-change rationale and parity gap.

## Rust desktop kit: Tauri 2 without React

**Selected strategy:** Tauri 2.

**WebView policy:** allowed for this product.

**Frontend policy:** no React, JSX, React-derived stack, Vue, or Svelte. Use vanilla HTML, CSS, and TypeScript. HTMX is allowed for authenticated server-driven fragments when it reduces client complexity. Privileged local behavior must use reviewed Tauri commands/events and capability scopes rather than an unauthenticated loopback service.

Hypesiege is dominated by media composition, forms, drafts, approvals, scheduling, account/channel selection, and publishing dashboards. These are a strong fit for lightweight HTML/CSS while Rust owns local persistence, secrets, validation, background jobs, and platform integration. Tauri also provides a mature deep-link plugin and single-instance integration.

The Rust repository must contain `docs/DESKTOP_TOOLKIT.md` covering Tauri 2 version policy, CSP/capabilities, allowed frontend tools, command boundaries, deep links, single-instance behavior, tests, packaging, and the Flutter companion.

## HTTPS-first deep linking

Canonical form:

```text
https://<verified-hypesiege-owned-host>/open/<route>?<bounded-query>
```

Fallback scheme:

```text
hypesiege://<route>?<bounded-query>
```

Routes must be defined in `hypesiege-interfaces` and shared by Rust, Flutter, clients, connectors, and browser fallback pages.

Required behavior:

- use `tauri-plugin-deep-link` plus `tauri-plugin-single-instance`;
- support cold start and already-running delivery;
- validate the exact HTTPS host, route, campaign/draft/account identifiers, action, and bounded query parameters;
- never place social-account credentials, publishing tokens, media contents, private drafts, or personal data in URLs;
- use short-lived, one-time, audience-bound codes for account connection, approvals, imports, and collaboration handoffs;
- require explicit confirmation before publishing, scheduling, or importing externally supplied content; and
- test macOS, Windows, Linux, Android, and iOS app/universal links plus browser fallback.

## Product boundary

Both implementations should support semantic parity for media composition, asset import/export, drafts, approvals, scheduling, account/channel selection, publishing state, retries, notifications, offline recovery, auditability, and deep links.

Shared schemas, clients, route fixtures, sample campaigns, publishing-state models, and conformance tests must be versioned deliberately.

## Repository-local documentation

The live Flutter repository records the companion contract in [`COMPANION_DESKTOP.md`](https://github.com/hypesiege/hypesiege-flutter-app/blob/main/COMPANION_DESKTOP.md), introduced through [PR #11](https://github.com/hypesiege/hypesiege-flutter-app/pull/11).

Central toolkit assignments: [`rust-desktop-strategies.md`](https://github.com/ORESoftware/project-registry/blob/main/docs/rust-desktop-strategies.md).

## Project routing

- GitHub Project: [`hypesiege-project` — Project 1](https://github.com/orgs/hypesiege/projects/1)
- Linear project: `github.com/hypesiege`
- Central registry: [`approved-private-registry`](private-registry://canonical/registry/desktop-applications.json)
- Portfolio rollout: [`DEN-2469`](https://linear.app/denman/issue/DEN-2469/roll-out-paired-rust-flutter-desktop-repositories-across-the-portfolio)

Repository creation, Flutter renaming, toolkit/frontend changes, deep-link changes, transfers, archival, or platform-status changes must update this document, Linear, the central registry/strategy, and both companion repositories together.
