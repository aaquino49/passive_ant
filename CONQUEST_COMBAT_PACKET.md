# Passive Ant — Combat & Campaign Supplement (Sprint C)

**Canonical source:** [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md) is decision-complete for **battle math, enemy lane values, formations, casualties, territory rewards, and base battle-log copy.** This file does **not** redefine any of those numbers — it supplements them with the things the design packet leaves open: the difficulty-curve rationale, expanded battle-log line pools, the result-screen UX copy (titles/buttons/toasts), reward-application order, and formation counter-strategy.

If any number here ever disagrees with the design packet, the design packet wins.

Copy budget: card title ≤18, one-liner ≤42, log line ≤48, result body ≤2 short lines.

---

## 1. Combat Model (reference only)

Per design packet §Battle Math — four lanes, three rounds. Repeated here as a read-only map so the copy below lines up:

- **Round 1 — Front Clash:** `frontPower / enemyFront`
- **Round 2 — Swarm Pressure:** `swarmPower / enemySwarm`
- **Round 3 — Ranged & Support:** `(rangedPower + supportPower) / (enemyRanged + enemySupport)`
- Outcome from `roundWins` + `averageScore` thresholds (clean ≥1.35 / costly ≥0.95 / retreat ≥0.65 / else defeat).
- Casualties: 70% wounded / 30% permanent (50% permanent on defeat); scouts only lost if round-3 score < 0.75.

Formations feed `formation.frontMult / swarmMult / rangedMult / casualtyMult` (design packet §Formations). Chitin Forge armor reduces `casualtyRate` via `armorReduction`.

---

## 2. Difficulty Curve & Unlock Order

Enemy totals are the sum of the four lanes from the design packet §Enemy Army Tables. This is the tuning lens, not new data.

```
Sunny Rock ─► Mossy Log ─► Compost Edge ─► Cracked Paver ─► Fallen Plum ─► Sunken Patio
  (first win)  (+Scout)     (+Acid)         (+Forge)         (boss warn)    (BOSS/Command)
```

| # | Territory | Enemy | Front/Swarm/Ranged/Support | Total | Intended difficulty | Best counter |
|---:|---|---|---|---:|---|---|
| 1 | Sunny Rock | Beetle Guard | 10 / 2 / 0 / 0 | 12 | First win, easy | Any army; round 3 free (no enemy ranged) |
| 2 | Mossy Log | Spider Scouts | 5 / 10 / 0 / 3 | 18 | Easy — fast swarm | Shield Wall (blunt swarm) |
| 3 | Compost Edge | Earwig Swarm | 3 / 14 / 0 / 2 | 19 | Medium — massed swarm | More Swarmers / Shield Wall |
| 4 | Cracked Paver | Ironclad Beetles | 18 / 4 / 0 / 0 | 22 | Med-hard — armor wall | Majors + Chitin Forge; Acid Line |
| 5 | Fallen Plum | Hornet Nest | 4 / 6 / 14 / 2 | 26 | Hard — flying ranged | Spitters + Acid Line; Shield Wall front |
| 6 | Sunken Patio | Rival Colony Army | 16 / 14 / 10 / 6 | 46 | Boss — all lanes | Command Chamber stance + full roster |

**Tuning intent (why each fight exists):**
- **Beetle Guard** has zero ranged/support → round 3 is a free win, guaranteeing the player's first battle is winnable and teaching "you can win."
- **Spider Scouts / Earwig Swarm** spike the swarm lane → first real reason to pick **Shield Wall** (front +35%) or train more Swarmers.
- **Ironclad Beetles** front of 18 is a wall → needs Majors (`×3.2` front coefficient) plus Chitin Forge armor; the first "build before you fight" gate. Cracked Paver also *grants* the Forge, so the very next fight is easier — reward pacing checks out.
- **Hornet Nest** dumps 14 into ranged → the graduation fight that forces **Spitters** (Acid Gallery, gated behind Compost Edge) and **Acid Line**.
- **Rival Colony Army** is strong in every lane (total 46) → no single counter; requires Command Chamber `supportPower` (`+2/level`) to push a lane over 1.0.

**Readiness badge** (Campaign card + Army tab) compares total player lane power vs. total enemy lane power: **Safe ≥1.3×**, **Risky 0.85–1.3×**, **Dangerous <0.85×**. When Scout Tunnel is not built, hide exact enemy lane numbers (show "??") and display only the Safe/Risky/Dangerous estimate.

---

## 3. Territory Card Copy

Names, rewards, unlocks, and descriptions are canonical in design packet §Campaign Territories. State labels and reusable strings for the card UI:

