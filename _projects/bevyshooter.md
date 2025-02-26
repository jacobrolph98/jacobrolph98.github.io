---
title: Bevy Game
excerpt: Built using Rust & Bevy
---

Rationale 
=========

While working on Unity I discovered Entity Component Systems as part of their Data-Oriented Tech Stack. It's an alternative programming paradigm that offers greater memory-efficiency, parrelisation & modularity than Object-Oriented programming when it comes to games. Shortly afterward I encountered Rust, a programming language that promises performant memory-safety without manual allocation or the overhead of a garbage collector. Both of these are exciting on their own, so needless to say when I discovered Bevy, a free, open-source ECS framework written in Rust, I was excited to get started.

This project is still in its infancy however, its features are lacking when it comes to physics, UI (And thus also lacks an editor), and there is no stable-release so far. There are many community-made plugins to fill the gaps however, but tutorials are somewhat lacking so I wanted a relatively simple project to get started on. Since ECS shines with large numbers of entities, I wanted to create a game with large numbers of enemies & bullets active on screen.


Summary
============
The game is a top-down shooter, where the player must fend off hordes of hundreds of enemies using a variety of weapons. Drones will also follow the player and can have their equipped weapon or targetting priorities customised. Enemies will have a variety of behaviours, strengths & weaknesses to encourage strategic configuration of drone loadouts & weapon switching in game. 

While there can be a steep learning-curve to Rust & Bevy, my experience using Unity's implemention of an ECS helped in understanding what sort of architecture will be necessary/optimal to make progress. Plus, Bevy's API abstracts away much of the would-be paint-points of learning Rust for a complex project that includes multi-threading, rendering, time-keeping, etc... 
For example, when implementing a system, I define a function where the parameters denote which data to access and whether it is read and/or write. Bevy can use this information to schedule functions to run in parallel while avoiding race-conditions, since it knows which functions are eligible to change data.
Here is an example of a function's signature, whos purpose is to move the drone toward a position orbiting the player. 
```
fn drone_follow_player(
    mut drone_query: Query<(&mut GoalVelocity, &Transform), With<Drone>>,
    plyr_query: Query<&Transform, With<Player>>,
    drone_count: Res<DroneCount>
) {
    // CODE
}
```
Since drone_query has mutable access to GoalVelocity, functions accessing that same component won't run in parallel with this one. However, any function which also has read-only access to a player's Transform, (or drone count) may well do. 
