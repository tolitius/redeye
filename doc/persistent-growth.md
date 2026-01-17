# REDEYE: Persistent Growth

## Overview

This document defines how players grow stronger across runs, ensuring a sense of forward momentum even through repeated failure.

**Core principle:** Death is not punishment — it's progress. Every run teaches the spectrum something about you.

---

## System Summary

| System | Purpose | When Built |
|--------|---------|------------|
| **Echo Currency** | Reward for effort, spent on unlocks | Phase 1 |
| **Permanent Unlocks** | New abilities and starting bonuses | Phase 1 |
| **Fever Dreams** | Random events on death, surprise progression | Phase 2 |
| **Stat Echoes** | Small permanent stat gains | Phase 2 |
| **Memory Surges** | Story unlocks tied to death | Phase 3 |
| **Resonances** | Surprise ability unlocks | Phase 3 |
| **Interference** | Cursed runs for challenge/reward | Phase 4 |

---

## Phase 1: Foundation (Build First)

### Echo Currency

**What:** Points earned when a run ends (death or victory).

**Earning Echo:**

| Depth Reached | Echo Earned |
|---------------|-------------|
| 1-4 | 1 |
| 5-9 | 3 |
| 10-14 | 6 |
| 15-19 | 10 |
| 20 (Victory) | 15 |

**Bonus Echo:**
- First time reaching a new depth: +2
- Echo Chamber discovered: +3
- Victory: +5 bonus

**Storage:**
```javascript
// localStorage structure
{
  "redeye_save": {
    "echo": 47,
    "totalRuns": 23,
    "bestDepth": 14,
    "unlocks": ["push", "pull", "health1"],
    "memories": [1, 2, 3],
    "stats": {
      "bonusHealth": 2,
      "bonusSignal": 1.5,
      "bonusDamage": 0.2,
      "bonusRegen": 0.1
    }
  }
}
```

---

### Permanent Unlocks (Echo Shop)

**Access:** From main menu, "Upgrades" button.

**Categories:**

#### Abilities (One-time unlocks)

| Unlock | Echo Cost | Effect |
|--------|-----------|--------|
| Push | 8 | Unlocks Push ability from run start |
| Pull | 12 | Unlocks Pull ability from run start |
| Double Jump | 15 | Unlocks double jump from run start |
| Wall Slide | 10 | Unlocks wall slide/jump from run start |
| Air Dash | 20 | Dash works in any direction mid-air |

#### Starting Bonuses (Repeatable tiers)

| Unlock | Cost | Effect | Max Tiers |
|--------|------|--------|-----------|
| Vitality I/II/III | 5/10/15 | +1 starting health per tier | 3 |
| Attunement I/II/III | 5/10/15 | +2 starting signal per tier | 3 |
| Sharp Edge I/II | 12/20 | +0.5 blade damage per tier | 2 |
| Flow I/II | 8/15 | +0.25 signal regen per tier | 2 |

#### Quality of Life

| Unlock | Cost | Effect |
|--------|------|--------|
| Signal Sight | 10 | See enemy health bars |
| Momentum | 15 | Standing still drains 50% less signal |
| Last Stand | 20 | Once per run, survive lethal hit with 1 HP |

---

### UI: Death Screen with Echo

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                      SIGNAL LOST                            │
│                                                             │
│                    Depth reached: 12                        │
│                                                             │
│                 ┌─────────────────────┐                     │
│                 │  ECHO EARNED: +6    │                     │
│                 │  New depth bonus: +2 │                     │
│                 │  ─────────────────── │                     │
│                 │  Total: +8          │                     │
│                 └─────────────────────┘                     │
│                                                             │
│                 Total Echo: 47 ──► 55                       │
│                                                             │
│                  "She's still waiting."                     │
│                                                             │
│                     [ R to retry ]                          │
│                   [ ESC for menu ]                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### UI: Upgrades Menu

