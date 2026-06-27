# Passive Ant Asset Style Guide And Checklist

This guide is written for Gemini asset generation. The game is a mobile-browser portrait idle game with a cozy colony-management surface and sudden stylized combat violence.

Use the checkboxes and status fields to track which assets have been generated, reviewed, revised, and integrated.

Status key:
- `Needed`: not generated yet.
- `Generated`: Gemini output exists.
- `Needs Revision`: output exists but needs changes.
- `Approved`: ready for implementation.
- `Integrated`: added to the game.

## Creative Direction

### Core Style
16-bit inspired pixel art with modern polish. The world should feel soft, tactile, and inviting at rest, then snap into bright, crunchy, exaggerated combat effects during danger.

### Style Phrase
Cozy brutalism: plush garden diorama meets candy-colored insect combat.

### Mood Contrast
- Normal play: warm, calm, productive, tiny, alive.
- Warning states: dimmer, tighter, more anxious, still readable.
- Combat: sharp impacts, hard shell fractures, neon ichor, short bursts of chaos.
- Prestige: airy, hopeful, mythic, and slightly strange.

### Visual Priorities
- Readable on mobile.
- Big silhouettes over tiny detail.
- Clear role identity for ants.
- Distinct node states.
- Strong resource icons.
- Effects that sell impact without becoming realistic gore.
- No text baked into assets unless specifically requested.

## Canvas And Technical Assumptions

- Target orientation: portrait mobile browser.
- Reference viewport: 390 x 844.
- Smallest test viewport: 360 x 740.
- Main viewport area: top 40% of screen.
- Controls: bottom 60% of screen.
- Preferred character format: transparent PNG sprite sheets.
- Preferred icon format: transparent PNG, square canvas.
- Preferred background format: PNG plates, with foreground and background layers when useful.
- Suggested icon source size: 256 x 256, downscaled in-game.
- Suggested sprite frame size: 64 x 64 for small ants, 96 x 96 or 128 x 128 for large enemies.
- Suggested background plate: 780 x 680 or larger, designed to crop safely inside the top viewport.
- Avoid UI text inside images; game text should be rendered by the app.

## Palette Direction

### Resting Colony Palette
- Moss green.
- Leaf green.
- Warm soil brown.
- Soft amber.
- Mushroom cream.
- Petal pink accents.
- Sky blue highlights.

### Combat Palette
- Lime ichor.
- Acid yellow.
- Shell black.
- Beetle teal.
- Warning red-orange.
- Bruised violet shadows.

### Prestige Palette
- Warm sunrise gold.
- Pale sky blue.
- Transparent wing cyan.
- Soft queen amber.
- New-biome accent color.

### Avoid
- Realistic blood red as the main combat fluid.
- Muddy low-contrast browns.
- Overly dark horror palettes.
- Monochrome green-only UI.
- Hyper-realistic insect textures.
- AI-generated text, logos, signatures, or watermarks.

## Pixel Art Rules

- Use clean pixel clusters, not noisy dithering.
- Silhouettes should read at a glance.
- Each unit should have a clear color accent and body shape.
- Use 1-2 highlight colors per sprite.
- Maintain consistent light direction across related assets.
- Use transparent backgrounds for sprites, effects, and icons.
- Keep animation frames aligned to a consistent baseline.
- Combat effects can be larger and more abstract than the bodies causing them.

## Asset Tracking Template

Copy this format for any new asset that gets added later.

```markdown
- [ ] Asset:
  - Status: Needed
  - Priority:
  - Milestone:
  - Filename:
  - Type:
  - Size / Frames:
  - Description:
  - Gemini Prompt:
  - Review Notes:
```

## Priority Batch For First Vertical Slice

**Status Update (M5): 18 assets generated & integrated. 25 remaining — see ASSET_PROMPTS_BATCH.md for ready-to-use Gemini prompts.**

### Integrated Assets (18): Nest cross-section background
  - Status: Generated
  - Priority: P0
  - Milestone: M2
  - Filename: `bg_nest_cross_section.png`
  - Type: Layered background plate
  - Size / Frames: 780 x 680 minimum, single image
  - Description: Warm soil cross-section with tunnels, nursery area, food pocket, material pocket, and queen chamber placeholder. Must leave space for ant sprites and resource overlays.
  - Gemini Prompt: Create a 16-bit inspired pixel art anthill cross-section background for a mobile portrait idle game. Warm soil layers, cozy tunnels, nursery chamber, storage pockets, no text, no UI, readable on phone screen.
  - Review Notes: Revised to remove all text labels (Nursery Chamber, Storage Pockets).

