---
layout: default
title: Goofy Lil Guys
---

# Goofy Lil Guys

> *2.5D Creature Collector • Local Multiplayer • Team of 10 • Released on Steam*

![Banner](https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/3565690/2810b94b751e7ecebb318644b9b0e020a3dfccf7/header.jpg?t=1742702968)

---

## 🎥 Trailer / Gameplay Demo

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
// Chain attack activation
if (Input.GetButtonDown("ChainAttack")) {
    if (canChain) {
        TriggerChainEffect();
        comboCount++;
    }
}
    </code></pre>
  </div>

  <div style="flex: 1; min-width: 300px;">
    <img src="assets/goofy-lil-guys/chain-attack.gif" alt="Chain attack demo" style="max-width: 100%; border-radius: 8px;">
    <p style="text-align: center;"><em>GIF: Chain attack mechanic in action</em></p>
  </div>

</div>

### Local Multiplayer Input
![Input Setup](assets/goofy-lil-guys/input.gif)  
Using Unity’s new Input System, I configured dynamic controller detection and split input between multiple players sharing the same screen. Each controller was linked to a specific player prefab.

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


