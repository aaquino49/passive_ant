# Passive Ant Development Milestones

Source reviewed: `ant_colony_game_concept.md`

This roadmap expands the initial five-milestone plan into buildable requirements for a mobile-browser idle game. The game should be playable in portrait orientation, support short check-ins, and keep the player's main interaction focused on macro-allocation rather than individual ant control.

## Design Pillars

- Mobile-first colony control: the player should be able to play comfortably with one thumb.
- Passive systems with meaningful pressure: most production runs automatically, but allocation choices should matter.
- Cozy brutalism: calm colony growth interrupted by short, severe, stylized combat payoffs.
- Active danger, safe offline progress: catastrophic events only escalate while the game is open.
- Readable at a glance: every major state must be understandable on a phone screen without dense tooltips.

## Milestone 1: Core Simulation Prototype

### Goal
Prove the idle economy and colony state model before investing in final UI, art, map content, or combat spectacle.

### Player Experience
The player opens a simple mobile layout, sees colony resources ticking, adjusts population allocations, and immediately understands the tradeoff between growth, building, and safety. The prototype can be mostly text and simple shapes, but the resource loop must already feel responsive.

### Required Gameplay Scope
- Time-step simulation loop for active play.
- Save/load with browser local storage.
- Offline progression calculation using timestamp delta.
- Three starting roles:
  - Foragers generate Food.
  - Excavators generate Materials.
  - Soldiers generate Security.
- Population model:
  - Total population.
  - Available workers.
  - Larvae or brood count.
  - Assigned population by role.
  - Unassigned idle population, if assignment is not forced to 100%.
- Resource model:
  - Food current amount.
  - Food cap.
  - Materials current amount.
  - Materials cap.
  - Security score.
- Food upkeep tied to population.
- Growth loop where Food surplus supports larvae or population growth.
- Materials loop where Materials are spent on early nest upgrades.
- Starvation warning state when Food trends negative.
- Conservative offline income that cannot trigger catastrophic events.

### Detailed Requirements
- Allocation controls must enforce valid totals and prevent impossible states.
- Production and consumption rates must be visible in debug form.
- The simulation must support speed-up testing so a developer can observe long-term balance quickly.
- The game must handle returning from sleep, background tab throttling, and clock deltas without producing absurd rewards.
- Offline rewards must have a maximum cap.
- Starvation must have at least one non-lethal early consequence, such as reduced growth or reduced production.
- The first upgrade costs must be low enough to complete within a short test session.
- All tuning values should live in one clearly editable data area.

### Data And State Requirements
- `resources`: Food, Materials, optional Chitin placeholder.
- `caps`: Food cap, Materials cap.
- `population`: total, larvae, assigned counts or percentages.
- `rates`: production per role, upkeep, growth, storage modifiers.
- `upgrades`: purchased nest upgrades and current levels.
- `session`: last saved timestamp, current tick, offline reward summary.
- `status`: stable, hungry, starving, capped, growing.

### Mobile Requirements
- Portrait-first layout target around 390 x 844.
- Must remain usable at 360 x 740.
- All core controls reachable in the lower portion of the screen.
- Sliders, steppers, or segmented controls must be large enough for touch.
- Resource changes must be readable without tiny text.
- No hover-only interactions.
- Avoid precision dragging as the only way to adjust allocations.

### Implementation Notes
- Keep the first version deterministic and inspectable.
- Use plain placeholder visuals.
- Build simulation logic separately from rendering and UI.
- Add a reset button for testing.
- Add a debug readout for active rates, offline delta, warnings, and save status.

### Acceptance Criteria
- Player can assign ants across Foragers, Excavators, and Soldiers.
- Resources update continuously while the game is open.
- Closing and reopening the browser restores state.
- Offline progress grants resources but does not trigger combat, starvation panic, or supply breaches.
- The game can run for at least 30 simulated minutes without the economy breaking.
- A developer can tune production, upkeep, storage, and growth values without touching UI code.