- [ ] Asset: Worker / Forager sprite sheet
  - Status: Generated
  - Priority: P0
  - Milestone: M2
  - Filename: `ant_worker_forager_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, idle 4, walk 6, carry 4, panic 4, collapse 4
  - Description: Small warm-brown ant with leaf-green satchel or carried leaf fragment. Friendly, busy, non-threatening silhouette.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a small worker forager ant in a cozy mobile idle game. Include idle, walk, carry leaf, panic, and collapse animations. Warm brown body, leaf green accent, readable at small size.
  - Review Notes: Revised to strip black background, giving true transparency (Format32bppArgb).

- [ ] Asset: Excavator sprite sheet
  - Status: Generated
  - Priority: P0
  - Milestone: M2
  - Filename: `ant_excavator_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, idle 4, dig 6, haul dirt 4, exhausted 4, celebrate 3
  - Description: Stockier worker ant with darker soil markings and oversized digging forelegs or mandibles.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a stocky excavator ant. Cozy 16-bit style, dark soil markings, oversized digging forelegs, idle, dig, haul dirt, exhausted, and celebrate poses.
  - Review Notes: Revised to cute chibi proportions matching the worker's style, stripped black background for true transparency.

- [ ] Asset: Soldier sprite sheet
  - Status: Generated
  - Priority: P0
  - Milestone: M2
  - Filename: `ant_soldier_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, guard 4, march 6, bite 4, recoil 3, defeated 4, victory 3
  - Description: Medium ant with darker shell, raised mandibles, and protective stance.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a defender soldier ant. Darker shell, raised mandibles, protective stance, guard, march, bite, recoil, defeated, and victory animations.
  - Review Notes: Revised again to completely remove baked-in labels and text bubbles (like 'ow!'). Stripped black background for true transparency.

- [ ] Asset: Core resource icon set
  - Status: Generated
  - Priority: P0
  - Milestone: M2
  - Filename: `icons_resources_core.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Food, Materials, Population, Security
  - Description: Four readable mobile icons: leaf/fungus Food, chitin/dirt Materials, ant cluster Population, mandible shield Security.
  - Gemini Prompt: Create a transparent PNG icon sheet for a cozy 16-bit ant colony mobile game. Icons: Food as leaf and fungus, Materials as chitin shard and dirt pellet, Population as tiny ant heads or eggs, Security as mandibles or ant-head shield. No text.
  - Review Notes: Revised to standalone borderless icons without card backgrounds or frames, stripped black background for true transparency.

## Ant Unit Assets

- [ ] Asset: Major / Crusher sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `ant_major_crusher_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 96 x 96 frames, idle 4, stomp 6, wind-up 4, crush 5, stagger 3, cracked 3
  - Description: Large heavy ant with oversized head and mandibles. Slow, armored, tank-like, almost cute because of its exaggerated proportions.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a massive ant major crusher. Oversized head and mandibles, tank-like silhouette, cozy but powerful, idle, stomp, wind-up, crush attack, stagger, shell-cracked pose.
  - Review Notes:

- [ ] Asset: Swarmer sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `ant_swarmer_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, run 6, leap 4, bite 4, cling 3, scatter 4, defeated 3
  - Description: Small fast red-orange ant variant with sharp silhouette and forward-leaning posture. Fragile but numerous.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a small fast swarmer ant. Red-orange accent, sharp forward-leaning silhouette, run, leap, bite, cling, scatter, and defeated poses.
  - Review Notes:

