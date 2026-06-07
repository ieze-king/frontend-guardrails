# Frontend Guardrails

A Claude skill that makes AI-generated frontend code **consistent, reusable, secure, and production-quality by default** — without you having to ask for it every time.

Framework-agnostic: React, Vue, Angular, Svelte, and plain HTML/CSS/JS. Works whether you're scaffolding a brand-new project or extending an existing one.

---

## Why this exists

AI coding assistants are capable, but left to their own defaults they drift. Ask for "a login page" ten times and you get ten slightly different structures, colors, and patterns. The same logic ends up copy-pasted in three files. Secrets leak into the browser. A component balloons to 500 lines. The result *looks* fine and quietly isn't.

The fix isn't a smarter model — it's a **standing set of defaults** the model reads before it writes anything. This skill is that set. It encodes the discipline of a careful senior frontend engineer so that "do it right" is the default, not a thing you have to remember to prompt for.

Crucially, it's designed to **help you regardless of experience**:

- **Beginners / non-coders** get professional output they couldn't have specified themselves — and the skill scales its rigor down so it never buries a small project in enterprise machinery.
- **Experienced developers** get consistent, disciplined output across sessions and teammates — and the skill *defers* to their conventions and judgment instead of bulldozing them.

---

## How it works (read this once)

A skill is **not** a command you run. There's no `/frontend-guardrails`. Once it's installed and enabled, Claude consults it automatically whenever a task involves frontend work. You just build normally; the standards apply in the background.

It's structured for efficiency using **progressive disclosure**:

- **`SKILL.md`** — the lean core (the principles and a self-audit checklist). Loaded only when the skill triggers on frontend work.
- **`references/`** — deeper detail loaded *only when the relevant task comes up*: framework specifics (`react`, `vue`, `angular`), plus `security`, `notifications`, `accessibility`, and `ecosystem`. You never pay the token cost for detail you aren't using.

So a non-frontend chat costs almost nothing; a frontend task loads the core and, at most, the one or two references it actually needs.

---

## The problems it solves

Each item below is a real pattern AI assistants (and rushed humans) fall into, and what the skill does about it.

**Duplicated code — "one bug, five fixes."** AI re-writes the same markup or logic in several places, so a later fix has to be made in all of them. → The skill requires reusing or extracting components, hooks, and constants: write it once.

**Do-everything components.** Logic, presentation, and data-fetching get tangled into one giant component that can't be reused or tested. → It enforces small, single-responsibility components with separated concerns and a clear data layer.

**Inconsistent styling.** One color here, a slightly different one there; mismatched fonts and spacing — the hallmark of unpolished work. → It bans hardcoded values and requires design tokens, so every color/font/spacing has one source of truth.

**Breaks on other screens.** UI built for one screen size that falls apart on mobile. → It requires mobile-first design against defined breakpoints and verification at multiple widths.

**Missing polish.** No page title, no favicon in the tab, blank screens while data loads. → It requires meaningful titles, favicons, meta tags, and proper loading/empty states.

**Messy state and navigation.** Prop-drilling, everything dumped into global state, duplicated state that falls out of sync. → It enforces state at the right level, deriving values instead of duplicating them, and one consistent routing setup.

**Backend logic leaking into the frontend.** Database calls, business rules, or secrets ending up in client code. → It enforces a strict boundary: the frontend talks to the backend only through a defined API layer.

**Insecure defaults.** Secrets shipped to the browser, trusting the frontend to enforce auth, over-fetching user data, unsanitized HTML (XSS). → It applies the security fundamentals — no client-side secrets, backend-enforced authorization, least data exposure, no unsanitized HTML — with full detail in `references/security.md`.

**Risky dependencies.** Pulling in bleeding-edge, abandoned, or unvetted packages. → It requires stable, supported (LTS where available) versions, and vetting plus vulnerability scanning before adding anything.

**Slow pages.** Huge bundles, unoptimized images, thousands of un-virtualized rows. → It requires code-splitting, image optimization, and pagination/virtualization for long lists.

**Inaccessible UI.** Div-soup with no keyboard support and meaning conveyed by color alone. → It requires semantic HTML, full keyboard operability, sufficient contrast, and labeled fields — detail in `references/accessibility.md`.

**Poor feedback on failure.** Blank or frozen screens when something goes wrong, and every message crammed into a generic bottom toast. → It requires handling the unhappy paths and matching each message to the right vehicle (inline vs toast vs banner), with detail in `references/notifications.md`.

**Weak typing.** `any` everywhere; untyped API responses that blow up at runtime. → In TypeScript projects it requires typed props and responses and bans casual `any`.

**Invisible to search.** Content rendered only on the client, with no meta or structured data, so crawlers can't see it. → It requires per-route titles/meta, structured data, and crawlable (SSR/pre-rendered) content where discoverability matters.

**Stale documentation.** Docs written once and never updated, which then mislead. → It requires keeping the README and (for Claude Code) `CLAUDE.md` current as part of finishing meaningful changes.

