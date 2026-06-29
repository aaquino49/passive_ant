# Passive Ant — UX, Copy & Tuning Packet (Phases 0, 1, 2, 4, 7)

The connective-tissue deliverables Claude owns outside the big tables: the one-page loop brief, naming, tab/button copy, empty states, training tutorial copy, and the "next action" recommendation rules.

**Canonical source:** [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md) owns starting state, the resource economy (production/upkeep rates), chambers, units, formations, and wireframes. This file does not redefine those numbers — it supplies the UX copy and behavioral rules that sit on top. See also [CONQUEST_COMBAT_PACKET.md](CONQUEST_COMBAT_PACKET.md).

Copy budget: button ≤14, card title ≤18, line ≤42, toast ≤48.

---

## 1. One-Page Design Brief — The Core Loop (Phase 0)

**Fantasy:** You are the Queen. A handful of ants becomes an empire by digging chambers, raising soldiers, and conquering the garden one patch at a time. Cozy to manage, dramatic to fight.

**The loop, five verbs:**

| Verb | Tab | What the player does | Payoff |
|---|---|---|---|
| **Build** | Build | Dig chambers into 6 slots | More caps, growth, unlocks |
| **Train** | Army | Spend food + adults on units | A stronger army |
| **Battle** | Campaign → Battle | Pick a target, watch 3 rounds | Win or learn |
| **Expand** | Campaign | Claim the won territory | Economy bonus + new land |
| **Unlock** | Build / Army | New chambers & units open | Deeper choices, next tier |

**Loop spiral:** Build economy → Train army → Battle territory → Expand (claim reward) → Unlock new chamber/unit → Build again, bigger. Chitin (the combat-tied resource) gates the deeper chambers, so fighting is required to keep building.

**Session shape:** 30–90s active check-ins, optional 3–5 min sessions (matches design packet Product Target). Offline advances production / construction / training / recovery — never battles. A returning player sees progress, taps one recommended action, and is back in the loop in seconds.

**Tone:** warm address ("Queen", "the colony", "the brood"), light drama in combat ("The field is yours"). No jargon, no ninja, no gore.

---

## 2. Naming Guidance (locked) (Phase 0)

| Use this | Not this | Where |
|---|---|---|
| Found New Colony | Nuptial Flight | Prestige action / Settings |
| Campaign | Map / Conquest | Tab + expansion |
| Chambers | Buildings / Upgrades | Build tab |
| Adults | Workers (when meaning trainable pop) | Army training |
| Larvae (count/label) | Brood/eggs as a *number* label | Nest / Nursery counters |
| Wounded | Casualties / Dead | Post-battle |

Resource display names: **Food, Materials, Chitin, Pheromones, Population**. State keys (per design packet): `resources.food/materials/chitin/pheromones`, `population.adults/larvae`.

---

## 3. Tab Labels & Button Copy (Phase 1)

**Bottom tabs (4):** `Nest` · `Build` · `Army` · `Campaign`. Settings = gear icon in the resource bar (per design packet wireframe), not a tab.

| Context | Label (≤14) | Disabled label |
|---|---|---|
| Build a chamber | Build | Can't afford |
| Choose chamber | Choose | Locked |
| Upgrade chamber | Upgrade | Maxed |
| Train a unit | Train | Locked |
| Cancel queue item | Cancel | — |
| Set formation | Set Formation | — |
| Open battle preview | To Battle | Not ready |
| Start the fight | Fight! | — |
| Claim reward | Claim Reward | — |
| Return | Back to Nest | — |
| Recover wounded | Recover | None hurt |
| Found prestige | Found Colony | Not yet |

**Settings menu items:** Reduced Motion · Haptics · Sound · Reset Tutorial · Found New Colony · Hard Reset.

---

## 4. Empty-State Copy (Phase 1)

One short centered line + the highlighted action below it.

| Tab / panel | Empty-state line (≤42) | Sub-action |
|---|---|---|
| Build — no chamber selected | "No chamber here yet." | "Choose a chamber" |
| Build — slot locked | "Claim land to dig here." | (none) |
| Army — no army trained | "No soldiers yet." | "Train your first ant" |
| Campaign — no target | "No campaign target selected." | "Pick a territory" |
| Construction done banner | "Construction complete." | "See your new chamber" |
| Training queue empty | "Training idle." | "Raise a soldier" |
| Wounded panel empty | "All ants are healthy." | (none) |

---

