# REDEYE: Narrative Design

## Overview

This document defines how REDEYE's story and core concept are communicated to players without exposition dumps or walls of text.

**Core concept to convey:** Redeye feels and connects with the Universe via red waves, drawing power from the vast network of threads that permeate existence.

**Design principle:** Show, don't tell. The player should *feel* the connection, not read about it.

---

## Opening Sequence

### Purpose

Establish the core concept visually before gameplay begins. No text, no narration - pure visual storytelling.

**What it communicates:**
1. You are the red eyes (identity)
2. You perceive/feel in threads (the spectrum)
3. Someone else is out there (the sister)
4. You're reaching for them (the journey)

### Timing

**Total duration:** 5 seconds (skippable)

```
[0.0s] Black screen

[0.5s] Single red dot fades in (center)
       └── This is Redeye's eye

[1.0s] Second eye fades in beside it
       └── Now clearly a face/presence

           ◉ ◉

[1.5s] SKIPPABLE FROM THIS POINT
       └── "Press any key" hint fades in (subtle, bottom of screen)

[2.0s] Faint red threads begin extending outward
       └── Slow, organic movement
       └── Reaching toward edges of screen

                │
            ╲   │   ╱
              ╲ │ ╱
               ◉ ◉
              ╱ │ ╲
            ╱   │   ╲
                │

[3.0s] Distant red dot pulses once (top right area)
       └── Sister's signal
       └── One thread visibly connects toward it

[4.0s] Threads fade, eyes remain
       └── Brief pause

[4.5s] Fade to menu screen

[5.0s] Menu fully visible, game ready
```

### Skip Behavior

```
INPUT
├── Any key press or mouse click triggers skip
├── Not skippable before 1.5s (eyes must appear)
└── After 1.5s: instant skip available

SKIP TRANSITION
├── Quick fade to black (0.2s)
├── Fade to menu (0.3s)
└── Total skip transition: 0.5s

VISUAL
└── "Press any key" text at bottom, 30% opacity, fades in at 1.5s
```

### Frequency

```
WHEN SEQUENCE PLAYS

First run ever:
├── Full sequence plays
├── Skippable after 1.5 seconds
└── Player gets full context

Runs 2-9:
├── Sequence does NOT play
└── Straight to menu

Every 10th run (10, 20, 30...):
├── Sequence plays again
├── Skippable immediately (from 0.0s)
└── Serves as reminder/recontextualization

IMPLEMENTATION
├── Track total run count in local storage
├── Check: if (runCount === 1 || runCount % 10 === 0) showSequence()
└── First run: skipDelay = 1.5s, subsequent: skipDelay = 0s
```

### Optional Enhancement

On milestone runs (every 10th), the sister's distant dot pulses **brighter** than on run 1 - subtle indication that across many runs, you're making progress toward her.

---

## Menu Screen

### Layout

```
┌─────────────────────────────────────────────────┐
│                                                 │
│                                                 │
│                    REDEYE                       │
│                                                 │
│          "She's still out there.                │
│               Follow the signal."               │
│                                                 │
│                                                 │
│               [ PRESS SPACE ]                   │
│                                                 │
│                                                 │
│     Arrow keys: Move | Space: Jump | X: Slash   │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Elements

**Title:** "REDEYE" - Large, red glow effect

**Tagline:** Two short lines that establish:
- Someone is missing ("she")
- You're going after them ("follow")
- Core mechanic hint ("signal")

**Controls:** Always visible, player knows how to play immediately

### What's NOT Here

- No lore paragraphs
- No "Story" button
- No character backstory
- No world-building text

The menu is a door, not a library.

---

## In-Game Narrative: Sister's Voice

### Delivery Method

Between rooms, brief text appears for 2-3 seconds. Her voice breaking through the static.

```
VISUAL
├── Black or dim transition screen
├── Text fades in (center, italicized)
├── Holds for 2 seconds
├── Fades out
└── Next room loads

AUDIO (future)
└── Faint, reverbed female voice - as if traveling vast distance
```

### Fragments by Depth

**Depth 1-5 (Early) - Establishing Connection**

```
"You came."

"I knew you'd find me."

"Keep coming..."

"I can feel you. Faintly."

"They don't understand what I am."
```

**Depth 6-10 (Middle) - Revealing Threat**

```
"The Static... it's not random. It's searching."

"They want to use me to silence everything."

"You're the only signal I can still feel."

"Every step you take, I feel it through the threads."

"Don't stop. Whatever happens, don't stop."
```

**Depth 11-15 (Late) - Growing Urgency**

```
"I can see the whole spectrum now. It's beautiful... and terrifying."

"Don't trust the silence. That's where they hide."

"Almost. You're almost here."

"I've been holding on. Waiting for your signal."

"They're scared of you. I can feel their fear."
```

**Depth 16-20 (Final) - Reunion Near**

```
"I can hear your blade. I can feel you fighting."

"One more push. I'm right here."

"Together we can fix this. All of it."