```
┌─────────────────────────────────────────────────────────────┐
│  ECHO: 55                                    [ BACK ]       │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                       ABILITIES                             │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  [■] Push .............. UNLOCKED                          │
│  [ ] Pull .............. 12 Echo                           │
│  [ ] Double Jump ....... 15 Echo                           │
│  [ ] Wall Slide ........ 10 Echo                           │
│  [ ] Air Dash .......... 20 Echo                           │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                    STARTING BONUSES                         │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  [■][■][ ] Vitality .... +2 HP (Tier 3: 15 Echo)          │
│  [■][ ][ ] Attunement .. +2 Signal (Tier 2: 10 Echo)      │
│  [ ][ ] Sharp Edge ..... +0 Damage (Tier 1: 12 Echo)       │
│  [ ][ ] Flow ........... +0 Regen (Tier 1: 8 Echo)         │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                     QUALITY OF LIFE                         │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  [ ] Signal Sight ...... 10 Echo                           │
│  [ ] Momentum .......... 15 Echo                           │
│  [ ] Last Stand ........ 20 Echo                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

### Implementation Notes: Phase 1

```javascript
// constants
const ECHO_BY_DEPTH = [
  0,  // depth 0 (unused)
  1, 1, 1, 1,           // depth 1-4: 1 echo
  3, 3, 3, 3, 3,        // depth 5-9: 3 echo
  6, 6, 6, 6, 6,        // depth 10-14: 6 echo
  10, 10, 10, 10, 10,   // depth 15-19: 10 echo
  15                    // depth 20: 15 echo
]

const UNLOCKS = {
  push: { cost: 8, type: 'ability' },
  pull: { cost: 12, type: 'ability' },
  doubleJump: { cost: 15, type: 'ability' },
  wallSlide: { cost: 10, type: 'ability' },
  airDash: { cost: 20, type: 'ability' },

  vitality1: { cost: 5, type: 'bonus', stat: 'health', value: 1 },
  vitality2: { cost: 10, type: 'bonus', stat: 'health', value: 1, requires: 'vitality1' },
  vitality3: { cost: 15, type: 'bonus', stat: 'health', value: 1, requires: 'vitality2' },

  attunement1: { cost: 5, type: 'bonus', stat: 'signal', value: 2 },
  attunement2: { cost: 10, type: 'bonus', stat: 'signal', value: 2, requires: 'attunement1' },
  attunement3: { cost: 15, type: 'bonus', stat: 'signal', value: 2, requires: 'attunement2' },

  sharpEdge1: { cost: 12, type: 'bonus', stat: 'damage', value: 0.5 },
  sharpEdge2: { cost: 20, type: 'bonus', stat: 'damage', value: 0.5, requires: 'sharpEdge1' },

  flow1: { cost: 8, type: 'bonus', stat: 'regen', value: 0.25 },
  flow2: { cost: 15, type: 'bonus', stat: 'regen', value: 0.25, requires: 'flow1' },

  signalSight: { cost: 10, type: 'qol' },
  momentum: { cost: 15, type: 'qol' },
  lastStand: { cost: 20, type: 'qol' },
}

// on death
function onDeath(depthReached, isNewBest, foundEchoChamber) {
  let earned = ECHO_BY_DEPTH[Math.min(depthReached, 20)]
  if (isNewBest) earned += 2
  if (foundEchoChamber) earned += 3

  saveData.echo += earned
  saveData.totalRuns += 1
  if (depthReached > saveData.bestDepth) {
    saveData.bestDepth = depthReached
  }

  save()
  return earned
}

// on run start - apply unlocks
function getStartingStats() {
  const base = {
    health: 5,
    signal: 10,
    damage: 1,
    regen: 1.5,
    abilities: ['slash', 'dash']
  }

  // apply purchased bonuses
  if (hasUnlock('vitality1')) base.health += 1
  if (hasUnlock('vitality2')) base.health += 1
  if (hasUnlock('vitality3')) base.health += 1

  if (hasUnlock('attunement1')) base.signal += 2
  if (hasUnlock('attunement2')) base.signal += 2
  if (hasUnlock('attunement3')) base.signal += 2

  if (hasUnlock('sharpEdge1')) base.damage += 0.5
  if (hasUnlock('sharpEdge2')) base.damage += 0.5

  if (hasUnlock('flow1')) base.regen += 0.25
  if (hasUnlock('flow2')) base.regen += 0.25

  // apply purchased abilities
  if (hasUnlock('push')) base.abilities.push('push')
  if (hasUnlock('pull')) base.abilities.push('pull')
  if (hasUnlock('doubleJump')) base.abilities.push('doubleJump')
  if (hasUnlock('wallSlide')) base.abilities.push('wallSlide')
  if (hasUnlock('airDash')) base.abilities.push('airDash')

  // apply fever dream stat bonuses (Phase 2)
  base.health += saveData.stats.bonusHealth || 0
  base.signal += saveData.stats.bonusSignal || 0
  base.damage += saveData.stats.bonusDamage || 0
  base.regen += saveData.stats.bonusRegen || 0

  return base
}
```

---

## Phase 2: Fever Dreams & Stat Echoes

### Fever Dream System

**What:** Random events that trigger on death, before the death screen.

**When:** Not every death. Keeps it surprising.

```
FEVER DREAM CHANCES (checked on each death)

