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
<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
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
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>
</div>

</div>

The chain attack system allows players to execute combo attacks by timing button presses during specific animation windows. If the attack is input early, it queues until chaining is allowed. The system ensures responsive combat while preventing unintended inputs, using internal flags and timing buffers. Animation events control the chain window and reset logic after the combo ends. 

### Local Multiplayer Input
<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
In Goofy Lil Guys, the character select screen is a fully custom, hardcoded Unity UI system built to overcome the limitations of multiple players interacting with a single canvas.

- Players join in and are each assigned a UISelector — a floating cursor tied to their PlayerInput
- These selectors allow players to navigate independently across the character selection screen
- The screen is composed of three character cards, each with unique starters
- Because Unity's native UI systems struggle with differentiating multiple players on the same canvas, all navigation, submission, and cancel events are manually coded
- Each player’s inputs are routed to their selector, which sends feedback to the menu and updates visuals dynamically (e.g., color, shape, locked-in state)
- The system supports joining, backing out, and tutorial prompting, all fully synced across multiple players
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>
</div>
</div>

---

### AI Personalities
![Creature AI](assets/goofy-lil-guys/creature-ai.gif)  
Wild creatures have different personalities (aggressive, cautious, curious) using a lightweight FSM and steering behaviors to avoid obstacles, engage in combat, or retreat.

---

### Editor Tools
![Editor Tools](assets/goofy-lil-guys/editor-tool.png)  
To help designers speed up level-building, I created tools to:
- Auto-ground objects to terrain
- Tag and sort props
- Apply batch adjustments to collider settings

---


