# HypeSiege Rust server modularization and provisioning ledger

This document is the HypeSiege organization view of the cross-organization program tracked by Linear [DEN-1682](https://linear.app/denman/issue/DEN-1682), [DEN-1757](https://linear.app/denman/issue/DEN-1757), and repository-publication reconciliation [DEN-2328](https://linear.app/denman/issue/DEN-2328).

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

## Wave 2 published repositories

| Repository | Responsibility | Verified publication state |
| --- | --- | --- |
| [`hypesiege/hypesiege-scheduler.rs`](https://github.com/hypesiege/hypesiege-scheduler.rs) | Deterministically select due scheduled posts and enqueue publish jobs. | Private repository `1324464386`; `main` `e8a739d9e658e9cef8f1dc938a412b923dbff57d`; exact sealed history verified. |
| [`hypesiege/hypesiege-publishing-worker.rs`](https://github.com/hypesiege/hypesiege-publishing-worker.rs) | Classify idempotent official-provider attempts into acknowledge, retry, dead-letter, or permanent rejection. | Private repository `1324464349`; `main` `0278b9cc86e7ea3b11d33dd987be6689dc06aba0`; exact sealed history verified. |
| [`hypesiege/hypesiege-analytics.rs`](https://github.com/hypesiege/hypesiege-analytics.rs) | Aggregate bounded reviewed metrics from official-provider APIs with checked arithmetic. | Private repository `1324459288`; `main` `3eb8efba49bd4f932b7cc673c66b3788e3f458c1`; exact sealed history verified. |

The deterministic source manifest, generator, tests, archives, and provenance are sealed at `ORESoftware/ai-agent-coordinator.rs@5d9a0c2cb44dff607bc3953954ce4b9af08e5789`. The durable coordination card remains [hypesiege/hypesiege-monorepo#15](https://github.com/hypesiege/hypesiege-monorepo/issues/15).

## Repository publication result — August 5, 2026

Trusted-main GitHub Actions run `31045540736` authenticated as `ORESoftware`, reconstructed the exact 32-repository source fleet—888 tracked files and 30 gitlinks—and verified the combined HypeSiege/StreemPilot publication as 4/4. HypeSiege preflight found all three repositories already present from earlier attempts and preserved their repository IDs and exact `main` heads without force updates or visibility changes.

Bounded non-secret evidence is merged in [ORESoftware/k8s-cluster#1069](https://github.com/ORESoftware/k8s-cluster/pull/1069) as commit `4e9df62da54479c9f52d850c16703b5e112bb282`. Artifact `8946360080`, `den-2328-encrypted-exact-gaps-31045540736`, has SHA-256 `c87ff38d687d81def5c419297dc28445d6cf659ef1d262c3c02d6b4a18ed99ec`.

The final evidence states:

```text
created=1 preserved_exact=3 verified=4 failures=0
```

The one newly created repository in the final run was the StreemPilot media router; these three HypeSiege repositories were preserved exactly. Earlier attempts that created the HypeSiege repositories failed only during post-push GitHub propagation checks, and later direct verification established that every repository was private on `main` at its reviewed sealed SHA.

## Repository-local follow-up sequence

For each published repository:

1. Open a focused follow-up branch and PR; do not rewrite the sealed bootstrap commit.
2. Add canonical Project/Linear routing and repository-specific operational ownership.
3. Run locked Cargo metadata, rustfmt, strict Clippy, all-target tests, architecture tests, and startup probes already defined by the starter.
4. Add observability, security, retry, and idempotency tests with each behavioral change.
5. Merge only the exact green reviewed head.
6. Add or update the exact child commit as a monorepo gitlink in a separate PR.
7. Update DEN-1757, DEN-2328, this document, issue #15, and the GitHub Project item with exact evidence.

## GitHub Project update contract

Keep these durable inputs linked to [HypeSiege Project 1](https://github.com/orgs/hypesiege/projects/1):

- `hypesiege/.github#2` — organization routing card;
- `hypesiege/hypesiege-monorepo#15` — Wave 2 publication and artifact card;
- follow-up PRs in scheduler, publishing-worker, and analytics;
- DEN-2328 and the merged evidence PR.

Use fields `Workstream`, `Repository`, `Linear ID`, `Status`, `Priority`, `Release gate`, `Blocked by`, and `Evidence`. The current connector does not expose Projects v2 item mutation; the issues, PRs, and this ledger are the stable board-ready inputs for a Projects-capable GitHub App or authenticated `gh project` runner.

## Credential boundary

The protected GitHub App path failed before mutation because no repository-admin App ID/private-key pair was present. The successful publication therefore used an exceptional one-time RSA-OAEP handoff bound to one Actions run and issue. Exactly one ciphertext was accepted; the decrypted PAT was immediately masked, held only in a mode-0600 runner-temporary file, and destroyed with the keypair and payload in an unconditional cleanup step. No plaintext credential entered source, workflow configuration, artifacts, issue text, PR text, logs, or Linear. Permanent organization administration should use reviewed, least-privilege GitHub App installation tokens. Any PAT pasted into chat must be revoked or rotated.

## Merge and evidence requirements

A PR is merge-ready only when:

- it is based on the current default branch;
- all conflicts are resolved semantically;
- the exact head has passed every required workflow;
- no unresolved review thread or changes-requested review remains;
- generated artifacts are content-addressed and reproducible;
- credentials and private payloads are excluded from source, logs, reports, and artifacts; and
- Linear and organization documentation identify the exact head, workflow run, artifact digest, and merge commit.