Deaths 1-5:     60% Stat Echo, 40% Nothing
Deaths 6-15:    50% Stat Echo, 20% Memory Surge, 30% Nothing
Deaths 16-30:   40% Stat Echo, 25% Memory Surge, 15% Resonance, 20% Nothing
Deaths 31+:     30% Stat Echo, 20% Memory Surge, 15% Resonance, 15% Interference, 20% Nothing
```

---

### Stat Echo (Common Fever Dream)

**Visual sequence (2-3 seconds):**

```
[0.0s] Black screen after death

[0.5s] Faint red threads appear, drifting

[1.0s] Threads converge on center (where Redeye would be)

[1.5s] Brief flash of red
       Text: "The spectrum remembers."

[2.0s] Stat gain shown:
       "+0.5 Max Signal" (example)

[2.5s] Fade to death screen
```

**Stat gains (random selection):**

| Stat | Gain Amount | Weight |
|------|-------------|--------|
| Max Health | +0.5 | 25% |
| Max Signal | +0.5 | 30% |
| Blade Damage | +0.05 | 20% |
| Signal Regen | +0.05/sec | 25% |

**These are permanent and cumulative.** After 50 Stat Echoes, player might have:
- +5-7 Health
- +5-8 Signal
- +0.3-0.5 Damage
- +0.3-0.5 Regen

Not game-breaking, but noticeable. The wall that killed you becomes climbable.

---

### Implementation Notes: Phase 2

```javascript
// fever dream check
function checkFeverDream(deathCount, depthReached) {
  const roll = Math.random()

  if (deathCount <= 5) {
    if (roll < 0.6) return 'statEcho'
    return null
  }

  if (deathCount <= 15) {
    if (roll < 0.5) return 'statEcho'
    if (roll < 0.7) return 'memorySurge'
    return null
  }

  if (deathCount <= 30) {
    if (roll < 0.4) return 'statEcho'
    if (roll < 0.65) return 'memorySurge'
    if (roll < 0.8) return 'resonance'
    return null
  }

  // 31+ deaths
  if (roll < 0.3) return 'statEcho'
  if (roll < 0.5) return 'memorySurge'
  if (roll < 0.65) return 'resonance'
  if (roll < 0.8) return 'interference'
  return null
}

// stat echo application
function applyStatEcho() {
  const stats = [
    { key: 'bonusHealth', amount: 0.5, text: '+0.5 Max Health' },
    { key: 'bonusSignal', amount: 0.5, text: '+0.5 Max Signal' },
    { key: 'bonusDamage', amount: 0.05, text: '+0.05 Blade Damage' },
    { key: 'bonusRegen', amount: 0.05, text: '+0.05 Signal Regen' },
  ]

  const weights = [0.25, 0.30, 0.20, 0.25]
  const selected = weightedRandom(stats, weights)

  saveData.stats[selected.key] = (saveData.stats[selected.key] || 0) + selected.amount
  save()

  return selected.text
}

