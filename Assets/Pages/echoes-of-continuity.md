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

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// Base item effect interface
public interface IItemEffect {
    void Apply(PlayerStats stats);
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/echoes-of-continuity/item-effect.gif" alt="Item effect demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Player activating an item with on-kill effects</em></p>
</div>

</div>

---

### Procedural Dungeon Generation

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// Room placement with structure rules
if (CanPlaceRoom(x, y)) {
    dungeon[x, y] = new Room();
    AddSpawnRules(dungeon[x, y]);
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/echoes-of-continuity/dungeon-gen.gif" alt="Dungeon generation demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Procedural dungeon layout forming with rules applied</em></p>
</div>

</div>

---

### Finite State Machine AI (Melee & Ranged)

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: flex-start; margin-bottom: 2rem;">

<div style="flex: 1; min-width: 300px;">
<pre><code class="language-csharp">
// FSM update for ranged enemy
switch (currentState) {
    case EnemyState.Idle:
        SearchForPlayer();
        break;
    case EnemyState.Attack:
        ShootProjectile();
        break;
}
</code></pre>
</div>

<div style="flex: 1; min-width: 300px;">
  <img src="assets/echoes-of-continuity/enemy-ai.gif" alt="Enemy FSM AI demo" style="max-width: 100%; border-radius: 8px;">
  <p style="text-align: center;"><em>GIF: Ranged enemy switching between idle and attack states</em></p>
</div>

</div>

---
