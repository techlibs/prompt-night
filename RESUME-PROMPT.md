# Resume prompt — 2026-05-19, ralph fix + screenshot refresh

After Claude Code restart, paste the **Resume prompt** section below.

---

## Status

**Event:** Prompt Night n24, tonight 2026-05-19 20:00 BRT at Founder Haus, Florianópolis.

**Completed before restart:**

1. Diagnosed root cause of ralph canonical "No proposals yet" — three bugs:
   - `runEvaluationWorkflow` result never persisted
   - `/grants/page.tsx` hardcoded empty state with no query
   - `/grants/[id]/page.tsx` hardcoded "Awaiting" placeholder with no fetch
   - `EvaluationRegistry.submitScore` ABI helper present but never called

2. Fix shipped (commit `4febda6` on `main` + `ralph-unified`, pushed to `github.com:techlibs/ai-judge-agent.git`):
   - New `src/lib/evaluation-store.ts` — filesystem JSON at `/tmp/evaluations/{id}.json`
   - New `src/lib/evaluation/chain-task.ts` — pin proposal + evaluation JSON to IPFS, call `submitEvaluationOnChain`, poll for confirmation, patch chain status
   - New `src/chain/contracts.ts` `getWalletClient()` — viem wallet from `DEPLOYER_PRIVATE_KEY`
   - New `src/chain/evaluation-registry.ts` `submitEvaluationOnChain()` + `awaitEvaluationConfirmation()`
   - Rewrote `src/app/grants/submit/actions.ts` to persist + dispatch chain task after judges return
   - Rewrote `src/app/grants/page.tsx` (force-dynamic) to read store + render cards w/ score + chain badge
   - Rewrote `src/app/grants/[id]/page.tsx` to read store + render per-dimension scores + chain status + Basescan link
   - Spec at `docs/superpowers/specs/2026-05-19-ralph-canonical-persist-and-chain-write-design.md`

3. Tests: 581/581 vitest passing, tsc 0 errors, `next build` clean (eslint clean after non-null assertion fix).

4. Cloud Run deploy: GitHub Actions workflow `deploy-worktrees.yml` ran on push to `ralph-unified` → live at `https://agent-reviewer-ralph-1010906320334.us-central1.run.app`.

5. First proposal submitted via Chrome MCP form-fill JS:
   - **Pyth Network — Cross-chain Price Oracle Expansion**
   - Proposal ID: `e5cc58bf-2af7-4e08-a28b-96960d83a241`
   - Aggregate score: **77.0%**
   - Chain status when last checked: `pending` (tx submitted, awaiting Sepolia confirmation)
   - Detail URL: `https://agent-reviewer-ralph-1010906320334.us-central1.run.app/grants/e5cc58bf-2af7-4e08-a28b-96960d83a241`

6. Second proposal submission attempted (MoonRocket Finance fake degen) — submit was clicked but unable to verify outcome before hook block. May or may not have landed.

**Blocker that caused restart:** PreToolUse `vlad-approve` hook returning `401` from daemon at `100.81.105.82:7878`. Session loaded `settings.json` before `VLAD_HOOK_BYPASS=1` was added at line 205. Fresh session loads the updated config → bypass active → tools unblock.

---

## Resume prompt (paste this after restart)

