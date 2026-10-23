---
title: Bevy Health Plugin
excerpt: A plugin for Bevy for storing & mutating health values on entities, including modifiers for incoming changes
---

Summary 
=========
Health systems are, in some form or another, exceedingly common within video games. Although this plugin is fairly simple it provides a single point for commonly used code. 
The systems in this plugin are prompted by writing either Damage or Healing messages, which contain a reference to a target Entity, an amount, an optionally a source Entity. 
These systems will read those messages and make changes to targetted entities containing the "Health" component.
There are a variety of components which can modify how healing or damage is applied, including minimum/maximum damage, multipliers, and armour which allows for a custom function & rating. 

I hope to implement damage types in the future, but this is something I'll consider when it comes to it. I'll need to be mindful of the variety of combinations of damage types that games may use, and accomodating for custom types rather than/on top of providing existing ones. 

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

Create entities with health
```
fn spawn(
    mut commands: Commands
) {
    commands.spawn((
        Health(100.0),
        MaxHealth(100.0) // Optional (but recommended) component
    ));

    commands.spawn((
        Health(100.0),
        MaxHealth(100.0),
        MinDamage(5.) // Damage under this value will be ignored
    ));

    commands.spawn((
        Health(100.0),
        MaxHealth(100.0),
        MaxDamage(5.) // Damage over this value will be reduced to this value
    ));

    commands.spawn((
        Health(100.0),
        MaxHealth(100.0),
        DamageMultiplier(2.) // Damage will be multiplied by 2
    ));

    commands.spawn((
        Health(100.0),
        MaxHealth(100.0),
        DamageMultiplier(2.) // Healing will be multiplied by 2
    ));

    commands.spawn((
        Health(100.0),
        MaxHealth(100.0),
        Armour { // Halves incoming damage
            rating: 2.,
            formula: |damage, rating| damage * (1.0 / rating)
        },
    ));
}
```

Write messages to mutate health values
```
fn damage(
    mut damage_msg: MessageWriter<DamageMessage>,
    entities_hp: Query<Entity, With<Health>>
) {
    // Damage all entities
    for entity in &entities_hp {
        damage_msg.write(DamageMessage {
            target: entity,
            source: None,
            amount: 10.
        });
    }
}

fn heal(
    mut heal_msg: MessageWriter<HealMessage>,
    entities_hp: Query<Entity, With<Health>>
) {
    // Heal all entities
    for entity in &entities_hp {
        heal_msg.write(HealMessage {
            target: entity,
            source: None,
            amount: 5.
        });
    }
}
```
Respond to an entity receiving a fatal hit
```
fn fatality(
    mut fatality_msg: MessageReader<FatalHitMessage>
) {
    for fatality in fatality_msg.read() {
        info!("Entity died");
    }
}
```