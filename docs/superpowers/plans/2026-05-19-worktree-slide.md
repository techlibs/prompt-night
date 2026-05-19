# Worktree-as-Enabler Slide Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Insert a dedicated slide (`04a.5`) between slides `04a` (subagents vs agent teams) and `04b` (cost dials) that frames `git worktree` as the prerequisite for multi-agent work, and demote the worktree card in `04b` to a new dial so the deck does not duplicate the concept.

**Architecture:** Two HTML files require parallel edits: `deck/index-v2.html` (public build, no speaker notes) and `deck/speaker.html` (presenter build with `<aside class="notes">` blocks). Both files use the same Reveal.js 5.1.0 structure, the same `theme-v2.css` styles (`.card`, `.dag`, `.cols`, `.cols-3`, `.eyebrow`, `.pt`, `.muted`, `.glow`), and identical slide ordering — slide `07` is the SUBAGENTS slide, slide `08` is the DIALS slide. We insert a new `<section data-atmo="teal-dark">` between them and swap one card in the dials slide. No CSS, JS, or build changes.

**Tech Stack:** HTML, Reveal.js 5.1.0, Highlight.js for code blocks (`language-bash`), existing `theme-v2.css` classes only.

**Source spec:** `docs/superpowers/specs/2026-05-19-worktree-slide-design.md`

---

## File Structure

Files modified:
- `deck/index-v2.html` — insert new slide after line 240 (closing `</section>` of slide 07); modify line 252-255 (Worktrees card in slide 08) to swap to new dial
- `deck/speaker.html` — insert new slide after closing `</section>` of slide 07 (around line 327); modify the Worktrees card in slide 08 with same dial swap, plus add `<aside class="notes" data-markdown>` block in the new slide

No new files. No CSS edits — all classes reused.

---

## Task 1: Verify deck renders cleanly before edits

**Files:**
- Read: `deck/index-v2.html`
- Read: `deck/speaker.html`

- [ ] **Step 1: Confirm slide insertion anchors in `index-v2.html`**

Run:
```bash
grep -n "<!-- 07. SUBAGENTS\|<!-- 08. OPERATING CRAFT" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
```

Expected output: two lines, slide `07` comment around line 216, slide `08` comment around line 242. The closing `</section>` of slide 07 sits one line above the slide 08 comment. If line numbers have shifted, use the comment anchors as the source of truth — do not rely on the literal numbers below in Task 2.

- [ ] **Step 2: Confirm slide insertion anchors in `speaker.html`**

Run:
```bash
grep -n "<!-- 07. SUBAGENTS\|<!-- 08. OPERATING CRAFT" /Users/libardo/carlos/projects/prompt-night/deck/speaker.html
```

Expected output: two lines, slide `07` comment around line 283, slide `08` comment around line 328. Use these comment anchors as the source of truth for Task 3.

- [ ] **Step 3: Open the current decks in the browser to baseline visual**

Run:
```bash
cd /Users/libardo/carlos/projects/prompt-night/deck && python3 -m http.server 8765 >/dev/null 2>&1 &
sleep 1
echo "open http://localhost:8765/index-v2.html#/7 and http://localhost:8765/index-v2.html#/8"
```

Expected: terminal echoes the two URLs. Open both in a browser, screenshot slide `04a` (subagents) and slide `04b` (dials) for visual comparison after edits. Leave the server running for Task 4 and Task 5 verification.

- [ ] **Step 4: Commit nothing — verification step only**

No commit. Move to Task 2.

---

## Task 2: Insert new slide `04a.5` in `index-v2.html`

**Files:**
- Modify: `deck/index-v2.html` — insert a new `<section>` block between the closing `</section>` of slide 07 (the SUBAGENTS slide) and the `<!-- 08. OPERATING CRAFT — dials -->` comment.

- [ ] **Step 1: Locate the insertion point**

Run:
```bash
grep -n "</section>" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html | grep -B1 -A1 "242"
```

Or simply re-grep for the slide 08 comment and use the blank line directly above it as the insertion point. The exact line number may shift between sessions — use the `<!-- 08. OPERATING CRAFT — dials -->` comment as the literal anchor for the `Edit` tool.

- [ ] **Step 2: Insert the new slide using the `Edit` tool**