- [ ] Asset: Spitter sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `ant_spitter_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, idle 4, aim 4, spit 5, reload 4, panic 4, defeated 3
  - Description: Slim ant with acid-yellow abdominal markings and visible acid sac.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a ranged spitter ant. Slim body, acid-yellow abdomen markings, visible acid sac, idle, aim, spit, reload, panic, defeated poses.
  - Review Notes:

- [ ] Asset: Winged Queen sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M5
  - Filename: `ant_winged_queen_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 96 x 96 frames, resting 4, wing flare 5, launch 6, sky silhouette 3, landing 4
  - Description: Elegant larger ant with translucent wings, soft gold highlights, and calm focal presence.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a winged queen ant in a prestige scene. Elegant larger ant, translucent wings, soft gold highlights, resting, wing flare, launch, sky silhouette, and landing poses.
  - Review Notes:

## Enemy Creature Assets

- [ ] Asset: Soft-bodied scavenger sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `enemy_soft_scavenger_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, crawl 6, lunge 4, hit 3, burst 5, flee 4
  - Description: Squishy pale grub-like enemy. Low armor, unsettling but stylized.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a soft-bodied garden scavenger enemy. Pale grub-like body, squishy shape, not realistic gore, crawl, lunge, hit, burst, and flee poses.
  - Review Notes:

- [ ] Asset: Armored beetle sprite sheet
  - Status: Needed
  - Priority: P0
  - Milestone: M4
  - Filename: `enemy_armored_beetle_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 96 x 96 frames, idle 4, charge 6, crack stage 1, crack stage 2, topple 4, defeat 4
  - Description: Dark teal or black beetle with glossy segmented shell, designed for Major/Crusher counterplay.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for an armored beetle enemy. Dark teal glossy segmented shell, candy-like crack stages, idle, charge, two shell crack poses, topple, defeat. Stylized, no realistic gore.
  - Review Notes:

- [ ] Asset: Fast predator sprite sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `enemy_fast_predator_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 96 x 96 frames, stalk 4, dash 5, grab 4, carry-off 5, hit 3, defeated 4
  - Description: Spider-like or mite-like garden hunter with long angular legs and sharp motion. Threatens Workers and Spitters.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a fast garden predator. Spider-like or mite-like silhouette, angular legs, stalk, dash, grab, carry-off, hit, and defeated poses. Stylized and readable on mobile.
  - Review Notes:

- [ ] Asset: Flying threat sprite sheet
  - Status: Needed
  - Priority: P2
  - Milestone: M4
  - Filename: `enemy_flying_threat_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 64 x 64 frames, hover 4, dive 5, snatch 4, acid-hit 3, crash 5, defeated 3
  - Description: Small wasp or gnat-like predator hovering over the trail.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a small flying garden predator. Wasp or gnat inspired, hover, dive, snatch, acid-hit, crash, and defeated poses. Bright readable silhouette, no text.
  - Review Notes:

- [ ] Asset: Heavy boss insect sprite sheet
  - Status: Needed
  - Priority: P2
  - Milestone: M5
  - Filename: `enemy_heavy_boss_sheet.png`
  - Type: Transparent sprite sheet
  - Size / Frames: 128 x 128 frames, idle 4, intimidation 4, attack 6, armor break 4, enraged 4, collapse 6
  - Description: Large stylized centipede, beetle, or mantis-like threat for a major siege moment.
  - Gemini Prompt: Create a transparent PNG pixel art sprite sheet for a large boss insect in a cozy brutal ant game. Garden predator inspired, imposing silhouette, idle, intimidation, multi-hit attack, armor break, enraged state, collapse. Stylized, no realistic gore.
  - Review Notes:

## Colony And Environment Assets

- [ ] Asset: Nest chamber tile set
  - Status: Generated
  - Priority: P1
  - Milestone: M2
  - Filename: `tiles_nest_chambers.png`
  - Type: Tile or modular background sheet
  - Size / Frames: 128 x 128 or 256 x 256 per chamber/state
  - Description: Food storage, material storage, nursery, guard chamber, and queen chamber. Each needs empty, active, and upgraded variants.
  - Gemini Prompt: Create a pixel art modular chamber tile set for an ant colony nest. Include food storage, material storage, nursery, guard chamber, queen chamber, each in empty, active, and upgraded states. Warm soil, cozy, no text.
  - Review Notes: Generated in grid, stripped black background for true transparency.

