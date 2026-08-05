# HypeSiege Rust server modularization and provisioning ledger

This document is the HypeSiege organization view of the cross-organization program tracked by Linear [DEN-1682](https://linear.app/denman/issue/DEN-1682) and [DEN-1757](https://linear.app/denman/issue/DEN-1757).

## Architecture contract

Every Rust service binary keeps `main.rs` as process wiring only. Independently testable modules own configuration, observability initialization, dependency construction, routes or MCP tools, transport, bounded validation, security middleware, background-task lifecycle, and product handlers.

Product policy stays in the product repository. A shared cross-organization runtime crate is introduced only after two merged consumers demonstrate the same stable contract and compatibility tests. MCP services remain read-only visibility layers and never become alternate persistence paths.

## Wave 1 implementation evidence

| Repository | Boundary | Merged evidence |
| --- | --- | --- |
| `hypesiege/hypesiege-api-server.rs` | Required Postgres bootstrap; explicit outbox and publishing-worker lifecycle; route/listener composition. | PR #19, merge `9d3dbf596ad7d0f60b8a3e618f96ebf1702e0bb1`; exact-head run `30907747140`. |
| `hypesiege/hypesiege-web-server.rs` | Axum app composition, optional DB state, seven-header browser security contract, real Chromium tests, runtime lifecycle. | PR #8, merge `d888347290c0fb07048090118b74e7fed7e9e279`; exact-head run `30908019881`. |
| `hypesiege/hypesiege-mcp-server.rs` | Domain/tool routing, stdio lifecycle, bounded OpenTelemetry resource policy, fixed-cardinality metrics. | Modular runtime series on `main`; OTEL hardening PR #9, exact-head run `30908851989`. |

All three services retain product-specific authorization, routing, outbox, publishing, provider, and retry policy. Rollback is by reviewed revert; shared history is not rewritten.

## Wave 2 reviewed repository targets

| Target | Starter responsibility | Repository state |
| --- | --- | --- |
| `hypesiege/hypesiege-scheduler.rs` | Deterministically select due scheduled posts and enqueue publish jobs. | `provisioning_required` |
| `hypesiege/hypesiege-publishing-worker.rs` | Classify idempotent official-provider attempts into acknowledge, retry, dead-letter, or permanent rejection. | `provisioning_required` |
| `hypesiege/hypesiege-analytics.rs` | Aggregate bounded reviewed metrics from official-provider APIs with checked arithmetic. | `provisioning_required` |

The source manifest, deterministic generator, offline compilation tests, canonical archives, and provenance receipts are merged in `hypesiege/hypesiege-monorepo`. The durable coordination card is [hypesiege/hypesiege-monorepo#15](https://github.com/hypesiege/hypesiege-monorepo/issues/15).

## Repository provisioning result — August 5, 2026

The reviewed provisioning workflow and exact issue-comment trigger are merged. The trigger run validated the manifest and trusted event but stopped before the first repository write:

```text
no GitHub App candidates found: app_ids=0 private_keys=0
```

The runner exposed only the default repository `GITHUB_TOKEN`; it did not have a repository-admin GitHub App ID/private key. No repository was created and no starter archive was pushed.

### Required administrator action

Configure one GitHub App for the `hypesiege` organization with:

- all-repositories installation scope;
- organization repository administration: write;
- repository contents: write;
- pull requests: write;
- metadata: read;
- Actions secrets containing the App ID and PEM private key under the selector’s reviewed naming contract.

Then rerun the exact trusted provisioning command recorded in issue #15. The workflow probes candidate App pairs, requires exactly one valid pair, creates or verifies only the three manifest targets, revokes probe and creation tokens, destroys temporary private-key files, and uploads non-secret JSONL evidence.

A classic PAT is not an approved substitute. The token pasted into chat is not copied into any GitHub or Linear surface and should be revoked.

## Initialization sequence after repository creation

1. Verify each repository is private, issue-enabled, project-enabled, wiki-disabled, and initialized on `main`.
2. Download the exact reviewed Wave 2 artifact and verify its GitHub artifact digest plus internal `SHA256SUMS`.
3. Unpack one starter archive into a repository-specific initialization branch.
4. Open one draft PR per repository.
5. Run offline locked Cargo metadata, rustfmt, strict Clippy, all-target tests, architecture tests, and startup probe.
6. Merge only the exact green reviewed head.
7. Add the exact child commit as a monorepo gitlink in a separate PR.
8. Update DEN-1757, this document, issue #15, and the GitHub Project item with exact evidence.

## GitHub Project update contract

Add these durable items to [HypeSiege Project 1](https://github.com/orgs/hypesiege/projects/1):

- `hypesiege/.github#2` — organization routing card.
- `hypesiege/hypesiege-monorepo#15` — Wave 2 provisioning and artifact card.
- Initialization PRs for the scheduler, publishing worker, and analytics repositories after creation.

Projects v2 mutations require a Projects-capable App or an authenticated GitHub CLI/GraphQL runner with project write permission. Until that exists, the issues above are the stable board-ready inputs; no board mutation is claimed.

## Merge and evidence requirements

A PR is merge-ready only when:

- it is based on the current default branch;
- all conflicts are resolved semantically;
- the exact head has passed every required workflow;
- no unresolved review thread or changes-requested review remains;
- generated artifacts are content-addressed and reproducible;
- credentials and private payloads are excluded from source, logs, reports, and artifacts; and
- Linear and organization documentation identify the exact head, workflow run, artifact digest, and merge commit.