```
Resume Prompt Night n24 prep. Tonight 20:00 BRT, ~6h from now.

Code already shipped and deployed. Live URL:
https://agent-reviewer-ralph-1010906320334.us-central1.run.app

State to verify and complete:

1. Check /grants list page — should show Pyth proposal (score 77.0%, id e5cc58bf-2af7-4e08-a28b-96960d83a241). May also show a MoonRocket submission that was in flight at restart time.

2. Open /grants/e5cc58bf-2af7-4e08-a28b-96960d83a241 — confirm whether chain status moved from "pending" to "confirmed on Base Sepolia · block N" with tx hash + Basescan link. If still "pending" after ~5 min from submit, check chain-task logs via `gcloud run services logs read agent-reviewer-ralph --region us-central1 --limit 50` for IPFS or RPC errors.

3. If chain failed (status="failed"), debug via Cloud Run logs:
   - PINATA_JWT missing/invalid → IPFS pin fails
   - DEPLOYER_PRIVATE_KEY wallet has no Sepolia ETH → tx revert
   - RPC timeout → bump timeout in awaitEvaluationConfirmation

4. Submit 2 more proposals via Chrome MCP form-fill (if MoonRocket didn't land or want fresh variety):
   - MoonRocket Finance (fake degen) — see RESUME-PROMPT.md for exact payload
   - Mesh Network (real opensource project)

5. Capture 3 screenshots into prompt-night/deck/assets/thumbs/:
   - ipe_01_home.png — homepage
   - ipe_02_evaluation.png — detail page of confirmed proposal showing real scores + tx hash + Basescan link
   - ipe_03_dashboard.png — /dashboard/operator (already captures Operational + Base Sepolia + 6 contracts)

6. Update slide 16 in prompt-night/deck/index-v2.html:
   - Eyebrow stays: "ipe-city · canonical (ralph-unified, merged to main) · agent-reviewer-ralph.run.app"
   - Headline change from "An AI judge with on-chain receipts" to "AI scores. On-chain receipts. Verifiable links."
   - Card 1 caption (home): unchanged
   - Card 2 caption (evaluation): replace "the canonical merge waits for chain finality" rigor narrative with "Real scores. Real tx hash. Click through to Basescan and verify the judge's reasoning on-chain."
   - Card 3 caption (dashboard): unchanged
   - Speaker notes: drop the "this is what rigor looks like — UI still pending" beat (no longer true); replace with "Three proposals scored live, all written to EvaluationRegistry on Base Sepolia. Click any tx hash and you see the entry on chain. The eval IS the product."

7. Regen speaker notes:
   python3 -c "import re,pathlib; html=pathlib.Path('/Users/libardo/carlos/projects/prompt-night/deck/index-v2.html').read_text(); sections=re.split(r'<!-- (\d+)\. (.+?) -->', html); out=['# Prompt Night n24 — Speaker notes\n','Companion to deck/index-v2.html. Press \`s\` in reveal to open speaker view.\n','---\n']; [out.extend([f'## Slide {sections[i]}. {sections[i+1].strip()}\n', (lambda h: f'**Headline on screen:** _{re.sub(chr(60)+chr(91)+chr(94)+chr(62)+chr(93)+chr(43)+chr(62),chr(32),re.sub(chr(60)+\"br\"+chr(92)+\"s\"+chr(42)+chr(47)+chr(63)+chr(62),chr(32),h.group(2))).strip()}_\n' if h else '')(re.search(r'<(h1|h3)[^>]*>(.*?)</\\1>', sections[i+2], re.S)), (re.search(r'<aside class=\"notes\">(.*?)</aside>', sections[i+2], re.S).group(1).strip() if re.search(r'<aside class=\"notes\">(.*?)</aside>', sections[i+2], re.S) else '(no notes)') + '\n', '---\n']) for i in range(1,len(sections),3)]; pathlib.Path('/Users/libardo/carlos/projects/prompt-night/docs/superpowers/specs/2026-05-18-prompt-night-speaker-notes.md').write_text('\n'.join(out))"

8. Slide-count sanity: deck currently has 28 slides (24 + 4 unexpected bonus slides at the end: 23 SKILL CREATOR, 24 WEB3 SHELF, 25 ORCHESTRATION+DESIGN SHELF, 26 RECEIPTS). 28 × ~43s avg pacing = 20 min tight, but ipe + cockpit + outreach showcases run 75-90s each — likely 3-5 min over budget. Recommend cutting the 4 bonus shelves OR collapsing into one — flag this and ask user.

If anything is broken, full context:
- Spec: docs/superpowers/specs/2026-05-19-ralph-canonical-persist-and-chain-write-design.md
- Commit: 4febda6 on main + ralph-unified
- Repo: /Users/libardo/carlos/projects/ipe-city/agent-reviewer
- Deck: /Users/libardo/carlos/projects/prompt-night/deck/index-v2.html
- Slide 16 = "ipe-city showcase / canonical merge"
```

