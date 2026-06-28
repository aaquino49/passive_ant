# Passive Ant — UI Requirements (2026-06-27)

These are prioritized, testable requirements derived from a code review of `index.html`
(single-file HTML5/CSS/vanilla-JS, ~3,200 lines). Each requirement lists the **problem**,
the **requirement**, the **affected code**, and **acceptance criteria**.

Line numbers are a snapshot taken on 2026-06-27 and may drift if `index.html` changes
before implementation — treat the cited functions/ids as the source of truth.

---

## Priority / Sequencing

Suggested order:
1. **R2** — number formatting (quick correctness fix)
2. **R5** — speed controls (small, high-visibility)
3. **R3** — rename
4. **R4 + R1** — menu restructure + no-scroll (interdependent, design together)
5. **R6** — immersive top visual (largest, iterative)

---

## R1 — No vertical scrolling; fit-to-viewport with menus

**Problem:** The Nest tab stacks Status, Role Allocation, Combat Stance, and Nest Upgrades
in one `overflow-y:auto` container (`#deck-scroll` / `#nest-deck-content`, index.html:1109-1135),
forcing up/down scrolling.

**Requirement:** The primary play screen must fit within the viewport with **no vertical
scroll**. When content exceeds available height, split it behind menu/tab selection (see R4)
rather than scrolling. Target: everything reachable in ≤1 tap, nothing clipped, no scrollbar
on the main deck.

**Affected:** `#deck-scroll` CSS (index.html:98-106); section markup (index.html:1117-1134).

**Acceptance:** On a 360×640 mobile viewport, no element requires scrolling; the deck
container has `overflow: hidden` (or content fits) in normal play.

---

## R2 — All numbers ≤ 1 decimal place

**Problem (root cause):** The main POP value is already floored (index.html:2863), but the
**"X idle" detail** under POP renders `unassigned(s)` — a raw float of `adults − assigned` —
producing 8+ decimals (index.html:2877-2879; `unassigned()` at index.html:1470).

**Requirement:** No number anywhere in the UI shows more than one decimal place. Counts of
discrete things (population, idle ants, unit counts, levels) display as **integers**;
continuous rates/multipliers display with **at most one decimal**.

**Implementation note:** Introduce one shared formatter (e.g. `fmtInt(n)=Math.floor(n)`,
`fmtRate(n)=n.toFixed(1)`) and route all displays through it. Audit every `.toFixed(2)` /
raw-float render — e.g. combat ratio (index.html:2024), upgrade mult (index.html:2937),
per-ant `perSec.toFixed(2)` (index.html:2901) — and reduce to ≤1 decimal or justify the
exception.

**Acceptance:** Idle count shows e.g. `3 idle`, never `3.04217...`. A grep for
`.toFixed(2)` / `.toFixed(3)` in render paths returns only justified cases.

---

## R3 — Rename "Nuptial Flight" → "Found New Colony"

**Problem:** "Nuptial Flight" is opaque ant jargon; users don't understand the prestige/reset.

**Requirement:** Replace all user-facing instances with **"Found New Colony"** and add a
one-line explainer: *"Restart your colony to inherit a permanent mutation and advance a
generation."* Internal variable/id names may stay; every label the player sees changes.

**Affected strings:** prestige tracker label (index.html:1103); progress text
(index.html:2345); modal title (index.html:2397); launch button (index.html:2465).

**Acceptance:** "Nuptial Flight" appears nowhere on screen; the explainer is visible at the
prestige entry point.

---

## R4 — Role / Combat / Upgrades as selectable menu, not one scroll

**Problem:** All three management panels render at once in a tall column (see R1).

**Requirement:** Present Role, Combat, and Upgrades as **three menu/sub-tab options** within
the Nest view. Only one panel is shown at a time; selecting a menu item swaps the panel. Each
panel must fit the viewport without scrolling (works with R1).

**Reuse:** Keep the existing builders `buildAllocationUI()` (index.html:2711),
`buildStanceUI()` (index.html:2765), `buildUpgradeUI()` (index.html:2793) and mount them into
a single swappable panel slot. Mirror the existing `switchTab()` pattern (index.html:1941).

**Acceptance:** Nest view shows a Role | Combat | Upgrades selector; exactly one panel
visible; switching is one tap; no scroll.

---

## R5 — Speed = 1× / 2× / 5× only, at the top

**Problem:** Speed controls are 1× / 2× / **5×** / **10×** and sit in the **bottom** bar
(index.html:1151-1156).

**Requirement:** Offer exactly **1×, 2×, 5×** (remove 10×) and place the control at the
**top** of the screen (in/near the resource header / a new top bar). Default `speed = 2`
(index.html:1452) remains valid since 2× stays.

**Affected:** speed buttons markup (index.html:1151-1156); handler (index.html:3139-3140);
active-state sync (index.html:2956-2958); game loop scaling (index.html:3124).

**Acceptance:** Only three speed buttons exist (1×/2×/5×), rendered at the top; the ⚙
settings button relocates as needed; the game loop still scales by `speed`.

---

## R6 — Top visual must feel immersive and reflect game state

**Problem:** The top 40% viewport shows a static nest image plus 17 decorative wandering ants
whose movement/count is essentially unrelated to actual population, assignments, combat, or
growth (`#viewport-ants`, spawn/wander at index.html:2660-2706; only loosely gated by
`G.assigned` in `updateViewportAnts()` index.html:2694).

**Requirement:** The top visual must visibly correlate with game state and feel alive. Two
complementary tracks (both required):

### (a) Living colony mirror — state-driven
- Ant **types and counts** on screen reflect real role assignments
  (foragers / excavators / soldiers) from `G.assigned`, scaling as the player reallocates.
- **Nursery / larvae:** larvae clustered in a nursery chamber, quantity scaling with
  `population.larvae`; hatching pulse when larvae mature.
- **Food / materials stockpiles:** a visible pile/granary that grows or shrinks with
  `resources.food` / `materials`, and visibly drains during starvation states.
- **Combat:** when a battle resolves (index.html:2031 onward), flash soldiers swarming toward
  the threat / impact FX (art already present: `fx_acid_line`, bite-impact FX).
- **Upgrades:** chambers light up / expand as upgrade levels increase (nursery, storage,
  guard chamber).
- **Status coupling:** tint/behavior shifts with `s.status` (growing / starving / under
  attack), reusing the existing status-strip signal.

### (b) Ambient / cinematic depth — atmosphere
- Layered parallax depth (surface entrance vs. deep chambers); subtle camera drift.
- Day/night or light-shaft cycling so the screen changes over time even when idle.
- Gentle particle ambience (dust motes, drifting spores).
- Soft vignette / lighting that focuses the eye on active chambers.

**Constraint:** All motion must respect the existing **reduced-motion** accessibility toggle;
performance must stay smooth on mobile (prefer CSS transforms over per-frame JS where
possible).

**Acceptance:** Reassigning ants, growing larvae, gaining/losing resources, and entering
combat each produce a **visible, corresponding** change in the top visual; idle screens still
show ambient life; reduced-motion disables non-essential animation.
