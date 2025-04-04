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
