# Passive Ant — Mobile QA & Handoff Packet (Sprint D / Phase 8)

Claude's QA deliverables: per-milestone acceptance checklists, the bug-prioritization framework, the first-3-minutes UX rubric, the tone rubric, and the screenshot-review checklist. Use these to sign off the redesign once Codex's build is testable on device/emulator.

Pairs with [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md), [CONQUEST_COMBAT_PACKET.md](CONQUEST_COMBAT_PACKET.md), [CONQUEST_UX_COPY_PACKET.md](CONQUEST_UX_COPY_PACKET.md).

**Relationship to the design packet:** the design packet's own §Mobile QA Checklist and §Implementation Tickets are the canonical release gate. The per-phase checklists below *expand* that flat list into phase-scoped, testable boxes — they don't replace it. Ticket 7 ("Copy and Tuning Review", owner Claude) is executed across these three Claude packets.

Test viewports: **360x740**, **390x844**, plus **420x900** as headroom.

---

## 1. Acceptance Checklists by Milestone

Each box is a single testable pass/fail. Cross-reference the plan's per-phase acceptance criteria.

### Phase 0 — Alignment & hygiene
- [ ] `index.pre-conquest-backup.html` exists and opens.
- [ ] `saveVersion: 2` written on new saves.
- [ ] Old localStorage either migrates or offers "Start New Conquest Save".
- [ ] All `assets/` references load (no 404 in console).

### Phase 1 — Mobile shell
- [ ] Four bottom tabs render: Nest / Build / Army / Campaign.
- [ ] Top resource bar shows Food, Materials, Chitin, Pheromones, Population.
- [ ] Settings reachable via gear, not crowding tabs.
- [ ] Every primary control ≥44px (measure on 360px).
- [ ] No primary tab scrolls vertically at 360x740.
- [ ] Reduced motion / haptics / reset settings preserved.

### Phase 2 — State model
- [ ] Fresh save initializes from design packet §Starting State (`STARTING_STATE`), zero console errors.
- [ ] Existing save migrates per design packet §Save Migration Policy, OR the fresh-save fallback toast fires.
- [ ] Production rates derive from assigned workers + chambers + territory % (design packet §Resource Economy).
- [ ] Net Food stays positive at the starting state (≈ +2.0/s) — no negative-food soft-lock.

### Phase 3 — Chambers
- [ ] 6 slots in 2x3; correct states (Empty/Building/Complete/Upgrade/Locked).
- [ ] Empty slot → picker; existing → details + upgrade.
- [ ] One active construction; completes automatically.
- [ ] Timer persists after refresh.
- [ ] Offline advances construction, never triggers combat.
- [ ] Each chamber effect alters the right cap/rate/unlock.
- [ ] Completion message matches design-packet §2.

### Phase 4 — Army & formations
- [ ] Rows for Major/Swarmer/Spitter/Scout with icon, count, wounded, role, train, cost, time.
- [ ] Training consumes adults + resources; queue works.
- [ ] Unit unlocks respect chamber state (locked until chamber built).
- [ ] Casualties split 70% wounded / 30% permanent (50% permanent on Defeat), per design packet §Casualties.
- [ ] Wounded recover over time (20s + 3 Food each); recovering units don't fight.
- [ ] All four formations selectable per unlock rules; preview updates.
- [ ] Readiness shows Safe/Risky/Dangerous correctly vs. selected target.
- [ ] Training timers advance offline safely.

### Phase 5 — Campaign
- [ ] Six territories render with name/state/enemy/reward/unlocks/readiness/battle.
- [ ] Claiming unlocks adjacent territories per combat-packet §2.
- [ ] Rewards affect economy or unlock chambers.
- [ ] Scout Tunnel toggles enemy-lane reveal vs. "??".
- [ ] No combat starts offline.

### Phase 6 — Combat
- [ ] Battle preview fits mobile; resolves in 3 rounds with no taps.
- [ ] Each round maps to a lane; round log streams from combat-packet §4.
- [ ] All four outcomes reachable with correct result copy (§5).
- [ ] Results update army, resources, territory state.
- [ ] Reduced motion removes heavy shake/flash.
- [ ] Offline elapsed time never yields a battle result or loss.

