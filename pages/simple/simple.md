---
layout: default
title: Simple
nav_order: 2
has_children: true
last_modified_date: 2022-05-23
---

{: .no_toc }

Simple software used to build gameplay at Pistol Shrimp which we are making available in binary form at [pistolshrimp.itch.io](https://pistolshrimp.itch.io/simple).

If you're ready to dive in and start playing, see [the Quick Start guide]({% link pages/simple/quickstart/simple_quickstart_1.md %}).

For common questions, see [the FAQ]({% link pages/simple/simple_faq.md %}).

Otherwise, keep reading for a high-level overview.

# What's Simple?

Simple is an editing tool, language, and a runtime for simulating physics and logic with the purpose of making gameplay. Let’s focus on the part of Simple that matters to the player: the runtime - or in layperson’s terms, the part of the game software. We use two words to describe the halves of an eventual game:

**A Simulation** - what Simple provides and what designers author:
- Is where gameplay happens with physical interactions, like objects bumping into other objects.
- Contains rules about how those interactions work, like how fast an object moves through space or if one is destroyed upon touching another.
- Informs the Viewer of which objects are important to represent to a player through visuals, audio, etc.

**A Viewer** - what the player sees and interacts with:
- Provides the player with a camera so they can observe and participate in a simulation. It also informs the simulation what buttons the player is pressing, letting them control objects.
- Constructs representations of what’s going on in the simulation, like visuals, audio, effects, and UI.
- Has interactive components which do not directly affect the simulation in real time, like UI for menus or selecting things.

The relationship between the viewer and the simulation is crucial to understanding both how and why we’re doing what we’re doing but also the scope of what we’ll be building in Simple, which is just the Simulation. If we were to reference pinball, you could say that the gameplay, with the controls for flippers, a rolling ball, and physical contraints of the machine would all be built in the Simulation. However, the graphics and lights on the table, the sounds of hitting bumpers, and the interface where score is tracked, would all be done in the Viewer.

The only thing a Simulation receives from the player - generally speaking - is their input (button presses). Think of a simulation as its own universe, with the player being able to peer into and potentially receive control of some objects in that universe by way of a Viewer. The universe, otherwise, sustains itself, whether or not the player is participating.

# What's the Language?

Simple provides a scripting language, a kind of simplified programming language, which reads and looks like text but can be authored visually with a graphical interface. To create these simulations, users can make gameplay using three parts:

**Components** - any discrete entity in a simulation.
- Can be used for physical objects, like making walls, a bouncing ball, or a missile.
- Can be used for non-physical entities, like a component describing what buttons a player is pressing or what’s in their inventory.
- Contain Properties and Operations.

**Properties** - parts of Components used to store or represent information.
- Can be numbers, like how fast an object is moving and in what direction, or how much damage a missile should do.
- Can be references to other components, like an object knowing what type of shape it should have (a rectangle, a sphere, etc.) or a missile knowing what object it collided with.
- Can be collections, like a wall knowing all of the objects touching it.

**Operations** - rules the designer authors to decide what a Component should do.
- Can run when a Component starts, dies, or updates (i.e. constantly).
- Can inspect or change Properties.

That’s it! There is nothing else to the language. The language provides some built-in Components for processing physics, but know that the flexibility to do almost anything is available, while the language provides very little that requires you to do things in a given way. You’ll see there are a few things Components have built-in depending on their type - mostly for helping to simulate physics - but the rules for how Components behave are almost entirely up to you.

There is no magic in how anything behaves: once you understand the rules that govern all components, properties, and operations, creating or understanding behavior is entirely in the designer’s hands.

# Why, Though?

Just looking at the scripting interface, it helps non-programmers to be introduced to programming. Although it's written and readable in plain text, avoiding one of the challenges of node-based scripting, we use a mouse and menus to provide a user interface. This helps people understand what is or isn't possible, and mistakes like typos or invalid commands can be avoided. The ideal is that non-technical users are able to create fun things without getting tripped up by the "hard" parts of programming!

The more people who are able to try making fun experiences without any assistance, which is often a process of prototyping and iterating, the better. We need engineers for other things! By having a scripting layer, an engineer can also optimize how it’s being run, leaving designers to not necessarily concern themselves with writing highly performant behaviors. Designers of different technical abilities may help assist in making certain things bulletproof, but anyone can get something off the ground.

People are much more willing to try things when they don’t need to convince others of their merits. As creators, ourselves, we aren’t even completely sure what will really be fun until we put pencil to paper. Being able to do that independently without consuming resources from the team just to get experiments going helps us find out what feels fun.

As for the language, itself, our goal is to give it a very limited surface area without limiting the things a user can concoct. Once a user understands how one particular part works, even if there is some depth to it, they will be able to apply that knowledge all over. There are a very limited number of operations (only 6!), so script logic will never include surprising, new functionality. Anything more complicated than that is made by end users.

Looking at the division between simulation and view, this gives us tremendous freedom from multiple angles. For one, we can start making and prototyping gameplay to prove it’s fun, which is pretty cheap in our system to try, before we invest anything more expensive (like art, sounds, and visual effects) into it. Secondly, it is portable and can be attached to any viewer we wish, letting us start making gameplay without waiting to choose or be tied to the right tool for the job. It is possible different devices may even use different viewers depending on their capabilities. Lastly, as a boon to people working on gameplay, being able to run the game without any visual heft means we can start and restart the game many times with no delay.

Without getting into all of the technical aspects, the runtime also supports multiplayer. Designers never need to consider the challenges of writing network-worthy code, since whatever simulation they build will just work with additional players participating. It's kind of magical.

