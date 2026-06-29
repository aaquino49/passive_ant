# Passive Ant Mobile Conquest Redesign Project Plan

## Purpose

Redesign Passive Ant into a portrait mobile-browser ant colony conquest game inspired by the structure of Kairosoft's Ninja Village:

Build colony economy -> train army pools -> fight enemy armies in short auto battles -> claim territory -> unlock new chambers, resources, and units.

This plan breaks the work into parallelizable lanes for Codex, Claude, and Gemini. The current implementation is a single-file HTML/CSS/JS prototype in `index.html`, and the first pass should keep that shape unless explicitly changed later.

## Non-Negotiable Product Constraints

- Platform: mobile browser, portrait orientation, no install required.
- Primary viewport targets: 360x740 and 390x844.
- No primary tab should require vertical scrolling.
- Touch targets must be at least 44px.
- No hover-only interactions.
- No precision dragging.
- Offline progress may advance production, construction, and training timers.
- Offline progress must never start battles or apply combat losses.
- Theme remains ant colony, not ninja reskin.
- Use existing generated assets in `assets/` wherever possible.

## Agent Responsibilities

### Codex

Codex owns implementation, integration, refactors, state migration, asset wiring, browser verification, and bug fixes.

Codex should:
- Modify `index.html`.
- Keep the game playable after every phase.
- Preserve save/load safety.
- Run local browser or static checks after major changes.
- Keep generated assets referenced from `assets/`.
- Avoid new external libraries for the first pass.

### Claude

Claude owns product/design detail, balancing tables, copy, QA scenarios, and handoff reviews.

Claude should:
- Produce clear data tables and tuning values for Codex.
- Review mobile UX flows against the plan.
- Write acceptance checklists for each milestone.
- Draft combat log copy, building descriptions, territory descriptions, onboarding copy, and tutorial microcopy.
- Review implementation screenshots and flag layout/clarity issues.

### Gemini

Gemini owns visual and musical assets only.

Gemini should:
- Generate any missing art assets.
- Revise unusable or unclear generated assets.
- Generate music and sound-effect prompts/assets if audio generation is available.
- Follow `ASSET_STYLE_GUIDE.md`.
- Avoid baked-in text, watermarks, signatures, and realistic gore.

## Current Asset Inventory

The project already has a broad visual asset set in `assets/`, including:

- Ant units: workers, excavators, soldiers, majors, swarmers, spitters, winged queen.
- Enemies: armored beetle, soft scavenger, wolf spider, flying threat, heavy boss.
- Backgrounds: nest, garden, patio, compost.
- Node sheets: mossy log, sunny rock, cracked paver, compost edge, fallen plum, sunken patio.
- UI icons: resources, extended resources, roles, actions, warnings, settings.
- FX: trail states, acid, bite, crush, ichor, shell crack, supply breach, warning pulse, sparkle.
- Prestige scenes and mutation cards.

Gemini should not regenerate these by default. Only request revisions when a specific implementation or QA issue is found.

## Parallel Workstreams

## Phase 0: Alignment And File Hygiene

### Codex Tasks

- Create a backup copy of the current `index.html` before the major rewrite, for example `index.pre-conquest-backup.html`.
- Add a save migration version key, for example `saveVersion: 2`.
- Audit existing localStorage keys and decide whether to migrate or reset old saves.
- Confirm all currently referenced assets load from `assets/`.
- Fix obvious text encoding artifacts only if touching nearby strings.

### Claude Tasks

- Create a one-page design brief for the new loop:
  - Build.
  - Train.
  - Battle.
  - Expand.
  - Unlock.
- Write final naming guidance:
  - "Found New Colony" instead of "Nuptial Flight" for user-facing prestige.
  - "Campaign" for territory expansion.
  - "Chambers" for buildings.
- Define tutorial goals for the first 3 minutes of play.

### Gemini Tasks

- No new asset work unless Codex identifies an unusable existing file.

### Acceptance Criteria

- Current project files are understood.
- Backup exists.
- Implementation can proceed without losing current prototype reference.

## Phase 1: Mobile Shell And Four-Tab Navigation

### Codex Tasks

- Replace the current Nest/Map structure with four bottom tabs:
  - Nest.
  - Build.
  - Army.
  - Campaign.
- Keep a persistent top resource bar:
  - Food.
  - Materials.
  - Chitin.
  - Pheromones.
  - Population.
- Keep the top 40-45% as the contextual visual area.
- Keep the bottom 55-60% as the active control panel.
- Ensure each tab fits in portrait without primary scrolling.
- Add settings access without crowding the bottom tabs.
- Preserve reduced motion, haptics, and reset settings.

