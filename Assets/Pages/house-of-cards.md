---
layout: default
title: House of Cards
---

# House of Cards

> *2D Puzzle Platformer • Grapple + Card-Based Mechanics • Team Size: 10 • Released on itch.io*

![Banner](https://img.itch.zone/aW1nLzExOTIwODQwLnBuZw==/original/lluwrN.png)

---

## My Role

I worked as a **Gameplay Programmer** on House of Cards, contributing across multiple systems:

- Implemented card power-ups with activation effects that modified physics and movement
- Developed a grapple mechanic tied to puzzle design and environmental interaction
- Built a movement state switcher for transitioning between standard and pivot-based movement
- Tweaked player colliders for smoother interaction with puzzles and terrain
- Debugged various elements across systems to support level design and player flow

---

## Features I Developed

### Card Power-Up Activation

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// Activate a card's effect
void UseCard(CardData card) {
    switch(card.effectType) {
        case EffectType.Bounce:
            ApplyBounce();
            break;
        case EffectType.Phase:
            EnablePhaseMode();
            break;
    }
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/house-of-cards/card-activation.gif" alt="Card effect demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Player activating a card with temporary effect</em></p>
</div>

</div>

---

### Grapple Mechanic (Physics + Puzzle Traversal)

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// Basic grapple logic
void FireGrapple(Vector2 target) {
    grappleLine.enabled = true;
    grappleLine.SetPosition(0, transform.position);
    grappleLine.SetPosition(1, target);
    rb.velocity = CalculateGrapplePull(target);
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/house-of-cards/grapple.gif" alt="Grapple traversal demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Grapple mechanic used to swing between puzzle points</em></p>
</div>

</div>

---

### Movement State Switcher (Platforming ↔ Pivot Mode)

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// Toggle player movement mode
public void SwitchMovementState() {
    if (currentState == MovementState.Normal) {
        currentState = MovementState.Pivot;
        ActivatePivotControls();
    } else {
        currentState = MovementState.Normal;
        ActivateStandardControls();
    }
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/house-of-cards/state-switch.gif" alt="Movement mode switch demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Swapping between standard and pivot movement</em></p>
</div>

</div>

---

