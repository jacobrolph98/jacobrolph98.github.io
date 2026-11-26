---
title: Bevy Attack Plugin
excerpt: A plugin for Bevy for running spatial queries to check whether attacks of various kinds hit a valid target
---

[Github Repo](https://github.com/jacobrolph98/attack_plugin)

Summary 
=========

Games which feature combat in a spatial environment use a variety of means to determine how an attack can land. This plugin includes functionality for 3;
##### Projectiles
Spawns an entity at a given Transform & speed, and a system will step the projectile along it's trajectory using Raycasts each step to determine whether anything was hit. 
##### Radius
Checks for colliders that intersect with a sphere at a given position & radius. 
##### Sweep 
Prompts a set number of raycasts at a regular interval with a given formula to determine angle offset throughout. Can be used to create a melee system similar to Left 4 Dead 2. Can originate from a given Transform or be attached to an Entity that contains a Transform. 

Each attack can be provided an optional Entity field, pointing to a source. This source entity should contain component data which provides context of what should happen upon a hit. Ie, if the source entity is a gun (or player holding a gun), components should point to the gun's damage/knockback/etc stats. 

Considering both 2d & 3d spatial queries made this plugin more challenge to implement -  I took inspiration from the Bevy Avian physics crate itself to use 2d & 3d crates within a cargo workspace which depend on core functionality, enabling appropriate functions using cargo's features. 

There are a handful of features I'd like to implement in the future, such as line of sight checks for radius checks & projectile rays hitting multiple entities for bullet penetration mechanics.

Usage
============
Initialise the plugin
```
use bevy::prelude::*;
use damage_plugin::plugin::DamagePlugin;

fn main() {
    let mut app = App::new();
    app.add_plugins((DefaultPlugins, DamagePlugin));
    app.run();
}
```

Spawn a projectile
```
projectile_msg.write(SpawnProjectile {
    pos: Vec3::ZERO,
    direction: Quat::default(),
    speed: 1.,
    track: None,
    source: None
});
```

Do a radius check
```
radius_msg.write(RadiusCheckMessage {
    pos: Vec3::ZERO,
    radius: 3.,
    line_of_sight: false, // Not yet implemented
    source: None
});
```

Begin a sweep
```
/// Example function to use as a formula, providing an angle offset at different stages. 
fn test_sweep(stage: usize) -> Quat {
    if stage == 0 {
        let angle = std::f32::consts::FRAC_PI_4;
        Quat::from_rotation_z(angle)
    } else if stage == 1 {
        let angle = 0.0;
        Quat::from_rotation_z(angle)
    } else if stage == 2 {
        let angle = -std::f32::consts::FRAC_PI_4;
        Quat::from_rotation_z(angle)            
    } else {
        Quat::IDENTITY            
    }
}

sweep_msg.write(StartSweep {
    attach: Attachment::Transform(Transform::default()),
    stats: Sweep {
        stages: 3,
        interval: 0.015,
        range: 5.,
        formula: test_sweep,
    },
    source: None
});
```

React to hits
```
fn hit(
    mut hit_msg: MessageReader<HitMessage>
) {
    for hit in hit_msg.read() {
        if let Some(src_entity) = hit.source {
            info!("Entity hit");
        }
    }
}
```