- [ ] Asset: Garden surface plate
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `bg_garden_surface.png`
  - Type: Background plate
  - Size / Frames: 780 x 680 minimum, single image
  - Description: Felt-like moss, pebbles, grass stems, leaf litter, and warm light. Supports trails and combat overlays.
  - Gemini Prompt: Create a 16-bit inspired pixel art garden surface background for a mobile ant colony game. Felt-like moss, pebbles, grass stems, leaf litter, warm daylight, no text, readable on phone.
  - Review Notes:

- [ ] Asset: Pheromone trail line states
  - Status: Needed
  - Priority: P0
  - Milestone: M3
  - Filename: `fx_trail_states.png`
  - Type: Transparent overlay sheet
  - Size / Frames: Straight and curved segments for normal, active, scouting, siege, threatened, breached, repaired
  - Description: Trail visuals must clearly communicate route status on the map.
  - Gemini Prompt: Create transparent pixel art pheromone trail line overlays for a mobile ant game. Include normal, active, scouting, siege, threatened, breached, and repaired states. No text, readable at small size.
  - Review Notes:

- [ ] Asset: Patio biome background
  - Status: Needed
  - Priority: P2
  - Milestone: M5
  - Filename: `bg_patio_surface.png`
  - Type: Background plate
  - Size / Frames: 780 x 680 minimum, single image
  - Description: Cracked stone, dry dust, human-made edges, harsher light, more dangerous than Garden.
  - Gemini Prompt: Create a 16-bit inspired pixel art patio biome background for a mobile ant game. Cracked stone, dry dust, human-made edges, harsher sunlight, no text, readable on phone.
  - Review Notes:

- [ ] Asset: Compost biome background
  - Status: Needed
  - Priority: P3
  - Milestone: Future
  - Filename: `bg_compost_surface.png`
  - Type: Background plate
  - Size / Frames: 780 x 680 minimum, single image
  - Description: Rich dark soil, fungi, organic scraps, humid glow.
  - Gemini Prompt: Create a 16-bit inspired pixel art compost biome background for a mobile ant colony game. Rich dark soil, fungi, organic scraps, humid glow, cozy but strange, no text.
  - Review Notes:

## Map Node Assets

Each node should have a base tile plus visual states: locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached.

- [ ] Asset: Mossy Log node tile set
  - Status: Needed
  - Priority: P0
  - Milestone: M3
  - Filename: `node_mossy_log_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Soft green log fragment with fungus and tiny tunnels. Bonus direction: Food or nursery growth.
  - Gemini Prompt: Create a pixel art node tile state sheet for Mossy Log in a mobile ant colony game. Soft green log, fungus, tiny tunnels. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

- [ ] Asset: Sunny Rock node tile set
  - Status: Needed
  - Priority: P0
  - Milestone: M3
  - Filename: `node_sunny_rock_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Warm stone with bright light and heat shimmer. Bonus direction: faster scouting or soldier readiness.
  - Gemini Prompt: Create a pixel art node tile state sheet for Sunny Rock in a mobile ant colony game. Warm stone, bright light, heat shimmer. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

- [ ] Asset: Cracked Paver node tile set
  - Status: Needed
  - Priority: P0
  - Milestone: M3
  - Filename: `node_cracked_paver_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Stone crack with shelter shadows. Bonus direction: material storage or defense.
  - Gemini Prompt: Create a pixel art node tile state sheet for Cracked Paver in a mobile ant colony game. Cracked stone paver, shelter shadow, tiny ant trail. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

- [ ] Asset: Compost Edge node tile set
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `node_compost_edge_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Dark organic mound with fungi and scraps. Bonus direction: Food production but higher predator risk.
  - Gemini Prompt: Create a pixel art node tile state sheet for Compost Edge in a mobile ant colony game. Dark organic mound, fungi, scraps, humid glow. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

