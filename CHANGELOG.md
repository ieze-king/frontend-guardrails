# Changelog

All notable changes to this plugin are documented here.
The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project aims to follow [Semantic Versioning](https://semver.org/).

## [0.1.1] - 2026-06-07

### Fixed
- `.gitignore` updated to `**/.DS_Store` to cover nested subdirectories.
- Removed internal author note accidentally left in `references/ecosystem.md`.
- Clarified `claude plugin validate .` in README to specify it must be run from the repository root.

### Added
- `references/svelte.md` — full Svelte/SvelteKit reference covering stores, SvelteKit data loading, `{@html}` XSS risk, `PUBLIC_` env var exposure, `onDestroy` cleanup, `{#each}` keying, and `use:enhance`.
- `"svelte"` added to `plugin.json` keywords for marketplace discoverability.
- `SKILL.md` updated to route Svelte/SvelteKit projects to the new reference.

### Changed
- `references/ecosystem.md`: replaced unnamed "Karpathy-inspired" behavioral skill with the concrete built-in `/code-review` skill; replaced unnamed "Vercel-guidelines audit skill" with `ui-ux-pro-max` including its install command.

---

## [0.1.0] - 2026-06-07

Initial release.

### Added
- `frontend-guardrails` skill with 16 principles plus four behavioral layers
  (scale rigor to the project, defer to the user/project, hard rules vs.
  sensible defaults, and surface-the-trade-off).
- Self-audit checklist run before any task is considered done.
- Framework references for React/Next.js, Vue/Nuxt, and Angular.
- Topic references for security, notifications/feedback, accessibility, and
  the ecosystem (mature tools/skills to use instead of reinventing).
- Bundled Context7 MCP for current, version-accurate library documentation.
- MIT license (no-warranty) and full README.

### Scope
- Client-side frontend only. Deployment, CI/CD, and server/database concerns
  are intentionally out of scope.