### Claude Tasks

- Review proposed tab labels and button copy.
- Draft concise empty-state text for each tab:
  - No building selected.
  - No army trained.
  - No campaign target selected.
  - Construction complete.

### Gemini Tasks

- Optional: revise UI panel textures if they reduce readability.

### Acceptance Criteria

- Four bottom tabs work on mobile.
- All controls are 44px or larger.
- No primary tab requires vertical scrolling at 360x740.
- The game remains save/load capable.

## Phase 2: Core State Model Redesign

### Codex Tasks

Add these state branches:

```js
G.buildings = {
  slots: [
    { id, buildingId, level, status, buildTimer, buildDuration }
  ],
  activeBuildSlotId,
  unlockedBuildingIds
}

G.army = {
  units: { majors, swarmers, spitters, scouts },
  wounded: { majors, swarmers, spitters, scouts },
  trainingQueue,
  formation
}

G.campaign = {
  selectedTerritoryId,
  territories: {
    [id]: { state, unlocked, claimed, battleCleared }
  }
}

G.battle = null | {
  territoryId,
  round,
  phase,
  playerSnapshot,
  enemySnapshot,
  log,
  result
}
```

- Add save migration:
  - Existing resources map into the new resource model.
  - Existing upgrades map into equivalent completed chambers when possible.
  - Existing claimed nodes map into claimed campaign territories when possible.
  - If migration is risky, provide a clear "Start New Conquest Save" fallback.
- Add derived selectors:
  - production rates.
  - army power.
  - battle readiness.
  - unlocked chambers.
  - territory rewards.

### Claude Tasks

- Define exact starting values:
  - Initial resources.
  - Initial population.
  - Initial chamber slots.
  - Starting unlocked chambers.
  - Starting unlocked territory.
- Create a tuning table for resource production and upkeep.

### Gemini Tasks

- No new assets.

### Acceptance Criteria

- New state initializes cleanly.
- Existing or fresh save loads without console errors.
- Resource rates calculate from buildings, workers, and territory rewards.

## Phase 3: Timed Chamber Building System

### Codex Tasks

Build the `Build` tab:

- Show six fixed chamber slots in a 2x3 layout.
- Slot states:
  - Empty.
  - Building.
  - Complete.
  - Upgrade available.
  - Locked.
- Tapping an empty slot opens a chamber picker.
- Tapping an existing chamber opens details and upgrade action.
- Add one active construction timer for the first pass.
- Construction completes automatically.
- Offline time advances construction timers safely.

Initial chamber definitions:

| Chamber | Unlock | Cost | Build Time | Effect |
|---|---:|---:|---:|---|
| Granary Chamber | Start | 20 Materials | 20s | +80 Food cap |
| Reinforced Store | Start | 20 Materials | 20s | +60 Materials cap |
| Brood Nursery | Start | 35 Materials | 45s | +25% population growth |
| Barracks | Start | 30 Materials | 45s | Unlock Major training |
| Acid Gallery | Compost Edge | 55 Materials, 10 Chitin | 90s | Unlock Spitter training |
| Scout Tunnel | Mossy Log | 40 Materials | 60s | Unlock Scouts and campaign preview bonuses |
| Chitin Forge | Cracked Paver | 75 Materials, 25 Chitin | 120s | +20% army armor |
| Command Chamber | Sunken Patio | 100 Materials, 40 Chitin | 180s | Unlock advanced formations |

### Claude Tasks

- Refine chamber names, descriptions, and unlock copy.
- Write short completion messages for each chamber.
- Produce balance notes for costs and build times.

### Gemini Tasks

- If current `tiles_nest_chambers.png` cannot map cleanly to these chambers, generate a new chamber icon sheet:
  - Granary.
  - Store.
  - Nursery.
  - Barracks.
  - Acid Gallery.
  - Scout Tunnel.
  - Chitin Forge.
  - Command Chamber.

### Acceptance Criteria

- Player can start and finish chamber construction.
- Timers persist after refresh.
- Offline time completes chambers but does not trigger combat.
- Chamber effects alter rates, caps, or unlocks.

## Phase 4: Army Training And Formation System

### Codex Tasks

Build the `Army` tab:

- Show unit rows:
  - Major.
  - Swarmer.
  - Spitter.
  - Scout.
- Each row includes:
  - sprite/icon.
  - current count.
  - wounded count.
  - role.
  - train button.
  - resource cost.
  - training time.
