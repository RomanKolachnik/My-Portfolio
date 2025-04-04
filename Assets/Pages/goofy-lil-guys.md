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
/// Called by PlayerController.cs when the player presses attack
public void AttemptAttack()
{
	if (IsInSpecialAttack || isDead || ((playerOwner != null) && !isAttacking)) return;

	// If first attack in the combo, trigger immediately
	if (currentComboCount == 0 && Time.time - lastAttackTime > comboBufferTime)
	{
		PerformAttack();
	}
	else if (currentComboCount > 0)
	{
		// Otherwise, queue attack to execute when animation allows it
		attackQueued = true;
	}
}

/// Animation Event: Called at the right frame to allow chaining.
public void EnableChainAttack()
{
	canChainAttack = true;

	// If attack is queued (player pressed attack within the window), continue the combo
	if (attackQueued && currentComboCount < maxCombo && isAttacking)
	{
		attackQueued = false; // Clear the queue since we're attacking now
		PerformAttack();
	}
}

public float GetTotalComboTime()
{
	if (anim == null) return 0.5f; // Default safety buffer

	// Get the length of the attack animation
	float attackAnimTime = anim.GetCurrentAnimatorStateInfo(0).length;

	// Total time = animation length * combo count, with slight padding for reaction time
	return (attackAnimTime * maxCombo) + 0.1f;
}

/// Executes the attack only if allowed by animation timing.
private void PerformAttack()
{
	currentComboCount++;
	lastAttackTime = Time.time; // Update last attack time

	if (anim != null)
	{
		if (!isInSpecialAttack)
		{
			anim.Play("BasicAttack", 0, 0f); // Restart the attack animation
		}
	}
	else
	{
		SpawnHitbox(); // Animations not done yet, just spawn hitbox.
	}
}

/// Animation Event: Called near the end of animation to check for a combo.
public void CheckCombo()
{
	if (canChainAttack && attackQueued && currentComboCount < maxCombo && isAttacking)
	{
		attackQueued = false; // Reset queue so it doesn't trigger twice
		PerformAttack();
	}
	else if (currentComboCount >= maxCombo)
	{
		// Only now, after the full combo is complete, apply the buffer
		lastAttackTime = Time.time; // Mark when the combo ended
		ResetCombo();
	}
	else
	{
		ResetCombo(); // If combo is over, reset
	}
}

private void ResetCombo()
{
	currentComboCount = 0;
	canChainAttack = false;
	attackQueued = false; // Make sure attack queue is cleared
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


