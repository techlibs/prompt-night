# Prompt Night n24 — Deck Design

**Event:** AI Prompt Nights n24 — Multi-Agent Workflows & Web3 Vibe Coding
**Host venue:** Founder Haus, Jurerê Internacional, Florianópolis
**Presenter:** Carlos Libardo — Shippit
**Date authored:** 2026-05-18
**Source URL:** https://luma.com/eoums7t5

## Goal

Single-file HTML deck for a 20-min talk (24 slides) covering SDD, TDD, agent skills,
multi-agent orchestration, real-world orchestration via vlad + vlad-cockpit,
E2E testing, and the four canonical frameworks (Superpowers, GSD, OpenSpec,
GitHub Spec Kit). Anchored by the ipe-city case study (web3 grant-judging
product built across three framework branches and deployed on Base L2).

## Deliverables

```
prompt-night/
├── deck/
│   ├── index.html          # reveal.js deck, fully self-contained (CDN deps)
│   ├── theme.css           # Shippit dark theme override
│   └── assets/             # logos, screenshots
└── docs/superpowers/specs/2026-05-18-prompt-night-deck-design.md
```

Open by double-click → loads reveal.js + highlight.js + Google Fonts from CDN.
No build step. Press `s` for speaker view (notes live in `<aside class="notes">`).

## Tech stack

- **reveal.js 5** via `cdn.jsdelivr.net` (CSS reset + JS bundle + notes plugin + highlight plugin)
- **highlight.js** dark theme for code blocks
- **Google Fonts**: Instrument Serif (headings, italic), Barlow (body), JetBrains Mono (code)
- Custom `theme.css` overrides reveal's white-base styles to match Shippit dark identity
- No JS frameworks, no bundler, no package.json required

## Brand contract (from shippit-landing)

Source: `shippit-landing/.design/branding/shippit/patterns/STYLE.md` + `src/app/globals.css`.

| Token | Value |
|---|---|
| Background | `#000000` |
| Foreground | `#FAFAFA` |
| Card surface | `#0A0A0A` |
| Muted text | `#797980` |
| Border | `#1F1F1F` |
| Accent (single dot only) | `#8B5CF6` (brand purple) |
| Heading font | Instrument Serif italic |
| Body font | Barlow |
| Mono font | JetBrains Mono |

Voice: precise, challenging, inevitable. Active voice. Contractions. Numerals for strategic numbers.
Banned: synergy, leverage, seamless, comprehensive, innovative, journey, ecosystem.

## Slide flow (24)

1. Title — *Multi-agent workflows. Any platform.* / Carlos Libardo — Shippit / Prompt Night n24.
2. The shift — single prompt → orchestrated systems.
3. Map of the night — 6 chapters.
4. **SDD** — spec-driven development. brainstorm → plan → tasks → implement.
5. SDD in action — diff vibes-prompt vs spec-driven prompt.
6. **TDD** for agents — agent writes failing test first.
7. TDD receipt — failing test + agent fix commit.
8. **Agent skills** — name + description + body, lazy-loaded.
9. Skill anatomy — frontmatter example (`superpowers:brainstorming`).
10. **Multi-agent orchestration** — one conductor, many specialists.
11. Orchestration DAG — planner → parallel researchers → synthesizer.
12. **vlad** — mobile Claude Code on GCP. mosh + tmux + Tailscale + ntfy.
13. **vlad-cockpit** — orchestration UI over N parallel sessions. Cron + GitHub webhooks → auto-spawn.
14. **E2E testing** — browser as final gate.
15. E2E demo frame — headless run + green pipeline.
16. **Frameworks** — table: Superpowers · GSD · OpenSpec · Spec Kit (philosophy / killer-move / when).
17. Platform-agnostic — Claude Code · Codex · Cursor · Windsurf · OpenCode.
18. Design engineering — GSP × Claude DS side by side (brand-as-code).
19. **ipe-city bake-off** — same product, 3 branches, 3 live URLs.
20. ipe-city journey — brief → build → audit → deploy. Base L2 contracts.
21. The receipts — 6 contract addresses, 3 URLs, 1103/992/258 files per branch.
22. What broke / what stuck — honest 2-col list (heavy speaker notes).
23. Try it tonight — 4 copy-paste commands. Builder track CTA.
24. Thanks — shippit.app · luma.

### vlad + vlad-cockpit framing

- **vlad** (`carlos/projects/vlad`) = bootstrap script for a GCP VM running Ubuntu 24.04 + Node 22 + Claude Code + tmux + mosh + Tailscale + ntfy. Mobile-first: phone (Termux) connects via mosh over Tailscale, sessions survive disconnects, push notifications via ntfy.
- **vlad-cockpit** (`carlos/projects/vlad-cockpit`) = single-user orchestration layer over many Claude sessions on the same VM. Daemon (Bun) + Next.js cockpit served on one Tailscale Funnel URL. Features: live session dashboard, tmux mirror WebSocket with "take control" toggle, file diff viewer, cron-scheduled sessions, GitHub webhook ingestion that spawns sessions from templated prompts. Closes the loop: GitHub issue labeled `bug` → spawn agent → ntfy → phone deep link → review or take over.
- Together they are the **real-world receipt** of multi-agent orchestration — the layer above the patterns shown in slides 10-11.

## Speaker notes

Inline `<aside class="notes">` per slide. Reveal's notes plugin opens a separate window on `s`.
Audience never sees them.

## Out of scope

- Live coding demos (only static screenshots / code blocks)
- Backend, analytics, telemetry
- Light mode, mobile-portrait tuning
- Gallium-ai branding (Shippit only)

## Acceptance

- Opens offline-after-first-load (CDN cached) in any modern browser.
- All 22 slides render with Shippit dark theme.
- `s` opens speaker view with notes populated.
- No external link broken at write time.
- Voice/copy passes the Shippit banned-words filter.
