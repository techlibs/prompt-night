# Worktree-as-Enabler Slide — Design

**Date:** 2026-05-19
**Author:** Carlos Libardo
**Status:** Approved, awaiting implementation plan
**Target deck:** `deck/index-v2.html`

## Problem

The deck mentions `git worktree` four times but never frames it as the prerequisite for multi-agent work:

- L44 — passing mention ("three worktrees") in intro paragraph
- L171–191 — slide `04a` covers *how* to spawn parallel agents (subagents vs agent teams) but skips the filesystem isolation problem
- L205–206 — slide `04b` lists worktrees as one of three "cost dials" alongside model split and token compression, framing it as an optimization rather than an enabler
- L329 — ASCII diagram on a later slide shows worktree boxes without explanation
- L602 — pitfalls list: "Parallel agents stomping shared files. Worktrees fixed it."

The audience hears about parallel agents (slide `04a`) and then immediately jumps to cost knobs (slide `04b`) without seeing the filesystem collision problem that makes parallelism non-trivial. Worktrees are buried as a cost optimization when they are actually the unlock.

## Goal

Insert a new slide between `04a` and `04b` that frames `git worktree` as the prerequisite for multi-agent work. Structure: problem first, solution second.

**Audience takeaway:** "No worktree, no parallel."

## Slide Spec

### Position

New slide `04a.5`, inserted after the closing `</section>` of slide `04a` (currently ending around L192) and before the opening `<section>` of slide `04b` (currently L195).

### Atmosphere

`data-atmo="teal-dark"` — matches `04a` so the orchestration arc reads as one continuous chapter before `04b` opens the dials chapter on `slate-steel`.

### Header

- Eyebrow: `Orchestration · 04a.5  ·  the filesystem problem`
- Title: `One repo.` / `*Three agents. Boom.*` (italic second line matches deck title pattern with `<span class="pt">.</span>`)

### Body Layout

Top-to-bottom flow on one slide:

1. **ASCII collision diagram** — 3 agents → 1 working tree → collision indicators (e.g., `package.json` conflict, dirty index, branch thrash). Use the existing `.dag` class so it matches the diagram on L329.

2. **One-line pivot** — small muted text:
   > Same physics as threads sharing memory. Need isolation.

3. **ASCII fix diagram** — 3 agents → 3 `git worktree add` → 3 isolated checkouts → 1 shared `.git` history. Reuse the visual vocabulary from L329 (three `worktree` boxes converging into one repo).

4. **Code strip** — minimal commands the audience can copy:
   ```bash
   git worktree add ../agent-a feature/sdd-mode
   git worktree add ../agent-b feature/tdd-mode
   git worktree add ../agent-c feature/yolo-mode
   ```

5. **Muted footer** — single sentence:
   > Same repo, N filesystems. Merge later, in one place.

### Demotion in slide `04b`

Slide `04b` (L194–214) currently has three cards: Model split, Worktrees, Token compression. Replace the Worktrees card with a new dial so the slide does not duplicate content that is now elevated in `04a.5`.

**Replacement card candidate:** `Context budget` — when to compact, when to start fresh, what to keep in `MEMORY.md`. Keeps the "three dials" framing intact and pairs naturally with the other two cost levers.

Slide `04b`'s footer sentence should also be reviewed for any worktree reference that no longer fits.

### Other references — leave as-is

- L44 intro mention: keep, it now foreshadows `04a.5`
- L329 ASCII diagram: keep, it now reads as a payoff for a concept the audience already understands
- L602 pitfalls callout: keep, it reinforces the lesson

## Non-Goals

- No restructure of slide numbering past `04a.5` (no renaming of `04b` → `04c` etc. unless the build script requires it)
- No new content about merge strategies, rebasing, or worktree cleanup — out of scope for a 3-minute slide
- No code changes to the deck's CSS / theme — reuse existing classes (`card`, `dag`, `cols`, `eyebrow`, `pt`, `muted`)
- No screenshots — ASCII only, matches deck style

## Acceptance Criteria

1. New `<section data-atmo="teal-dark">` exists between current slides `04a` and `04b` in `deck/index-v2.html`
2. Slide title reads `One repo.` / `Three agents. Boom.` with the deck's italic-period pattern
3. Both ASCII diagrams use the `.dag` class and render without horizontal scroll on a 1280px viewport
4. Code block uses `language-bash` and shows three `git worktree add` commands
5. Slide `04b`'s Worktrees card is replaced; the three-card layout is preserved
6. Speaker notes (if a separate file exists for the v2 deck) are updated to cover the new slide
7. Deck still passes whatever lint / build step the repo uses (check `package.json` / build script)

## Open Questions for Implementation

- Does the deck have a slide-numbering build step that auto-renumbers, or are the `04a` / `04b` labels hand-written? (Check before inserting.)
- Is there a separate speaker-notes file for `index-v2.html`? The repo has `RESUME-PROMPT.md` and earlier speaker-notes specs — confirm scope.
- The replacement dial card on `04b` (`Context budget` is the proposed candidate) should be confirmed against the talk's existing narrative — there may be a better fit.

## Out of Scope

- Live demo of `git worktree`
- Coverage of `git worktree prune`, `--detach`, or bare repos
- Discussion of `jj`, `sapling`, or alternative VCS approaches
- Cost numbers for parallel worktree-based agent runs (belongs on `04b`)
