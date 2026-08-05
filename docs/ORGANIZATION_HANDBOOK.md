# hypesiege organization handbook

> Shared operating defaults for repositories maintained under **hypesiege**. Repository-local policy may strengthen these rules but should not silently weaken them.

## Mission

hypesiege maintains social publishing, campaign orchestration, scheduling, audience-engagement, and provider-integration software. This `.github` repository is the canonical home for shared policy, reusable templates, community health files, and planning links.

## Repository contract

Each active repository must document purpose, ownership, maturity, supported providers and platforms, development and test commands, authoritative content and scheduling interfaces, release and rollback procedures, compatibility policy, and GitHub Project/Linear links. Publishing components should also document authorization and consent, provider limits, scheduling semantics, idempotency, retries, drafts and approvals, moderation, analytics attribution, retention, deletion, and degraded modes.

## Change workflow

1. Anchor work in an issue, Linear item, or documented maintenance objective.
2. Keep branches and pull requests focused.
3. Explain motivation, scope, account and audience impact, abuse risk, validation, compatibility, migration, and rollback.
4. Test permission denial, draft/approval, duplicate, schedule drift, rate limit, retry, partial publish, deletion, and provider failure paths as relevant.
5. Resolve conflicts semantically by reconstructing both sides' intent.
6. Prefer squash merges for focused work unless commit structure materially improves auditability.

## Evidence, security, and documentation

Pull requests should include reproducible commands, synthetic content fixtures, expected and observed provider behavior, negative-path coverage, documentation updates, and CI or local-equivalent results. Never commit credentials, provider tokens, customer content, production identities, or sensitive logs. Follow `SECURITY.md` for private reporting. Keep authorization, review gates, limits, moderation, compatibility, and important privacy and operational decisions explicit.

## Planning ownership

GitHub owns code, reviews, checks, releases, and delivery evidence. Linear owns priority, dependencies, sequencing, and cross-project planning. The organization GitHub Project is the cross-repository execution view; see `PROJECTS.md` for routing details.

## Organization health

- [ ] Profiles, descriptions, topics, and READMEs are current.
- [ ] Community health files and reusable issue/PR guidance are present.
- [ ] Authorization, approvals, scheduling, retries, provider limits, moderation, retention, and deletion are documented.
- [ ] Required checks cover duplicate/partial publish, provider failure, privacy, compatibility, and supply-chain risk.
- [ ] Stale repositories are archived or clearly marked.
- [ ] GitHub Project and Linear links resolve and reflect completed work.
