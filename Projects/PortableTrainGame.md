---
title: Portable Train Game
layout: default
tabs: game
tagline: In progress, but lots of tools!
---
<img src="/Images/portable_train_grass.png" width="75%" alt="Grass system in Godot accounting for terrain material" /> <br>

Sole programmer. Uses a mix of hand modeled and purchased assets. Made in Godot using GDScript.

After buying myself a Steam Deck I decided I wanted to see some trains driving around on it! There are a number of existing train layout building games but few that have full controller support. This isn't a targeted commercial project so a lot of development has been following the fun and making a bright and joyous game for me to play.

The main pillars of design are:
- For the Steam Deck first, PC second
- Highly moddable from start to finish
- Chase my own sense of fun - this is a game designed for me

The result is still in progress, but has forced me to question a number of my default assumptions about control schemes, asset management, and what I want in a game. I've also developed a number of tools that help support those design pillars:

The entire game is built on [a Godot mod manager](/Projects/Tools#godot-mod-loader) that supports modding mods and hotswapping active assets without restarts.

[A non-destructive heightmap editor tool](/Projects/Tools#non-destructive-terrain-editing-tool) that is used at runtime to blend track, structures, and terrain together.

To support lucious grass on a lower powered portable device and to make my foliage bright and cheerful I've implemented both [a floating origin GPU grass particle system](/Projects/Tools#floating-origin-gpu-particle-grass) and a [Blender to Godot foliage pipeline](/Projects/Tools#blender-to-godot-foliage-pipeline) that improves the look of the world.

While they aren't necessarily worth writeups by themselves it's hard not to overstate the assistance provided by CD tooling to speed up deploys to my device as well as on device profiling using RenderDoc and remote debugging.