### Phase 7 — Nest & economy
- [ ] Nest shows resources, construction timer, training timer, rates, territory bonus, recommendation.
- [ ] Recommendation follows UX-packet §7 priority order.
- [ ] Top visual area reflects the selected tab's scene.
- [ ] No key status hidden in a scroll area.

---

## 2. Bug Prioritization Framework

**Severity definitions:**

| Tier | Definition | Examples |
|---|---|---|
| **P0** | Blocks the core loop, corrupts saves, or breaks on a target viewport. Ship-stopper. | Battle never resolves; save wipes on refresh; tab unreachable at 360px; offline triggers combat losses. |
| **P1** | Loop works but a step is confusing, mistuned, or visually broken. Fix before release. | Readiness badge wrong; chamber effect not applied; text overflow; timer resets on refresh; tutorial step skips. |
| **P2** | Polish, cosmetic, or edge-case. Fix if time allows. | Log line repeats too often; icon slightly misaligned; toast timing; minor copy tweak. |

**Bug report template (one row each):**

| ID | Tier | Viewport | Area | Steps to reproduce | Expected | Actual | Asset/screenshot |
|---|---|---|---|---|---|---|---|

**Triage rule:** any failure of a Phase acceptance box above is at least **P1**; any failure that touches save safety or offline-combat safety is **P0** by default.

---

## 3. First-3-Minutes UX Rubric

Score each; all must pass for sign-off.

| Check | Pass condition |
|---|---|
| Comprehension | A new player can state "build, train, fight" after the tutorial without help. |
| Flow | Tutorial completes in ≤3:30 with no dead-wait > 22s (longest timer). |
| Friction | Every prompt has exactly one obvious tap target. |
| Payoff | Player completes ≥1 chamber and ≥1 unit before the tutorial ends. |
| Freedom | Tutorial never forces a battle; ends by handing control over. |
| Recoverability | Skipping the tutorial leaves all tabs usable and no soft-lock. |
| Resource sanity | Net Food never goes negative during the tutorial. |

---

## 4. Tone & Theme Review Rubric

| Check | Pass condition |
|---|---|
| Ant identity | Copy reads ant colony, not ninja — no shuriken/clan/sensei language. |
| Warmth | Player addressed as Queen; colony framed as "yours", "the brood". |
| Drama (combat only) | Battle copy heightens ("The field is yours!") without grimdark/gore. |
| Brevity | No UI string exceeds its budget (button 14 / title 18 / line 42 / toast 48). |
| Consistency | Locked naming used everywhere (Chambers, Campaign, Found New Colony). |
| Clarity | No jargon; a non-gamer understands every label. |

---

## 5. Screenshot Review Checklist

Run once per viewport (360x740, 390x844) on each tab + battle. For each screenshot, confirm:

- [ ] No vertical scroll on primary content.
- [ ] No text clipped, wrapped awkwardly, or overflowing its container.
- [ ] All touch targets visibly ≥44px and not overlapping.
- [ ] Resource bar fully visible, numbers not truncated.
- [ ] Active timers readable; recommendation legible.
- [ ] Selected-tab scene present in top visual area; sprites readable at size.
- [ ] Icons distinguishable from each other (no ambiguous pair).
- [ ] Contrast sufficient — text legible over panel textures.

**Per-screenshot finding format:** `[viewport] [tab] — [P-tier] — issue → suggested fix`. Feed P0/P1 findings into the §2 bug table; route asset issues (unclear icon, busy bg, baked-in text) to Gemini with the exact filename.

---

## 6. Final Handoff Checklist (gate to "complete")

- [ ] Four mobile tabs exist and fit without primary scrolling.
- [ ] Chamber construction works with timers and offline progress.
- [ ] Unit training works with timers and unlocks.
- [ ] Campaign territories unlock through victories.
- [ ] Battles are three-round auto-resolving army fights.
- [ ] Battle results update army, resources, and territory state.
- [ ] Offline progress never triggers battle damage.
- [ ] Existing assets are used from `assets/`.
- [ ] Gemini-only asset requests recorded with filenames + prompts.
- [ ] 360x740 and 390x844 checks pass.
- [ ] End-to-end: build a chamber → train units → win a battle → claim territory → unlock a new chamber, all on mobile browser.