**AI-specific failure modes.** Charging ahead on wrong assumptions, over-engineering, editing unrelated code, inventing APIs, and handing over code that doesn't compile or is littered with `console.log`s. → It encodes behavioral discipline: check assumptions, build only what's asked, touch only relevant code, use real APIs, and verify the code builds and typechecks (and is free of debug junk) before calling it done.

---

## Design principles (what makes it more than a checklist)

- **Scale rigor to the project.** It applies the fundamentals everywhere, but does *not* drag CI, tests, TypeScript, or tooling into a project that doesn't use them. Small stays small.
- **Defer to you and your project.** Your instructions and existing conventions always win. The principles are baselines, not mandates; a deliberate, reasoned exception is not a violation.
- **Hard rules vs sensible defaults.** Security and not-shipping-broken-code are non-negotiable; everything stylistic (toast placement, font count, SSR vs SSG) yields to the project.
- **Surface the trade-off, then let you decide.** When a request heads toward a real risk, it names the risk concretely, suggests the cleaner approach, and asks — rather than silently obeying or silently overriding.
- **Compose, don't reinvent.** Where a mature tool or skill already does the job (linters, scanners, accessibility auditors, Context7, code review), it points you there instead of duplicating it. See `references/ecosystem.md`.

---

## Scope

**In scope:** everything client-side — structure, styling, state, accessibility, performance, frontend security, and code correctness (it builds and typechecks).

**Out of scope (on purpose):** deployment, CI/CD, git hooks, servers, and databases. Those are DevOps concerns. This skill stays on the client.

It's also a deliberately tight baseline — it leaves out things like i18n, error monitoring, and visual-regression testing so it stays lean and composable. Add those per project when you need them.

---

## Getting started

First, a one-time prerequisite on Claude.ai: open **Settings → Capabilities** and make sure **Code Execution and File Creation** is enabled. Skills won't run without it.

### Option A — Claude.ai (simplest, best for beginners)

1. Download/clone this repository.
2. Inside it, find the folder `skills/frontend-guardrails` (it contains `SKILL.md` and a `references/` folder) and **zip that folder**.
3. In Claude.ai, go to **Customize → Skills**, click **+ / Create skill**, and upload the ZIP.
4. Toggle the skill **on**.
5. That's it — just start building. Ask Claude to build or change any frontend, and the standards apply automatically. No command needed.

(Note: the Claude.ai upload route uses only the skill itself. The bundled Context7 documentation tool comes with the Claude Code route below.)

### Option B — Claude Code (for developers)

Register this repo as a marketplace and install it:

```
/plugin marketplace add <your-github-username>/frontend-guardrails
/plugin install frontend-guardrails@frontend-guardrails
```

Or, for personal use, copy the `skills/frontend-guardrails` folder into `~/.claude/skills/` (or `.claude/skills/` for a single project), then restart Claude Code.

Once installed, it activates automatically on frontend tasks. You can also mention it by name to nudge it explicitly. Installing via the plugin route also sets up the bundled **Context7** MCP, which feeds Claude current, version-accurate library documentation (no API key needed for basic use; add a free key from context7.com for higher rate limits).

Before publishing your own copy, validate the manifests from the repo root with `claude plugin validate .`

---

## Updating

Updates are **pull-based** — installing a newer version is something you do, not something that happens automatically. Claude Code does not notify you in-app when a new version is published.

To get the latest version (Claude Code):

```
/plugin marketplace update frontend-guardrails
/plugin install frontend-guardrails@frontend-guardrails
```

Then run `/reload-plugins` (or restart Claude Code) to apply it.

To be told when there's a new version, open this repository on GitHub and choose **Watch → Custom → Releases**. GitHub will email you whenever a new release is published — that's the closest thing to an update notification. Releases and their changes are listed in [`CHANGELOG.md`](./CHANGELOG.md).

Claude.ai upload users: re-download the latest `skills/frontend-guardrails` folder, re-zip it, and re-upload it under Customize → Skills.

---

## Repository structure

```
frontend-guardrails/
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest
│   └── marketplace.json     # marketplace catalog (lists this plugin)
├── .mcp.json                # bundles the Context7 MCP
├── skills/
│   └── frontend-guardrails/
│       ├── SKILL.md         # the lean core
│       └── references/      # on-demand depth (frameworks, security, a11y, etc.)
├── README.md
└── LICENSE
```

## Contributing

This is open source — contributions welcome. Good first contributions:

- Additional framework references (Svelte, SolidJS, etc.) following the pattern in `references/`.
- Improvements to existing principles, with a short rationale.
- Fixes to keep the `ecosystem.md` tool/skill references current.

Please keep the core `SKILL.md` lean — depth belongs in `references/`. Open an issue or a pull request with the problem you're solving and why.

---

## A note on responsibility

This skill **recommends; it does not warrant.** It points at reputable, verifiable tools and leaves security and adoption decisions to you and to the dedicated tools built for them (linters, scanners, code review). It is provided as-is, without warranty — see `LICENSE`.