"I see you. I finally see you."

"..."  [just static - she's saving strength]
```

### Frequency

- Not every room (would become noise)
- Every 2-3 rooms on average
- Random selection from depth-appropriate pool
- Never repeat same fragment in one run

---

## Echo Chamber Memories (Rare Rooms)

### Purpose

Deep lore for players who want it. Completely optional.

### Trigger

1 in 25 chance per room. Player can walk through without engaging.

### Format

No text. Visual scenes only - like looking through a window into the past.

**Memory 1: Discovery**
```
Two small silhouettes (children)
One points at something glowing red
The other reaches out - red threads extend from their hand
First discovery of the power
```

**Memory 2: The Static Arrives**
```
Peaceful scene with red threads visible everywhere
Suddenly: white noise creeps in from edges
Threads begin to flicker and break
A shadow falls across the scene
```

**Memory 3: The Taking**
```
Two figures (older now, recognizable as Redeye and sister)
Sister's signal flares bright
Dark shapes surround her
Redeye reaches out - threads extend but can't reach
She's pulled away into static
```

### Reward

- Completing (watching) an Echo Chamber grants bonus Echo at run end
- Also unlocks corresponding Memory Fragment in meta-progression menu
- Fragments can be re-viewed from menu once unlocked

---

## UI as Storytelling

### Sister's Signal Beacon

The red dot at the top of screen isn't just a UI element - it's the thread you saw in the opening.

```
BEHAVIOR

Signal strength affects visibility:
├── High Signal (8-10): Dot glows bright, pulses gently
├── Medium Signal (4-7): Dot visible, steady
├── Low Signal (1-3): Dot flickers
└── Zero Signal: Dot nearly invisible (connection severed)

Progress affects brightness:
├── Depth 1-5: Distant, small
├── Depth 6-10: Slightly larger
├── Depth 11-15: Noticeably brighter
├── Depth 16-20: Pulsing, close
└── Final room: Fills top of screen
```

### Damage Connection

When Redeye takes damage, the sister's dot flickers sympathetically.

She feels what you feel. The connection is two-way.

### Signal Meter as Connection Strength

The Signal meter isn't "mana" or "energy" - it's your connection to the Red Spectrum.

- Full signal = Strong connection to the universe
- Empty signal = Cut off, alone, vulnerable

This reframes resource management as narrative: you're not "running out of power," you're "losing your connection."

---

## Death Screen

### Layout

```
┌─────────────────────────────────────────────────┐
│                                                 │
│                                                 │
│               SIGNAL LOST                       │
│                                                 │
│              Depth reached: 12                  │
│                                                 │
│             Echo earned: +4                     │
│                                                 │
│                                                 │
│           "She's still waiting."                │
│                                                 │
│                                                 │
│              [ R to retry ]                     │
│           [ ESC for menu ]                      │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Key Elements

- **"Signal Lost"** - Not "Game Over" or "You Died." The connection was severed.
- **Depth reached** - Progress marker
- **Echo earned** - Reward for the attempt
- **"She's still waiting."** - Motivation to try again

---

## Victory Screen (Reaching Sister)

### Sequence

1. Final room is empty except for a bright red glow
2. Player approaches the glow
3. Screen slowly fills with red light
4. Two sets of eyes visible in the light (reunion)
5. Text: "Signal Restored"
6. Fade to stats screen

### Stats Screen

```
┌─────────────────────────────────────────────────┐
│                                                 │
│               SIGNAL RESTORED                   │
│                                                 │
│                                                 │
│              Final Depth: 20                    │
│              Time: 24:32                        │
│              Echo earned: +15                   │
│                                                 │
│                                                 │
│     "The threads are clear again.               │
│          Until the next silence."               │
│                                                 │
│                                                 │
│              [ SPACE to continue ]              │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Implication

"Until the next silence" suggests the Static will return. Victory is a battle won, not a war ended. Sets up replayability.

---

## Summary: The Core Truth

**The concept:** Redeye connects to the Universe via red waves.

**How it's communicated:**

| Method | What Player Experiences |
|--------|------------------------|
| Opening sequence | Sees the threads extending outward, reaching for sister |
| Menu tagline | "Follow the signal" - connection is the verb |
| Signal meter | Resource IS connection - empty = alone |
| Sister's beacon | The thread made persistent in UI |
| Beacon responds to Signal | High connection = bright beacon |
| Damage shared | She flickers when you're hurt |
| Her voice fragments | Coming through the threads |
| Death screen | "Signal Lost" - not death, disconnection |
| Victory screen | "Signal Restored" - connection renewed |

**What we never do:**
- Text box explaining the Red Spectrum
- Lore dump about Redeyes
- Narrator describing the threads
- Loading screen "tips" with world-building

**The player understands through experience.** By the time they've played 10 runs, they *know* Redeye is connected to something vast, reaching for someone dear, fighting through interference. They know it in their gut, not their head.

That's the goal.

---

*End of Narrative Design Document*
