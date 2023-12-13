---
title: "Discovering ECS and Data-Oriented programming"
last_modified_at: 2023-12-13T16:20:02-05:00
categories:
  - Blog
tags:
  - Post Formats
  - readability
  - standard
---

Some time ago when browsing forums and documentation for my Unity projects, I discovered an upcoming Data Oriented Tech Stack or "DOTS". This includes 3 main components;
* Burst Compiler
* Job System for multi-threading
* Entity Component System (ECS)

Burst compiler is used with Unity to translate from IL/.NET bytecote to optimized native code using LLVM. 

It is designed to work effectively with the Job System, which provides an interface to easily create safe, multi-threaded code. 
Simply create a struct that implements an IJob interface, provide a method named Execute and fields for any data required. Then run the Schedule and Complete methods on a job. 
There are a variety of interfaces for different use cases, such as IJobFor and IJobParallelFor. 

In Object Oriented programming, I may define an Enemy class that contains fields for health, speed, damage, appearance, sound effects, and methods to change or use that data. However, most of the time I only need access to a subset of their data, or do the same to hundreds (or even thousands) of objects without sharp impacts to performance. This approach results in poor cache locality, as much irrelevant data is included when accessing an object for a subset of its data. 
ECS breaks apart data from processes when defining functionality. All data is defined in Components and are associated to Entities (Which are simply an index to a collection). Components should contain data that will commonly be processed at the same time, and or are related to a common feature. Entities with common components are then stored together and Systems can be defined to query for entities that include a specific set of components to process data. 
Since queries are explicit about their intent with which components (which are together in memory) this provides much better cache locality, as well as parallelism for improved multi-threaded performance. 

On top of the performance benefits, this separation of functionality and decoupling complements game developments very nicely, as most features require common interactions with one another, and user settings or creative decisions can result in many changing or being removed completely. Using an ECS, I found it much easier to add functionality to existing systems, or change them. Having a system process a new entity can be as simple as adding the necessary component, and maybe adjust query parameters. 

As great as Object Oriented programming is for pioneering readable, maintainable code, working on games showed it clearly not appropriate for this use case. It has been a joy to use and I'm eager to apply it in projects from now on, and excited to see the eco-system evolve. Discovering this has taught me a lot as a programmer about the inner workings of memory usage, multi-threading, a paradigm of programming and experiencing first hand where it is more appropriate for my use case than my usual goto. 

Though Unity DOTS was my introduction to ECS, I have began using an open-source ECS game engine called Bevy, built using Rust and using ECS from the ground-up. It's in infancy at the moment, lacking common features such as physics and an editor, with others like audio and UI being barebones and hard to work with. However, the community has made open-source plugins available to provide this functionality, and the entire source code is open to be read and changed when needed. 
This provides much more agency to the developers, and being built with ECS in mind from the start (and using a performant but memory-safe language like Rust) means Bevy provides a more intuitive and performant experience with ECS. Presently, using ECS in Unity contains remnants of OOP, mostly when using features implemented that pre-date DOTS (Which is a lot; Pathfinding, Audio, UI, etc...) which can be jarring to work with.
While Bevy doesn't have Burst Compiler or Job System, the API provides options to specify any necessary orders of execution, and automatically parallelise systems around these constraints, and the Rust compiler does not have a Garbage Collector (Is memory safe via Ownership) and so is already optimized for performance. 

I'm hoping to have a new project page up soon for my current Bevy project.