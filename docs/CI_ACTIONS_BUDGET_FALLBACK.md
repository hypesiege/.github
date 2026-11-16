# Alternate GitHub Actions runner fallback (private repositories)

Use this procedure **only after verifying that the source organization cannot
admit Actions jobs because of minutes/budget**. A test failure, queued runner,
code defect, or dependency audit finding is **not** an Actions-budget shortage.

## Security and admission preconditions

1. Confirm source commit SHA, branch, tree SHA, and whether the repository is
   private. Record the actual Actions billing/admission error.
2. Confirm the destination organization has available Actions capacity and that
   a specifically authorized destination repository is **private**. Moving a
   private Git tree across organizations discloses source code to members and
   administrators of the destination. Never mirror to a public or unknown repo.
3. Do not copy credentials, `.env`, private keys, deployment configuration
   containing secrets, or GitHub Actions secrets. Review the source tree and
   workflows for credential assumptions before mirroring.
4. Do not use cross-organization checkout with a source PAT, app token, deploy
   key, or a forwarded `GITHUB_TOKEN`. The destination's own Actions token
   only accesses the destination repository.
5. Only copy commits and blobs into an approved, temporary isolated branch,
   using GitHub connector writes. Do not push, rewrite, or merge into the
   original source repository. Never force-push another agent's branch.

## Mirror and validate

1. Read the exact source commit and its recursive Git tree through GitHub.
   Reject truncated trees and unresolved or unavailable objects.
2. Reconstruct its tracked-file tree in the authorized private test repository
   through GitHub connector tree/commit operations. Preserve file mode,
   executable bits and symlink entries; reject submodules or inaccessible
   objects until explicitly handled. GitHub REST tree creation is not a
   byte-for-byte source mirror merely because filenames look similar.
3. Verify every source path/mode/blob SHA and verify the resulting tree SHA
   matches the source tree, or document any explicitly approved, intentional
   test-only differences separately. Reject unexpected differences.
4. Run the exact testing workflow on the mirrored **destination commit** with
   its own token and minutes. Use Rust test and audit utilities; never introduce
   Python scripts for this migration. Record both the source SHA and destination
   SHA and provide an authenticated SHA-to-SHA verification receipt.
5. Require the same substantive CI assertions (unit, negative, integration,
   supply-chain and runtime checks). A mirror pass does not override failing
   source security gates, branch protections, reviews or approvals.
6. Preserve mirror branch references and logs for review. Do not delete test
   branches or evidence automatically; cleanup needs a separately authorized,
   non-destructive retention procedure.

## Reporting

Report the source commit, its expected tree SHA, private destination repository
and branch, reconstructed tree SHA, per-blob verification result, destination
Actions run IDs, passed/failed/skipped checks, and any differences in runtime
environment. Say explicitly when the fallback was **not used** because source
Actions were running normally.

This is an operational fallback, **not an authorization to expose private
sources in arbitrary organizations** or to bypass failing policy checks.