### Questions To Resolve
- Is population assignment percentage-based only, or can players later lock exact numbers?
- Should Food represent leaves, fungus, or a combined food resource?
- Does population grow automatically from surplus Food, or through a player-triggered nursery upgrade?
- How harsh should starvation be in the earliest playable version: slowdown, population loss, or only warning state?
- Should there be a hard idle population penalty, or is unassigned population simply neutral?

## Milestone 2: Mobile Colony Dashboard

### Goal
Turn the simulation into the first real mobile play surface: top living viewport, bottom control deck, clear resource feedback, and early upgrades.

### Player Experience
The player should feel like a hive mind making calm but consequential colony decisions. The top viewport should show a soft, living nest, while the bottom deck gives fast control over allocations, upgrades, and colony status.

### Required Gameplay Scope
- Vertical split-screen layout:
  - Top 40% living nest viewport.
  - Bottom 60% control deck.
- Resource header with Food, Materials, Population, and Security.
- Allocation controls for Foragers, Excavators, and Soldiers.
- Role-specific production previews.
- Nest status strip:
  - Stable.
  - Hungry.
  - Food capped.
  - Materials capped.
  - Growing.
  - Recovering.
- Upgrade panel for early nest improvements:
  - Food storage.
  - Material storage.
  - Nursery chamber.
  - Guard chamber.
- Simple animated nest activity using placeholder or first-pass art.
- First-run onboarding for the first few interactions.
- Settings panel with audio, haptics, reduced motion, and reset controls.

### Detailed Requirements
- Allocation changes must update production previews immediately.
- The UI must show why a role matters, not only its current percentage.
- The player must see when a resource is capped and what upgrade increases the cap.
- Upgrades must show current level, cost, benefit, and whether the player can afford them.
- The player must have a clear primary action at all times.
- Warning states must be visually distinct without relying only on color.
- Onboarding should be dismissible and should not block repeated play.
- The dashboard must handle empty, loading, first-run, normal, warning, and capped states.

### UI Requirements
- Use stable vertical zones so the viewport and control deck do not jump when numbers change.
- Use large touch targets for role controls and upgrade buttons.
- Use compact labels and icons where possible.
- Use a bottom-sheet or tab structure only if the dashboard becomes too dense.
- Avoid deep menus for core actions.
- Keep important status information visible while adjusting allocations.

### Mobile Requirements
- One-handed use should be comfortable.
- Controls must avoid accidental changes during scrolling.
- Important numbers should be visible near the controls they affect.
- The bottom deck should not require precision tapping.
- The viewport should remain readable on small phones.
- Text must not overlap or overflow at 360 px wide.

### Implementation Notes
- Use CSS layout constraints instead of viewport-scaled text.
- Keep the first art pass modular so generated assets can replace placeholders.
- Separate nest animation state from economy state.
- Store onboarding completion in local storage.

### Acceptance Criteria
- The game is playable entirely on mobile browser in portrait orientation.
- A new player can identify what each role produces or protects within one minute.
- Resource warnings are visible without opening a menu.
- Allocation changes cause immediate visual and numerical feedback.
- Layout remains stable at 360 x 740, 390 x 844, and desktop mobile emulation.
- Upgrades can be bought, persisted, and reflected in production or caps.

### Questions To Resolve
- Should the top viewport show an anthill cross-section, an outdoor trail, or switch between both?
- Is the player meant to spend most time in a single dashboard, or should there be tabs for Nest, Map, Combat, and Evolution?
- Should the game use explicit tutorial text, or mostly visual nudges?
- How dense should the bottom deck be: cozy and spacious, or compact and systems-heavy?
- Should the resource header remain sticky when the player scrolls the control deck?

## Milestone 3: Trail System And Expansion

### Goal
Add the node-based outdoor map so the colony has strategic direction beyond local production optimization.

### Player Experience
The player scouts nearby garden locations, commits ants to slow-burn sieges, claims nodes, and starts feeling the tension of supply lines. Expansion should feel tempting but risky, especially when the colony pushes too far without enough Soldiers.

