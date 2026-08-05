<!-- org-project-routing:start -->
# Project routing

- **GitHub organization:** [hypesiege](https://github.com/hypesiege)
- **Canonical GitHub Project:** [hypesiege-project](https://github.com/orgs/hypesiege/projects/1) (project 1)
- **Canonical Linear project:** [planning workspace](https://linear.app/denman/project/githubcomhypesiege-12bdb95b4116)
- **Organization documentation repository:** [hypesiege/.github](https://github.com/hypesiege/.github)

## Source-of-truth boundaries

GitHub is authoritative for repositories, commits, pull requests, reviews, CI checks, releases, deployable artifacts, and runtime evidence. Linear is authoritative for product planning, priorities, ownership, dependencies, milestones, and status reporting. The GitHub Project is the organization-level execution board and should contain the governance issue maintained by this repository.

## Change and merge policy

Documentation branches must be reviewed through pull requests and merged after checks pass. Concurrent edits are reconciled semantically against the latest default branch: this managed routing block is regenerated while all unrelated prose outside the block is preserved. Do not resolve conflicts by blindly choosing one side.
<!-- org-project-routing:end -->

## Rust server modularization program

The cross-organization Rust-server program is tracked in Linear by [DEN-1682](https://linear.app/denman/issue/DEN-1682), [DEN-1757](https://linear.app/denman/issue/DEN-1757), and the repository-publication reconciliation [DEN-2328](https://linear.app/denman/issue/DEN-2328). The organization-specific technical and operational ledger is [RUST_SERVER_MODULARIZATION.md](./RUST_SERVER_MODULARIZATION.md).

### Current HypeSiege state

| Unit | GitHub evidence | State |
| --- | --- | --- |
| API runtime | `hypesiege/hypesiege-api-server.rs#19` | Merged; dependency bootstrap and outbox/publish worker ownership are modularized. |
| Web runtime | `hypesiege/hypesiege-web-server.rs#8` | Merged; routes, state, security middleware, browser tests, and runtime lifecycle are modularized. |
| MCP runtime | `hypesiege/hypesiege-mcp-server.rs` merged runtime series | Merged; tool routing, domain policy, telemetry, stdio lifecycle, and OpenTelemetry hardening are separated. |
| Scheduler | [`hypesiege/hypesiege-scheduler.rs`](https://github.com/hypesiege/hypesiege-scheduler.rs), repository `1324464386`, `main` `e8a739d9e658e9cef8f1dc938a412b923dbff57d` | Published private; exact reviewed sealed history verified. |
| Publishing worker | [`hypesiege/hypesiege-publishing-worker.rs`](https://github.com/hypesiege/hypesiege-publishing-worker.rs), repository `1324464349`, `main` `0278b9cc86e7ea3b11d33dd987be6689dc06aba0` | Published private; exact reviewed sealed history verified. |
| Analytics | [`hypesiege/hypesiege-analytics.rs`](https://github.com/hypesiege/hypesiege-analytics.rs), repository `1324459288`, `main` `3eb8efba49bd4f932b7cc673c66b3788e3f458c1` | Published private; exact reviewed sealed history verified. |

The reviewed Wave 2 source is sealed at `ORESoftware/ai-agent-coordinator.rs@5d9a0c2cb44dff607bc3953954ce4b9af08e5789`. Trusted-main run `31045540736` reconstructed the exact 32-repository fleet, preserved these three private repositories at their reviewed heads, and verified the combined HypeSiege/StreemPilot result as 4/4. Bounded evidence is merged in `ORESoftware/k8s-cluster#1069` at `4e9df62da54479c9f52d850c16703b5e112bb282`. Artifact `8946360080` has SHA-256 `c87ff38d687d81def5c419297dc28445d6cf659ef1d262c3c02d6b4a18ed99ec`.

### GitHub Project fields

Use these organization-wide fields for the Rust program:

- **Workstream:** Runtime, API, Web, MCP, Scheduler, Publishing, Analytics, Infrastructure, E2E, Release.
- **Repository:** exact `owner/name` identity.
- **Linear ID:** DEN issue identifier.
- **Status:** Backlog, Ready, In progress, In review, Blocked, Done.
- **Priority:** Urgent, High, Normal, Low.
- **Release gate:** exact PR, check, artifact, or activation gate.
- **Blocked by:** issue, credential class, capacity lane, or repository prerequisite.
- **Evidence:** exact head SHA, workflow run, artifact digest, and merge commit.

Keep `hypesiege/.github#2`, the Wave 2 coordination issue, the three repository follow-up PRs, and DEN-2328 linked to [HypeSiege Project 1](https://github.com/orgs/hypesiege/projects/1). The current connector does not expose Projects v2 item mutation, so the durable issues and this ledger remain the board-ready source until a Projects-capable GitHub App or authenticated `gh project` runner performs the item updates.

### Credential boundary

Do not place personal access tokens, GitHub App private keys, or other credentials in commits, workflow inputs, issue bodies, PR descriptions, artifacts, logs, or Linear. The August 5 publication used a one-time run-bound RSA-OAEP handoff after the protected host proved that no repository-admin App key material was available. Exactly one ciphertext was accepted, the decrypted credential was masked and held only in a mode-0600 runner-temporary file, and all credential material was destroyed unconditionally. Permanent repository administration should use a reviewed organization GitHub App with short-lived installation tokens and exact target allowlists. Any PAT pasted into chat must be revoked or rotated.
