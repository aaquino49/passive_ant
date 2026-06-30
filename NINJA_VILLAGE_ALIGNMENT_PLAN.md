# Passive Ant — Ninja Village Alignment Plan

**Status:** Proposal / not yet started
**Author:** Design pass, 2026-06-29
**Goal:** Reshape combat and economics so the build ⇄ conquer flywheel matches Kairosoft's *Ninja Village*, while strengthening (not straining) the ant facade.

---

## 0. How to read this document

- **Canonical code:** the live game is the `saveVersion: 2` conquest system in the **lower half of [index.html](index.html)** (search `const SAVE_VERSION = 2`). The older single-ratio combat code above it (`COMBAT_CONFIG`, `calculateCombat`) is **dead** for the shipping build and should not be extended.
- This plan **reuses existing systems** — chamber slots, unit traits, formations, chitin, pheromones, prestige biomes, the supply-line/breach scaffolding. It is connective tissue, not a rewrite.
- Companion docs: [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md) (battle math, territory tables), [CONQUEST_COMBAT_PACKET.md](CONQUEST_COMBAT_PACKET.md) (difficulty curve, copy). Where this plan changes a number defined there, **this plan wins for the new features only**; existing battle math is preserved unless a ticket says otherwise.
- **Facade rule:** every mechanic must map to real or plausible ant biology. Aphid farming and brood-raiding are genuine ant behaviors and are used deliberately.

---

## 1. Design thesis

*Ninja Village* feels balanced because its two halves are a **flywheel**, not parallel tracks:

> province conquered → recruits + tribute → bigger village → better-equipped, higher-level party → next province

Five pillars drive it. This plan ports each into the ant facade:

| # | Ninja Village pillar | Ant translation | Plan phase |
|---|---|---|---|
| 1 | Persistent, leveling, named warriors | **Champion ants** that molt, gain names/traits, can permanently die | P2 |
| 2 | Building-adjacency combos | **Chamber/tunnel combos** via shared pheromone trails | P1 |
| 3 | Research/invention points | **Pheromones as an evolution tech tree** | P4 |
| 4 | Weapon/armor crafting | **Chitin adaptations** (mandible plating, caustic sacs…) | P2 |
| 5 | Conquest cycle (recruit, tribute, defend) | **Recruiting nodes + rival raids** | P1 / P3 |

**Acceptance for the whole initiative:** a player can name a champion, see a placement combo light up, conquer a node that then *sends them recruits*, lose a champion to a rival raid they failed to garrison, and feel the economy and army growing each other.

---

## 2. Current-state baseline (what exists today)

So requirements below are concrete, here is the live model.

### Economy (`tick`, `getNetRates`, ~[index.html:3806](index.html#L3806))
- **Resources:** `food`, `materials`, `chitin`, `pheromones`, each with a cap.
- **Population:** `adults` + `larvae`; `popCap = 24 + 8×nurseryLevel + 6×claimedTerritories`.
- **Allocation:** `assigned.{foragers, excavators, soldiers, nurses}`.
  - Forager → `+0.35 food/s`, Excavator → `+0.22 mats/s`, Soldier → `+0.02 chitin/s` (+ forge), Nurse → larvae growth.
  - `pheromones` rate = `0.015 × scoutLevel` — **near-zero sink today.**