- **State badges:** Locked · Available · Scouted · Battle Ready · Claimed · Threatened.
- **Locked card:** "Claim the path here first."
- **Threatened card:** "Enemies stir. Defend soon."
- **Scouted card adds:** exact enemy lane chips (Front/Swarm/Ranged/Support).
- **Battle Ready card:** readiness badge + **To Battle** button.

---

## 4. Battle-Log Line Pools (expanded)

The design packet gives 4 base lines per lane. These pools **extend** that set with win/loss variants, round banners, scout support, and close lines so a 3-round battle never visibly repeats. Pull at random, no immediate repeat, ≤48 chars, present tense, cozy-dramatic ant voice.

**Round banners:**
- "Round 1 — The lines crash together!"
- "Round 2 — The swarm pours forward!"
- "Round 3 — Acid and signals fly!"

**Front win (round-1 score ≥1):**
- "Majors crack the enemy shell wide open."
- "A wall of pincers shatters their front."
- "Heavy mandibles break their charge."

**Front loss (round-1 score <1):**
- "Enemy armor absorbs the charge."
- "The front line buckles but holds on."
- "Their shells turn aside your blows."

**Swarm win:**
- "Swarmers flood every gap at once."
- "A living tide drowns their flank."
- "Countless legs overrun the field."

**Swarm loss:**
- "The swarm overextends and is cut down."
- "They scatter your rush before it lands."
- "Too few legs — the flank gives way."

**Ranged/support win:**
- "Acid rains down in hissing arcs."
- "Spitters melt their backline to slime."
- "Scouts mark a weak trail — strike there!"

**Ranged/support loss:**
- "Their fliers scatter your backline."
- "No acid falls — the rear stands firm."
- "Without scouts, the signals go dark."

**Victory close:**
- "The field is yours, Queen!"
- "The enemy breaks and scatters."

**Defeat close:**
- "Sound the retreat — fall back!"
- "Too strong. The colony withdraws."

---

## 5. Result-Screen UX Copy

Design packet §Results gives the body sentence per outcome. This adds the screen scaffolding: title, action button, and a follow-up toast. Title + ≤2 lines + one button.

| Outcome | Title | Body (canonical, design packet) | Button |
|---|---|---|---|
| clean_victory | Clean Victory! | "The territory falls before the colony's momentum breaks." | Claim Reward |
| costly_victory | Costly Victory | "The colony claims the ground, but the nursery will feel the losses." | Claim Reward |
| retreat | Retreat | "The army pulls back before the trail collapses." | Back to Nest |
| defeat | Defeat | "The enemy holds the ground. The wounded need recovery before another push." | Back to Nest |

**Follow-up toast (after the result screen closes):**
- Victory + a chamber unlocked → "New ground claimed — new chambers await."
- Retreat → "Tip: try Shield Wall, or raise more Swarmers."
- Defeat → "Tip: heal the wounded, then strike again."

---

## 6. Reward Application Order (for Codex)

On a victory result, apply in this exact order so unlocks and rewards never race:

1. Apply economy reward (production %/cap from design packet §Campaign Territories).
2. Set territory `claimed = true`, `state = "claimed"`.
3. Unlock adjacent territories (set `unlocked`, `state = "available"`).
4. Unlock any chamber gated on this claim (Acid Gallery / Chitin Forge / Command Chamber / Scout Tunnel per unlock conditions).
5. Roll casualties → split wounded/permanent per design packet §Casualties.
6. Start wounded-recovery timers.
7. Fire the §5 follow-up toast.

Retreat/Defeat skip steps 1–4.

---

## 7. Formation Counter-Strategy (player-facing tips)

Built on the canonical formation bonuses (design packet §Formations). Surface these as one-line hints on the formation selector when a territory is selected:

| Selected enemy trait | Hint (≤42) | Suggests |
|---|---|---|
| armored / heavy (Beetle, Ironclad) | "Armor ahead — bring acid or Majors." | Acid Line / Majors |
| fast / numerous (Spider, Earwig) | "Fast swarm — hold with Shield Wall." | Shield Wall |
| flying (Hornet) | "They strike from above — answer with acid." | Acid Line + Spitters |
| organized / mixed (Rival) | "A real army. Command them as one." | Command Chamber stance |

---

## 8. Sprint C Acceptance (Claude-verifiable)

- Every territory has name, description, reward, unlocks, and an enemy with four lane values (design packet) — no gaps.
- Battle resolves in exactly 3 rounds, no taps; each round maps to its lane per §1.
- All four outcomes reachable; each shows §5 title/body/button and correct army/economy effects.
- Reward application follows §6 order.
- Scout Tunnel toggles enemy-lane reveal vs. "??".
- Offline elapsed time never produces a battle result or loss.
