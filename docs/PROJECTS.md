<!-- org-project-routing:start -->
# Project routing

- **GitHub organization:** [hypesiege](https://github.com/hypesiege)
- **Canonical GitHub Project:** [hypesiege-project](https://github.com/orgs/hypesiege/projects/1) (project 1)
- **Canonical Linear project:** [github.com/hypesiege](https://linear.app/denman/project/githubcomhypesiege-12bdb95b4116)
- **Organization documentation repository:** [hypesiege/.github](https://github.com/hypesiege/.github)
- **Durable organization routing card:** [hypesiege/.github#2](https://github.com/hypesiege/.github/issues/2)

## Source-of-truth boundaries

GitHub is authoritative for repositories, commits, pull requests, reviews, CI checks, releases, deployable artifacts, and runtime evidence. Linear is authoritative for product planning, priorities, ownership, dependencies, milestones, and status reporting. The GitHub Project is the organization-level execution board and should contain the governance issue maintained by this repository.

## Change and merge policy

Documentation branches must be reviewed through pull requests and merged after checks pass. Concurrent edits are reconciled semantically against the latest default branch: this managed routing block is regenerated while all unrelated prose outside the block is preserved. Do not resolve conflicts by blindly choosing one side.
<!-- org-project-routing:end -->

## Rust server modularization program

The cross-organization Rust-server program is tracked in Linear by [DEN-1682](https://linear.app/denman/issue/DEN-1682) and [DEN-1757](https://linear.app/denman/issue/DEN-1757). The organization-specific technical and operational ledger is [RUST_SERVER_MODULARIZATION.md](./RUST_SERVER_MODULARIZATION.md).

### Current HypeSiege state

| Unit | GitHub evidence | State |
| --- | --- | --- |
| API runtime | `hypesiege/hypesiege-api-server.rs#19` | Merged; dependency bootstrap and outbox/publish worker ownership are modularized. |
| Web runtime | `hypesiege/hypesiege-web-server.rs#8` | Merged; routes, state, security middleware, browser tests, and runtime lifecycle are modularized. |
| MCP runtime | `hypesiege/hypesiege-mcp-server.rs` merged runtime series | Merged; tool routing, domain policy, telemetry, stdio lifecycle, and OpenTelemetry hardening are separated. |
| Scheduler starter | `hypesiege-monorepo` Wave 2 artifact | Reviewed deterministic starter; target repository is not yet provisioned. |
| Publishing-worker starter | `hypesiege-monorepo` Wave 2 artifact | Reviewed deterministic starter; target repository is not yet provisioned. |
| Analytics starter | `hypesiege-monorepo` Wave 2 artifact | Reviewed deterministic starter; target repository is not yet provisioned. |

The reviewed Wave 2 starter artifact is recorded by `hypesiege/hypesiege-monorepo#15`. The repository-provisioning controller and exact issue-comment trigger are merged, but the trusted run on August 5, 2026 failed before repository creation because no repository-admin GitHub App ID/private-key pair was configured in Actions. The default repository `GITHUB_TOKEN` had read-only contents permission and was not treated as repository-admin authority.

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

The current integration cannot mutate Projects v2. Keep the durable routing card and linked implementation issues current so a Projects-capable GitHub App or authenticated `gh project` runner can add and update the corresponding board items without reconstructing state from chat history.

### Credential boundary

Do not place personal access tokens in commits, workflow inputs, issue bodies, PR descriptions, artifacts, logs, or Linear. Repository creation must use a reviewed GitHub App installation token with organization repository-administration permission, short lifetime, explicit target allowlist, non-secret evidence, token revocation, and temporary-key destruction.
