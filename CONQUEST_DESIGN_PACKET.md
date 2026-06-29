# Passive Ant Conquest Design Packet

This packet fills in the implementation details for `MOBILE_CONQUEST_PROJECT_PLAN.md`. Treat this file as the decision-complete design companion for the mobile-browser conquest redesign.

## Product Target

- Platform: portrait mobile browser.
- Primary test sizes: 360x740 and 390x844.
- Session shape: 30-90 second check-ins, with optional 3-5 minute active sessions.
- Core loop: Build chambers -> train army -> fight territory armies -> claim territory -> unlock new chambers and resources.
- Theme: ant colony conquest, not ninja reskin.
- First playable goal: the player can build a Barracks, train Swarmers and Majors, defeat Sunny Rock, claim the reward, and unlock two adjacent territories.

## Starting State

```js
const STARTING_STATE = {
  saveVersion: 2,
  resources: {
    food: 60,
    materials: 35,
    chitin: 8,
    pheromones: 5
  },
  caps: {
    food: 120,
    materials: 80,
    chitin: 40,
    pheromones: 25
  },
  population: {
    adults: 18,
    larvae: 6
  },
  assigned: {
    foragers: 8,
    excavators: 6,
    nurses: 2,
    idle: 2
  },
  buildings: {
    activeBuildSlotId: null,
    unlockedBuildingIds: ["granary", "store", "nursery", "barracks"],
    slots: [
      { id: "slot_1", buildingId: "nursery", level: 1, status: "complete", buildTimer: 0, buildDuration: 0 },
      { id: "slot_2", buildingId: null, level: 0, status: "empty", buildTimer: 0, buildDuration: 0 },
      { id: "slot_3", buildingId: null, level: 0, status: "empty", buildTimer: 0, buildDuration: 0 },
      { id: "slot_4", buildingId: null, level: 0, status: "empty", buildTimer: 0, buildDuration: 0 },
      { id: "slot_5", buildingId: null, level: 0, status: "locked", buildTimer: 0, buildDuration: 0 },
      { id: "slot_6", buildingId: null, level: 0, status: "locked", buildTimer: 0, buildDuration: 0 }
    ]
  },
  army: {
    units: { majors: 0, swarmers: 4, spitters: 0, scouts: 0 },
    wounded: { majors: 0, swarmers: 0, spitters: 0, scouts: 0 },
    trainingQueue: [],
    formation: "balanced"
  },
  campaign: {
    selectedTerritoryId: "sunny_rock",
    territories: {
      sunny_rock: { state: "available", unlocked: true, claimed: false, battleCleared: false },
      mossy_log: { state: "locked", unlocked: false, claimed: false, battleCleared: false },
      compost_edge: { state: "locked", unlocked: false, claimed: false, battleCleared: false },
      cracked_paver: { state: "locked", unlocked: false, claimed: false, battleCleared: false },
      fallen_plum: { state: "locked", unlocked: false, claimed: false, battleCleared: false },
      sunken_patio: { state: "locked", unlocked: false, claimed: false, battleCleared: false }
    }
  },
  battle: null
};
```

## Resource Economy

### Base Production

| Source | Output |
|---|---:|
| Forager | +0.35 Food/sec |
| Excavator | +0.22 Materials/sec |
| Nurse | +0.006 Larvae/sec |
| Scout Tunnel | +0.015 Pheromones/sec per level |
| Chitin Forge | +0.025 Chitin/sec per level |

### Base Upkeep

| Cost | Formula |
|---|---|
| Adult food upkeep | adults * 0.035 Food/sec |
| Army food upkeep | active army units * 0.012 Food/sec |
| Wounded recovery food | wounded units recovering * 0.01 Food/sec |

### Territory Reward Modifiers

| Modifier | Stack Rule |
|---|---|
| Food production | additive percentage |
| Materials production | additive percentage |
| Growth | additive percentage |
| Army armor | additive percentage |
| Campaign intel | additive percentage |

## Chamber Definitions