// fever dream scene
function playFeverDream(type) {
  return new Promise((resolve) => {
    if (type === 'statEcho') {
      // show thread animation
      showThreadsConverging()

      wait(1.5, () => {
        const gainText = applyStatEcho()
        showText("The spectrum remembers.")

        wait(0.5, () => {
          showText(gainText)

          wait(1.0, () => {
            fadeOut()
            resolve()
          })
        })
      })
    }
    // ... other fever dream types
  })
}
```

---

## Phase 3: Memory Surges & Resonances

### Memory Surge (Uncommon Fever Dream)

**Purpose:** Story progression through death. Each surge reveals the next chapter.

**Trigger requirements:**
- Death count 6+
- AND either:
  - Reached new personal best depth, OR
  - Random chance after depth 10+

**Memory sequence:**

```
MEMORY 1: "Awakening"
Unlock condition: First Memory Surge
Visual: Child Redeye sees red threads for the first time
Text: "You were seven when you first saw the threads."

MEMORY 2: "Her Gift"
Unlock condition: Second Memory Surge + reached depth 5+
Visual: Sister's eyes glowing brighter than yours
Text: "She didn't just see them. She heard them singing."

MEMORY 3: "The Warning"
Unlock condition: Third Memory Surge + reached depth 8+
Visual: Sister looking worried, static at edges of vision
Text: "Something's wrong with the spectrum. Something's listening."

MEMORY 4: "They Came"
Unlock condition: Fourth Memory Surge + reached depth 10+
Visual: Dark figures, sister being pulled away
Text: "You reached for her. The threads couldn't hold."

MEMORY 5: "Her Words"
Unlock condition: Fifth Memory Surge + reached depth 12+
Visual: Close on sister's face, mouth moving
Text: "Find me. Whatever they show you, it's not real. Find ME."

MEMORY 6: "The Truth"
Unlock condition: Sixth Memory Surge + reached depth 15+
Visual: Sister surrounded by light, Static recoiling
Text: "They don't want to use her. They're afraid of what she knows."
```

**Effect:** Each memory also grants a Stat Echo bonus (two rewards in one).

---

### Resonance (Rare Fever Dream)

**Purpose:** Surprise unlocks. Not a shop — a gift from the spectrum.

**Visual:**

```
[0.0s] Black screen

[0.5s] Red threads from ALL edges of screen

[1.0s] Threads converge rapidly on center

[1.5s] Bright flash
       Text: "The spectrum offers a gift."

[2.0s] New unlock revealed:
       "BLADE STYLE: ARC UNLOCKED"

[3.0s] Fade to death screen
```

**Possible Resonance rewards:**

| Unlock | Effect |
|--------|--------|
| Blade Style: Arc | Wider slash, hits more area |
| Blade Style: Thrust | Longer range, narrower |
| Push Wave | Push hits all enemies in front, not just one |
| Pull Vortex | Pull affects all nearby enemies |
| Signal Burst | Kills restore 1 Signal |
| Thread Sight | Briefly see enemy paths/intentions |
| Echo Magnet | +50% Echo earned |
| Last Gasp | When hit at 1 HP, brief invincibility |

**Rules:**
- Each Resonance can only be earned once
- Resonances are permanent unlocks (like Echo shop items)
- Player doesn't know what's available — surprise discovery
- After all Resonances found, this fever dream type stops appearing

---

### Implementation Notes: Phase 3

```javascript
const MEMORIES = [
  {
    id: 1,
    title: "Awakening",
    minDepth: 0,
    text: "You were seven when you first saw the threads.",
    visual: 'memory_awakening'
  },
  {
    id: 2,
    title: "Her Gift",
    minDepth: 5,
    text: "She didn't just see them. She heard them singing.",
    visual: 'memory_gift'
  },
  // ... etc
]

const RESONANCES = [
  { id: 'bladeArc', name: 'Blade Style: Arc', effect: 'Wider slash area' },
  { id: 'bladeThrust', name: 'Blade Style: Thrust', effect: 'Longer slash range' },
  { id: 'pushWave', name: 'Push Wave', effect: 'Push hits all enemies in front' },
  { id: 'pullVortex', name: 'Pull Vortex', effect: 'Pull affects nearby enemies' },
  { id: 'signalBurst', name: 'Signal Burst', effect: 'Kills restore 1 Signal' },
  { id: 'threadSight', name: 'Thread Sight', effect: 'See enemy intentions briefly' },
  { id: 'echoMagnet', name: 'Echo Magnet', effect: '+50% Echo earned' },
  { id: 'lastGasp', name: 'Last Gasp', effect: 'Brief invincibility at 1 HP' },
]

