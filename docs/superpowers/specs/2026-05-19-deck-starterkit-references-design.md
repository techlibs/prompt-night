# Deck — Starter Kit References Section

**Date:** 2026-05-19
**File touched:** `deck/index-v2.html`
**Scope:** Insert 4 new reveal.js sections between current slide 22 (`What broke / What stuck`, line ~1019) and current slide 23 (`CTA — Open the laptop`, line ~1022). CTA + Thanks renumber to 27/28.

## Goal

Give the audience a categorized, skim-fast resource list — a startkit — for skills, web3, orchestration, design, and SDD frameworks. Frame everything as drop-in, with the explicit reminder: *ask AI to find alternatives that fit your stack.*

## Slide-by-slide

### Slide N1 (new 23) — Skill Creator concept
- **`data-atmo="teal-dark"`**
- **Eyebrow:** `Anatomy lesson · skill-creator`
- **H3:** `Skills are markdown. <em>Anyone can write one.<span class="pt">.</span></em>`
- **Body (cols):**
  - Card A — *What is a skill*: frontmatter `name` + `description` = index the agent memorizes. Body loads on demand. Portable: Claude Code, Cursor, Codex, Gemini CLI.
  - Card B — *How you write one*: invoke `skill-creator`. Describe the trigger. Paste reference docs. Agent scaffolds folder, fills frontmatter, writes the body.
- **Link row:** `github.com/anthropics/skills` · `skill-creator`
- **Closer (muted):** *"Every link on the next slides is one prompt away from being a local skill."*
- **Notes:** 60s. Beat 1 = frontmatter is the index. Beat 2 = lazy-loaded body keeps context cheap. Beat 3 = `skill-creator` is itself a skill — recursion is the point.

### Slide N2 (new 24) — Web3 shelf
- **`data-atmo="green-forest"`**
- **Eyebrow:** `Drop-in skills · web3 / solidity`
- **H3:** `Your agent stops <em>hallucinating ABIs.<span class="pt">.</span></em>`
- **Body (6 rows, mono font, link-styled):**
  - `ethskills.com` » 24 markdown skills · corrects stale ETH training data
  - `OpenZeppelin/openzeppelin-skills` » contract patterns from the OG
  - `OpenZeppelin/openzeppelin-mcp` » NL → contract via MCP
  - `coinbase/agentkit` » agent → onchain actions, any framework
  - `bwb-tokenization/.claude/skills/*` » solidity-auditor, token-integration-analyzer
  - same shelf: dos-griefing, external-call-safety, signature-replay, input-arithmetic
- **Notes:** 70s. Beat 1 = ethskills proves the pattern: a website is the skill, agents `curl` it. Beat 2 = OZ skills + MCP = no more half-baked ERC scaffolds. Beat 3 = bwb shelf is what we used to audit ipê-city — six skills, real findings.

### Slide N3 (new 25) — Orchestration + Design shelf
- **`data-atmo="warm-ember"`**
- **Eyebrow:** `Swap the dialect · keep the pattern`
- **H3:** `Same loop. <em>Different paint.<span class="pt">.</span></em>`
- **Body (cols):**
  - Card A — **Orchestration alternative**: `mastra.ai` » TS-first agent framework. Alternative to vlad-cockpit / Claude Agent SDK when the conductor lives in Node, not CLI.
  - Card B — **Design bridge**: `figma · dev-mode-mcp` » Figma file → component code via MCP. Bridges `/gsp` brand contract to designer-owned source of truth.
- **Notes:** 60s. Beat 1 = orchestration is a slot — pick the framework that matches your runtime. Beat 2 = Figma MCP closes the designer↔code loop without re-typing tokens.

### Slide N4 (new 26) — Frameworks recap + startkit closer
- **`data-atmo="slate-steel"`**
- **Eyebrow:** `Receipts · all the links`
- **H3:** `Steal everything. <em>Then ask AI for more.<span class="pt">.</span></em>`
- **Body (link table):**
  - `github.com/github/spec-kit` · Spec Kit
  - `github.com/obra/superpowers` · Superpowers
  - `github.com/gsd-build/gsd-2` · GSD (`npx gsd-pi`)
  - `github.com/Fission-AI/OpenSpec` · OpenSpec
  - `npx get-shit-pretty` · /gsp
  - `shippit.app` · Simply Shippit
- **Closer (large, italic):** *"This is a startkit. Anything not here — your agent can find a fitter alternative in one prompt."*
- **Notes:** 60s. Beat 1 = the four frameworks are dialects of the same physics. Beat 2 = gsp + shippit = the Brazilian opinionated stack. Beat 3 = the punchline of the whole section: agents find skills better than humans find skills. Ask them.

## URL verification (2026-05-19)

| URL | HTTP |
|---|---|
| github.com/anthropics/skills | 200 |
| github.com/OpenZeppelin/openzeppelin-skills | 200 |
| github.com/OpenZeppelin/openzeppelin-mcp | 200 |
| github.com/coinbase/agentkit | 200 |
| mastra.ai | 200 |
| github.com/github/spec-kit | 200 |
| github.com/obra/superpowers | 200 |
| ethskills.com | 200 |
| github.com/Fission-AI/OpenSpec | 200 |
| openspec.dev | 200 |
| github.com/gsd-build/gsd-2 | 200 |
| shippit.app | 200 |
| figma.com/blog/introducing-figmas-dev-mode-mcp-server/ | 200 |

## Constraints / non-goals

- No edits to slides 1–22 or CTA/Thanks (other than implicit renumbering).
- No JS / theme.css changes.
- Slide density: name » one-line » URL. No code snippets, no install lines except where URL is `npx ...`.
- All speaker-notes follow existing pattern (opening line, beats, transition, time, if-asked, physical cue).

## Testing

1. Open `deck/index-v2.html` in browser.
2. Step from slide 22 → N1 → N2 → N3 → N4 → CTA.
3. Verify: speaker notes panel renders, links clickable in `?print-pdf` / presentation mode, no overflow at 16:9.
4. Confirm slide count in footer = 28.

## Risks

- New section dilutes momentum between honest-debrief (22) and CTA (was 23). Mitigation: keep each new slide ≤ 60–70s. Total added ≈ 4–5 min — acceptable in 20-min slot if other slides hold time.
- Links break later. Mitigation: spec includes URL table dated 2026-05-19; presenter re-verifies day-of.