| Id | Name | Unlock | Cost | Build Time | Effect | Mobile Description | Completion Message |
|---|---|---|---:|---:|---|---|---|
| granary | Granary Chamber | Start | 20 Materials | 20s | +80 Food cap per level | Stores more Food for growth and campaigns. | Food stores expanded. |
| store | Reinforced Store | Start | 20 Materials | 20s | +60 Materials cap per level | Holds more dirt, stone, and shell scrap. | Material stores reinforced. |
| nursery | Brood Nursery | Start | 35 Materials | 45s | +25% growth per level | Speeds larvae growth into adult ants. | The nursery warms with new brood. |
| barracks | Barracks | Start | 30 Materials | 45s | Unlocks Major training; +10% army health per level | Trains heavy front-line soldiers. | Majors can now be trained. |
| acid_gallery | Acid Gallery | Compost Edge claimed | 55 Materials, 10 Chitin | 90s | Unlocks Spitter training; +10% ranged power per level | Cultivates acid glands for ranged units. | Spitters can now be trained. |
| scout_tunnel | Scout Tunnel | Mossy Log claimed | 40 Materials | 60s | Unlocks Scouts; +15% campaign preview accuracy per level | Sends trail-readers ahead of the army. | Scouts can now be trained. |
| chitin_forge | Chitin Forge | Cracked Paver claimed | 75 Materials, 25 Chitin | 120s | +20% armor per level; produces Chitin | Hardens shell plates for combat. | Chitin armor production begins. |
| command_chamber | Command Chamber | Sunken Patio claimed | 100 Materials, 40 Chitin | 180s | Unlocks advanced formation bonuses | Coordinates army-wide battle signals. | The colony can command larger wars. |

### Chamber Upgrade Rule

- Max level for first pass: 3.
- Upgrade cost = base cost * `1.8 ^ currentLevel`.
- Upgrade time = base build time * `1.5 ^ currentLevel`.
- Upgrades use the same active construction slot rule: only one build/upgrade timer can run at a time.

## Army Unit Definitions

| Id | Name | Unlock | Cost | Train Time | Role | Mobile Description |
|---|---|---|---:|---:|---|---|
| swarmers | Swarmers | Start | 6 Food, 1 adult | 10s | Fast melee pressure | Fragile ants that overwhelm exposed enemies. |
| majors | Majors | Barracks complete | 8 Food, 4 Chitin, 1 adult | 18s | Front armor breaker | Heavy soldiers that crack armor and hold the line. |
| spitters | Spitters | Acid Gallery complete | 10 Food, 6 Chitin, 1 adult | 22s | Backline ranged | Acid attackers that punish flying and clustered foes. |
| scouts | Scouts | Scout Tunnel complete | 5 Food, 1 Pheromone, 1 adult | 12s | Intel and support | Trail-readers that improve preview and battle support. |

### Training Rules

- Training queue length: 3.
- Each queue item trains one unit.
- Offline time can advance training timers.
- Training consumes resources and adult population when queued, not when completed.
- If the player cancels queued training, refund 75% of resources and restore the adult population.
- Wounded units recover before new units of the same type are trained if the player taps `Recover`.

### Wounded Recovery

- Recovery time: 20s per wounded unit.
- Recovery cost: 3 Food per unit.
- Wounded units do not fight.
- Defeat can convert some wounded into permanent losses.

## Formations

| Formation | Unlock | Bonuses | Penalties | Preview Copy |
|---|---|---|---|---|
| balanced | Start | No modifiers | None | Flexible formation with no clear weakness. |
| shield_wall | Barracks | +35% front power, -20% casualty rate | -20% swarm power | Safer against armored or heavy enemies. |
| swarm_rush | Start | +45% swarm power, +10% crit chance | +20% casualty rate | Fast assault that can win quickly but bleeds ants. |
| acid_line | Acid Gallery | +45% ranged power, +25% vs flying | -25% front power if no Majors | Strong against flying or clustered enemies. |

## Campaign Territories

| Id | Name | Node Asset | Enemy Army | Reward | Unlocks | Description |
|---|---|---|---|---|---|---|
| sunny_rock | Sunny Rock | node_sunny_rock_states.png | Beetle Guard | +10% Food production | Mossy Log, Compost Edge | Warm stone with soft trails and beetle sentries. |
| mossy_log | Mossy Log | node_mossy_log_states.png | Spider Scouts | +15% Growth | Scout Tunnel, Cracked Paver | Damp wood full of fungus and hidden predators. |
| compost_edge | Compost Edge | node_compost_edge_states.png | Earwig Swarm | +25 Food cap, Acid Gallery unlock | Fallen Plum | Fertile rot rich with food and danger. |
| cracked_paver | Cracked Paver | node_cracked_paver_states.png | Ironclad Beetles | +20% Materials, Chitin Forge unlock | Sunken Patio | Broken stone sheltering armored rivals. |
| fallen_plum | Fallen Plum | node_fallen_plum_states.png | Hornet Nest | +10% Food, +10% Growth | Sunken Patio warning | Sweet fruit guarded from above. |
| sunken_patio | Sunken Patio | node_sunken_patio_states.png | Rival Colony Army | Command Chamber unlock, Patio biome access | Future biome | A distant crack held by a rival colony. |