### Required Gameplay Scope
- Outdoor map view or map panel.
- Initial biome: Garden.
- Fixed starting node graph with at least five nodes:
  - Mossy Log.
  - Sunny Rock.
  - Cracked Paver.
  - Compost Edge.
  - Fallen Plum.
- Node states:
  - Hidden.
  - Scoutable.
  - Scouting.
  - Revealed.
  - Siege Ready.
  - Under Siege.
  - Claimed.
  - Threatened.
  - Breached.
- Scouting timer.
- Siege preparation timer.
- Claim action and claim reward.
- Node bonuses that modify production, storage, scouting, defense, or growth.
- Supply line length score.
- Security requirement based on distance and number of active nodes.
- Breach warning system that only escalates during active sessions.

### Detailed Requirements
- Each node must have a name, short description, distance, threat type, scouting duration, siege duration, reward, and supply cost.
- The map must communicate which node is the next best action without forcing a specific path.
- Scouting should reveal enough information to make siege decisions meaningful.
- Claimed nodes should provide bonuses strong enough to feel worth the risk.
- Breach warnings should include cause, affected node, time pressure, and recommended response.
- Breached nodes should be recoverable through repair, resupply, or combat.
- Frontier pressure should increase gradually, not surprise-punish the player.
- Offline time must pause breach escalation and active siege danger.

### Data And State Requirements
- `nodes`: id, name, biome, position, connections, distance, state.
- `nodeRewards`: production multiplier, cap bonus, unlock, or modifier.
- `nodeThreats`: enemy type, risk rating, preferred counters.
- `trailStatus`: active, threatened, breached, repairing.
- `timers`: scouting, siege, breach warning, repair.
- `mapProgress`: claimed count, unlocked nodes, current frontier.

### Mobile Requirements
- Nodes must be large tap targets.
- The player must be able to inspect a node without losing map context.
- Timers should be legible without opening a modal.
- The map should support a small fixed graph without requiring pinch zoom.
- If the map scrolls, it must not conflict with slider controls.

### Implementation Notes
- Start with a fixed hand-authored graph rather than procedural generation.
- Store node definitions in data so future biomes can reuse the structure.
- Implement warnings before implementing harsh punishment.
- Build node rewards in a generic modifier system.
- Use placeholder icons until final assets arrive.

### Acceptance Criteria
- Player can scout, reveal, siege, and claim at least five nodes.
- Claimed nodes modify production, storage, growth, or combat in noticeable ways.
- Overextension risk increases as claimed nodes get farther from the nest.
- Supply Line Breach cannot trigger while the app is closed.
- The player receives a clear warning before a breach becomes costly.
- A breached node can be repaired or reclaimed.

### Questions To Resolve
- Is the map a grid, a branching trail, or a radial colony territory view?
- Should claimed nodes be permanent forever, or can predators temporarily disable them?
- How much agency does the player have during a siege: allocation only, unit composition, consumables, or none?
- Should every node have a unique story flavor, or only important frontier nodes?
- Should a player be able to abandon a claimed node intentionally?

## Milestone 4: Combat, Loss, And Recovery

### Goal
Deliver the game's signature contrast: cozy passive management leading into short, brutal, readable combat events.

### Player Experience
Combat should be hands-off, decisive, and intense. The player should feel the cost of bad preparation without needing twitch skill. Victory should feel crunchy and satisfying; defeat should hurt but clearly explain what went wrong.

### Required Gameplay Scope
- Auto-resolving combat sequence lasting about 5-10 seconds.
- Battle preview before commitment.
- Combat result generated from colony prep, unit mix, threat type, node conditions, and supply status.
- Unit types:
  - Workers: defenseless, vulnerable, can be carried off.
  - Majors: slow armored crushers.
  - Swarmers: fast fragile melee attackers.
  - Spitters: ranged acid attackers.
- Enemy archetypes:
  - Soft-bodied scavenger.
  - Armored beetle.
  - Fast predator.
  - Flying threat.
  - Heavy boss insect.