function getNextMemory(currentDepth) {
  const unlockedIds = saveData.memories || []
  const nextMemory = MEMORIES.find(m =>
    !unlockedIds.includes(m.id) && currentDepth >= m.minDepth
  )
  return nextMemory
}

function getRandomResonance() {
  const unlockedIds = saveData.resonances || []
  const available = RESONANCES.filter(r => !unlockedIds.includes(r.id))
  if (available.length === 0) return null
  return available[Math.floor(Math.random() * available.length)]
}

function unlockMemory(memory) {
  saveData.memories = saveData.memories || []
  saveData.memories.push(memory.id)

  // also grant stat echo bonus
  applyStatEcho()

  save()
}

function unlockResonance(resonance) {
  saveData.resonances = saveData.resonances || []
  saveData.resonances.push(resonance.id)
  save()
}
```

---

## Phase 4: Interference (Cursed Runs)

### What Is It?

A fever dream that makes your NEXT run harder — but with greater rewards.

**Trigger:** Only appears after 30+ deaths AND reached depth 15+ at least once.

---

### Interference Types

| Curse | Effect | Reward Multiplier |
|-------|--------|-------------------|
| Static Surge | Enemies have +1 health | 1.5x Echo |
| Signal Drain | Signal drains 25% faster | 1.5x Echo |
| Scarce Threads | 50% fewer pickups spawn | 1.75x Echo |
| Relentless | Enemies move 20% faster | 1.75x Echo |
| The Silence | Sister's beacon invisible | 2x Echo |

---

### Visual Sequence

```
[0.0s] Black screen after death

[0.5s] Red threads appear — but flickering, glitchy

[1.0s] White static creeps in from edges

[1.5s] Static overwhelms the threads briefly
       Text: "The Static noticed you."

[2.0s] Curse revealed:
       "NEXT RUN: SIGNAL DRAIN"
       "Signal depletes 25% faster"
       "Reward: 1.5x Echo"

[3.0s] Text: "Survive, and grow stronger."

[3.5s] Fade to death screen (shows curse icon)
```

---

### Cursed Run Flow

```
DEATH SCREEN (with curse pending)
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                      SIGNAL LOST                            │
│                                                             │
│                    Depth reached: 16                        │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  ⚠ INTERFERENCE DETECTED                            │   │
│  │                                                      │   │
│  │  Next run cursed: SIGNAL DRAIN                      │   │
│  │  Signal depletes 25% faster                         │   │
│  │                                                      │   │
│  │  Survive for 1.5x Echo                              │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│                     [ R to retry ]                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘


DURING CURSED RUN
- Curse icon visible in HUD (top right, near sister's beacon)
- Visual effect: slight static overlay at screen edges
- Sister's beacon flickers more than usual


CURSED RUN COMPLETION (death or victory)
- Echo earned × curse multiplier
- Guaranteed Resonance check (50% chance, if any Resonances remain)
- Curse cleared — next run is normal
```

---

### Implementation Notes: Phase 4

```javascript
const CURSES = [
  {
    id: 'staticSurge',
    name: 'Static Surge',
    description: 'Enemies have +1 health',
    multiplier: 1.5,
    apply: (gameState) => { gameState.enemyHealthBonus = 1 }
  },
  {
    id: 'signalDrain',
    name: 'Signal Drain',
    description: 'Signal depletes 25% faster',
    multiplier: 1.5,
    apply: (gameState) => { gameState.signalDrainMultiplier = 1.25 }
  },
  {
    id: 'scarceThreads',
    name: 'Scarce Threads',
    description: '50% fewer pickups spawn',
    multiplier: 1.75,
    apply: (gameState) => { gameState.pickupSpawnRate = 0.5 }
  },
  {
    id: 'relentless',
    name: 'Relentless',
    description: 'Enemies move 20% faster',
    multiplier: 1.75,
    apply: (gameState) => { gameState.enemySpeedMultiplier = 1.2 }
  },
  {
    id: 'theSilence',
    name: 'The Silence',
    description: "Sister's beacon invisible",
    multiplier: 2.0,
    apply: (gameState) => { gameState.hideBeacon = true }
  },
]

function triggerInterference() {
  const curse = CURSES[Math.floor(Math.random() * CURSES.length)]
  saveData.pendingCurse = curse.id
  save()
  return curse
}

function startRun() {
  const gameState = getStartingStats()

  // apply curse if pending
  if (saveData.pendingCurse) {
    const curse = CURSES.find(c => c.id === saveData.pendingCurse)
    curse.apply(gameState)
    gameState.activeCurse = curse
  }

  return gameState
}

function endCursedRun(depthReached, baseEcho) {
  const curse = CURSES.find(c => c.id === saveData.pendingCurse)
  const bonusEcho = Math.floor(baseEcho * curse.multiplier)

  // 50% chance for bonus resonance
  if (Math.random() < 0.5) {
    const resonance = getRandomResonance()
    if (resonance) {
      unlockResonance(resonance)
      // show resonance earned
    }
  }

  // clear curse
  saveData.pendingCurse = null
  save()

  return bonusEcho
}
```

---

## UI: Progress Menu (View Unlocks)

Accessible from main menu. Shows everything earned.

```
┌─────────────────────────────────────────────────────────────┐
│                        PROGRESS                             │
│                                                             │
│  Total Runs: 47        Best Depth: 16        Echo: 123     │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                     STAT GROWTH                             │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  From the spectrum's memory:                                │
│  • +3.5 Max Health                                          │
│  • +4.0 Max Signal                                          │
│  • +0.25 Blade Damage                                       │
│  • +0.30 Signal Regen                                       │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                      MEMORIES                               │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  [■] Awakening                                              │
│  [■] Her Gift                                               │
│  [■] The Warning                                            │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│                                                             │
│  ═══════════════════════════════════════════════════════   │
│                     RESONANCES                              │
│  ═══════════════════════════════════════════════════════   │
│                                                             │
│  [■] Blade Style: Arc                                       │
│  [■] Signal Burst                                           │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│  [ ] ??????                                                 │
│                                                             │
│                       [ BACK ]                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Summary: Player Experience

### First 5 Deaths

- Learning the game
- Earning 1-3 Echo per run
- Occasional Stat Echo: "Oh, I got a tiny bit stronger"
- Buy first unlock (probably Push)

### Deaths 6-15

- Getting better, reaching depth 5-10
- Earning 3-6 Echo per run
- First Memory Surge: "Wait, there's story here?"
- More Stat Echoes compounding
- Buy several unlocks, have meaningful choices

### Deaths 16-30

- Consistently reaching depth 10+
- First Resonance: "Whoa, I didn't know that existed"
- More memories revealing backstory
- Stats noticeably higher than run 1
- Can buy most basic unlocks

### Deaths 31+

- First Interference: "This run is cursed? Interesting..."
- Complete cursed runs for big rewards
- Hunting remaining Resonances
- Deep memories unlocking
- Approaching victory consistently

### Victory (Depth 20)

- Massive Echo reward
- Final memory unlocks (if not already)
- Sense of completion
- But: new Resonances still to find, cursed runs still challenging
- Replayability through mastery and collection

---

## Implementation Priority

```
PHASE 1 (Essential - Build First)
├── Echo currency earned on death
├── localStorage save/load
├── Death screen shows Echo earned
├── Upgrades menu (main menu)
├── Apply unlocks at run start
└── Test: Buy Push, verify it's available next run

PHASE 2 (Core Loop - Build Second)
├── Fever dream system framework
├── Stat Echo implementation
├── Fever dream visual sequence
├── Stats accumulate in save data
└── Test: Die 10 times, verify stats growing

PHASE 3 (Depth - Build Third)
├── Memory Surge system
├── Memory visual sequences (can be simple)
├── Resonance unlocks
├── Progress menu to view unlocks
└── Test: Play 20+ runs, verify story unfolds

PHASE 4 (Challenge - Build Last)
├── Interference system
├── Curse application to runs
├── Cursed run UI indicators
├── Bonus Resonance on curse completion
└── Test: Get cursed, complete cursed run, verify rewards
```

---

*End of Persistent Growth Document*