Use the `Edit` tool with this exact `old_string`:

```html
      </section>

      <!-- 08. OPERATING CRAFT — dials -->
```

And this exact `new_string`:

```html
      </section>

      <!-- 07b. WORKTREE — the filesystem problem -->
      <section data-atmo="teal-dark">
        <div class="grain"></div>
        <div class="eyebrow"><span class="dot"></span>Orchestration · 04a.5 &nbsp;·&nbsp; the filesystem problem</div>
        <h3>One repo.<br><em>Three agents. Boom.<span class="pt">.</span></em></h3>
        <div class="dag" style="margin:0.8em auto 0.4em;max-width:42em;">agent-a    agent-b    agent-c
   │          │          │
   └──────────┼──────────┘
              ▼
      ┌──────────────┐
      │  one repo    │
      │  one branch  │
      │  one index   │
      └──────────────┘
              │
              ▼
   package.json conflict
   dirty index · branch thrash
              💥</div>
        <p class="muted" style="font-size:0.78em;text-align:center;max-width:38em;margin:0.4em auto 0.6em;">Same physics as threads sharing memory. Need isolation.</p>
        <div class="dag" style="margin:0.2em auto 0.6em;max-width:42em;">agent-a    agent-b    agent-c
   │          │          │
   ▼          ▼          ▼
┌────────┐ ┌────────┐ ┌────────┐
│worktree│ │worktree│ │worktree│
│   a    │ │   b    │ │   c    │
└────┬───┘ └────┬───┘ └────┬───┘
     └──────────┼──────────┘
                ▼
         ┌──────────┐
         │  .git    │  one history
         └──────────┘</div>
<pre><code class="language-bash">git worktree add ../agent-a feature/sdd-mode
git worktree add ../agent-b feature/tdd-mode
git worktree add ../agent-c feature/yolo-mode</code></pre>
        <p class="muted" style="font-size:0.78em;margin-top:0.6em;max-width:42em;">Same repo, N filesystems. Merge later, in one place. No worktree, no parallel.</p>
      </section>

      <!-- 08. OPERATING CRAFT — dials -->
```

- [ ] **Step 3: Verify the file parses and the slide appears**

Run:
```bash
grep -c "<!-- 07b. WORKTREE" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
```

Expected: `1`

- [ ] **Step 4: Visual check in browser**

The dev server from Task 1 should still be running. Hard-reload `http://localhost:8765/index-v2.html` and navigate with `j` / `arrow-right` past the SUBAGENTS slide. Expected: a new slide appears with the title "One repo. / Three agents. Boom." and two ASCII diagrams stacked vertically. The teal-dark atmosphere matches the previous slide. Both diagrams fit horizontally at a 1280px viewport (no scroll).

If either diagram overflows or wraps mid-line, edit the inline `max-width` on the `.dag` div or shrink it to `.dag sm` (smaller font, matches the L329-style diagram).

- [ ] **Step 5: Commit**

```bash
cd /Users/libardo/carlos/projects/prompt-night
git add deck/index-v2.html
git commit -m "Deck: add slide 04a.5 framing worktree as multi-agent prerequisite

New slide between subagents/teams (04a) and cost dials (04b). Shows
the collision problem (one repo + N agents) then the worktree fix
(N checkouts, one .git history). Public build only — speaker.html
follows in next commit."
```

---

## Task 3: Mirror the new slide into `speaker.html` with speaker notes

**Files:**
- Modify: `deck/speaker.html` — insert the same new `<section>` plus an `<aside class="notes" data-markdown>` block.

- [ ] **Step 1: Insert the new slide using the `Edit` tool**

Use the `Edit` tool with this exact `old_string`:

```html
      </section>

      <!-- 08. OPERATING CRAFT — dials -->
```

And this exact `new_string`:

