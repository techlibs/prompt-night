# Prompt Night n24 — Knowledge transfer map

Companion doc to `2026-05-18-prompt-night-deck-design.md`. Derived from
parallel agent reviews (tone, storytelling, learning outcomes). The deck
HTML at `deck/index-v2.html` carries inline `<aside class="notes">` for
each slide; this file holds the meta — what the talk is *for*.

## The one big idea

You are not a faster typist with AI — you are a conductor of a small
fleet of disciplined agents, and the discipline (specs, tests, skills,
receipts) is what makes the fleet ship instead of flail.

## The three sub-ideas

1. **Specs and failing tests are not overhead — they are the prompt.**
   A 90-second markdown spec buys hours of unattended agent work; a red
   test is proof the agent understood you.
2. **Orchestration beats raw model power.** One conductor + N specialists
   in worktrees, coordinated by a daemon, on a VM you can poke from your
   phone — that's the unlock, not a smarter model.
3. **The patterns are portable; the CLI is a costume.** Skills, specs,
   plans are all markdown — Superpowers / GSD / OpenSpec / Spec Kit are
   dialects of the same grammar, and Claude Code / Codex / Cursor /
   OpenCode are interchangeable runtimes.

## Walk-out outcomes

### Builders track (laptops out, tonight)

1. Run `npx superpowers init` in a repo, write a 5-bullet acceptance
   spec, and tell the agent to commit a failing test before the fix.
2. Clone `vlad`, provision a GCP VM, and mosh into a tmux session from
   their phone via Tailscale.
3. Spawn 3 worktrees on the same product spec and run three frameworks
   (Spec Kit / GSD / Superpowers) in parallel — the ipe-city pattern.

### Observers track (watching now, building later)

1. Explain to a teammate why a markdown spec + red test *is* the prompt,
   not the preamble.
2. Recognize the conductor-plus-specialists DAG when they see it (and
   name worktrees as the fix for shared-file stomping).
3. Name four interchangeable frameworks (Superpowers / GSD / OpenSpec /
   Spec Kit) and assert they run on any CLI — so the choice is
   reversible.

## Recall test (24 hours after the talk)

> "Carlos from Shippit showed how he runs multi-agent workflows from his
> phone — there's this thing called vlad that spins up a GCP VM and
> vlad-cockpit that's a phone-friendly dashboard over a bunch of parallel
> Claude sessions, triggered by cron and GitHub webhooks. The whole
> pitch was that specs and failing tests *are* the prompt, and
> orchestration matters more than which model you pick. He proved it by
> building the same web3 app (ipe-city, on Base mainnet) three times in
> parallel with three different frameworks — Spec Kit, GSD, Superpowers
> — and all three shipped. Takeaway: pick any framework, any CLI, but
> commit to spec-first + TDD + a conductor pattern, or you're just
> typing fast."

## Honest gaps (likely "but what about" questions)

1. **"What does this cost?"** — No token spend, GCP VM bill, Claude
   API/Max pricing, or ipe-city total in the deck. Answer prepared in
   speaker notes (under $1/day with mocked RPC; e2-small VM); honest
   answer for total spend: ~$280 across three branches.
2. **"How do I review what the swarm produced?"** — vlad-cockpit hints
   at it (PR triage, diff review on phone) but no concrete pattern is
   shown. Default answer: PR-per-task, GitHub mobile, ntfy click-through.
3. **"Is this safe?"** — Webhook-triggered agents auto-deploying to
   mainnet sounds terrifying. The trust gradient is in speaker notes
   (mainnet = one-tap human approve; Sepolia = fully autonomous; OAuth
   allow-list + HMAC-signed deep links + 15-min token expiry).

Honorable-mention questions: "what about Cursor's background agents /
Devin / Replit Agent — why roll your own?" and "does this work for
non-greenfield code?"

## Opening script (~90s, before slide 1)

> "Boa noite. Quick orientation before I start the clock. There are
> two ways to be here tonight. If you brought a laptop, you're on the
> builder track — tables at the back, Shippit crew is parked there,
> and you're going to push a commit before you leave. If you didn't,
> you're an observer, and that's fine — your job is to steal
> everything you see. I'm Carlos. I run Shippit. We build production AI
> in 11 days for companies that don't have time for a six-month
> roadmap. I've been shipping AI to production every week for the last
> 18 months, and somewhere in there I stopped typing the code myself.
> 20 minutes. One question I want you holding the whole time: *what
> would you build tonight if your laptop kept working after you closed
> it?* Hold that. Here we go."

*Lights dim. Walk to stage left. Hit the clicker.*

## Closing script (~100s, post-fade, before Q&A)

> "Quick framing before questions. Three rules for the floor: keep it
> short so more people get a turn; push back hard if I said something
> wrong; and if your question is 'how would you build X' — even
> better, because half the room probably wants to build the same thing.
> I'll stay on this stage for the next 10 minutes, then I'm moving to
> the builder tables in the back. Anything we don't finish here, we
> finish there with a keyboard. Floor's open — who's first?"

*Walk one step toward the front row. Hold the open-palm stance. Wait.
Do not fill silence — the first question always comes within seven
seconds.*

## Delivery contingencies

- **Running long at slide 10:** compress slide 6 (orchestration DAG)
  to 50s and slide 13 (frameworks) to 60s — both conceptually familiar
  to this audience.
- **Never compress:** slide 9 (vlad-cockpit climax) or slide 10 (E2E).
  Load-bearing emotional beats.
- **Cold-open fallback** if the room isn't web3-fluent: swap slide 1
  headline to "Last month I shipped the same product three times. Two
  are on Base mainnet."
- **Demo screen dies:** the slides are static markdown — open the
  speaker-notes window (`s` key) and read from there. Speaker notes
  carry every beat the slides imply.
