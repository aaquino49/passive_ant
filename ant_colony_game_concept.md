# Project: Ant Colony Idle Game (Working Title)
## Product Vision & Design Document Summary

A mobile-first, macro-management idle/incremental game where players grow an ant colony through passive resource loops and strategic expansion, juxtaposing a cozy, stylized aesthetic with brutal, high-stakes combat.

---

## 🎯 Core Philosophy: "Cozy Brutalism"
* **The Hook:** The game leverages the relaxing, low-friction dopamine loops of an incremental simulator but subverts expectations with intense, visceral, and consequential combat payoffs.
* **The Player's Role:** The player acts as the collective hive mind ("The Pheromone Controller") managing macro-allocations and resource pipelines rather than micromanaging individual units.

---

## 🔄 Core Gameplay Loops & Mechanics

### 1. Macro-Allocation Economy
Instead of directing units, players use a thumb-friendly slider deck to allocate total population percentages to simple roles:
* **Foragers (Farming):** Passively generate **Food** (Leaves/Fungi) to maintain colony growth and feed larvae.
* **Excavators (Mining):** Passively generate **Materials** (Chitin/Dirt) to upgrade nest storage and unlock deeper chambers.
* **Soldiers (Security):** Defend active supply lines and siege new territories.

### 2. Node-Based Expansion ("The Trail System")
* The world is mapped as a grid of distinct outdoor nodes (e.g., *The Mossy Log*, *The Sunny Rock*).
* **The Cycle:** Players assign ants to **Scout** a node -> **Siege** it via a passive, slow-burn timer -> **Claim** it to secure its unique permanent resource multipliers.
* **The Over-Extension Trap:** Distant outposts require longer supply lines. If left undefended, predators can trigger a **Supply Line Breach**, severing the pipeline and endangering the frontier ants.

### 3. The Prestige System: Nuptial Flight
When the colony hits its structural limit, players initiate a *Nuptial Flight*. Winged Queens launch to start a brand new colony in a tougher biome (e.g., moving from the Garden to the Patio), resetting baseline progress in exchange for permanent genetic mutations and evolutionary modifiers.

---

## ⚔️ Combat System: Simple but Severe

To respect the mobile idle format, combat is **hands-off and decisive** (resolving in 5–10 seconds), relying heavily on presentation and strict unit specialization.

### Visual Language of Brutality
* **Impact and Fracture:** Attacks carry immense physical weight. Armored insect carapaces shatter like hard candy to reveal vibrant, colorful pulp. 
* **The Organic "Ick" Factor:** Replaces gritty realism with bright, stylized biology. Crushed enemies erupt in lime-green ichor splashes that temporarily stain the plush, felt-like ground.
* **Mobile Haptics Integration:** Heavy, sharp device vibrations trigger precisely on shell cracks and fatal mandibles strikes, delivering a physical "crunch" directly to the player's hand.

### Unit Specialization
* **Workers:** Completely defenseless; dynamically targeted and carried off by predators if caught unprotected in the wild.
* **Majors (Crushers):** Visually massive, slow-moving tanks that crush heavy enemy armor but are easily outmaneuvered.
* **Swarmers (Melee):** Fragile, rapid units that physically engulf and tear down massive targets through sheer numbers.
* **Spitters (Ranged):** Backline artillery that project melting, corrosive acid lines but are instantly executed if reached.

### Fail-States
* **The Harvesting Phase:** Victorious enemies can harvest your fallen soldiers for biological material. Conversely, players can salvage their own dead workers for Chitin to fund a counter-attack.
* **Starvation Panic:** Over-allocating to military while neglecting food production dims the cozy background lighting, shifts the ambient music to a frantic, discordant edge, and causes workers to visibly collapse from exhaustion.

---

## 📱 Mobile-First Design & Guardrails

* **Vertical Split-Screen Layout:** The top 40% is a living, gorgeous 16-bit pixel art viewport showing the anthill cross-section. The bottom 60% is a clean, minimalist control deck optimized for one-handed thumb navigation.
* **Offline Progression Guardrails:** To prevent frustration, catastrophic events like *Starvation Panics* or *Supply Line Breaches* are locked while the app is closed. Offline time is strictly a safe zone for conservative resource accumulation; high-stakes combat only triggers during active sessions.
* **Aesthetic Push Notifications:** Leveraged as a narrative tool to maintain world stakes (e.g., *“The Scout trail to The Sunken Patio has gone silent. Send soldiers when you return.”*).

---

## 🚀 Initial 5 Development Milestones

1. **M1: The Spreadsheet Engine** - Text-based time-step loop & math validation.
2. **M2: The Vertical Dashboard** - Wired UI sliders, phone layout, and local nest loops.
3. **M3: The "Crunch"** - Automated combat logic, haptic engine integration, and blood/ichor FX pass.
4. **M4: The Frontier** - Node mapping, scouting timers, and supply line breach logic.
5. **M5: The Vertical Slice** - Integration of final 16-bit cozy art, dynamic audio systems, and offline loop testing.