```html
      </section>

      <!-- 07b. WORKTREE — the filesystem problem -->
      <section data-atmo="teal-dark">
        <div class="grain"></div>
        <div class="eyebrow"><span class="dot"></span>Orchestration · 04a.5 &nbsp;·&nbsp; the filesystem problem</div>
        <h3>One repo.<br><em>Three agents. Boom.<span class="pt">.</span></em></h3>
        <div class="dag" style="margin:0.8em auto 0.4em;max-width:42em;">agent-a    agent-b    agent-c
   │          │          │
   └──────────┼──────────┘
              ▼
      ┌──────────────┐
      │  one repo    │
      │  one branch  │
      │  one index   │
      └──────────────┘
              │
              ▼
   package.json conflict
   dirty index · branch thrash
              💥</div>
        <p class="muted" style="font-size:0.78em;text-align:center;max-width:38em;margin:0.4em auto 0.6em;">Same physics as threads sharing memory. Need isolation.</p>
        <div class="dag" style="margin:0.2em auto 0.6em;max-width:42em;">agent-a    agent-b    agent-c
   │          │          │
   ▼          ▼          ▼
┌────────┐ ┌────────┐ ┌────────┐
│worktree│ │worktree│ │worktree│
│   a    │ │   b    │ │   c    │
└────┬───┘ └────┬───┘ └────┬───┘
     └──────────┼──────────┘
                ▼
         ┌──────────┐
         │  .git    │  one history
         └──────────┘</div>
<pre><code class="language-bash">git worktree add ../agent-a feature/sdd-mode
git worktree add ../agent-b feature/tdd-mode
git worktree add ../agent-c feature/yolo-mode</code></pre>
        <p class="muted" style="font-size:0.78em;margin-top:0.6em;max-width:42em;">Same repo, N filesystems. Merge later, in one place. No worktree, no parallel.</p>
        <aside class="notes" data-markdown>

**The pivot beat.** You just saw two ways to spawn N agents — subagents and agent teams. Both assume those agents can actually *work*. They can't, not on one checkout.

**The problem.** Three Claude instances editing the same `package.json` at the same time. Stage step from agent A racing the format-on-save from agent B. Branch checkouts stomping each other. Dirty index that nobody owns. You don't get a merge conflict — you get a *corrupted* working tree.

**The fix.** `git worktree add` gives each agent its own filesystem, sharing one `.git` directory. Three agents, three branches, three working trees, one history. Each agent commits to its own branch. You merge at the end, in one place, like a human team.

**The frame.** Same mental model as threads with shared memory: if the agents share state, you need locks; if they don't, parallelism is free. Worktrees make parallelism free.

**Land it.** "No worktree, no parallel." That's the line. Everything else in this talk — vlad, agent teams, the bake-offs — assumes this is solved.

        </aside>
      </section>

      <!-- 08. OPERATING CRAFT — dials -->
```

- [ ] **Step 2: Verify the slide and notes appear**

Run:
```bash
grep -c "<!-- 07b. WORKTREE" /Users/libardo/carlos/projects/prompt-night/deck/speaker.html
grep -c "No worktree, no parallel" /Users/libardo/carlos/projects/prompt-night/deck/speaker.html
```

Expected: `1` and `1` (the second match comes from inside the notes block; the slide body says "No worktree, no parallel." in the muted footer, plus once in the speaker notes — total 2 if the regex is greedy across newlines; if grep is line-based, expect `2` for the second command. Adjust the expectation by inspecting the file).

- [ ] **Step 3: Visual check in browser**

Open `http://localhost:8765/speaker.html#/7b` (or navigate from slide 7 with arrow-right). Expected: the same teal-dark slide as in `index-v2.html`. Press `s` to open Reveal's speaker view — the speaker notes pane should render the five bold-led paragraphs as Markdown.

- [ ] **Step 4: Commit**

```bash
cd /Users/libardo/carlos/projects/prompt-night
git add deck/speaker.html
git commit -m "Speaker deck: mirror slide 04a.5 with presenter notes

Adds the same worktree-as-enabler slide to the presenter build with
notes covering the pivot from spawn-mechanics to filesystem isolation,
plus the 'no worktree, no parallel' closing line."
```

---

## Task 4: Replace the Worktrees card in slide `04b` (`index-v2.html`)

**Files:**
- Modify: `deck/index-v2.html` — slide 08, the Worktrees card around lines 252-255.

- [ ] **Step 1: Swap the Worktrees card for a Context-budget card**

Use the `Edit` tool with this exact `old_string`:

```html
          <div class="card glow">
            <h4>Worktrees</h4>
            <p><code>git worktree add</code> per agent. Same repo, isolated checkouts. Three agents, zero merge conflicts on <code>package.json</code>.</p>
          </div>
```

