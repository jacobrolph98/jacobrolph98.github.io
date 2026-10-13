---
title: "My new approach to completing projects"
last_modified_at: 2026-10-13T13:25:02-05:00
categories:
  - Blog
tags:
  - Post Formats
  - readability
  - standard
---

In the 15 years I've been programming, much of my experience has been leisurely working on hobby projects & prototypes, not necessarily using my time and effort efficiently, sticking to some inefficient habits. Studying computer science & software engineering from school through to University taught me a lot about making code modular, reliable, and readable for the sake of collaboration, or to make it easier to come back to later, among many other things. Since then I've continued to develop my own projects and continue to learn of new technologies, albeit retaining some of the bad habits from my roots without mentorship. One in particular stood out to me as a result of project-hopping whereever my muse took me, I found I was writing code for similar functionality across multiple games. I had written & rewritten systems for projectile tracking, hitbox detection, health & damage counting, etc... This is despite the fact that Bevy, my current favourite engine, is written specifically for modularity at every level. Because of this, I decided I would revise my approach to working on projects.

Instead of having multiple game projects each having their own implementation of common systems, I would create separate crates (Or plugins at the Bevy level) for each different systems, and each game can instead only include the code specific for that game that patches the component systems together in the correct way, and provides its own assets & user experiences. This works well with Bevy as it is in the spirit of composition, a core concept of Entity Component Systems that Bevy is built upon. This way, I can direct my passion for a certain game idea towards shared systems that will benefit me later down the line, if I am to begin working on a different game. 
Some of the plugins I have been working on include; 
- Damage Plugin
  - Responsible for tracking current health, dealing damage and accounting for a variety of modifiers
- Attacks Plugin
  - Responsible for triggering raycasts/projectiles, tracking the latter, and triggering events when each records a hit.
- Items Plugin
  - Responsible for creating item entities which can stack, exist in the world/containers, be crafted
I have more in mind, such as voxel & movement plugins, but there are different design considerations when making generic systems, so I am leaving the more complex systems until after I have tested and implemented the prior simpler systems within a project. 

Writing plugins to be used in a variety of contexts brought two significant differences in design considerations to the forefront;
- Making the API accessible and not restrictive is a much higher priority
- Carefully considering the scope of the plugin and what functionality surrounding its intended features it should be responsible for

My Items plugin includes limits for maximum stack size per item type, and normally I would create a resource containing that data and each Item entity would contain an ID that acts as a key pointing toward stats such as those, rather than storing Max Stack size in each entity, for the sake of reducing data redundancy. However, in a generic plugin that developers implement, this would require implementing an item registry system, which isn't a concept an Items plugin should worry about, and it imposes data architecture decisions upon developers, who may have a wide-range of contexts which wouldn't fit nicely. For this reason, I decided to allow some data redunancy for the sake of a more accessible API - Entities & Components are efficiently stored as is, and the data stored is not large enough to become an issue with a reasonable number of items. This might become a consideration later once a game demands such memory-efficiency due to an abundance (Thousands and thousands) of separate item stacks. 

Having these systems in isolation from others within a game also makes running tests much easier & more reliable, which is something I have made a more concerted effort to do recently. This new approach to how I spend my effort & time programming has been an enlightening learning experience, I've enjoyed the process of trying the more efficient method and I'm excited to be able to carry the fruits of this effort forward onto other projects which will reuse this code. 