## 5. Economy Behavior Notes (Phase 2)

**Rates are canonical in design packet §Resource Economy** (Forager +0.35 Food/s, Excavator +0.22 Mat/s, Nurse +0.006 Larvae/s, Scout Tunnel +0.015 Phero/s/lvl, Chitin Forge +0.025 Chitin/s/lvl; upkeep = adults×0.035 + army×0.012 + recovering×0.01 Food/s). This section only clarifies behavior built on those rates.

**Multiplier stacking** (territory rewards are additive %):
`effectiveRate = baseRate × (1 + Σ territoryBonuses) × (1 + Σ chamberBonuses)`

**Worked example from the canonical starting state** (Materials 35, 6 excavators → +1.32 Mat/s):
- Granary (20 Mat) affordable instantly; leaves 15, refills in ~4s.
- Barracks (30 Mat) reachable ~12s after Granary — the first economy-vs-military beat lands inside the tutorial with no grant needed.
- Net Food at start: +0.35×8 foragers = +2.8/s gross, − upkeep (18 adults×0.035 = 0.63 + small army) ≈ **+2.0/s net** → never negative in the tutorial. ✅

**Caps** (design packet): Food 120, Materials 80, Chitin 40, Pheromones 25; Granary +80 Food cap/lvl, Store +60 Mat cap/lvl.

**Recovery** (design packet §Wounded Recovery): 20s + 3 Food per wounded unit; wounded don't fight; defeat can convert wounded → permanent loss.

---

## 6. Training Tutorial Copy (Phase 4)

Army-tab micro-copy, used during onboarding and as first-time hints.

| Trigger | Coach line (≤48) |
|---|---|
| First open of Army tab | "These are your soldiers. Raise them here." |
| Tap a unit row | "Each ant costs food and one adult." |
| Train tapped (first time) | "An adult becomes a soldier. Watch it hatch." |
| Training timer running | "Soldiers take time. The nest keeps working." |
| First unit completes | "A soldier joins the ranks. Train more!" |
| Locked unit tapped | "Build the right chamber to unlock this." |
| First formation view | "Formations decide how your army fights." |
| Readiness shows Dangerous | "Too few for this foe. Train, then attack." |
| Readiness shows Safe | "Your army is ready. To the Campaign!" |

**Unit-row role labels (short, under each unit):** Major → "Armor breaker" · Swarmer → "Fast melee" · Spitter → "Acid range" · Scout → "Intel + support".

---

## 7. "Next Recommended Action" Rules (Phase 7)

The Nest tab shows one recommendation. Evaluate top-down; first match wins. Each has an exact line + the tab it deep-links to.

| Priority | Condition | Recommendation line (≤42) | Goes to |
|---:|---|---|---|
| 1 | Wounded ≥ 30% of army AND no Battle Ready target | "Heal your wounded before the next fight." | Nest (recover) |
| 2 | Construction slot idle AND an affordable chamber exists | "Dig a new chamber." | Build |
| 3 | Selected territory readiness = Safe | "Your army is ready — attack!" | Campaign |
| 4 | Adults ≥1 AND an affordable unit exists AND readiness < Safe | "Train more soldiers." | Army |
| 5 | A territory is Available but unscouted AND Scout Tunnel built | "Scout the next territory." | Campaign |
| 6 | Any resource ≥90% of cap AND a cap chamber available | "Build storage — you're nearly full." | Build |
| 7 | (fallback) nothing actionable | "The colony hums along. Well done, Queen." | (none) |

**Nest status block (always visible, no scroll, per design packet wireframe):** resource bar · construction timer or "Idle" · training timer or "Idle" · production rates (Food/Mat per sec) · territory bonus summary · the recommendation above.

---

## 8. Label Clarity Pass (Phase 7)

- All labels fit ≤14 chars → verified against 360px (no wrap on tab bar or primary buttons).
- "Found New Colony" used for prestige in UI; asset filename `scene_nuptial_flight.png` stays, label does not.
- Numbers abbreviate at 1,000+ → "1.2k", "3.4M" (carry over from `UI_Requirements_6-27.md`).
- Timers display `mm:ss` under 1h; rates display `+0.35/s` style.
- No UI string exceeds the toast budget (≤48); anything longer splits into title + body or a tap-to-expand detail.
- Use "Larvae" for any UI *count/label* (matches the `population.larvae` state key). "The brood" is fine as flavor prose (the design packet uses it), just never as the numeric label.