- Add a simple training queue.
- Training consumes adult population plus resources.
- Wounded units recover over time instead of being permanently lost in every battle.
- Formation selector:
  - Balanced.
  - Shield Wall.
  - Swarm Rush.
  - Acid Line.
- Readiness display:
  - Safe.
  - Risky.
  - Dangerous.

Initial unit definitions:

| Unit | Unlock | Cost | Time | Role |
|---|---|---:|---:|---|
| Major | Barracks | 8 Food, 4 Chitin, 1 adult | 18s | Front armor breaker |
| Swarmer | Start | 6 Food, 1 adult | 10s | Fast melee pressure |
| Spitter | Acid Gallery | 10 Food, 6 Chitin, 1 adult | 22s | Backline ranged |
| Scout | Scout Tunnel | 5 Food, 1 Pheromone, 1 adult | 12s | Campaign intel/support |

### Claude Tasks

- Write role descriptions for unit rows.
- Define formation bonuses and penalties in plain language.
- Create training tutorial copy.

### Gemini Tasks

- No new assets unless unit sprites fail mobile readability.
- Optional: generate audio snippets/prompts for training complete and army ready.

### Acceptance Criteria

- Units can be trained and persist.
- Unit unlocks respect chamber state.
- Formation changes battle preview.
- Training timers advance offline safely.

## Phase 5: Campaign Map Redesign

### Codex Tasks

Build the `Campaign` tab:

- Replace scout/attack/claim with territory progression.
- Use large illustrated nodes from existing node sheets.
- Each territory card shows:
  - name.
  - state.
  - enemy army summary.
  - reward.
  - unlocks.
  - readiness.
  - battle button.
- Territory states:
  - Locked.
  - Available.
  - Scouted.
  - Battle Ready.
  - Claimed.
  - Threatened.
- Do not start combat while offline.

Initial campaign territories:

| Territory | Enemy | Reward | Unlocks |
|---|---|---|---|
| Sunny Rock | Beetle Guard | +10% Food | Mossy Log, Compost Edge |
| Mossy Log | Spider Scouts | +15% Growth | Scout Tunnel, Cracked Paver |
| Compost Edge | Earwig Swarm | +25 Food cap | Acid Gallery, Fallen Plum |
| Cracked Paver | Ironclad Beetles | +20% Materials | Chitin Forge |
| Fallen Plum | Hornet Nest | +10% Food, +10% Growth | Boss warning |
| Sunken Patio | Rival Colony Army | New biome access | Command Chamber |

### Claude Tasks

- Write territory descriptions and reward copy.
- Define enemy army compositions for each territory.
- Define territory unlock order and difficulty curve.

### Gemini Tasks

- No new assets needed unless a territory node sheet is missing or unclear.
- Optional: generate a campaign background plate if node map feels too plain.

### Acceptance Criteria

- Claimed territories unlock adjacent territories.
- Claimed rewards affect the economy or unlock chambers.
- Campaign can be understood at a glance on mobile.

## Phase 6: Three-Round Army Combat

### Codex Tasks

Build the new battle system:

- Battle preview opens from Campaign.
- Battle runs for exactly 3 auto-resolving rounds.
- Each round resolves:
  - Frontline clash.
  - Swarm pressure.
  - Ranged/support.
- Use unit pools rather than individual soldiers.
- Show grouped player army sprites on the left.
- Show grouped enemy army sprites on the right.
- Show round-by-round log in the bottom panel.
- Outcomes:
  - Clean Victory.
  - Costly Victory.
  - Retreat.
  - Defeat.
- Results apply:
  - territory claimed on victory.
  - resource reward.
  - wounded units.
  - possible permanent losses only on severe defeat.
  - recovery timer.
- Reduced motion disables heavy shake/flashes.

Suggested combat math:

```text
frontPower = majors * formationMajorMult + swarmers * 0.6
swarmPower = swarmers * formationSwarmerMult + scouts * 0.4
rangedPower = spitters * formationSpitterMult + scouts * 0.3

roundScore = playerLanePower / enemyLanePower
3 round scores determine final outcome.
```

### Claude Tasks

- Define enemy army tables for all territories.
- Write battle log line pools:
  - Major hit.
  - Swarmer surge.
  - Spitter volley.
  - Scout support.
  - Enemy counter.
  - Victory.
  - Defeat.
- Write result explanations that tell the player what to do next.

### Gemini Tasks