## Enemy Army Tables

| Enemy Army | Front | Swarm | Ranged | Support | Traits |
|---|---:|---:|---:|---:|---|
| Beetle Guard | 10 | 2 | 0 | 0 | armored |
| Spider Scouts | 5 | 10 | 0 | 3 | fast |
| Earwig Swarm | 3 | 14 | 0 | 2 | soft, numerous |
| Ironclad Beetles | 18 | 4 | 0 | 0 | armored, heavy |
| Hornet Nest | 4 | 6 | 14 | 2 | flying, fast |
| Rival Colony Army | 16 | 14 | 10 | 6 | organized, mixed |

## Battle Math

### Player Lane Power

```js
frontPower =
  majors * 3.2 * formation.frontMult +
  swarmers * 0.5;

swarmPower =
  swarmers * 1.7 * formation.swarmMult +
  scouts * 0.6;

rangedPower =
  spitters * 2.4 * formation.rangedMult +
  scouts * 0.4;

supportPower =
  scouts * 1.2 +
  completedCommandChamberLevel * 2;
```

### Enemy Lane Power

```js
enemyFront = enemy.front;
enemySwarm = enemy.swarm;
enemyRanged = enemy.ranged;
enemySupport = enemy.support;
```

### Three Round Resolution

Round 1: Front Clash

```js
round1Score = frontPower / Math.max(1, enemyFront);
```

Round 2: Swarm Pressure

```js
round2Score = swarmPower / Math.max(1, enemySwarm);
```

Round 3: Ranged And Support

```js
round3Score = (rangedPower + supportPower) / Math.max(1, enemyRanged + enemySupport);
```

### Outcome

```js
averageScore = (round1Score + round2Score + round3Score) / 3;
roundWins = count(roundScore >= 1.0);

if (roundWins === 3 && averageScore >= 1.35) outcome = "clean_victory";
else if (roundWins >= 2 && averageScore >= 0.95) outcome = "costly_victory";
else if (roundWins >= 1 && averageScore >= 0.65) outcome = "retreat";
else outcome = "defeat";
```

### Casualties

```js
baseCasualtyRate = {
  clean_victory: 0.06,
  costly_victory: 0.18,
  retreat: 0.28,
  defeat: 0.42
}[outcome];

casualtyRate = baseCasualtyRate * formation.casualtyMult * armorReduction;
```

- 70% of casualties become wounded.
- 30% of casualties become permanent losses.
- On defeat, permanent loss share rises to 50%.
- Scouts are only lost if support round score is below 0.75.

### Territory Result

- Clean Victory: claim territory, full reward, low wounded count.
- Costly Victory: claim territory, full reward, moderate wounded count.
- Retreat: no territory claim, low permanent loss, wounded recovery needed.
- Defeat: no territory claim, higher losses, territory remains available.

## Battle Log Copy Pools

### Front Clash

- Majors lock mandibles with the enemy front.
- Heavy shells crack under the first impact.
- The front line buckles but holds.
- Enemy armor absorbs the charge.

### Swarm Pressure

- Swarmers flood the exposed flank.
- Fast ants pour through a gap in the line.
- The enemy scatters under the swarm.
- The swarm overextends and takes losses.

### Ranged And Support

- Spitters arc acid over the front line.
- Scouts mark a weak trail through the chaos.
- Acid burns through enemy armor plates.
- Flying enemies force the backline to scatter.

### Results

- Clean Victory: The territory falls before the colony's momentum breaks.
- Costly Victory: The colony claims the ground, but the nursery will feel the losses.
- Retreat: The army pulls back before the trail collapses.
- Defeat: The enemy holds the ground. The wounded need recovery before another push.

## Mobile Wireframes

### Global Layout

```text
+--------------------------------+
| Resource Bar                   | 48px
| Food Mats Chitin Phero Pop Gear|
+--------------------------------+
| Context Visual                 |
| Nest / Build / Army / Campaign | 40-45vh
| / Battle                       |
+--------------------------------+
| Active Panel                   |
| Tab-specific controls          | 45-50vh
+--------------------------------+
| Nest | Build | Army | Campaign | 56px
+--------------------------------+
```