- Matchup rules based on specialization.
- Results screen with casualties, salvage, claimed rewards, breach status, and recovery recommendations.
- Harvesting phase where fallen units can become Chitin or enemy reward material.
- Starvation Panic warning state tied to Food deficit.
- Haptic hooks for supported mobile browsers.
- Reduced intensity, reduced motion, and vibration toggles.

### Detailed Requirements
- Battle preview must communicate expected danger without revealing exact deterministic outcomes.
- Combat must have at least three outcome bands: clean victory, costly victory, defeat.
- Unit counters must be legible:
  - Majors perform well against armor.
  - Swarmers perform well against large slow targets.
  - Spitters perform well when protected.
  - Workers suffer when exposed.
- Defeat must produce a recoverable setback such as casualties, lost resources, breached trail, disabled node, or repair cost.
- Salvage should soften losses without making defeat desirable.
- Combat events should be short enough for mobile sessions but dramatic enough to feel like a payoff.
- Haptics must never be required for understanding.
- Visual intensity settings must reduce screen shake and flashing.

### Combat State Requirements
- `combatants`: unit counts, enemy type, modifiers.
- `battlePreview`: risk, counters, expected casualty range.
- `battleTimeline`: scripted beats for animation playback.
- `battleResult`: victory state, casualties, salvage, rewards, disabled nodes.
- `recovery`: recommended allocation or repair action.

### Mobile Requirements
- Combat should be readable in the top viewport without tiny sprites.
- The player should not need to tap rapidly.
- Result details must fit in a compact readable panel.
- Haptics must be optional.
- Visual effects should be punchy but not obscure outcome information.

### Implementation Notes
- Build combat as a data-driven simulation first, then add animation timing.
- Use exaggerated hit pauses, screen shake, and color bursts instead of realistic gore.
- Respect browser support limits for vibration APIs.
- Keep severe outcomes fair by previewing risk and showing clear causes after defeat.
- Log combat details in debug mode for tuning.

### Acceptance Criteria
- Player can start a siege and watch a complete auto-battle.
- Combat result is determined by colony prep, unit mix, node threat, and supply condition.
- At least three enemy archetypes produce meaningfully different outcomes.
- Casualties and salvage feed back into the economy.
- Starvation Panic and Supply Line Breach states are visible, understandable, and recoverable.
- Reduced intensity mode visibly reduces shake, flashes, and vibration.

### Questions To Resolve
- How graphic should "cozy brutalism" be: candy-like fracture only, or visible stylized insect dismemberment?
- Are combat unit types unlocked over time, or available from the start?
- Should players choose specific combat squads, or does Soldier allocation automatically determine unit mix?
- Should defeat destroy progress, temporarily disable nodes, kill population, or mainly create recovery costs?
- Should combat be replayable as a log, or vanish after the results screen?

## Milestone 5: Prestige, Biomes, And Vertical Slice Polish

### Goal
Integrate the full loop into a polished vertical slice: colony growth, expansion, combat, setback, recovery, and prestige through Nuptial Flight.

### Player Experience
The player should reach the colony's structural limit, choose to launch a Nuptial Flight, and understand why starting over in a tougher biome is exciting. The first prestige should feel like a conclusion and a promise of a larger game.

### Required Gameplay Scope
- Colony structural limit based on nest upgrades, population, claimed nodes, queen maturity, or a combination.
- Nuptial Flight prestige event.
- Permanent genetic mutations or evolutionary modifiers.
- Second biome unlocked after prestige, such as Patio.
- Biome-specific node content and threat adjustments.
- Mutation choices:
  - Stronger mandibles: better Major damage.
  - Efficient digestion: lower Food upkeep.
  - Deep-memory trails: faster scouting.
  - Resin shells: better breach resistance.
  - Acid glands: stronger Spitters.
- Dynamic audio state hooks:
  - Calm colony.
  - Productive bustle.
  - Hunger warning.
  - Siege preparation.
  - Combat impact.
  - Recovery.