- Generate or revise music/SFX assets:
  - Calm colony loop.
  - Build complete sting.
  - Training complete tick.
  - Campaign map ambience.
  - Battle start sting.
  - Round hit crunch.
  - Acid volley.
  - Victory sting.
  - Defeat sting.
- If audio generation is not available, create detailed prompts and filenames for a later audio tool.

### Acceptance Criteria

- Battle preview fits mobile.
- Battle resolves without player tapping.
- Three rounds are visible and understandable.
- Results update army, resources, and campaign state.
- Offline time never triggers battle consequences.

## Phase 7: Nest And Economy Integration

### Codex Tasks

Update the `Nest` tab:

- Show resource overview.
- Show current construction timer.
- Show current training timer.
- Show production rates.
- Show territory bonus summary.
- Show next recommended action:
  - Build chamber.
  - Train army.
  - Start campaign battle.
  - Recover wounded.
- Use top visual area to reflect selected tab:
  - Nest: nest/chamber scene.
  - Build: chamber slot layout.
  - Army: army muster scene.
  - Campaign: territory map.
  - Battle: battlefield.

### Claude Tasks

- Write recommendation text rules.
- Review all player-facing labels for clarity and mobile length.
- Ensure "Found New Colony" terminology is used instead of ant jargon where needed.

### Gemini Tasks

- Optional: generate army muster background if current garden/nest backgrounds do not work.
- Optional: generate simple audio bed for nest/economy idle state.

### Acceptance Criteria

- Nest tab tells the player what is happening and what to do next.
- Economy changes are visible without opening multiple panels.
- No key status is hidden in a scroll area.

## Phase 8: Mobile QA And Polish

### Codex Tasks

- Test in browser at:
  - 360x740.
  - 390x844.
  - 420x900.
- Verify:
  - no primary tab scroll.
  - no text overflow.
  - 44px touch targets.
  - save/load after construction.
  - save/load after training.
  - save/load after campaign unlock.
  - save/load after battle.
  - offline timers advance.
  - offline battles do not happen.
  - reduced motion changes battle visuals.
- Add a small debug reset/migrate control if useful.

### Claude Tasks

- Run a design review from screenshots.
- Produce a bug list ranked P0/P1/P2.
- Check whether the game loop is understandable in the first 3 minutes.
- Review tone: cozy ant colony with dramatic combat, not ninja parody.

### Gemini Tasks

- Revise any visual assets called out by QA:
  - unclear icons.
  - bad sprite readability.
  - overly busy backgrounds.
  - assets with baked-in text.
- Revise audio assets if they clash or feel too intense.

### Acceptance Criteria

- The redesign is playable end to end on mobile browser.
- A player can build a chamber, train units, win a battle, claim a territory, and unlock a new chamber.
- No primary interaction requires desktop behavior.

## Implementation Sequence For Parallel Work

### Sprint A: Foundations

Codex:
- Four-tab mobile shell.
- New state model.
- Save migration.

Claude:
- Final starting values.
- Building and unit copy.
- First-run tutorial outline.

Gemini:
- No new work unless requested.

### Sprint B: Build And Train

Codex:
- Chamber slots and construction timers.
- Army training queue.
- Formation selector.

Claude:
- Balance table for costs/timers.
- Training tutorial copy.

Gemini:
- Chamber icon revisions if needed.
- Training complete audio prompt/assets.

### Sprint C: Campaign And Combat

Codex:
- Territory map.
- Battle preview.
- Three-round auto battle.
- Results and rewards.

Claude:
- Enemy army tables.
- Battle log copy.
- Result explanation copy.

Gemini:
- Battle SFX/music.
- Campaign ambience.
- Revise enemy/FX assets if needed.

### Sprint D: Mobile QA And Polish

Codex:
- Mobile layout fixes.
- Reduced motion polish.
- Save/load regression fixes.

Claude:
- Screenshot review.
- First-3-minutes UX review.
- Bug prioritization.

Gemini:
- Final art/audio revisions.

## Handoff Checklist

Before marking the redesign complete:

- [ ] Four mobile tabs exist and fit without primary scrolling.
- [ ] Chamber construction works with timers and offline progress.
- [ ] Unit training works with timers and unlocks.
- [ ] Campaign territories unlock through victories.
- [ ] Battles are three-round auto-resolving army fights.
- [ ] Battle results update army, resources, and territory state.
- [ ] Offline progress never triggers battle damage.
- [ ] Existing assets are used from `assets/`.
- [ ] Gemini-only asset requests are recorded with filenames and prompts.
- [ ] 360x740 and 390x844 mobile checks pass.