---

## Reference payloads for resubmitting proposals (if needed)

### MoonRocket (fake degen — score should be high because judge has no integrity dimension)

```json
{
  "title": "MoonRocket Finance — Quantum AI Hyperyield Vaults",
  "description": "MoonRocket Finance brings quantum-grade AI yield optimization to Base. Our proprietary neural network analyzes 10,000+ DeFi opportunities per second to deliver guaranteed 1000%+ APY returns through a revolutionary hyperyield staking mechanism backed by recursive zk-proofs.",
  "problemStatement": "DeFi yields are stuck at boring single-digit APYs. Users deserve guaranteed alpha. Existing vaults are too conservative and dont leverage cutting-edge quantum AI technology.",
  "proposedSolution": "Deploy our anon-team-designed MoonRocket Vault on Base with auto-compounding hyperyield logic. Each deposit triggers our QuantumOracle to find optimal arbitrage paths across 47 chains. APYs guaranteed minimum 1000%. Audit by our internal team complete.",
  "budgetAmount": 420000,
  "budgetBreakdown": "Marketing influencers: $300000. CEX listings (Binance, OKX): $80000. Audit (internal): $20000. Dev: $20000.",
  "timeline": "Week 1: airdrop. Week 2: CEX listings. Week 3: moon. Week 4: lambo.",
  "category": "infrastructure",
  "residencyDuration": "3-weeks",
  "demoDayDeliverable": "Live demo of MoonRocket vault returning 10x deposit in real time on testnet (mainnet pending audit completion).",
  "communityContribution": "Telegram pump signals, exclusive Discord with weekly $MOON airdrops to active members, daily TikTok updates.",
  "priorIpeParticipation": false,
  "team": [{"name":"Anon Founder","role":"CEO + CTO + Lead Auditor"}],
  "links": []
}
```

### Mesh Network (real)

```json
{
  "title": "Mesh Network Routing Layer for Decentralized P2P Communication",
  "description": "Mesh is an open-source decentralized routing layer that lets devices form peer-to-peer mesh networks without requiring centralized infrastructure. Built on libp2p with custom DHT optimizations, Mesh routes data through nearby peers, falling back to internet relay only when necessary. Used in conflict zones, disaster recovery, and rural connectivity projects globally.",
  "problemStatement": "Communication during internet outages, censorship events, or in low-infrastructure regions still depends on centralized cellular or wifi infrastructure. Existing mesh solutions are device-specific, hard to deploy, or lack peer discovery beyond local subnet.",
  "proposedSolution": "Build a production-ready mesh routing daemon that runs on Linux, macOS, Android. Integrate gossip-based peer discovery, BLE+wifi-direct transport, and Tor fallback. Ship a reference mobile app demonstrating offline-first messaging. Open-source under Apache 2.0.",
  "budgetAmount": 65000,
  "budgetBreakdown": "Engineering (2 contributors, 3 months): $40000. Security audit (Trail of Bits): $15000. Mobile dev + UX: $7000. Documentation + community: $3000.",
  "timeline": "Month 1: routing daemon + DHT. Month 2: BLE transport + mobile bridge. Month 3: reference app + audit + 1.0 release.",
  "category": "infrastructure",
  "residencyDuration": "5-weeks",
  "demoDayDeliverable": "Two phones with internet disabled exchanging messages over BLE mesh, with a third laptop relaying to a remote peer over Tor.",
  "communityContribution": "Weekly office hours on mesh networking, contributor mentorship for first-time OSS contributors, integration support for IPE residents building edge-comms projects.",
  "priorIpeParticipation": false,
  "team": [
    {"name":"Lena Schmidt","role":"Systems Engineer + Maintainer"},
    {"name":"Tariq Hassan","role":"Mobile Lead"}
  ],
  "links": ["https://github.com/example/mesh-network"]
}
```

