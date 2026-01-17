# REDEYE Game Design

## Overview

**Title:** REDEYE

**Genre:** Sidescroller Roguelike

**Engine:** KAPLAY.js (via Salvador game skill)

**Visual Style:** Geometric/simple shapes with glow effects, NEON color palette

**Core Fantasy:** You are a rare individual who perceives the Red Spectrum - a wavelength connecting all matter in the universe. Your sister, a stronger Redeye, has been taken. Follow her signal through hostile territory to rescue her.

---

## Story & Lore

### The Red Spectrum

A wavelength that permeates the universe, invisible to most. It appears as threads connecting all matter - a web of crimson light that pulses with the rhythm of existence itself.

Most beings see nothing. But rare individuals - **Redeyes** - perceive it clearly. To them, the universe is not empty space but a vast network of red threads, each one a connection that can be felt, pulled, pushed, listened to.

### The Redeyes

Those born with sensitivity to the Red Spectrum. Their eyes glow faintly red - a mark of their gift. They are:

- **Listeners** - They hear the hum of the universe, sensing danger before it arrives
- **Weavers** - They can pull threads, drawing objects (or enemies) toward them
- **Breakers** - They can push, amplifying waves to create repulsion
- **Shapers** - The rarest. They can condense the spectrum itself into solid form

A Redeye who masters all aspects can create a blade of pure focused intention - the **Red Edge**.

### The Corruption: Static

Something is polluting the Red Spectrum. A interference pattern - **Static** - that spreads like a virus through the threads.

Beings consumed by Static lose themselves. They become hollow vessels of noise, drawn to Redeyes like moths to flame. Not to join them - but to **silence** them. Every Redeye extinguished is another node of clarity lost, another victory for the spreading interference.

The Static has no face, no leader, no demands. It simply grows.

### The Sister

Your sister was always the stronger Redeye. Where you see threads, she sees the entire tapestry. Where you hear whispers, she hears symphonies.

That made her valuable.

She was taken by those who serve the Static - whether knowingly or as puppets, you don't know. Her signal, once bright and clear, now flickers at the edge of your perception. Faint. Desperate. But alive.

She's being held somewhere deep. Her power is being used - or will be used - to broadcast the Static everywhere. To end all Redeyes. To silence the spectrum forever.

Or perhaps they don't understand what they have. Perhaps she's the key to cleansing the corruption entirely.

Either way, you have to reach her.

### The Journey

You follow her signal. Each room, each depth, brings you closer. The Static's servants grow more desperate to stop you as you near their prize.

You don't know what you'll find. You only know you have to keep moving.

The signal calls. You answer.

---

## Characters

### Redeye (Protagonist)

**Appearance:**
- Dark silhouette body (cloak/robe shape, simple geometric form)
- Two glowing red eyes (the defining feature)
- Subtle red particle aura that trails when moving
- Size: ~32x48 pixels

**Personality (conveyed through action, not dialogue):**
- Determined - always pushing forward
- Connected - pauses briefly when sister's signal pulses
- Skilled - movements are precise, intentional

**Animations:**
- Idle: Slight breathing (scale pulse 1.0 → 1.03), eyes glow pulse, aura drifts upward
- Run: Cloak flows, aura trails behind
- Jump: Tucks slightly, aura streams downward
- Dash: Stretches horizontally, leaves red afterimage
- Slash: Red Edge extends in arc, bright trail, particles burst

### The Sister (Goal/Beacon)

**Appearance:**
- Never fully seen during gameplay
- Represented as a red dot/glow at top of screen
- In Echo Chamber memories: Similar to Redeye but taller, brighter aura, eyes glow stronger