- [ ] Asset: Fallen Plum node tile set
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `node_fallen_plum_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Bright fruit piece with sticky surface and fly hints. Bonus direction: big Food boost, high flying-threat risk.
  - Gemini Prompt: Create a pixel art node tile state sheet for Fallen Plum in a mobile ant colony game. Bright sticky plum fruit, tiny fly hints, garden floor. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

- [ ] Asset: Sunken Patio node tile set
  - Status: Needed
  - Priority: P2
  - Milestone: M5
  - Filename: `node_sunken_patio_states.png`
  - Type: Node tile state sheet
  - Size / Frames: 128 x 128 per state
  - Description: Dangerous distant node with broken tile and shadowed cracks. Bonus direction: prestige or second-biome unlock.
  - Gemini Prompt: Create a pixel art node tile state sheet for Sunken Patio in a mobile ant colony game. Broken patio tile, deep shadowed crack, dangerous distant frontier mood. Include locked, scoutable, scouting, revealed, under-siege, claimed, threatened, breached. No text.
  - Review Notes:

## Resource And UI Icon Assets

- [ ] Asset: Extended resource icon set
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `icons_resources_extended.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Larvae, Chitin, Mutation, Queen, Supply
  - Description: Additional resource/progression icons for deeper systems.
  - Gemini Prompt: Create a transparent PNG icon sheet for a cozy 16-bit ant colony game. Icons: Larvae as soft cream larva, Chitin as glossy shell shard, Mutation as pheromone swirl, Queen as winged ant crown silhouette, Supply as dotted pheromone trail bundle. No text.
  - Review Notes:

- [ ] Asset: Role icon set
  - Status: Generated
  - Priority: P1
  - Milestone: M2
  - Filename: `icons_roles.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Forager, Excavator, Soldier, Major, Swarmer, Spitter
  - Description: Simple readable role icons for allocation controls and unit panels.
  - Gemini Prompt: Create a transparent PNG role icon sheet for a 16-bit ant colony mobile game. Icons: Forager, Excavator, Soldier, Major Crusher, Swarmer, Spitter. No text, strong silhouettes, readable small.
  - Review Notes: Generated in 3x2 grid, stripped black background for true transparency.

- [ ] Asset: Action icon set
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `icons_actions.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Scout, Siege, Claim, Repair, Upgrade, Prestige
  - Description: Command icons for map and colony actions.
  - Gemini Prompt: Create a transparent PNG action icon sheet for a cozy 16-bit mobile ant colony game. Icons: Scout, Siege, Claim, Repair Trail, Upgrade Chamber, Prestige Nuptial Flight. No text.
  - Review Notes:

