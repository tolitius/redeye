# Changelog

All notable changes to REDEYE are documented in this file.

## [0.3.0] - 2026-01-17

### Added - Phase 3: Memory Surges & Resonances

- **Memories** (6 total) - Story fragments revealed through death
  - Each has minDepth requirement (0, 5, 8, 10, 12, 15)
  - Purple particles, shows title + story text
  - Also grants bonus stat echo
- **Resonances** (6 total) - Rare surprise ability unlocks
  - Blade Arc, Blade Thrust, Signal Burst, Echo Magnet, Last Gasp, Thread Sight
  - Golden particles, "The spectrum offers a gift."
  - Each can only be earned once
- **Progress Menu** (P key from main menu)
  - Shows: Runs, Best Depth, Echo
  - Shows: Spectrum memory stat bonuses
  - Shows: Memories list (unlocked titles or ??????)
  - Shows: Resonances list (unlocked names or ??????)
  - Hints when none unlocked yet

## [0.2.0] - 2026-01-17

### Added - Phase 2: Fever Dreams & Stat Echoes

- **Fever Dream System** - `checkFeverDream(depth)` probability-based on death count
  - Deaths 1-5: 60% stat echo
  - Deaths 6-15: 50% stat echo, 20% memory surge
  - Deaths 16-30: 40% stat echo, 25% memory, 15% resonance
  - Deaths 31+: 30% stat echo, 20% memory, 15% resonance
- **Stat Echoes** - Random permanent stat gains
  - +0.5 health/signal, +0.05 damage/regen
  - Weighted random selection
  - Accumulates in `saveData.stats`
- **Visual Sequence** - Black screen, red particles converge, flash, text reveal
- **Spectrum Memory Display** - Shows accumulated bonuses at game start, fades after 3s

## [0.1.0] - 2026-01-17

### Added - Phase 1: Foundation

- **Save System**
  - `localStorage` with key `"redeye_save"`
  - Migration from old highscore/runcount format
  - Structure: echo, totalRuns, bestDepth, unlocks, memories, resonances, introSeen, stats
- **Echo Currency**
  - Earned on death based on depth brackets (1-4: 1, 5-9: 3, 10-14: 6, 15-19: 10, 20: 15)
  - +2 bonus for new best depth
  - Displayed on death screen with breakdown
- **Upgrades Menu** (U key)
  - Starting bonuses: Vitality I/II/III, Attunement I/II/III
  - Quality of life: Signal Sight, Momentum, Last Stand
  - Tier dependencies (must own tier 1 before tier 2)
  - Buy/Sell toggle with full echo refund
- **Apply Unlocks** - `getStartingStats()` applies all purchased bonuses at run start

### Fixed

- **Focus on load** - Added `focus: true` to kaplay config
- **Intro once** - `introSeen` flag, only shows until localStorage reset
- **Reset progress** - Hold Backspace 2 seconds in menu
- **Checkbox crash** - Changed `[x]` to `(x)` to avoid KAPLAY styled text parser
- **Premature death** - Check `isDead` before applying damage, clamp health to 0
- **Debug logging** - `[DEATH]`, `[FEVER DREAM]`, `[GAMEOVER]`, `[GAME START]` console logs

---

## Planned

- **Resonance gameplay effects** (wider slash, kills restore signal, etc.)
- **Phase 4: Interference/Cursed Runs**