**Presence:**
- Her signal pulses slowly (she's alive)
- Flickers when you take damage (she feels your pain)
- Grows brighter as you descend deeper (getting closer)
- Brief text fragments between rooms: her voice breaking through static

**Voice fragments (examples):**
- "Keep coming..."
- "I can feel you getting closer..."
- "They're trying to silence me..."
- "Don't stop. Whatever you hear, don't stop."
- "I'm still here. I'm still—" [cuts to static]

### The Static (Enemy Faction)

**Nature:**
- Not a single entity but a corruption
- Consumes beings, turns them into hollow vessels
- Drawn to Redeyes to extinguish their signal
- Visual language: jagged shapes, white/grey coloring, noise/flicker effects, no eyes

---

## Core Mechanics

### Movement

**Philosophy:** Responsive, fluid, forward-momentum rewarded.

| Action | Input | Signal Cost | Description |
|--------|-------|-------------|-------------|
| Run | Arrow keys / WASD | 0 | Instant acceleration, 300 px/sec |
| Jump | Space | 0 | 550 px/sec initial, hybrid gravity (floaty apex, fast fall) |
| Double Jump | Space (air) | 1 | Unlockable upgrade |
| Dash | Shift | 1 | 128px distance, 0.15 sec, i-frames |
| Wall Slide | Hold toward wall | 0 | Unlockable, slow descent |
| Wall Jump | Space (on wall) | 0 | Unlockable |

**Movement Values:**
```
HORIZONTAL
├── Acceleration: Instant
├── Top speed: 300 px/sec
├── Deceleration: Instant
└── Air control: Full (can reverse mid-air)

VERTICAL
├── Jump force: 550 px/sec
├── Gravity (rising): 1400 px/sec²
├── Gravity (falling): 1800 px/sec²
├── Max fall speed: 600 px/sec
└── Jump height: ~108 pixels

DASH
├── Distance: 128 pixels
├── Duration: 0.15 seconds
├── Speed: ~850 px/sec
├── Invincibility: Full i-frames
└── Direction: Horizontal only
```

### The Signal (Resource System)

**Concept:** Your connection to the Red Spectrum. Powers everything.

```
SIGNAL METER
├── Starting value: 10
├── Maximum: 10 (upgradeable)
└── Display: Top-left bar with numeric value

REGENERATION (per second)
├── Moving right: +1.5 (following sister's call)
├── Standing still: -0.5 (hesitation weakens connection)
├── Moving left: -1.5 (fighting the current)

COSTS
├── Slash: 1 Signal
├── Dash: 1 Signal
├── Push: 2 Signal
├── Pull: 2 Signal

AT ZERO SIGNAL
├── Cannot slash (blade dissipates)
├── Cannot dash
├── Cannot use powers
├── Movement still works
├── Visual: Aura flickers, eyes dim
└── Recovery: Move right or wait
```

### Combat: The Red Edge

**Concept:** Your blade is condensed intention - focused Red Spectrum made solid.

```
SLASH
├── Duration: 0.12 seconds
├── Range: 48 pixels (1.5x character width)
├── Hitbox: 90° arc in facing direction
├── Base damage: 1
├── Recovery: 0.05 seconds
├── Movement: Can move while slashing
├── Air slash: Yes, forward only
├── Signal cost: 1

VISUAL
├── Blade extends from hand in arc
├── Bright red core, lighter red trail
├── Particles burst on swing
├── Screen shake on hit: 2px, 0.05 sec
```

### Powers

**Push (Unlockable)**
```
├── Cost: 2 Signal
├── Range: 100 pixels
├── Effect: Knockback enemies, deflect projectiles
├── Deflected projectiles damage enemies
├── Cooldown: 0.5 seconds
```

**Pull (Unlockable)**
```
├── Cost: 2 Signal
├── Range: 150 pixels
├── Effect: Yank enemies/items toward you
├── Yanked enemies are briefly stunned
├── Cooldown: 0.5 seconds
```

### Controls

```
KEYBOARD
├── Move: Arrow keys OR WASD
├── Jump: Space (or W / Up Arrow)
├── Dash: Shift
├── Slash: X or J or Left Click
├── Push: C or K (when unlocked)
├── Pull: V or L (when unlocked)
└── Pause: Escape
```

---

## Enemies: The Static

### Visual Language

All enemies share these traits:
- Jagged, geometric shapes
- White/grey base color with static/noise texture
- No eyes (blind to the spectrum)
- Erratic micro-jitter even when "still"
- Death: Burst into static particles

### Enemy Types

#### Crawler (Fodder)

**Role:** Teaches basic slashing, creates pressure through numbers.

```
VISUAL
├── Shape: Low, wide rectangle (cockroach-like)
├── Color: White/grey with static flicker
├── Size: 28x20 pixels
└── Animation: Legs scramble, body jitters

BEHAVIOR
├── Detection: Entire room
├── Movement: 80 px/sec toward player
├── Attack: Contact damage (1 HP)
├── Health: 1
└── Death: Static particle burst, 1-frame screen freeze

SPAWNS
├── From room edges
├── Groups of 2-5
└── Staggered timing
```

#### Spitter (Shooter)

**Role:** Forces use of Push (deflect) or Dash (dodge). Ranged threat.

```
VISUAL
├── Shape: Vertical blob with "mouth" opening
├── Color: White/grey, mouth glows before firing
├── Size: 24x36 pixels
└── Projectile: Small white orb with static trail

BEHAVIOR
├── Detection: Entire room
├── Movement: Stationary
├── Attack: Fires every 2 seconds
├── Projectile speed: 250 px/sec
├── Projectile damage: 1 HP
├── Health: 2
├── Telegraph: Mouth glows 0.5 sec before firing
└── Death: Collapses, static particles

COUNTERPLAY
├── Push to deflect (costs 2 Signal)
├── Deflected projectile damages enemies
├── Dash through (i-frames)
└── Jump over
```

#### Rusher (Charger)

**Role:** Punishes standing still. Tests positioning and dash timing.

```
VISUAL
├── Shape: Angular, pointed triangle/arrow
├── Color: White/grey, glows red before charge
├── Size: 32x32 pixels
└── Trail: Static smear during rush

BEHAVIOR
├── Detection: Horizontal line of sight
├── Idle: Paces on platform
├── Trigger: Spots player horizontally
├── Telegraph: Stops, vibrates, glows (0.7 sec)
├── Charge: 600 px/sec straight line
├── Charge distance: ~400 pixels or until wall
├── Stun: 1 second after charge (vulnerable)
├── Damage: 2 HP (contact during charge)
├── Health: 2
└── Death: Explodes into sharp static shards

COUNTERPLAY
├── Jump over charge
├── Dash through (i-frames)
├── Attack during stun window
└── Push during charge (advanced - cancels it)
```

#### Drifter (Floater)

**Role:** Vertical threat. Tests jump-slashing accuracy.

```
VISUAL
├── Shape: Jellyfish-like, amorphous
├── Color: Translucent white, static core
├── Size: 28x28 pixels
└── Animation: Gentle bob, tendrils wave

BEHAVIOR
├── Detection: 150 pixels
├── Movement: 40 px/sec drift toward player
├── Vertical: Sine wave bob, 30px amplitude
├── Attack: Contact damage (1 HP)
├── Health: 1
└── Death: Pops like bubble, static spray

SPAWNS
├── Mid-air positions
├── Groups of 2-3
└── Block vertical paths
```

#### Pillar (Tank)

**Role:** Resource management test. Cannot be ignored cheaply.

```
VISUAL
├── Shape: Tall, heavy rectangle (golem-like)
├── Color: Dark grey, white static veins
├── Size: 40x64 pixels
├── Animation: Slow lumber, ground shakes
└── Damage feedback: Cracks appear per hit

BEHAVIOR
├── Detection: 200 pixels
├── Movement: 50 px/sec toward player
├── Attack: Slam (1.5 sec windup), 2 HP damage
├── Slam creates ground shockwave (must jump)
├── Health: 4
├── Knockback resistance: Immune to Push
└── Death: Crumbles, static explosion

COUNTERPLAY
├── Hit and run (4 slashes minimum)
├── Jump to avoid shockwave
├── Dash through during slam windup
└── Sometimes better to avoid entirely
```

### Enemy Modifiers (Difficulty Scaling)

Applied to base enemies as depth increases:

| Modifier | Effect |
|----------|--------|
| Fast | +50% movement speed |
| Armored | +1 health |
| Splitter | Crawler splits into 2 mini-crawlers on death |
| Homing | Spitter projectiles curve toward player |
| Relentless | Rusher charges twice before stun |

---

## Room Generation

### Philosophy

**Chunk-based assembly with rules.** Hand-designed pieces combined procedurally. Every room is solvable, but the combination is fresh.

### Room Structure

```
DIMENSIONS
├── Width: 800 pixels (one screen)
├── Height: 600 pixels
├── Ground: Always exists at bottom
└── Exit: Right side, opens when clear (or speedrun past)

ASSEMBLY
Room = [Entry Chunk] + [Middle Chunk x 1-3] + [Exit Chunk]

DURATION
├── Target: 10-20 seconds to clear
├── Skilled: 5-8 seconds
└── Struggling: 30+ seconds (still winnable)
```

### Chunk Types

#### Entry Chunks (Safe Landing)

```
ENTRY_FLAT
══════════════════
(simple ground)

ENTRY_STEP
        ════════
══════════
(elevated view)

ENTRY_DROP
        ════
            ════
══════════════════
(see room below)
```

#### Exit Chunks (Transition Out)

```
EXIT_FLAT
══════════════════ ▶ DOOR

EXIT_CLIMB
              ════ ▶ DOOR
        ════
══════════

EXIT_JUMP
                   ▶ DOOR
              ════
══════════════════
```

#### Middle Chunks (The Challenge)

**Category A: Ground Combat**
```
GROUND_FLAT - Open ground, Crawler swarms
GROUND_PIT - Gap to jump, enemies both sides
GROUND_SPIKES - Spike pit, forces jumping while fighting
```

**Category B: Vertical Challenge**
```
VERTICAL_STEPS - Ascending platforms, Drifters between
VERTICAL_TOWER - High Spitter, Crawlers below
VERTICAL_PILLARS - Narrow platforms, weaving Drifters
```

**Category C: Pressure Zones**
```
PRESSURE_CORRIDOR - Two paths, Rusher on one, fodder on other
PRESSURE_CROSSFIRE - Multiple Spitters at angles
PRESSURE_ARENA - Open space with Pillar
```

### Assembly Rules

```
RULE 1: Entry always safe (no enemies, 1 second to assess)

RULE 2: Difficulty curve within room
├── First middle chunk: Easier (Ground category)
└── Later chunks: Can be harder (Vertical, Pressure)

RULE 3: No impossible combos
├── No consecutive spike pits
├── Max 1 Pillar per room
├── Max 2 Spitters per room
├── Rusher needs flat ground

RULE 4: Chunks must connect
├── Exit height must match next entry height
└── Or add bridge piece

RULE 5: Reward density
├── Pickup opportunity every 2-3 chunks
└── Slightly risky placement (not free)
```

### Difficulty Scaling by Depth

```
DEPTH 1-5 (Learning)
├── Middle chunks: 2
├── Enemy budget: 3-5
├── Enemy types: Crawlers only
├── Modifiers: None
└── Pickups: Health every 2 rooms

DEPTH 6-10 (Escalation)
├── Middle chunks: 2-3
├── Enemy budget: 5-8
├── Enemy types: Crawlers, Spitters, Rushers
├── Modifiers: Occasional Fast
└── Pickups: Health every 3 rooms

DEPTH 11-15 (Pressure)
├── Middle chunks: 3
├── Enemy budget: 8-12
├── Enemy types: All
├── Modifiers: Fast, Armored common
└── Pickups: Health every 4 rooms

DEPTH 16+ (Survival)
├── Middle chunks: 3-4
├── Enemy budget: 12+
├── Enemy types: All, multiple Pillars possible
├── Modifiers: Multiple per enemy
└── Pickups: Rare
```

### Special Rooms

```
AMBUSH ROOM (1 in 15 chance)
├── Doors lock on entry
├── Enemy waves until cleared
└── Reward: Guaranteed upgrade pickup

TREASURE ROOM (1 in 20 chance)
├── No enemies
├── Rare pickup (full heal, temp power boost)
└── Tricky platforming to reach

ECHO CHAMBER (1 in 25 chance)
├── Sister memory fragment
├── No enemies, exploration only
├── Visual storytelling scene
└── Bonus Echo on run end
```

---

## Progression Systems

### Run Structure

```
[Room 1] → [Room 2] → [Room 3] → [Room 4] → [Room 5: UPGRADE ROOM]
                                                      │
[Room 6] → [Room 7] → [Room 8] → [Room 9] → [Room 10: UPGRADE ROOM]
                                                      │
                            ... continues ...
                                                      │
                                              [Depth 20+: REACH SISTER]
```

### Upgrade Rooms (Every 5 Depths)

Safe room. No enemies. Choose one of two random upgrades.

**Movement Upgrades:**
- Double Jump
- Wall Slide + Wall Jump
- Dash Distance +50%
- Air Dash (any direction while airborne)

**Combat Upgrades:**
- Blade Reach +30%
- Blade Damage +1
- Slash costs 0 Signal
- Slash hits twice (quick double swing)

**Power Upgrades (require Push/Pull unlocked):**
- Push Range +50%
- Push costs 1 Signal (instead of 2)
- Pull yanks enemies all the way to you
- Deflected projectiles home toward enemies

**Signal Upgrades:**
- Max Signal +5
- Signal regen +50%
- Standing still no longer drains Signal
- Kills restore 2 Signal

**Survival Upgrades:**
- Max Health +2
- Dash i-frames last longer
- Take half damage from contact
- Health pickup effectiveness +100%

### Meta Progression (Between Runs)

**Echo Currency:**
Earned based on depth reached when you die.

```
├── Depth 5: 1 Echo
├── Depth 10: 3 Echo
├── Depth 15: 6 Echo
├── Depth 20: 10 Echo
└── Scaling continues
```

**Permanent Unlocks:**

| Unlock | Echo Cost | Effect |
|--------|-----------|--------|
| Push | 5 | Available from start of run |
| Pull | 10 | Available from start of run |
| Signal Boost | 8 | Start with +3 Max Signal |
| Vitality | 8 | Start with +2 Max Health |
| Blade Style: Arc | 15 | Cosmetic + wider slash |
| Blade Style: Thrust | 15 | Cosmetic + longer range, narrower |
| Memory Fragment 1 | 20 | Lore reveal |
| Memory Fragment 2 | 20 | Lore reveal |
| Memory Fragment 3 | 20 | Lore reveal |

---

## User Interface

### HUD Layout

```
┌─────────────────────────────────────────────────────────────┐
│ SIGNAL: ████████░░ 8/10          Depth: 7    ────────🔴     │
│ HEALTH: ♥♥♥♥♥                                    (sister)   │
│                                                             │
│                                                             │
│                         [GAMEPLAY AREA]                     │
│                                                             │
│                                                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Sister's Signal (Top Right)

- Red dot showing relative direction to goal
- Pulses when making progress
- Flickers when you take damage
- Grows brighter each room cleared

### Health Display

- Heart icons (default 5)
- Lost hearts dim/crack
- Regenerated hearts glow briefly

### Signal Bar

- Red fill on dark background
- Numeric value displayed
- Flickers when at zero
- Pulses when regenerating

### Between-Room Transitions

- Brief black screen
- Sister's voice fragment (text)
- Depth counter increments
- ~1 second duration

---

## Visual Specification

### Color Palette: NEON

```
BACKGROUNDS
├── Primary: #0f0f23 (deep void)
├── Secondary: #1a1a3e (slightly lighter)
└── Accent: #2a2a5e (platform hint)

REDEYE (Protagonist)
├── Body: #1a1a2e (dark silhouette)
├── Eyes: #ff0040 (bright red)
├── Aura: #ff0040 at 30% opacity
├── Blade: #ff0040 core, #ff6680 trail
└── Particles: #ff0040 to #ff9999

STATIC (Enemies)
├── Body: #cccccc (white-grey)
├── Noise: #ffffff flicker overlay
├── Damage: #ff4444 flash
└── Death particles: #ffffff to transparent

PICKUPS
├── Health: #4ade80 (green)
├── Signal boost: #60a5fa (blue)
└── Upgrade: #fbbf24 (gold)

UI
├── Text: #ffffff
├── Text dim: #8080a0
├── Health full: #ff0040
├── Health empty: #4a4a4a
├── Signal bar: #ff0040
└── Signal empty: #2a2a2a
```

### Procedural Sprite Guidelines

All sprites generated using Salvador's procedural system:

**Redeye:**
- Dark rounded rectangle body (cloak suggestion)
- Two bright circles for eyes
- Particle emitter for aura

**Enemies:**
- Basic geometric shapes
- Static noise overlay (animated)
- No eyes (key visual distinction)

**Platforms:**
- Rectangles with subtle top highlight
- Edge glow in dark environments

**Effects:**
- Particles: Small circles with lifespan fade
- Slash: Arc shape with trail
- Dash: Stretched afterimage

---

## Audio Direction (Notes for Future)

Not implemented in prototype, but design intent:

**Music:**
- Ambient, pulsing electronic
- Low hum that increases intensity with depth
- Sister's signal as melodic motif

**SFX:**
- Slash: Short metallic whoosh
- Dash: Quick air displacement
- Jump: Soft cloth rustle
- Enemy hit: Static crackle
- Enemy death: Burst of noise, then silence
- Push/Pull: Deep bass thrum
- Low Signal warning: Fading hum
- Sister's fragments: Distant, reverbed voice

---

## Technical Notes for Salvador

### Framework
- KAPLAY.js for 2D rendering
- Procedural sprites (no external assets)
- NEON color palette from visual-assets.md

### Prototype Priority

**Phase 1: Core Feel**
- Movement (run, jump, dash)
- Signal meter with drain/regen
- Blade slash with visual feedback
- Single test room

**Phase 2: Combat**
- Crawler enemy
- Collision/damage system
- Health display
- Death/restart

**Phase 3: Progression**
- Room transitions
- Procedural room assembly (basic)
- Depth counter
- Spitter and Rusher enemies

**Phase 4: Polish**
- Upgrade rooms
- All enemy types
- Special rooms
- Meta progression (Echo)

### Key Implementation Details

```javascript
// Movement constants
const SPEED = 300
const JUMP_FORCE = 550
const GRAVITY_RISE = 1400
const GRAVITY_FALL = 1800
const DASH_DISTANCE = 128
const DASH_DURATION = 0.15

// Signal constants
const SIGNAL_MAX = 10
const SIGNAL_REGEN_RIGHT = 1.5  // per second
const SIGNAL_DRAIN_STILL = 0.5
const SIGNAL_DRAIN_LEFT = 1.5
const SLASH_COST = 1
const DASH_COST = 1
const PUSH_COST = 2
const PULL_COST = 2

// Combat constants
const SLASH_DURATION = 0.12
const SLASH_RANGE = 48
const SLASH_DAMAGE = 1
```

---

## Win Condition

Reach Depth 20 (or configurable endpoint).

Final sequence:
1. Last room is empty except for a bright red glow
2. Approach the glow
3. Sister reunion scene (simple visual)
4. "Signal Restored" - run complete
5. Display final stats, Echo earned
6. Return to menu

---

## Summary

REDEYE is a sidescroller roguelike about connection, perseverance, and family. The player uses a unique resource system (Signal) that rewards forward momentum while allowing tactical retreats at a cost. Combat combines responsive melee with timing-based deflection. Procedural rooms assembled from hand-designed chunks ensure every run is different but fair. Meta-progression provides long-term goals while each individual run tells a complete story of the journey toward rescue.

The visual style is achievable with procedural generation: geometric shapes, glow effects, and high contrast create atmosphere without custom art assets.

The sister's presence as a UI element and distant voice keeps the emotional stakes present without cutscenes or dialogue-heavy storytelling.

Go save her.