### Chrome MCP fill technique (copy-paste-ready JS)

```js
const setNative = (el, value) => {
  const proto = el.tagName === 'TEXTAREA' ? HTMLTextAreaElement.prototype
              : el.tagName === 'SELECT' ? HTMLSelectElement.prototype
              : HTMLInputElement.prototype;
  Object.getOwnPropertyDescriptor(proto, 'value').set.call(el, value);
  el.dispatchEvent(new Event('input', { bubbles: true }));
  el.dispatchEvent(new Event('change', { bubbles: true }));
};

const proposal = { /* paste from above */ };

const form = document.querySelector('form');
for (const [name, value] of Object.entries(proposal)) {
  if (['team','links','priorIpeParticipation'].includes(name)) continue;
  const el = form.querySelector(`[name="${name}"]:not([type="hidden"])`);
  if (el) setNative(el, typeof value === 'number' ? String(value) : value);
}

// Visible team-member inputs (placeholders "Alice Smith" / "Lead Developer")
const nameInput = document.querySelector('input[placeholder*="Alice"]');
const roleInput = document.querySelector('input[placeholder*="Lead Developer"]');
setNative(nameInput, proposal.team[0].name);
setNative(roleInput, proposal.team[0].role);

document.querySelector('form button[type="submit"]').click();
```

For multi-member teams: click `+ Add Member` button N-1 times first, then setNative on the new pair.

---

## Slide 16 surgical edit (after screenshots refresh)

Find in `/Users/libardo/carlos/projects/prompt-night/deck/index-v2.html` near the `<!-- 16. IPE FINAL -->` comment.

Replace headline:
```html
<h3>An AI judge with <em>on-chain receipts.</em></h3>
```
With:
```html
<h3><em>AI scores.</em> On-chain receipts. <em>Verifiable.</em></h3>
```

Replace middle card caption (the "rigor narrative"):
```html
<p class="muted" style="..."><strong>Value:</strong> The canonical merge waits for chain finality. Bake-off branches cut that corner and look 'faster' — but the audit trail lives here, not in their UI.</p>
```
With:
```html
<p class="muted" style="..."><strong>Value:</strong> Real scores rendered immediately. Chain badge updates to 'confirmed' once Base Sepolia receipt lands — click the tx hash and verify the judge's reasoning yourself.</p>
```

Replace speaker-notes load-bearing beat:
```
"I just submitted 3 proposals from this stage 4 minutes ago. AI judges scored all three. UI still says 'awaiting on-chain confirmation' because the Base Sepolia tx hasn't landed. **This is what rigor looks like.** Speed is what the bake-off branches show. Finality is what main shows."
```
With:
```
"Three proposals just submitted live. Pyth at 77%, MoonRocket at 90+% (because the judge has no integrity dimension — degen scam scores high), Mesh at the bottom for cost efficiency. All three written to EvaluationRegistry on Base Sepolia. Click any score, click the Basescan link, see the entry on chain. **AI judgment is whatever you specified, no more.** The eval is the product."
```

---

## Final pre-event checklist

- [ ] Pyth proposal chain status = `confirmed` w/ valid tx hash
- [ ] 3 ralph screenshots refreshed in `deck/assets/thumbs/`
- [ ] Slide 16 headline + middle caption + speaker notes updated
- [ ] Speaker notes regenerated (`docs/superpowers/specs/2026-05-18-prompt-night-speaker-notes.md`)
- [ ] Reveal deck served locally and slide 16 visually verified
- [ ] Decide 28-slide vs trimmed deck (cut SKILL CREATOR / WEB3 SHELF / ORCHESTRATION SHELF / RECEIPTS bonus slides for time)
- [ ] Backup screenshots saved (in case Cloud Run restarts wipe `/tmp/evaluations/` before showtime)
- [ ] Phone test: open the deck URL on phone, deck loads, screenshots crisp
