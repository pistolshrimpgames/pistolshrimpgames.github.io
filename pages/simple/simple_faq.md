---
layout: default
title: Simple FAQs
parent: Simple
nav_order: 10
last_modified_date: 2022-05-23
---

# Using Simple

## What’s Simple?

Simple is an editing tool, language, and runtime for creating gameplay simulations. Using a mouse-driven scripting tool, users of any technical level can prototype and build anything they might imagine with 3D physics and logic, and it supports network play without creators having to even think about it. Gameplay is experienced by way of a viewer - which can support fancy visuals, audio, and UI - but our included viewer only represents collision shapes and a limited subset of UI.

Compared to other tools available, we think it's a much simpler way to approach scripting gameplay without needing to know how to program.

## What kinds of gameplay can I make with Simple?

We hesitate to say "anything," but Simple excels at making pretty much any _physical_ gameplay. Platformers, a racing game, or our own [top-down space game](https://pistolshrimpgames.com/uqm2/) are all great candidates (and making them multiplayer is easy). What you probably wouldn’t want to create in Simple is something that relies heavily on assets like text or UI to make sense of what’s going on.

## What do I need to use Simple?

Simple currently is only being developed for and running on Windows 10 devices. We have had some success running it via WINE and in virtual machines, but your mileage may vary. A mouse and keyboard are required to use the scripting tools, and a keyboard is required for the game runtime. There is likely a minimum CPU/RAM requirement, but we don’t know what it is. Since the viewer doesn’t do anything graphically intensive, we expect many GPUs to work. Also, since there’s no sound, you don’t even need a sound card! Woo!

## What is the development status of Simple?

Simple is in very active development and has many, many loose ends. We expect users to encounter problems, especially in places like the tool UI, and we wouldn't be surprised if we break certain functionality as we go. The team working on Simple is also working on our primary goal - completing [The Ur-Quan Masters 2](https://pistolshrimpgames.com/uqm2/) - so some things may be left by the wayside and will certainly not be as nice as they could. If you are encountering serious issues, [see below](#how-can-i-report-a-problem). We are using and improving our tools too, and we'll be posting updated builds over time.

## Where do I get Simple?

You can download the latest version at [pistolshrimp.itch.io](https://pistolshrimp.itch.io/simple).

## Is Simple free for me to use?

The Simple binaries (simple.exe, Simulation.dll, viewer.exe) are licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 License](https://creativecommons.org/licenses/by-nc-sa/4.0/). If you like what we're doing by providing our tools for all to use, all [support](#how-can-i-support-simple-the-ur-quan-masters-2-or-pistol-shrimp) helps fund our ongoing development of both Simple and UQM2.

# Learning Simple

## Are there any tutorials or documentation?

The best way to learn will be to look at [the example data]({% link pages/content/content.md %}), but we have some [introductory documentation]({% link pages/simple/quickstart/simple_quickstart_1.md %}). You will learn a lot if you [watch our live streams](https://twitch.tv/pebby) where Simple is used to build things for UQM2. In the near future, a community space for helping each other will be available.

## Do you have any examples of stuff you’ve made?

Yes! Check out our [content documentation]({% link pages/content/content.md %}). There is very primitive data meant for a beginner as well as much more complex data used for [UQM2](https://pistolshrimpgames.com/uqm2/)’s melee mode, which will eventually ship with the finished game.

## How do I add art, sounds, and VFX?

 All of the responsibility for audiovisual things resides in what’s called the viewer. The simulation is just for authoring gameplay, and connecting art, sounds, or other things that don’t have an effect on gameplay is handled by a program viewing the simulation. The included viewer (we call it the _simple_ viewer) only renders collision shapes, debug prints, and simple UI. There is a _game_ viewer which supports much fancier things, but that is currently not released.

# Contact

## Who's making Simple?

Simple is the creation of mad scientist, Fred Ford. He is part of [Pistol Shrimp](https://pistolshrimpgames.com), a development studio founded by him, Ken Ford, Dan Gerstein, and Paul Reiche. We have been making games for a long time and are currently working on [The Ur-Quan Masters 2](https://pistolshrimpgames.com/uqm2/).

## How can I report a problem?

Simple is in active development and is nowhere near alpha. We are aware of many, many problems already, especially in the simple UI, but we’re most interested in “show-stopper” issues like crashes in the game runtime or viewer. Since we have only had a limited chance to test the viewer with different hardware configurations, we anticipate most of the serious issues there. If you get some serious issues, you can do any of the following:
- [Join our Discord](https://discord.gg/rasVCDmYKp) to ask questions and get help.
- [Email us](mailto:help@pistolshrimpgames.com) with repro steps and, if needed, your `game` directory.

## How can I support The Ur-Quan Masters 2, Simple, or Pistol Shrimp?

We will have more ways to do this soon, but for now:
- **Funding**: Anything raised through [Twitch subscriptions](https://twitch.tv/pebby) or [itch](https://pistolshrimp.itch.io/simple/purchase) goes directly to funding development.
- **Documenting**: Have you become an expert in Simple? Improve these docs or make video tutorials for other new users!
- **Join the Community**: Join our [community Discord](https://discord.gg/rasVCDmYKp).
- **Tell Everyone!**: No, seriously. We think what we're doing is pretty weird/cool/crazy, so we would love any help getting the word out.

## I want to use Simple as part of my full-fledged game! Who can I talk to about it?

[Email us](mailto:contact@pistolshrimpgames.com)!