### Nest Tab

```text
Top Visual:
  Nest background with current chamber highlights.

Bottom Panel:
  Status card
  Current construction timer
  Current training timer
  Production rates
  Recommended action button
```

### Build Tab

```text
Top Visual:
  2x3 chamber slot grid.

Bottom Panel:
  Selected slot details
  Build/upgrade button
  Cost and timer
  Chamber picker drawer when empty slot selected
```

### Army Tab

```text
Top Visual:
  Army muster scene with unit groups.

Bottom Panel:
  Formation segmented control
  Unit rows, 2 visible per page if needed
  Training queue strip
  Readiness indicator
```

If unit rows do not fit at 360x740, use a two-page segmented control:

- Units.
- Formation.

Do not use a long scroll.

### Campaign Tab

```text
Top Visual:
  Large node map with 6 territory tiles.

Bottom Panel:
  Selected territory card
  Enemy army preview
  Reward/unlock summary
  Battle button
```

### Battle Overlay

```text
+--------------------------------+
| Round 1 / 2 / 3                |
| Player army       Enemy army   |
| FX, shake, damage flashes      |
+--------------------------------+
| Round log                      |
| Lane result chips              |
| Continue / Results button      |
+--------------------------------+
```

## Asset Integration Notes

### Existing Assets To Use

| Purpose | Asset |
|---|---|
| Nest visual | bg_nest_cross_section.png |
| Build/chambers | tiles_nest_chambers.png |
| Garden campaign | bg_garden_surface.png |
| Patio campaign | bg_patio_surface.png |
| Compost campaign | bg_compost_surface.png |
| Unit sprites | ant_major_crusher_sheet.png, ant_swarmer_sheet.png, ant_spitter_sheet.png, ant_worker_forager_sheet.png |
| Enemy sprites | enemy_armored_beetle_sheet.png, enemy_fast_predator_sheet.png, enemy_flying_threat_sheet.png, enemy_soft_scavenger_sheet.png, enemy_heavy_boss_sheet.png |
| Territory nodes | node_*_states.png |
| UI icons | icons_resources_core.png, icons_resources_extended.png, icons_actions.png, icons_warnings.png |
| Combat FX | fx_acid_line.png, fx_bite_impact.png, fx_crush_impact.png, fx_ichor_splash.png, fx_shell_crack.png |
| Status FX | fx_warning_pulse.png, fx_resource_sparkle.png, overlay_panic_dimmer.png |

### Sprite Sheet Rule

- Do not stretch whole sprite sheets directly into small UI icons.
- Use CSS `background-position` crops or helper classes for representative frames.
- If slicing is too time-consuming, use the existing icon sheets for UI and reserve sprite sheets for larger visual scenes.

### Gemini Revision Candidates

Only request these if QA confirms issues:

- `ant_spitter_sheet.png`: check for pale edge halo from white-key removal.
- `scene_new_colony_landing.png`: queen has a literal crown; revise if it feels too cartoon-literal.
- `fx_warning_pulse.png`: uses exclamation-like symbols; revise if abstract-only warning FX is preferred.

## Audio Plan For Gemini

If Gemini can generate audio, request these as short loop/sting assets. If it cannot, keep these as prompts for a later audio tool.

| Filename | Type | Duration | Prompt |
|---|---|---:|---|
| audio_colony_calm_loop.ogg | loop | 30s | Soft soil taps, tiny ant movement, warm low hum, cozy garden ambience, seamless loop. |
| audio_build_complete.ogg | sting | 2s | Satisfying dirt-settle pop with tiny chime, warm and not flashy. |
| audio_training_complete.ogg | sting | 1s | Tiny mandible click and leaf-rustle tick, compact mobile notification sound. |
| audio_campaign_ambience.ogg | loop | 30s | Outdoor garden ambience with faint wind, distant insects, soft trail rhythm, seamless loop. |
| audio_battle_start.ogg | sting | 2s | Low ant-war drum made from soil taps and shell clicks, tense but cute. |
| audio_round_hit_crunch.ogg | sting | 1s | Crunchy candy-shell impact, dry crack, no wet gore. |
| audio_acid_volley.ogg | sting | 1s | Quick bright acid sizzle and pop, stylized, not harsh. |
| audio_victory.ogg | sting | 3s | Hopeful tiny colony fanfare with warm chimes and leaf rustle. |
| audio_defeat.ogg | sting | 3s | Low muted shell rattle and soft retreat tone, sad but not horror. |

