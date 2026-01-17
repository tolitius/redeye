# redeye universe

> *"she's still out there. follow the signal."*

story, mechanics, systems, and implementation

🕹️ [play](https://gitpod.com/g/redeye/)

---

1. [Story & Lore](#story--lore)
2. [Gameplay Overview](#gameplay-overview)
3. [Player Abilities](#player-abilities)
4. [Enemies](#enemies)
5. [Rooms & Environments](#rooms--environments)
6. [Progression Systems](#progression-systems)
7. [Persistent Growth](#persistent-growth)
8. [Visual Design](#visual-design)
9. [Technical Reference](#technical-reference)
10. [Future Plans](#future-plans)

---

## Story & Lore

### The Premise

You are **Redeye**, a figure who can see the **spectrum** — red threads of energy that connect all things. Your sister could see them too, but she could do more. *She could hear them singing.*

Something took her. Something from the **Static** — the void between signals. Now you descend through layers of reality, following her fading signal, fighting through the interference that stands between you.

### The Sister

She waits at the bottom of the spectrum, at depth 20. Her signal grows stronger as you descend, manifesting as a pulsing red beacon in the corner of your vision. She speaks to you through the threads:

**Early depths (1-5):**
- *"You came."*
- *"I knew you'd find me."*
- *"They don't understand what I am."*

**Middle depths (6-10):**
- *"The Static... it's not random. It's searching."*
- *"They want to use me to silence everything."*
- *"You're the only signal I can still feel."*

**Late depths (11-15):**
- *"I can see the whole spectrum now. It's beautiful... and terrifying."*
- *"Almost. You're almost here."*
- *"They're scared of you. I can feel their fear."*

**Final depths (16+):**
- *"I can hear your blade. I can feel you fighting."*
- *"Together we can fix this. All of it."*
- *"I see you. I finally see you."*

### Memories (Unlocked Through Death)

The spectrum remembers. Through fever dreams, fragments of your past emerge:

| Memory | Depth Req. | Text |
|--------|------------|------|
| **Awakening** | 0 | *"You were seven when you first saw the threads."* |
| **Her Gift** | 5 | *"She didn't just see them. She heard them singing."* |
| **The Warning** | 8 | *"Something's wrong with the spectrum. Something's listening."* |
| **They Came** | 10 | *"You reached for her. The threads couldn't hold."* |
| **Her Words** | 12 | *"Find me. Whatever they show you, it's not real. Find ME."* |
| **The Truth** | 15 | *"They don't want to use her. They're afraid of what she knows."* |

### The Static

The enemies you face are manifestations of **Static** — interference in the spectrum. They seek to sever your connection to your sister, to silence the signal forever.

---

## Gameplay Overview

### Core Loop

1. **Descend** through procedurally arranged rooms
2. **Fight** Static enemies using your blade and dash
3. **Manage** your signal (energy) — it drains when moving backward or standing still
4. **Upgrade** at sanctuary rooms every 5 depths
5. **Die**, earn Echo, grow stronger, repeat
6. **Reach** depth 20 to find your sister

### Controls

| Action | Keys |
|--------|------|
| Move | Arrow Keys / A, D |
| Jump | Space |
| Dash | Shift |
| Slash | X / J |
| Restart | R |
| Menu | Escape |

### Win Condition

Reach **depth 20** while maintaining health and signal. Victory awards 15 Echo plus a +5 bonus.

### Death Conditions

- Health reaches 0 (enemy damage)
- Fall off the bottom of the screen

---

## Player Abilities

### Movement

| Parameter | Value |
|-----------|-------|
| Horizontal Speed | 300 px/s |
| Jump Force | 550 |
| Gravity (rising) | 1400 |
| Gravity (falling) | 1800 |
| Max Fall Speed | 600 |

**Jump mechanics:**
- Holding Space reduces gravity by 20% (floatier jumps)
- Particles spawn on jump
- One-way platforms can be jumped through from below

### Slash Attack

Your blade is made of pure spectrum energy.

| Parameter | Value |
|-----------|-------|
| Signal Cost | 1 |
| Duration | 0.2s |
| Range | 60px horizontal, 50px vertical |
| Base Damage | 1 |
| Cooldown | ~0.2s |

**Features:**
- Arc animation (-70° to +50°)
- Hits all enemies in range
- Deflects projectiles
- Creates particle burst and screen shake

### Dash

Phase through danger with a burst of speed.

| Parameter | Value |
|-----------|-------|
| Signal Cost | 1 |
| Distance | 128px |
| Duration | 0.15s |
| Speed | ~853 px/s |
| Cooldown | 0.2s |

**Features:**
- Full invincibility during dash
- Afterimage trail effect
- Player stretches visually (1.5x width, 0.7x height)
- Cannot dash while slashing

### Signal (Energy System)

Signal is your connection to the spectrum. It powers your abilities and represents your bond with your sister.

| Condition | Rate |
|-----------|------|
| Moving right | +1.5/s (regenerate) |
| Standing still | -0.5/s (drain) |
| Moving left | -1.5/s (drain) |
| Slash | -1 (instant) |
| Dash | -1 (instant) |

**Base max signal:** 10 (upgradeable)

When signal is low (<3), the bar pulses as a warning. At 0 signal, you cannot slash or dash.

---

## Enemies

All enemies are manifestations of Static — interference trying to sever your connection.

### Crawler

*Basic ground threat. Appears from depth 1.*

| Stat | Value |
|------|-------|
| Health | 1 |
| Damage | 1 |
| Speed | 120 px/s |
| Score | 10 |

**Behavior:** Moves directly toward player along the ground.

**Spawn scaling:** 1 → 1 → 2 → 2 → 3 → 3 → 4 (by depth)

---

### Spitter

*Ranged attacker. Appears from depth 3.*

| Stat | Value |
|------|-------|
| Health | 2 |
| Damage | 1 (projectile) |
| Fire Rate | Every 2.0s |
| Telegraph | 0.5s (mouth glows) |
| Projectile Speed | 250 px/s |
| Score | 25 |

**Behavior:** Stationary. Telegraphs with a glowing mouth, then fires a projectile at player's position.

**Visual tell:** Tilts toward player during telegraph.

---

### Rusher

*High-damage charger. Appears from depth 4.*

| Stat | Value |
|------|-------|
| Health | 2 |
| Damage | **2** |
| Charge Speed | 600 px/s |
| Telegraph | 0.7s (vibrates) |
| Stun Duration | 1.0s |
| Detection Range | 40px vertical |
| Score | 50 |

**State machine:**
1. **Idle** — Waits until player is horizontally aligned (±40px vertical)
2. **Telegraph** — Vibrates and flashes for 0.7s
3. **Charging** — Rockets horizontally at 600 px/s
4. **Stunned** — Vulnerable for 1.0s after hitting wall or traveling too far

---

### Drifter

*Ethereal floater. Appears from depth 6.*

| Stat | Value |
|------|-------|
| Health | 1 |
| Damage | 1 |
| Speed | 40 px/s |
| Bob Frequency | 2 Hz |
| Bob Amplitude | 15px |
| Score | 30 |

**Behavior:** Slowly drifts toward player while bobbing up and down in a sine wave pattern.

**Visual:** Purple glowing sphere with pulsing effect.

---

### Pillar

*Tank enemy. Appears from depth 7.*

| Stat | Value |
|------|-------|
| Health | **4** |
| Damage | 1 |
| Speed | 25 px/s |
| Score | 75 |

**Behavior:** Slowly walks toward player with menacing sway animation. Takes many hits to kill.

**Visual:** Metallic column with horizontal bands and glitch effects.

---

### Health Drops

When any enemy dies, there's a **25% chance** to spawn a health pickup.

- **Visual:** Red glowing circle (8px), floats and pulses
- **Duration:** 8 seconds before despawn
- **Effect:** +1 health (capped at max)

---

## Rooms & Environments

### Standard Rooms

Each room is 800x600 pixels with:
- Solid ground at the bottom (48px)
- Exit zone on right edge (30px wide, extends below screen)
- Procedurally placed platforms
- Enemy spawns based on depth

### Platform Patterns (5 types)

1. **Staircase Right** — Ascending platforms left to right
2. **Staircase Left** — Ascending platforms right to left
3. **Two Levels** — Ground level split with high platform
4. **Scattered** — Multiple small platforms at various heights
5. **High Path** — Emphasis on elevated movement

### Platform Sizes

| Size | Dimensions |
|------|------------|
| Small | 100×20 px |
| Medium | 160×24 px |
| Large | 240×24 px |

All platforms except ground are **one-way** — you can jump through from below.

---

### Special Rooms

#### Upgrade Room (Every 5 depths)

Safe sanctuary at depths 5, 10, 15, 20.

- No enemies
- Two upgrade choices presented
- Press 1 or 2 to select
- Must choose before exiting

**Available upgrades:**

| Upgrade | Effect |
|---------|--------|
| Vitality | +2 Max Health, restore 2 HP |
| Resilience | +1 Max Health, restore 1 HP |
| Attunement | +3 Max Signal |
| Flow | +50% Signal Regen |
| Sharp Edge | +1 Slash Damage |
| Swift | +20% Move Speed |
| Long Dash | +50% Dash Distance |
| Second Wind | Restore 3 Health |

---

#### Rest Room (12% chance, depth 2+)

Healing sanctuary with shrine.

- No enemies
- Green glowing shrine at center
- Automatically heals +2 HP on entry
- Floating green particles

---

## Progression Systems

### Depth

Your progress is measured in **depth** — how far you've descended into the spectrum.

- Start at depth 1
- Each room transition increases depth by 1
- Depth 20 is victory (your sister)

### Score

Points earned by defeating enemies:

| Enemy | Points |
|-------|--------|
| Crawler | 10 |
| Spitter | 25 |
| Drifter | 30 |
| Rusher | 50 |
| Pillar | 75 |

### Echo (Persistent Currency)

Echo is earned on death based on depth reached:

| Depth | Base Echo |
|-------|-----------|
| 1-4 | 1 |
| 5-9 | 3 |
| 10-14 | 6 |
| 15-19 | 10 |
| 20 (victory) | 15 |

**Bonuses:**
- New best depth: +2 Echo

---

## Persistent Growth

Death is not punishment — it's progress. Every run teaches the spectrum something about you.

### Upgrades Menu (U key)

Spend Echo on permanent unlocks:

#### Starting Bonuses

| Unlock | Cost | Effect | Requires |
|--------|------|--------|----------|
| Vitality I | 5 | +1 starting health | — |
| Vitality II | 10 | +1 starting health | Vitality I |
| Vitality III | 15 | +1 starting health | Vitality II |
| Attunement I | 5 | +2 starting signal | — |
| Attunement II | 10 | +2 starting signal | Attunement I |
| Attunement III | 15 | +2 starting signal | Attunement II |

#### Quality of Life

| Unlock | Cost | Effect |
|--------|------|--------|
| Signal Sight | 10 | See enemy health bars |
| Momentum | 15 | Standing drains 50% less signal |
| Last Stand | 20 | Once per run, survive lethal hit with 1 HP |

### Fever Dreams

Random events that trigger on death, before the death screen.

**Probability by death count:**

| Deaths | Stat Echo | Memory | Resonance |
|--------|-----------|--------|-----------|
| 1-5 | 60% | — | — |
| 6-15 | 50% | 20% | — |
| 16-30 | 40% | 25% | 15% |
| 31+ | 30% | 20% | 15% |

### Stat Echoes

Small permanent stat gains from the spectrum's memory:

| Bonus | Amount | Weight |
|-------|--------|--------|
| Max Health | +0.5 | 25% |
| Max Signal | +0.5 | 30% |
| Blade Damage | +0.05 | 20% |
| Signal Regen | +0.05 | 25% |

These accumulate infinitely. After 50+ deaths, you'll have noticeable permanent bonuses.

### Resonances

Rare surprise ability unlocks — gifts from the spectrum:

| Resonance | Effect |
|-----------|--------|
| Blade Style: Arc | Wider slash area |
| Blade Style: Thrust | Longer slash range |
| Signal Burst | Kills restore 1 signal |
| Echo Magnet | +50% echo earned |
| Last Gasp | Brief invincibility at 1 HP |
| Thread Sight | See enemy intentions briefly |

Each resonance can only be earned once.

### Progress Menu (P key)

View your accumulated progress:
- Total runs and best depth
- Spectrum memory (stat bonuses)
- Unlocked memories
- Unlocked resonances

---

## Visual Design

### Color Palette

| Element | Color | Hex |
|---------|-------|-----|
| Background (top) | Dark blue | #0f0f23 |
| Background (bottom) | Purple-blue | #2a2a5e |
| Player body | Dark grey | #1a1a2e |
| Eyes/Aura/Blade | Neon red | #ff0040 |
| Blade trail | Pink | #ff6680 |
| Enemies | Grey/White | #cccccc |
| Health | Green | #4ade80 |
| Signal | Blue | #60a5fa |
| Upgrade/Echo | Gold | #fbbf24 |
| Platforms | Grey-purple | #464666 |

### Z-Layer Ordering

```
BG:           -100
BG_PARTICLES: -50
PLATFORMS:    0
PICKUPS:      10
ENEMIES:      20
PLAYER_LEGS:  28
PLAYER:       30
PLAYER_ARM:   32
SLASH:        35
PARTICLES:    50
HUD:          100
```

### Procedural Sprites

All sprites are generated at runtime via Canvas API:

- **Redeye** — Cloaked figure with glowing red eyes and aura
- **Sword Arm** — Glowing energy blade with gradient
- **Slash Arc** — Semicircular attack effect
- **Enemies** — Each has distinct silhouette with static noise overlay
- **Platforms** — Gradient fill with highlight and red accent

### Particle Effects

| Effect | Trigger | Color |
|--------|---------|-------|
| Aura particles | Passive (every 0.15s) | Red |
| Slash burst | On attack | Red gradient |
| Static burst | Enemy death | White |
| Ambient | Background | White/Red |
| Jump dust | On jump | Grey |

---

## Technical Reference

### Engine

- **Framework:** KAPLAY.js v3001
- **Resolution:** 800×600
- **Platform:** HTML5 Canvas

### Game Constants

| Constant | Value | Description |
|----------|-------|-------------|
| SPEED | 300 | Player movement px/s |
| JUMP_FORCE | 550 | Jump velocity |
| GRAVITY_RISE | 1400 | Gravity while rising |
| GRAVITY_FALL | 1800 | Gravity while falling |
| MAX_FALL_SPEED | 600 | Terminal velocity |
| DASH_DISTANCE | 128 | Dash range px |
| DASH_DURATION | 0.15 | Dash time seconds |
| SIGNAL_MAX | 10 | Base max signal |
| SIGNAL_REGEN_RIGHT | 1.5 | Regen per second |
| SIGNAL_DRAIN_STILL | 0.5 | Drain per second |
| SIGNAL_DRAIN_LEFT | 1.5 | Drain per second |
| SLASH_COST | 1 | Signal per slash |
| SLASH_DAMAGE | 1 | Base damage |
| CRAWLER_SPEED | 120 | Enemy speed |
| SPITTER_FIRE_RATE | 2.0 | Seconds between shots |
| PROJECTILE_SPEED | 250 | Projectile px/s |
| RUSHER_CHARGE_SPEED | 600 | Charge px/s |
| HEALTH_DROP_CHANCE | 0.25 | 25% on kill |
| REST_ROOM_CHANCE | 0.12 | 12% per room |

### Save Data Structure

```javascript
{
  echo: number,
  totalRuns: number,
  bestDepth: number,
  unlocks: string[],      // ["vitality1", "attunement1", ...]
  memories: number[],     // [1, 2, 3, ...]
  resonances: string[],   // ["bladeArc", "signalBurst", ...]
  introSeen: boolean,
  stats: {
    bonusHealth: number,
    bonusSignal: number,
    bonusDamage: number,
    bonusRegen: number
  }
}
```

### Scenes

| Scene | Purpose |
|-------|---------|
| intro | Opening cinematic (first run only) |
| menu | Main menu with options |
| upgrades | Persistent unlock shop |
| progress | View unlocked content |
| game | Main gameplay |
| feverDream | Post-death dream sequence |
| gameover | Death screen with echo reward |

---

## Future Plans

### Phase 4: Interference (Cursed Runs)

A fever dream that makes your NEXT run harder — but with greater rewards.

**Curse types:**
- Static Surge — Enemies have +1 health (1.5× Echo)
- Signal Drain — Signal drains 25% faster (1.5× Echo)
- Scarce Threads — 50% fewer pickups (1.75× Echo)
- Relentless — Enemies move 20% faster (1.75× Echo)
- The Silence — Sister's beacon invisible (2× Echo)

### Resonance Gameplay Effects

Currently tracked but not implemented:
- Wider/longer slash attacks
- Signal restoration on kills
- Echo multipliers
- Invincibility triggers

### Additional Content Ideas

- New enemy types
- Boss encounters
- Alternative paths/routes
- New abilities (Push, Pull, Double Jump, Wall Slide, Air Dash)
- Story expansion
- Victory ending sequence

---

*The spectrum remembers. Keep dying. Keep growing. Find her.*
