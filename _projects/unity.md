---
title: Unity projects
excerpt: A collection of prototyping different game ideas and how it introduced me to a new paradigm of programming
---

Rationale
============

Programming games was how I first got into this hobby back in 2011, which became a field of study, and hopefully a career (Though would remain all 3)
Thus when I finished University, I decided to revisit games programming, but instead of using Roblox Lua, I decided to learn Unity and expand on C#. 

FPS Shooter
-----------

Inspired by the new Doom game, my first Unity project was an FPS Shooter. Though not a cohesive game experience, I did make good progress and complete a few features;

### Enemy Variety ###
Variety of enemies with different health, speed, attacks and behaviour. I had researched for methods to do this and found Behaviour Trees, an AI technique popularised by the Halo franchise. 
The structure of the tree can be set by the developer, where nodes can represent conditional operators or actions (which in turn run specified methods)
This made both creating/changing behaviours easier but also tracing through them to understand their working, while remaining flexible.

The first enemy would chase the player and periodically inflict damage when close enough. When the enemy is low on health they would run a certain distance away, before self-healing and repeating once full.
The second enemy would maintain a distance from the player and periodically fire a projectile at the player.

### Weapon Variety ###
When first tackling this problem, the variety of conditions and effects between weapons made this challenging to implement, but was successful.
Weapons can vary in;
  * Fire rate
  * Damage
  * Ammo type
  * Projectile with travel time (maybe even arcing) or instant raycast
  * Number of shots (like how shotgun fires multiple projectiles)
  * Explode on impact
  * Charge up before needing to fire (like minigun or bow)
  * Release mouse to fire instead of press (like bow) 
and more 

I ended up creating an abstract weapon class, with abstract functions for mouse inputs and fields for all common stats (fire rate, damage, ammo type, etc)
For more complex variations, such as whether raycast/projectile or charge/instant, I used composition to include this functionality when defining an individual weapon class. 


Voxel game
-----------
 TODO