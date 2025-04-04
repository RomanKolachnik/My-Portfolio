---
layout: default
title: Goofy Lil Guys
---

# Goofy Lil Guys

> *2.5D Creature Collector • Local Multiplayer • Team of 10 • Released on Steam*

![Banner](https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/3565690/2810b94b751e7ecebb318644b9b0e020a3dfccf7/header.jpg?t=1742702968)

---

## Trailer / Gameplay Demo

<iframe width="100%" height="400" src="https://video.fastly.steamstatic.com/store_trailers/257116853/movie480_vp9.webm?t=1742494208" frameborder="0" allowfullscreen></iframe>

---

## My Role

I worked as a **Gameplay Programmer** on this project, responsible for core systems including:

- Local multiplayer input using Unity’s Input System
- Combat mechanics for 9 unique characters with special moves
- Chain attack system to encourage team synergy
- AI behavior for wild creatures with personalities and obstacle avoidance
- Custom Unity editor tools for environment set-dressing (e.g., auto-grounding, sorters)

---

## Features I Developed

### Chain Attack System

<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>

```csharp
// Called when the player presses the attack button
function AttemptAttack():
    if playerIsDead or inSpecialAttack or not isAllowedToAttack:
        return

    if comboCount == 0 and timeSinceLastAttack > comboBufferTime:
        PerformAttack()
    else if comboCount > 0:
        queueNextAttack = true


// Triggered mid-animation (at chain window) to allow chaining
function EnableChainAttack():
    canChain = true

    if queueNextAttack and comboCount < maxCombo and isCurrentlyAttacking:
        queueNextAttack = false
        PerformAttack()


// Core attack logic: advances the combo and plays animation
function PerformAttack():
    comboCount += 1
    lastAttackTime = currentTime

    if animationAvailable:
        playAnimation("BasicAttack")
    else:
        spawnHitboxManually()


// Triggered near the end of an animation to check if a combo should continue
function CheckCombo():
    if canChain and queueNextAttack and comboCount < maxCombo:
        queueNextAttack = false
        PerformAttack()
    else if comboCount >= maxCombo:
        lastAttackTime = currentTime
        ResetCombo()
    else:
        ResetCombo()


// Resets the entire combo state
function ResetCombo():
    comboCount = 0
    canChain = false
    queueNextAttack = false
```

The chain attack system allows players to execute combo attacks by timing button presses during specific animation windows. If the attack is input early, it queues until chaining is allowed. The system ensures responsive combat while preventing unintended inputs, using internal flags and timing buffers. Animation events control the chain window and reset logic after the combo ends. 

---

### Local Multiplayer Input
<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>
In Goofy Lil Guys, the character select screen is a fully custom, hardcoded Unity UI system built to overcome the limitations of multiple players interacting with a single canvas.
<br><br>

- Players join in and are each assigned a UISelector — a floating cursor tied to their PlayerInput
- These selectors allow players to navigate independently across the character selection screen
- The screen is composed of three character cards, each with unique starters
- Because Unity's native UI systems struggle with differentiating multiple players on the same canvas, all navigation, submission, and cancel events are manually coded
- Each player’s inputs are routed to their selector, which sends feedback to the menu and updates visuals dynamically (e.g., color, shape, locked-in state)
- The system supports joining, backing out, and tutorial prompting, all fully synced across multiple players

---

### AI Personalities
<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>
In Goofy Lil Guys, I built a personality-driven AI behavior system for the “wild” creatures in the world. These AI-controlled Lil Guys make decisions based on randomly rolled traits (like timidness, intelligence, and charisma) that govern how they interact with players and the environment.
<br><br>

- AI can wander, chase, attack, flee, return home, or enter a dead state
- Every wild Lil Guy is assigned a unique personality profile when spawned
- Behavior dynamically shifts based on hostility level, distance to the player, and their current health
- A finite state machine (FSM) controls AI state transitions, while internal timers and spatial logic govern movement, prediction, and cooldowns
- The system scales elegantly across different archetypes (Strength, Speed, Defense) and supports both standard and legendary Lil Guys
<br><br>

Below is a table of each personality trait and what it does:

| Trait        | What It Affects |
|--------------|-----------------|
| Hostility    | Affects hostile behaviours such as attacking and chasing. Higher hostility can cause the enemy to their special attacks when in range |
| Timidness    | Affects how likely the wild Lil Guy is to flee from battle when at low health. Higher timidness values means a higher health cap for the wild Lil Guy to decide fighting isn't worth it, and will try to flee instead. |
| Intelligence | Affects how far ahead wild Lil Guys can predict player movements during chasing. Higher intelligence factors cause the Lil Guy to try and veer in front of the player's escape path, cutting them off. |
| Charisma     | Decides whether their hostility can spread to nearby wild Lil Guys. The charisma factor also increases this aggro radius the higher the charisma modifier is. |

Each trait is rolled between 0 and 10, with real-time behavioural changes tied to thresholds (ex. timid > 5 can flee, charisma > 5 will aggro others)

Below are pseudocode examples of how these traits affect decision making:
```csharp
// Main decision-making function
function UpdateAI():
    if health <= 0:
        ChangeState(Dead)
    else if timeAwayFromHome >= maxTime:
        ChangeState(ReturnHome)
    else if health < lowHealthThreshold and timid > 5:
        ChangeState(Flee)
    else if hostility > 3 and distanceToPlayer <= attackRange:
        ChangeState(Attack)
    else if hostility > 3 and distanceToPlayer <= chaseRange:
        ChangeState(Chase)
    else if isCatchable and time >= nextWanderTime:
        ChangeState(Wander)
    else:
        ChangeState(Idle)

// Chase logic affected by intelligence
function PredictPlayerPosition():
    if playerIsStationary:
        return playerPosition
    else if AI is behind player:
        return playerPosition + (playerDirection * speed * thinkSpeed)
    else:
        return playerPosition

// How charisma aggros wild lil guys in range
function AggroNearbyLilGuys():
    for each lilGuy in aggroRadius:
        if lilGuy is wild:
            lilGuy.IncreaseHostility(lerp(0, 2, charisma / 10))
```

---

### Editor Tools
Throughout development, I built a suite of custom editor tools to streamline the team’s workflow and level design process. These tools were created using Unity’s editor scripting API and were specifically tailored to solve recurring pain points.

#### Auto-Ground Objects

During level design, team members frequently had to hand-place and align environmental props on uneven terrain. This process was time-consuming and inconsistent. I built this tool to eliminate manual fine-tuning, enabling rapid, consistent scene dressing.

This custom Unity Editor tool allows designers to automatically snap props and objects to terrain surfaces with a single click or mouse release in the scene view.

**Features**

- Automatically grounds selected objects to the surface beneath using raycasting
- Includes optional random scale application within a defined range
- Can spawn prefabs directly into the scene, ground them, and scale them on instantiation
- Scene view integration: Automatically re-grounds objects when moved with the mouse
- Alignment factor, which controlled how much the tool would try to align the object with the surface normal
- Editor-safe: integrates Undo operations for safe rollbacks

<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>

#### Prop Sorter & Renamer

With multiple designers working on scenes, object names and hierarchy order often became chaotic — making it difficult to find or group related props. This tool automates cleanup and ensures everything is labeled consistently across the board.

This Unity Editor tool helps designers organize messy hierarchies by sorting and batch-renaming selected GameObjects in just a few clicks.

**Features**

- Sorts selected GameObjects by name or numerical suffix
- Can reorder objects in the hierarchy (ascending or descending)
- Renames selected GameObjects in a numbered format like Crate (1), Crate (2), etc.
- Includes a clean Editor GUI for adjusting base names and triggering actions
- Integrated with Unity’s Undo system and logging for safe operation

<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>

#### Custom Wall Collider

We had multiple wall types that didn’t conform to box colliders or Unity’s built-in polygon collider options. Hand-building colliders was slow and error-prone, especially for curved or angled walls. I built this tool so our level designers could visually shape walls and have accurate colliders generated automatically.

This tool dynamically builds polygon-based 3D mesh colliders for irregular or curved walls placed in the level, solving the problem of Unity’s limited primitive colliders when dealing with complex wall geometry.

**Features**

- Allows designers to define a custom-shaped base polygon using Transform points in the editor
- Generates a 3D mesh collider by extruding the base shape vertically to the desired height
- Mesh is automatically built, assigned to a MeshFilter (for visual feedback) and MeshCollider (for gameplay collision)
- Includes in-editor Gizmos for live visual debugging of the collider structure
- Handles fan triangulation, side wall extrusion, and accurate physics collision setup

---


