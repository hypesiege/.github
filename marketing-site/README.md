# HypeSiege marketing site

Complete Astro source staged for the future public repository `hypesiege/hypesiege.github.io` and URL `https://hypesiege.github.io/`.

## Canonical planning

- Linear project: `github.com/hypesiege`
- GitHub Project: [hypesiege project #1](https://github.com/orgs/hypesiege/projects/1)
- Official clients: [hypesiege-clients](https://github.com/hypesiege/hypesiege-clients)
- Organization: [hypesiege](https://github.com/hypesiege)

## Client truth

`hypesiege-clients` provides TypeScript, Dart, and Rust surfaces for accounts, posts, scheduling, and queue operations. The marketing page uses those operation families and preserves the distinction between content intent, approval, scheduled execution, account credentials, and delivery outcomes.

## Publish

1. Create public repository `hypesiege.github.io` in the `hypesiege` organization.
2. Copy this directory to the new repository root.
3. Run `npm install && npm run build`.
4. Add the standard Astro GitHub Pages workflow and enable GitHub Actions as the Pages source.
5. Verify `https://hypesiege.github.io/` and update the linked GitHub and Linear tickets.
