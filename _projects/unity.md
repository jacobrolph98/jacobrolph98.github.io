---
title: Unity projects
excerpt: A collection of prototyping different game ideas and how it introduced me to a new paradigm of programming
---

Rationale
============

Programming games was how I first got into this hobby back in 2011, which became a field of study, and hopefully a career (Though would remain all 3)
Thus when I finished University, I decided to revisit games programming, but instead of using Roblox Lua, I decided to learn Unity and expand on C#. My only prior experience with C# was some years ago in college making a matrices calculator for my final project, which I enjoyed using. Though my optimization at the time wasn't great, I've 

FPS Shooter
-----------

Inspired by the new Doom game, my first Unity project was an FPS Shooter. This was mainly to get first-hand experience in learning Unity, and I would shortly after start from scratch with some beneficial hindsight. Though, over both projects, I managed to implement a variety of features;
The player can press a button to briefly deflect projectiles toward where the player is looking.
Random pre-made structures are placed in a grid with random offsets and rotations.

### Enemy Variety ###
Variety of enemies with different health, speed, attacks and behaviour. I had researched for methods to do this and found Behaviour Trees, an AI technique popularised by the Halo franchise. 
The structure of the tree can be set by the developer, where nodes can represent conditional operators or actions (which in turn run specified methods) with nodes returning a success, in progress or failure state upon activation..
This made both creating/changing behaviours easier but also tracing through them to understand their working, while remaining flexible.

![Behaviour tree example](/assets/projects/unity/behaviourtree.png)

The tree is activated at the top periodically (in my case, 10x a second). Sequences and Selectors are two examples of built-in nodes to determine how to activate branches. Sequences activate branches from left to right until failure, whereas Selectors activate branches left to right until success. So, the above example shows a tree to handle different approaches of getting through a doorway. 

The first enemy would chase the player and periodically inflict damage when close enough. When the enemy is low on health they would run a certain distance away, before self-healing and repeating once full.
The second enemy would maintain a distance from the player and periodically fire a projectile at the player.

In my second project I included an enemy type which would follow the player and leap toward them to deal damage. I had also made use of some free assets to rig them to a basic humanoid model with some animations, and an outline to indicate their health.

### Weapon Variety ###
When first tackling this problem, the variety of conditions and effects between weapons made this challenging to implement, but I was successful.
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
Each weapon inherits from this class where I can define their properties, what conditions do they fire and how.
For more complex variations, such as whether to shoot raycast/projectile or charge/fire instantly, I used composition to include this functionality when defining an individual weapon class. 


My second project expanded even further, with some weapons having weapon effects on hit such as ricochets, explosions, or even shooting a whole different projectile with its own further effects. 
These effects can trigger under certain circumstances, such as missing an enemy, hitting an enemy, or getting a headshot.



Below is the latest demo footage for my first shooter project, including random maps, slightly more enemy variety and some other differences.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hGmLgs615Tk?si=ovyOnulMBzysWaaC" title="YouTube video player" frameborder="0" allow="encrypted-media; picture-in-picture; web-share" allowfullscreen></iframe>

And here is the latest demo for the follow-up project. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/hen9__XVPzk?si=u9XXtAfe7pueVWnf" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Voxel game
-----------
 Though this voxel project didn't get as feature complete, I did implement a system which generates chunks of voxel data in radius around the player using perlin noise as a heightmap. Loaded chunks have data serialized to persist changes made to chunks across sessions. Multi-threading was used to generate voxel data per chunk so player experience can remain smooth while many chunks update as the player moves around. 
 I was particularly performance conscious in this project, as 3d voxel data can quickly use up large abouts of memory to store, and precious frame time for calculations. 
 One such technique was to use a 1d array instead of a 3d array to represent voxel data per chunk. This can result in faster access times due to C# having different optimizations for bounds checking between single and multi-dimensional arrays. The index can be converted to and from relatively easily;
 Flat[x + WIDTH * (y + DEPTH * z)] = Original[x, y, z]


 Moving Forward
 =============