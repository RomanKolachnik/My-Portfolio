---
layout: default
title: High-Tech Havoc
---

# High-Tech Havoc

> *3D Arena Shooter • Local Multiplayer • Team Size: ~10 • Released on itch.io*

![Banner](https://img.itch.zone/aW1nLzE2MDMxMjg1LnBuZw==/original/%2B60ABF.png)

---

## Trailer / Gameplay Demo

<iframe width="100%" height="400" src="https://www.youtube.com/embed/IQU4GTxazKo" frameborder="0" allowfullscreen></iframe>

---

## My Role

I worked as a **Gameplay Programmer** on High-Tech Havoc, with responsibilities including:

- Implemented local multiplayer setup using Unity’s Input System
- Developed modular power-up system with synergy logic and activation priority
- Designed stage event interactions like dynamic lighting that reacts to game triggers
- Contributed to debugging across core systems like UI and enemy logic

---

## Features I Developed

### Modular Power-Up Synergy System

High-Tech Havoc features a modular modifier system where each power-up is self-contained, stackable, and independently handles its own logic. I implemented all 9 modifiers in the game and they are as follows:
| Modifier | What It Does |
|----------|--------------|
| Overcharged | Increased player movement speed and roll speed (which let you get temporary buffs faster |
| You're Going Down With Me | You can self-destruct and defeat any players in your range instantly. Their deaths count before your own (so you don't auto-lose the round) |
| Power Saving Mode | The amount of energy you consume to dash or roll for a temporary buff is cut in half |
| Unstable Blast | Your bullets explode on impact with the environment and over time |
| Trifecta | You can shoot 3 bullets instead of 1 bullet |
| Overheated | Your bullets can cause players to get burned, taking nonlethal damage overtime |
| Homing Bullet | Your bullets home in on the closest target to you |
| Fragmentation | Your bullets will fragment off of the environment and split into smaller bullets |

The modifiers were built using a base Modifier class (ScriptableObject) pattern and overrides methods to apply/remove effects on the player. Their effects are self-contained and require no central dependency, and the modifiers stack in real time, enabling emergent combinations, and apply via a priority.

For example, A player equipped with Homing Bullet, Unstable Blast, and Fragmentation will fire a projectile that:

1. Homes in on the nearest target
2. Explodes on impact or after a short delay
3. Fragments into multiple bullets
4. And each of those also home and explode

This synergy system was deliberately modular — each modifier affects only its relevant component (movement, bullets, player stats) — which keeps the system scalable and easy to expand upon.

---

### Stage Lighting Event Triggers

<img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
<p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>

```csharp
// On game start
function InitializeMaterials():
    enableEmissionForAllMaterials()
    setEmissionColor(Color.red)

// Every frame
function Update():
    if chaosFactorIsActive:
        transitionLightColorToRed()
        if fullyShiftedToRed:
            blinkRedOverTime()
    else:
        shiftHueOverTime()

    scrollTextureOffset() // Creates movement across lights
```

This system dynamically shifts stage lighting based on a global “Chaos Factor.” When the chaos is inactive, materials smoothly cycle through hues like a party light. But when chaos is triggered, the lighting transitions to a full red alert state — then blinks with a rhythmic pulse using Mathf.Cos to simulate intensity shifts.

- All materials are manually set to emit color (Unity’s _EmissionColor)
- During normal gameplay, the hue value rotates over time, creating a smooth rainbow gradient
- When chaos is triggered, the system interpolates the current color toward red
- Once fully red, the lights begin a sinusoidal blinking effect based on time
- The strip lights also scroll horizontally for added visual energy

---