And this exact `new_string`:

```html
          <div class="card glow">
            <h4>Context budget</h4>
            <p><code>/compact</code> on long sessions. <code>MEMORY.md</code> for state worth keeping. New chat when the cache window slips past 5 minutes — token cost cliff, not a curve.</p>
          </div>
```

- [ ] **Step 2: Verify the swap**

Run:
```bash
grep -A2 "<h4>Worktrees</h4>\|<h4>Context budget</h4>" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
```

Expected: a match on `<h4>Context budget</h4>` and zero matches on `<h4>Worktrees</h4>` (the only remaining `Worktrees` reference in the file should be the title bar of slide 11 if it exists — re-check below).

- [ ] **Step 3: Confirm no orphan worktree mentions in slide 08**

Run:
```bash
grep -n -i "worktree" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
```

Expected matches: line 44 (intro mention), the new slide `04a.5` (multiple lines), and the L329-area ASCII diagram on a later slide. **No match should fall between the `<!-- 08. OPERATING CRAFT` comment and its closing `</section>`.**

- [ ] **Step 4: Visual check in browser**

Hard-reload `http://localhost:8765/index-v2.html` and navigate to slide `04b` (the dials slide). Expected: three cards — Model split, Context budget, Token compression. The Worktrees card is gone. Layout still uses `cols-3`.

- [ ] **Step 5: Commit**

```bash
cd /Users/libardo/carlos/projects/prompt-night
git add deck/index-v2.html
git commit -m "Deck: swap Worktrees card on 04b for Context-budget dial

Worktree is now its own slide (04a.5) framed as a prerequisite, not
a cost optimization. Replace the dial slot with context-budget
guidance (compact, MEMORY.md, new-chat on cache miss) so the three-
dial framing stays intact."
```

---

## Task 5: Mirror the card swap into `speaker.html` and update notes

**Files:**
- Modify: `deck/speaker.html` — slide 08, same Worktrees card swap; review the existing `<aside class="notes">` block for slide 08 to ensure no orphan reference to the old card.

- [ ] **Step 1: Locate the slide-08 notes block in `speaker.html`**

Run:
```bash
awk '/<!-- 08. OPERATING CRAFT/,/<!-- 09\./' /Users/libardo/carlos/projects/prompt-night/deck/speaker.html | head -100
```

Read the output. Identify whether the existing speaker notes mention "worktree" or "Worktrees" as one of the three dials. If they do, those lines need to be rewritten to talk about Context budget.

- [ ] **Step 2: Swap the Worktrees card**

Use the `Edit` tool with this exact `old_string`:

```html
          <div class="card glow">
            <h4>Worktrees</h4>
            <p><code>git worktree add</code> per agent. Same repo, isolated checkouts. Three agents, zero merge conflicts on <code>package.json</code>.</p>
          </div>
```

And this exact `new_string`:

```html
          <div class="card glow">
            <h4>Context budget</h4>
            <p><code>/compact</code> on long sessions. <code>MEMORY.md</code> for state worth keeping. New chat when the cache window slips past 5 minutes — token cost cliff, not a curve.</p>
          </div>
```

- [ ] **Step 3: Update the slide-08 speaker notes if they reference the old card**

If Step 1 surfaced a sentence like "second dial, worktrees..." inside the slide-08 `<aside class="notes">` block, rewrite it to cover Context budget instead.

Use the `Edit` tool with a precise `old_string` that includes 2-3 lines of surrounding context from the notes block (read those lines from the file first to get them exact), and replace with a note such as:

```markdown
**Second dial — context budget.** The cache window in Claude Code is 5 minutes. Past that, every tool call re-reads your conversation uncached. `/compact` collapses old turns, `MEMORY.md` lifts the keepers out of the chat, and sometimes the right answer is "start a new chat." This is the dial people forget — it does nothing to your code, it does everything to your bill.
```

If no such reference exists (the notes were generic), skip this step.

- [ ] **Step 4: Verify**

Run:
```bash
grep -c "<h4>Context budget</h4>" /Users/libardo/carlos/projects/prompt-night/deck/speaker.html
```

Expected: `1`

Run:
```bash
awk '/<!-- 08. OPERATING CRAFT/,/<!-- 09\./' /Users/libardo/carlos/projects/prompt-night/deck/speaker.html | grep -i "worktree"
```