### Browser Audio Rules

- Audio starts muted by default until user taps.
- Settings must include audio on/off.
- Loops should not auto-play before a user gesture.
- Haptics and audio should be independently toggleable.

## Save Migration Policy

### Save Version

- New save version: `2`.
- Add `saveVersion` to all saved states.

### Migration From Old Prototype

If old save has no `saveVersion`:

- Keep resources where possible:
  - `food` -> `resources.food`.
  - `materials` -> `resources.materials`.
  - `chitin` missing -> `8`.
  - `pheromones` missing -> `5`.
- Keep population:
  - adults and larvae copy forward.
- Convert upgrades:
  - `foodStorage > 0` -> Granary Chamber complete at matching level, capped at 3.
  - `materialStorage > 0` -> Reinforced Store complete at matching level, capped at 3.
  - `nursery > 0` -> Brood Nursery complete at matching level, capped at 3.
  - `guardChamber > 0` -> Barracks complete at matching level, capped at 3.
- Convert claimed nodes:
  - matching campaign territory becomes claimed.
  - rewards and unlocks apply.
- Army:
  - existing assigned soldiers become Swarmers at `Math.max(4, soldiers)`.
  - Majors, Spitters, Scouts start at 0 unless unlocked by converted buildings.

If migration throws or produces invalid state:

- Back up the raw save under `passiveAntLegacySaveBackup`.
- Start a fresh version 2 save.
- Show one toast: "Colony systems changed. A fresh conquest save has begun."

## Implementation Tickets

### Ticket 1: Mobile Shell

Owner: Codex

- Add four bottom tabs.
- Add persistent resource bar.
- Add contextual visual area.
- Add tab panels with no primary scrolling.
- Acceptance: tabs work at 360x740 and 390x844.

### Ticket 2: Version 2 State And Migration

Owner: Codex

- Add `saveVersion`.
- Add `buildings`, `army`, `campaign`, `battle`.
- Add migration from old saves.
- Acceptance: fresh and old saves load without console errors.

### Ticket 3: Chamber Build System

Owner: Codex

- Add 2x3 chamber slots.
- Add chamber picker.
- Add construction/upgrade timers.
- Add offline timer advancement.
- Acceptance: build Granary and Barracks, refresh, timers persist.

### Ticket 4: Army Training System

Owner: Codex

- Add unit rows.
- Add train queue.
- Add formation selector.
- Add wounded recovery.
- Acceptance: train Swarmers/Majors, refresh, counts persist.

### Ticket 5: Campaign Map

Owner: Codex

- Add six territory nodes.
- Add territory cards.
- Add unlock rules.
- Add reward application.
- Acceptance: Sunny Rock victory unlocks Mossy Log and Compost Edge.

### Ticket 6: Three-Round Battle

Owner: Codex

- Add battle preview.
- Add 3-round auto-resolution.
- Add lane scores.
- Add log and results.
- Add casualties/wounded/rewards.
- Acceptance: battle resolves and updates campaign/army state.

### Ticket 7: Copy And Tuning Review

Owner: Claude

- Review all chamber copy.
- Review all battle log copy.
- Review first 3-minute flow.
- Tune costs and timers after first playable.
- Acceptance: no UI copy exceeds mobile panel comfort.

### Ticket 8: Asset And Audio Follow-Up

Owner: Gemini

- Generate audio assets or audio prompts.
- Revise any visual assets flagged during QA.
- Acceptance: all requested files follow style guide and contain no baked-in text.

## Mobile QA Checklist

- [ ] 360x740: no primary tab scroll.
- [ ] 390x844: no primary tab scroll.
- [ ] All controls are at least 44px.
- [ ] Text does not overflow buttons or cards.
- [ ] Bottom tabs are always reachable.
- [ ] Build timer persists through refresh.
- [ ] Training timer persists through refresh.
- [ ] Campaign unlock persists through refresh.
- [ ] Battle result persists through refresh.
- [ ] Offline progress advances production, construction, and training.
- [ ] Offline progress never starts battles.
- [ ] Offline progress never applies combat losses.
- [ ] Reduced motion disables heavy battle shake/flashes.
- [ ] Audio does not play before user gesture.
- [ ] Fresh save and migrated save both work.

