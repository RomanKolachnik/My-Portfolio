---
layout: default
title: Echoes of Continuity
---

# Echoes of Continuity

> *2D Roguelike Dungeon-Crawler • Solo Developed • Unity (C#) • Released on itch.io*

![Banner](https://img.itch.zone/aW1nLzE2MDI1OTU2LnBuZw==/original/soks8r.png)

---

## Trailer / Gameplay Demo

<iframe width="100%" height="400" src="https://www.youtube.com/embed/HWR3zxpfLxE" frameborder="0" allowfullscreen></iframe>

---

## My Role

I developed **Echoes of Continuity** solo from the ground up, handling every aspect of gameplay programming, including:

- Built a modular item system with interface-based stat scaling, activation effects, and synergy logic
- Designed character abilities that scale based on stat types (e.g., Strength, Wisdom)
- Implemented procedural dungeon generation with structure spawn rules and tile conditions
- Developed time-based difficulty scaling for both enemy stats and level generation
- Designed finite state machine (FSM) AI for melee and ranged enemy behaviors

---

## Features I Developed

### Modular Item System (Stat-Scaling + Effects)

<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>

```csharp
class ItemInfo (ScriptableObject):
    string name
    Sprite icon
    Rarity rarity
    float activateChance
    CharacterStatBuffs statModifiers
    bool hasOnKill, hasOnHit, hasOnShoot, hasPassive, hasOnDamaged

    function OnItemAdded(playerStats)
    function OnItemRemoved(playerStats, amount)
    function OnKill(playerStats, enemyStats)
    function OnShoot(playerStats)
    function OnHit(playerStats, enemyStats)
    function OnPassive(playerStats)
    function OnDamaged(playerStats)
```

Echoes of Continuity features a fully modular, event-driven item system using ScriptableObjects. Each item defines both passive stat buffs and custom trigger-based effects (e.g., on-kill, on-hit, on-shoot). Items are scalable, stackable, and plug directly into the game loop with minimal coupling.

- All items inherit from a base ItemInfo ScriptableObject
- Items can define their trigger behaviors (e.g., OnKill, OnShoot, OnDamaged, etc.)
- Flags (e.g., HasOnKill) allow the game to check only relevant triggers
- Items can scale based on stack count, and spawn in from enemy deaths based on their rarity
- Effects apply to the player who owns the item, enabling unique builds and synergy

---

### Procedural Dungeon Generation

<img src="assets/echoes-of-continuity/dungeon-gen.gif" alt="Dungeon generation demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Procedural dungeon layout forming with rules applied</em></p>

```csharp
function GenerateMap():
    for each (x, y) in map:
        map[x][y] = randomChance() < fillPercent ? 1 : 0

    repeat smoothingIterations times:
        for each (x, y) in map:
            neighborWalls = countNeighbors(x, y)
            if neighborWalls > 4:
                map[x][y] = 1
            else:
                map[x][y] = 0

function RenderMap():
    for each (x, y) in map:
        if map[x][y] == 1:
            tilemap.setTile(x, y, groundTile)
        else:
            tilemap.setTile(x, y, null)
```

Echoes of Continuity features a procedural dungeon generator built using cellular automata principles and Unity’s Tilemap system. The map layout is randomized and then iteratively smoothed, generating believable caverns and arenas. The system also spawns gameplay elements like item pedestals and portal rooms, dynamically adjusting to player position and ensuring valid spawn zones.

**How It Works**

1. Map Initialization
> - A 2D grid is seeded with random 1s and 0s based on a fill percentage (e.g., 45% land, 55% empty)
> - A random seed ensures replayable procedural layouts

2. Smoothing with Cellular Automata
> - The grid undergoes multiple smoothing passes, each using Moore neighborhood rules to determine whether a tile should be wall or ground based on its surroundings
> - This transforms random noise into organic, cave-like structures

3. Tilemap Rendering
> - After the map is finalized, the system draws tiles onto Unity’s Tilemap, setting ground and walls where appropriate
> - Portal rooms and overlays are placed manually with scripted tile patterns

4. Spawn Logic
> - The system finds valid player spawn points by scanning for areas with sufficient surrounding ground tiles (≥24 nearby)
> - Item pedestals and other objects are spawned in open areas, spaced out with retries

---

### Modular Enemy AI System

<iframe width="100%" height="400" src="https://romankolachnik.github.io/My-Portfolio/Assets/Images/enemy-comp.mp4" frameborder="0" allowfullscreen></iframe>
<p style="text-align: center;"><em>Video: Qasmine Alpha switching into ranged attack.</em></p>

```csharp
// Base Enemy Logic
class Enemy:
    bool canMove = true
    float sightDistance
    float attackDistance
    GameObject player
    Coroutine knockbackCoroutine

    function Knockback(force, duration):
        temporarily disable movement
        apply knockback
        re-enable movement
```

Echoes of Continuity features a modular, component-based enemy system, where each enemy derives from a common Enemy base class and assembles its behavior through independent behavior scripts. This composition-based approach allows for scalable, reusable, and diverse enemy types that can share logic or define completely unique behaviors.

For example, the Qasmine Alpha enemy is a mid-range enemy, capable of chasing the player, aiming when in range, and firing a projective after a short wind-up. As such, their script is as follows:

```csharp
// Qasmine Alpha Composition
class QasmineAlpha inherits Enemy:
    AimBehaviour aim
    ChaseBehaviour chase
    float rangedDistance
    float cooldown, timer

    if playerInRangedDistance and cooldownOver:
        disable chase
        enable aim
        play attack animation
    else if playerInSightDistance:
        enable chase
    else:
        disable all movement

    function Fire():
        spawn projectile at aim point

    function EndFire():
        destroy projectile
        start cooldown timer
```

By designing the enemy AI through composition instead of inheritance, I am able to reuse multiple behaviours across different enemies, and write cleaner, more readable, and modular code. 

---