Expected: zero output. (All worktree references should now live in slide `04a.5`, the intro, and the later ASCII diagram on a different slide.)

- [ ] **Step 5: Visual check in browser**

Open `http://localhost:8765/speaker.html#/8`. Press `s` for speaker view. Confirm: three cards (Model split, Context budget, Token compression) on the main slide, and the speaker notes pane no longer mentions Worktrees as a dial.

- [ ] **Step 6: Commit**

```bash
cd /Users/libardo/carlos/projects/prompt-night
git add deck/speaker.html
git commit -m "Speaker deck: mirror dial swap and update slide-08 notes

Replaces the Worktrees card with Context-budget on the dials slide
and rewrites the matching beat in the speaker notes so the three-dial
talk track no longer overlaps with the new 04a.5 slide."
```

---

## Task 6: Final acceptance pass

**Files:**
- Read: `deck/index-v2.html`
- Read: `deck/speaker.html`

- [ ] **Step 1: Run all acceptance checks from the spec**

Run, one at a time, and verify each output:

```bash
# 1. New section exists in both decks
grep -c "<!-- 07b. WORKTREE" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
grep -c "<!-- 07b. WORKTREE" /Users/libardo/carlos/projects/prompt-night/deck/speaker.html
```
Expected: `1` and `1`.

```bash
# 2. Title text correct
grep -A1 "<!-- 07b. WORKTREE" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html | grep "One repo"
```
Expected: a match.

```bash
# 3. Both ASCII diagrams use .dag class
grep -c 'class="dag"' /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
```
Expected: at least `3` (two new diagrams + the existing one near the old line 329).

```bash
# 4. Code block uses language-bash
awk '/<!-- 07b. WORKTREE/,/<!-- 08\. OPERATING CRAFT/' /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html | grep "language-bash"
```
Expected: one match (the new `<pre><code class="language-bash">` block).

```bash
# 5. Worktrees card replaced in 04b
grep -A2 "<!-- 08. OPERATING CRAFT" /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html | head -50 | grep -E "Worktrees|Context budget"
```
Expected: `<h4>Context budget</h4>` appears; `<h4>Worktrees</h4>` does not.

```bash
# 6. Speaker notes block present on new slide
awk '/<!-- 07b. WORKTREE/,/<!-- 08\. OPERATING CRAFT/' /Users/libardo/carlos/projects/prompt-night/deck/speaker.html | grep '<aside class="notes"'
```
Expected: one match.

- [ ] **Step 2: Full deck walk-through in the browser**

Navigate `index-v2.html` from slide 0 to the end with `j` / right-arrow. Confirm: no rendering glitches, no slide that lost its atmosphere, no overflowing diagrams, no broken code blocks. Repeat for `speaker.html`, this time pressing `s` once and walking through the notes pane.

- [ ] **Step 3: Stop the dev server**

Run:
```bash
pkill -f "python3 -m http.server 8765" || true
```

- [ ] **Step 4: Final state check**

Run:
```bash
cd /Users/libardo/carlos/projects/prompt-night
git log --oneline -5
git status
```

Expected: four new commits on top of `1681b8a`, working tree clean.

- [ ] **Step 5: No commit — work is already committed**

End of plan.

---

## Self-Review Notes

- **Spec coverage:** Every acceptance criterion in the spec (`docs/superpowers/specs/2026-05-19-worktree-slide-design.md`) is covered: new `<section>` with `data-atmo="teal-dark"` (Task 2), title `One repo. / Three agents. Boom.` (Task 2), both ASCII diagrams use `.dag` (verified Task 6), code block uses `language-bash` (verified Task 6), Worktrees card replaced on `04b` (Task 4), speaker notes updated (Tasks 3, 5).
- **Placeholder scan:** None — every code-touching step contains the literal `old_string` and `new_string`.
- **Type consistency:** N/A (HTML only, no types).
- **Open questions resolved:**
  - Slide numbering is hand-written (no auto-renumber build step) — verified in Task 1 by reading the deck.
  - Speaker notes file is `deck/speaker.html` — covered by Tasks 3 and 5.
  - Replacement dial is `Context budget` — committed to in Tasks 4 and 5.