- **Chambers:** 6 slots ([index.html:3778](index.html#L3778)), one build/upgrade timer at a time, 8 types, max level 3, cost `base × 1.8^level`. **Slot position is purely cosmetic.**
- **Territory reward:** flat additive `%` (`foodMult`, `matsMult`, `growthMult`, `foodCapBonus`) + one-time `chitin`/`pheromones` bumps. `bonuses()` at [index.html:3804](index.html#L3804).

### Combat (4-lane / 3-round, design-packet model)
- Army = trained **counts** `army.units.{majors, swarmers, spitters, scouts}`; fungible, no identity.
- Lanes: front / swarm / ranged / support. Formations multiply lane power. Outcome from `roundWins` + `averageScore`.
- Casualties → 70% wounded / 30% permanent (50% on defeat). Wounded recovery exists.

### Gaps vs. the flywheel
1. Units have no persistence/identity → no attachment, low combat stakes.
2. Chamber building is a queue, not a placement puzzle.
3. Pheromones and (largely) chitin have no deep sink → economy is one-axis.
4. Conquest is one-directional and pays only flat % → no recruit/tribute/defend cycle.

---

## 3. Phased delivery

Each phase **pairs one economic lever with one combat/loop lever** so the flywheel actually spins. Ship phases in order; each is independently playable.

| Phase | Theme | Tickets | Headline outcome |
|---|---|---|---|
| **P1** | The puzzle + the cycle | T1 Chamber Combos, T2 Recruiting Nodes | Both halves become decisions, not timers |
| **P2** | Attachment | T3 Champion Ants, T4 Chitin Adaptations | Combat gains identity; chitin gains a real sink |
| **P3** | Tension | T5 War-Party Muster, T6 Rival Raids | Conquest becomes two-directional |
| **P4** | Scale | T7 Pheromone Evolution Tree, T8 Region Unification | Long-term goal + second progression axis |

---

## 4. Detailed requirements

### Phase 1 — The puzzle + the cycle

#### T1 — Chamber adjacency combos `priority: highest`

**Intent:** Convert the cosmetic 2×3 slot grid into a Kairosoft-style placement puzzle.

**Facade:** Adjacent chambers share pheromone trails; neighbors reinforce each other.

**Requirements**
- R1.1 Add a stable adjacency model to the 6 slots. Use a fixed 2×3 layout: slots `1 2 3 / 4 5 6`; neighbors are orthogonal (1–2, 2–3, 1–4, 2–5, 3–6, 4–5, 5–6).
- R1.2 Add `comboTag` to each `CHAMBERS` entry and a `COMBOS` table. Initial set:
  | Combo | Requires adjacent | Effect |
  |---|---|---|
  | Well-Fed Brood | Nursery + Granary | +15% growth |
  | Deep Stores | Granary + Reinforced Store | +10% food & mats cap |
  | Hardened Glands | Acid Gallery + Chitin Forge | +10% ranged power |
  | War Foundry | Barracks + Chitin Forge | +10% army armor (stacks with forge) |
  | Signal Network | Scout Tunnel + Command Chamber | +15% support lane |
- R1.3 `bonuses()` ([index.html:3804](index.html#L3804)) must fold active combo effects into the same multiplier object it already returns; battle/economy code reads them transparently.
- R1.4 The **Build tab** shows, per slot and globally, which combos are active (`Combo: Well-Fed Brood ✓`) and *potential* combos when a slot is selected ("Place a Granary next to this Nursery → Well-Fed Brood").
- R1.5 Combos recompute on build/upgrade/relocate; never on tick (performance).
- R1.6 (Optional, recommended) **Relocate** action: move a completed chamber to an empty slot for a small materials cost, so the puzzle is replayable. If cut, document that placement is build-order-locked.

**Acceptance**
- Building Granary adjacent to the starting Nursery activates "Well-Fed Brood" and growth rate visibly rises.
- Combo state persists through refresh and offline.
- No combo can apply twice for the same pair.

---

#### T2 — Recruiting & tribute nodes `priority: highest`

**Intent:** Make conquest *feed* the colony (recruits + tribute), not just grant a flat %. This closes the flywheel.

**Facade:** Real myrmecology — ants farm **aphids for honeydew** and slave-making ants **raid colonies to steal brood**. Conquered nodes become livestock/recruit sources.

**Requirements**
- R2.1 Extend each `TERRITORIES` entry with an optional `yield` block:
  - `tributePerSec`: `{ food?, materials?, chitin? }` — passive trickle while claimed.
  - `recruit`: `{ type: 'brood' | 'aphid', everySec, amount }`.
- R2.2 Node yields:
  | Node | Tribute | Recruit |
  |---|---|---|
  | Sunny Rock | +0.05 food/s | — |
  | Mossy Log | +0.03 chitin/s | brood: +1 larva / 90s |
  | Compost Edge | +0.08 food/s | aphid livestock (food) |
  | Cracked Paver | +0.05 mats/s | — |
  | Fallen Plum | +0.06 food/s | brood: +1 larva / 75s |
  | Sunken Patio | +0.04 chitin/s | brood: +1 adult / 120s |
- R2.3 `getNetRates()` adds claimed-node tribute to the relevant rate lines and surfaces it in the Nest summary ("Territory tribute: +0.13 food/s").
- R2.4 **Brood recruits** add to `population.larvae` (or adults where specified), respecting `popCap`. **Aphid recruits** add a passive food source that does **not** consume pop.
- R2.5 Offline progression includes tribute and recruits (consistent with existing offline tick) but, per project guardrails, **never** triggers a raid/loss.
- R2.6 Keep the existing flat % rewards; tribute/recruits are *additive* to them, not a replacement.

**Acceptance**
- Claiming Mossy Log starts a visible larva trickle and chitin tribute.
- Tribute/recruits advance correctly after an offline period within `maxOfflineSeconds`.
- Removing a node from claimed (via P3 raid loss) stops its yield immediately.

---

### Phase 2 — Attachment

#### T3 — Champion ants `priority: highest`

**Intent:** Port Ninja Village's named, leveling, mortal warriors. Keep the fungible pool for bulk army; layer champions on top.

**Facade:** An elite soldier that survives enough sieges undergoes a final molt into a **Royal Guard / Praetorian** (real ants have super-majors).

**Requirements**
- R3.1 New state: `army.champions: [{ id, name, caste, level, xp, traits:[], gearId, alive:true }]`.
- R3.2 **Promotion:** after a unit's caste survives `N` victorious battles, offer a molt that converts one unit into a champion. Generate a name from an ant-flavored pool (e.g. "Mandible of the East Trail", "Warden of Cracked Paver").
- R3.3 **Leveling:** champions gain XP per battle survived; each level adds a small flat bonus to their caste's lane.
- R3.4 **Traits/skills** (1–2 per champion), fired in the battle log during the matching round:
  - *Acid Burst* — +50% to ranged round, once per battle.
  - *Bulwark* — −50% casualties for one round.
  - *Frenzy* — +30% swarm round if outnumbered.
- R3.5 **Mortality:** champions persist across battles; on a **Defeat** outcome each champion has a defined chance to die permanently (surface clearly in the result screen). This is the stakes mechanic.
- R3.6 Battle math: champion contributions are added into the existing lane sums — **no change to the 3-round resolution structure.**
- R3.7 **Army tab** shows champion cards (name, caste, level, traits, equipped adaptation).

**Acceptance**
- A surviving Major can be promoted to a named champion that appears on the Army tab.
- The champion's skill appears in the battle log and changes a round's result.
- A Defeat can permanently kill a champion, shown explicitly on the result screen.
- Champions persist through refresh.

---

#### T4 — Chitin adaptations (equipment) `priority: high`

**Intent:** Give chitin a permanent military sink and a parallel power ladder (Ninja Village crafting).

**Facade:** Biological molts/adaptations, not weapons.

**Requirements**
- R4.1 New `ADAPTATIONS` table, each with tiers 1–3, cost in chitin (+ a pheromone recipe unlock from T7 when that ships; until then unlocked by chamber):
  | Adaptation | Lane affected | Unlock |
  |---|---|---|
  | Mandible Plating | +front | Barracks |
  | Caustic Sacs | +ranged | Acid Gallery |
  | Spiked Tarsi | +swarm | start |
  | Hardened Carapace | −casualty rate | Chitin Forge |
- R4.2 Adaptations apply **colony-wide to a caste** by default (simpler) OR equip onto a champion's `gearId` (richer). Ship the colony-wide version first; champion-equip is a fast-follow.
- R4.3 Tier effect: +/− `tier × stepValue` to the relevant lane multiplier or casualty multiplier; fold into the existing lane math.
- R4.4 **Build/Army tab** surfaces crafted adaptations and their effect.

**Acceptance**
- Crafting Mandible Plating I costs chitin and raises front-lane power in the next battle preview.
- Effects stack with formations and forge armor without double-counting.

---

### Phase 3 — Tension

#### T5 — War-party muster `priority: high`

**Intent:** Make the player *choose who deploys* (Ninja Village party select). Precondition for defense mattering.

**Requirements**
- R5.1 Before a siege, a **muster step**: choose which champions and how many of each unit deploy. Default = full army (one tap to keep current behavior).
- R5.2 Undeployed champions/units are flagged **garrisoned** and are unavailable for the siege but defend the nest (T6).
- R5.3 Battle preview/readiness recomputes from the *mustered* party only.

**Acceptance**
- Player can leave a champion home; that champion does not appear in the battle and is not at risk in that fight.
- Readiness badge reflects mustered party, not total roster.

---

#### T6 — Rival raids & garrison defense `priority: high`

**Intent:** Two-directional conquest. Claimed border nodes can be attacked back; you must garrison or lose them.

**Facade:** Rival queens send raiding columns along your supply trails.

**Requirements**
- R6.1 Reuse the existing supply-line/breach + `Threatened` territory state ([CONQUEST_COMBAT_PACKET.md](CONQUEST_COMBAT_PACKET.md) §3). A claimed border node periodically enters `Threatened`.
- R6.2 **Garrison:** units/champions assigned to a node (via T5) raise its defense. Defense vs. raid strength decides the outcome.
- R6.3 **Loss condition:** an undefended `Threatened` node, if unaddressed within a grace window, is **lost** — reverts to unclaimed, yields stop (T2), adjacent unlocks may relock per design.
- R6.4 **Offline guardrail (hard requirement):** raids never resolve or cause loss while the app is closed. On return, surface as a *pending threat* ("Rival scouts mass at Compost Edge — defend within the hour") resolved only in an active session. This preserves the cozy-offline promise in [ant_colony_game_concept.md](ant_colony_game_concept.md).
- R6.5 Push-notification copy hook (optional) consistent with the existing "aesthetic push notification" direction.

**Acceptance**
- A claimed node can enter Threatened and, if ignored in an active session, be lost.
- Garrisoning sufficient force defends it.
- Closing the app during a threat never loses a node; the threat is pending on return.

---

### Phase 4 — Scale

#### T7 — Pheromone evolution tree `priority: medium`

**Intent:** Turn pheromones into the research/invention currency; give the cozy half a combat-independent progression axis.

**Facade:** Pheromones = accumulated colony memory/instinct the queen "remembers."

**Requirements**
- R7.1 New **Evolution** screen. Spend pheromones to unlock: new chamber types, caste-promotion options (T3), and adaptation recipes (T4).
- R7.2 Tree is gated (prereqs), so progress is meaningful even after a lost battle.
- R7.3 Raise pheromone income meaningfully (currently `0.015 × scoutLevel`) so the tree is reachable; tune against `maxOfflineSeconds`.

**Acceptance**
- A player can spend pheromones to unlock an adaptation recipe before they can craft it.
- Pheromone income supports steady tree progress without trivializing it.

---

#### T8 — Region unification & bigger map `priority: low / later`

**Intent:** The long-term "unify Japan" hook, mapped onto prestige biomes.

**Requirements**
- R8.1 Each prestige biome (Garden → Patio, [index.html](index.html) `PRESTIGE_CONFIG`) is a "region"; expand to 10–12 nodes once T2/T6 make holding nodes interesting.
- R8.2 **Unifying** all nodes in a region is the gate/bonus for Nuptial Flight (prestige).
- R8.3 Carry a permanent unification bonus into the next biome.

**Acceptance**
- Claiming every node in a region unlocks an enhanced Nuptial Flight reward.

---

## 5. Cross-cutting requirements

- **Save migration:** bump `SAVE_VERSION` when champion/combo/yield state is added; `migrate()` ([index.html:3783](index.html#L3783)) must default new fields safely so existing saves load without console errors. Back up raw save on failure (existing policy).
- **Mobile UX:** all new controls ≥44px, no primary-tab scroll at 360×740 and 390×844 (existing QA bar in [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md) §Mobile QA Checklist).
- **Offline integrity:** tribute/recruits/build/train advance offline; **battles, raids, and losses never do.**
- **Copy budget:** card title ≤18, one-liner ≤42, log line ≤48 (per combat packet).
- **Reduced motion / audio gesture-gate:** honor existing settings.
- **Performance:** combos and yields recompute on state-change events, not per tick.

---

## 6. Risks & decisions to resolve

| Risk / open question | Recommendation |
|---|---|
| Champion bookkeeping bloats save size | Cap champions (e.g. 6); store minimal fields |
| Adaptations: colony-wide vs. per-champion equip | Ship colony-wide first; per-champion as fast-follow |
| Combo system makes early game too generous | Start with 5 combos, modest %; tune after first playable |
| Raids feel punishing on mobile/idle | Generous grace window + active-session-only resolution |
| Relocate (R1.6) scope creep | Optional; cut to build-order-locked for v1 if needed |
| Two coexisting combat code paths in index.html | Confirm dead `calculateCombat` path stays untouched; consider removing in a cleanup ticket |

---

## 7. Suggested first slice

**Phase 1 (T1 + T2)** is the highest value-to-effort: it makes *both* halves decisions and closes the flywheel, reusing the existing slot grid, `bonuses()`, `getNetRates()`, and claim logic — mostly connective code, no new subsystems. Recommended starting point.

---

## 8. Definition of done (per phase)

- Feature works at 360×740 and 390×844 with no primary scroll.
- Persists through refresh and a representative offline period.
- No console errors on fresh save and migrated save.
- New combat/economy effects are visible in-UI (preview, summary, or card).
- Offline guardrails verified: no battle/raid/loss while closed.