- [ ] Asset: Warning icon set
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `icons_warnings.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Hunger, Starvation, Breach, Predator, Capped, Offline
  - Description: High-readability status icons that do not rely only on color.
  - Gemini Prompt: Create a transparent PNG warning icon sheet for a 16-bit ant colony mobile game. Icons: Hunger, Starvation, Supply Breach, Predator Threat, Resource Capped, Offline Progress. No text, readable on phone.
  - Review Notes:

- [ ] Asset: Settings icon set
  - Status: Generated
  - Priority: P2
  - Milestone: M2
  - Filename: `icons_settings.png`
  - Type: Transparent icon sheet
  - Size / Frames: 256 x 256 per icon, Settings, Audio On, Audio Off, Haptics On, Haptics Off, Reduced Motion
  - Description: Utility icons for settings and accessibility controls.
  - Gemini Prompt: Create a transparent PNG settings icon sheet for a cozy pixel art mobile game. Icons: Settings, Audio On, Audio Off, Haptics On, Haptics Off, Reduced Motion. No text.
  - Review Notes: Generated in 3x2 grid, stripped black background for true transparency.

## Combat Effect Assets

- [ ] Asset: Shell crack effect sheet
  - Status: Needed
  - Priority: P0
  - Milestone: M4
  - Filename: `fx_shell_crack.png`
  - Type: Transparent effect sheet
  - Size / Frames: 128 x 128 frames, small 3, medium 3, large 3
  - Description: Bright angular fracture overlays with hard-candy shell look.
  - Gemini Prompt: Create a transparent PNG pixel art effect sheet for hard-candy insect shell cracks. Small, medium, and large angular fracture bursts, bright highlights, no realistic gore, no text.
  - Review Notes:

- [ ] Asset: Lime ichor splash effect sheet
  - Status: Needed
  - Priority: P0
  - Milestone: M4
  - Filename: `fx_ichor_splash.png`
  - Type: Transparent effect sheet
  - Size / Frames: 128 x 128 frames, ground splat 4, burst 4, spray arc 4, fading stain 4
  - Description: Lime-green stylized insect fluid, readable as organic but not realistic gore.
  - Gemini Prompt: Create a transparent PNG pixel art effect sheet for stylized lime-green insect ichor. Ground splat, burst, spray arc, fading stain. Candy-colored, not realistic gore, no text.
  - Review Notes:

- [ ] Asset: Acid line effect sheet
  - Status: Needed
  - Priority: P0
  - Milestone: M4
  - Filename: `fx_acid_line.png`
  - Type: Transparent effect sheet
  - Size / Frames: 128 x 128 frames, launch 3, travel 4, impact 4, sizzling puddle 4
  - Description: Yellow-green corrosive projectile or stream from Spitters.
  - Gemini Prompt: Create a transparent PNG pixel art acid projectile effect sheet. Yellow-green corrosive line, launch, travel, impact, sizzling puddle, readable on mobile, no text.
  - Review Notes:

- [ ] Asset: Bite impact effect sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `fx_bite_impact.png`
  - Type: Transparent effect sheet
  - Size / Frames: 96 x 96 frames, snap 3, starburst 3, recoil mark 2
  - Description: Small starburst and mandible snap effect.
  - Gemini Prompt: Create a transparent PNG pixel art bite impact effect sheet for ant combat. Mandible snap, small starburst, recoil mark, sharp but stylized, no text.
  - Review Notes:

- [ ] Asset: Crush impact effect sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `fx_crush_impact.png`
  - Type: Transparent effect sheet
  - Size / Frames: 128 x 128 frames, wind-up dust 3, slam 4, dust ring 4, shell shards 4
  - Description: Heavy ground-slam effect with dust ring and shell shards.
  - Gemini Prompt: Create a transparent PNG pixel art crush impact effect sheet. Heavy ground slam, dust ring, shell shards, crunchy candy-shell feeling, no realistic gore, no text.
  - Review Notes:

- [ ] Asset: Panic dimmer overlay
  - Status: Needed
  - Priority: P1
  - Milestone: M4
  - Filename: `overlay_panic_dimmer.png`
  - Type: Transparent overlay
  - Size / Frames: 780 x 680 minimum, 2 variants
  - Description: Semi-transparent warning overlay for starvation panic; should dim the cozy background without hiding UI.
  - Gemini Prompt: Create transparent pixel art overlay textures for starvation panic in a cozy ant colony game. Dim amber-red edge treatment, anxious but readable, no text, no UI frame.
  - Review Notes:

- [ ] Asset: Supply breach effect sheet
  - Status: Needed
  - Priority: P1
  - Milestone: M3
  - Filename: `fx_supply_breach.png`
  - Type: Transparent effect sheet
  - Size / Frames: 128 x 128 frames, broken trail 3, warning pulse 4, predator shadow 3, repaired sparkle 3
  - Description: Broken pheromone trail effect with torn line, warning pulse, and predator shadow.
  - Gemini Prompt: Create a transparent PNG pixel art supply breach effect sheet for an ant trail. Broken pheromone line, warning pulse, predator shadow, repaired sparkle. Readable on mobile, no text.
  - Review Notes:

## Prestige Assets

- [ ] Asset: Nuptial Flight scene
  - Status: Needed
  - Priority: P1
  - Milestone: M5
  - Filename: `scene_nuptial_flight.png`
  - Type: Background scene
  - Size / Frames: 780 x 1000 minimum, single image
  - Description: Vertical scene of winged queens launching from the nest into warm sky. Hopeful, mythic, and still connected to the garden scale.
  - Gemini Prompt: Create a vertical 16-bit inspired pixel art Nuptial Flight scene for an ant colony prestige event. Winged queens launch from the nest into warm sunrise sky, hopeful and mythic, no text.
  - Review Notes:

- [ ] Asset: Genetic mutation card art set
  - Status: Needed
  - Priority: P2
  - Milestone: M5
  - Filename: `cards_mutations.png`
  - Type: Card art or icon sheet
  - Size / Frames: 256 x 384 per card or 256 x 256 per icon
  - Description: Efficient Digestion, Strong Mandibles, Acid Glands, Deep-Memory Trails, Resin Shell.
  - Gemini Prompt: Create a pixel art mutation card art set for a cozy ant colony prestige system. Five cards: Efficient Digestion, Strong Mandibles, Acid Glands, Deep-Memory Trails, Resin Shell. No text, strong symbolic art.
  - Review Notes:

- [ ] Asset: New colony landing scene
  - Status: Needed
  - Priority: P2
  - Milestone: M5
  - Filename: `scene_new_colony_landing.png`
  - Type: Background scene
  - Size / Frames: 780 x 680 minimum, single image
  - Description: Tiny queen landing in a new biome with a few first workers or eggs.
  - Gemini Prompt: Create a 16-bit inspired pixel art scene of a winged queen ant landing in a new colony site. Tiny eggs or first workers nearby, hopeful new biome mood, no text.
  - Review Notes:

## UI Frame And Feedback Assets

- [ ] Asset: Mobile panel texture set
  - Status: Generated
  - Priority: P2
  - Milestone: M2
  - Filename: `ui_panel_textures.png`
  - Type: UI texture sheet
  - Size / Frames: 9-slice panels or simple repeatable textures
  - Description: Subtle soil, leaf, and chamber panel textures for buttons, status strips, and bottom deck backgrounds. Should not overpower text.
  - Gemini Prompt: Create subtle pixel art UI panel textures for a cozy ant colony mobile game. Soil, leaf, and chamber-inspired panels, no text, low contrast, suitable behind readable UI.
  - Review Notes: Generated 2x2 grid panel textures, stripped black background for true transparency.

- [ ] Asset: Resource gain sparkle
  - Status: Needed
  - Priority: P2
  - Milestone: M2
  - Filename: `fx_resource_sparkle.png`
  - Type: Transparent effect sheet
  - Size / Frames: 64 x 64 frames, 4-frame loop
  - Description: Small satisfying sparkle or pulse used when resources increase or upgrades become affordable.
  - Gemini Prompt: Create a transparent PNG pixel art resource sparkle effect for a cozy mobile idle game. Small warm sparkle, 4-frame loop, readable but subtle, no text.
  - Review Notes:

- [ ] Asset: Warning pulse
  - Status: Needed
  - Priority: P2
  - Milestone: M3
  - Filename: `fx_warning_pulse.png`
  - Type: Transparent effect sheet
  - Size / Frames: 96 x 96 frames, 4-frame loop
  - Description: Non-text warning pulse for threatened nodes, hunger, and breach warnings.
  - Gemini Prompt: Create a transparent PNG pixel art warning pulse effect for a mobile ant colony game. Amber-red pulse, readable at small size, no text, not too flashy.
  - Review Notes:

## Audio Direction For Future Requests

Gemini may only be used for visual assets, but the game will eventually need audio with the same cozy/brutal contrast.

- Calm colony loop: soft soil taps, tiny movement, warm hum.
- Productivity layer: gentle clicks and leaf rustles.
- Hunger warning: thinner, dissonant pulse.
- Siege preparation: low rhythmic tapping and distant rustle.
- Combat hits: crunchy candy-shell cracks, not wet realism.
- Acid: quick sizzle and pop.
- Breach: snapped trail sting and low warning tremor.
- Nuptial Flight: airy, hopeful lift.

## Gemini Prompt Template

Use this structure when asking Gemini for assets:

```text
Create [asset name] for a mobile browser idle game called Passive Ant.
Style: 16-bit inspired pixel art with modern polish, cozy garden diorama, readable on a phone screen.
Mood: [calm / warning / brutal combat / prestige].
Description: [specific subject, pose, silhouette, colors, state].
Technical: transparent PNG, clean silhouette, no text, no UI frame, readable at small size.
Palette: [palette notes].
Avoid: realistic gore, photorealism, muddy low-contrast colors, excessive tiny details, text, watermark, signature.
```

## Review Checklist For Generated Assets

- [ ] Asset has no baked-in text, watermark, or signature.
- [ ] Silhouette reads clearly on a phone.
- [ ] Transparent assets actually have transparent backgrounds.
- [ ] Sprite frames share a consistent baseline.
- [ ] Colors match the cozy/brutal palette direction.
- [ ] Asset does not use realistic gore.
- [ ] Asset is not too noisy when displayed small.
- [ ] Asset can be cropped safely in the top mobile viewport.
- [ ] Filename matches the planned filename or is recorded above.
- [ ] Status above has been updated.