- Push-notification copy plan for future PWA implementation.
- Final mobile UX pass.
- Mobile performance pass.

### Detailed Requirements
- Prestige eligibility must be visible before the player reaches it.
- The player must see what will reset and what will persist.
- Mutation choices must show plain-language benefits.
- Prestige should not erase settings or accessibility preferences.
- The second biome must feel mechanically different, not only visually different.
- The vertical slice must include at least one full loop from new colony to prestige.
- The player should receive a concise post-prestige summary.
- Audio and haptics must be gated by browser permission and user settings.

### Prestige State Requirements
- `prestige`: generation count, available mutations, selected mutations, permanent modifiers.
- `biomes`: unlocked biomes, current biome, biome modifiers.
- `resetRules`: resources reset, nodes reset, upgrades reset, mutations persist.
- `progression`: first prestige target and future prestige scaling.

### Mobile Requirements
- Prestige must be understandable on a phone without long tooltips.
- Major choices should fit in simple readable panels.
- Performance should stay stable on mid-range phones.
- Audio and haptics must respect browser autoplay and permission limitations.
- All prestige and mutation panels must fit at 360 px width.

### Implementation Notes
- Make the first prestige short enough to test repeatedly during development.
- Use a clear before-and-after comparison for mutation choices.
- Keep biome content data-driven so future areas can be added without structural rewrites.
- Treat push notifications as optional future platform work unless the game becomes a PWA.
- Add a credits/version/debug area for QA builds.

### Acceptance Criteria
- Player can complete a full loop from new colony to first Nuptial Flight.
- Prestige resets baseline progress and grants a permanent modifier.
- Second biome changes difficulty or node behavior in a visible way.
- Audio, haptics, visuals, and UI states support the cozy/brutal contrast.
- The vertical slice is playable on mobile browser without installation.
- The game remains playable after prestige without save corruption.

### Questions To Resolve
- How long should the first full prestige loop take: 20 minutes, 1 hour, several hours, or multiple days?
- Should Nuptial Flight be player-triggered only, or sometimes forced by colony pressure?
- Are mutations random choices, a tree, achievements, or purchased upgrades?
- Does the game need PWA features for offline play and notifications, or should it remain a normal browser page first?
- Should future biomes be linear, branching, or selectable?

## Cross-Milestone QA Requirements

- Test on mobile browser dimensions at 360 x 740 and 390 x 844.
- Test in desktop browser mobile emulation.
- Confirm save/load after every milestone.
- Confirm offline progress cannot trigger catastrophic events.
- Confirm the UI remains readable with long resource values.
- Confirm reduced motion and haptic settings persist.
- Confirm reset works without clearing unrelated browser data.
- Confirm no core action depends on hover.
- Confirm no control requires fast tapping.

## Cross-Milestone Design Questions

These questions should be answered before final tuning and content production.

1. What is the target session shape: frequent 30-second check-ins, 5-minute active bursts, or longer evening sessions?
2. Is the intended audience casual idle-game players, strategy players, cozy-game players, horror-comedy fans, or a blend?
3. Should combat be rare and memorable, or frequent and rhythmic?
4. What level of player punishment feels right for the fantasy: gentle setbacks, meaningful losses, or colony-threatening failures?
5. Should the tone be cute with shocking moments, darkly funny, or genuinely tense?
6. Are ants meant to be named or individualized at all, or always treated as a collective?
7. Should predators be biologically grounded, fantastical, or stylized versions of real garden threats?
8. Is monetization off the table, or should the design avoid patterns that would later conflict with premium, ads, or cosmetic models?
9. Should the game be playable with one thumb only, or is two-handed portrait play acceptable?
10. What browsers and devices matter most for testing: iPhone Safari, Android Chrome, desktop preview, or all three?
11. Should the game be designed as a normal browser page first, or as a PWA from the start?
12. How much written flavor should appear in the game: minimal labels, short node text, or story-rich colony events?

