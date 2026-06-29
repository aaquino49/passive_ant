# Passive Ant — First 3-Minute Tutorial Flow (Phase 0)

The onboarding deliverable. Aligns to the canonical **design packet §Starting State**: Brood Nursery already complete in `slot_1`, empty `slot_2/3/4`, locked `slot_5/6`, army of 4 Swarmers, Food 60 / Materials 35, Sunny Rock pre-selected.

**Canonical source:** [CONQUEST_DESIGN_PACKET.md](CONQUEST_DESIGN_PACKET.md) for all numbers; [CONQUEST_UX_COPY_PACKET.md](CONQUEST_UX_COPY_PACKET.md) §6 for the reusable Army-tab hint lines. Copy budget: every prompt ≤48 chars.

---

## Goal

By minute ~3 the player has **built a chamber, trained a unit, and selected a campaign target** — understanding Build → Train → Campaign. The tutorial **previews but never forces** a battle (combat is never offline-safe to auto-run, and the first fight should be the player's choice). Skippable at any time; one bottom-anchored coach bubble with a single highlighted tap target per step. No step pauses production timers.

Because the colony starts with a Nursery and 4 Swarmers, the tutorial opens from a *living* colony, not an empty one — it teaches "grow what you have," which is cozier than "you have nothing."

---

## Flow

| # | Player sees | Player taps | Teaches | Prompt copy (exact, ≤48) |
|---|---|---|---|---|
| 1 | Nest scene, resource bar pulses | (auto, 2s) | The colony is alive | "Welcome, Queen. Your colony is waking." |
| 2 | Food & Materials icons glow, ticking up | Food icon | Resources grow on their own | "Food and Materials grow on their own." |
| 3 | Bottom tabs; **Build** bounces | Build tab | Where chambers are dug | "Tap Build to dig a new chamber." |
| 4 | 2x3 grid: slot 1 = Nursery, slot 2 glows | Empty slot 2 | Slots hold chambers; one's already yours | "One chamber stands. Dig another." |
| 5 | Chamber picker, **Granary** highlighted | Granary Chamber | Choosing + cost | "Granary stores food. Tap to dig it." |
| 6 | Build timer counts down (20s) | (auto) | Construction takes time | "Workers are digging. Watch it grow." |
| 7 | "Food stores expanded" toast + ✨ | Slot 2 (dismiss) | Completion payoff | "Granary sealed. Food stores grow." |
| 8 | **Army** tab bounces | Army tab | Where soldiers are raised | "Now strengthen your soldiers." |
| 9 | Swarmer row shows ×4, Train glows | Train (Swarmer) | Cost = food + 1 adult | "You have four. Raise one more." |
| 10 | Training timer (10s), count ticks | (auto) | Units take time too | "An adult becomes a soldier. Almost there." |
| 11 | Swarmer count → ×5, ready toast | (dismiss) | Numbers build readiness | "Your swarm grows. Numbers win wars." |
| 12 | **Campaign** tab bounces | Campaign tab | Where conquest happens | "Beyond the nest lies open ground." |
| 13 | Sunny Rock card: enemy + reward shown | Sunny Rock card | Reading a territory | "Sunny Rock: beetles guard easy food." |
| 14 | Readiness badge + To Battle button | (read, not forced) | Loop close + freedom | "Build, train, then strike. Go, Queen!" |

**Why these steps:** steps 6 and 10 run on real timers (20s / 10s) so the tutorial *is* the player's first idle moment — teaching the offline rhythm by living it. Step 4 deliberately points at the pre-built Nursery before the empty slot so the player learns slot states without a wall of text.

---

## Tone rules applied

Cozy address ("Queen", "your colony, waking"), slight drama ("Numbers win wars", "strike"), zero jargon, one idea per bubble, every line ≤48 chars. Matches the design packet's cozy-management / dramatic-combat split.

---

## Tutorial state (for Codex)

```js
G.tutorial = {
  active: true,
  step: 1,          // 1..14
  completed: false,
  skipped: false
}
```

- **Skip** control top-right → sets `skipped: true`, unlocks all four tabs normally, no soft-lock.
- Tutorial never re-triggers once `completed || skipped`, unless reset via Settings → **Reset Tutorial**.
- Each step may advance on either the highlighted tap **or** the auto-timer (steps 1, 6, 10), whichever comes first after its minimum read time.
- A Materials top-up is **not** needed: at the canonical starting state (35 Materials, +1.32 Mat/s from 6 excavators) Granary is affordable instantly and Barracks within ~12s, so the flow never stalls.

---

## Acceptance (maps to QA packet §3 first-3-minutes rubric)

- [ ] A new player can state "build, train, fight" after step 14.
- [ ] Full flow completes in ≤3:30; longest dead-wait is the 20s Granary build.
- [ ] Every step has exactly one obvious tap target.
- [ ] Player finishes ≥1 chamber (Granary) and ≥1 unit (Swarmer) before it ends.
- [ ] Tutorial never auto-starts a battle; it ends by handing control over.
- [ ] Skipping leaves all tabs usable with no soft-lock.
- [ ] Net Food stays positive throughout.
