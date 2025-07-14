---
title: Tools
layout: default
tabs: main
---

During my time at <a href="https://controlzee.com" target="_blank">ControlZee</a> I developed and was in charge of a large number of internal tools for the company. These included asset management and modification tools, automated performance testing pipelines, status monitoring sites and more. In my spare time I've developed tools with 2k+ downloads as well as tools for my own game projects. Here are summaries and writeups of some of the ones that stand out to me!


Link|Project|Stack|Year
----|-----|----
Asset Management Tool|ControlZee|Python, Go, AWS, MongoDB|2022-2025
Automated Content Releases|ControlZee|Jenkins, Groovy, Bash, CI/CD|2022-2025
Automated Performance Testing|ControlZee|Jenkins, CI/CD, InfluxDB, Grafana|2024-2025
Development Status Monitoring Site|ControlZee|Go, REST, GitHub|2023-2025



Tools developed for editing [picoCAD](https://johanpeitz.itch.io/picocad) files, creator of both tools, implemented everything except for slight UI tweaks by Gokhan Solak
![picoCADPainter](https://jordanfb.github.io/Images/picoCADPainterScreenshot.png)
![picoCADToolkit](https://jordanfb.github.io/Images/picoCADToolkitScreenshot.png)<br>



This page is still in progress, sorry!

Asset Management Tool - Python, Go, AWS, MongoDB
Inherited sole ownership of this tool. Managed assets (games, models, sounds, terrain, etc.) across the company. Coordinated content releases on live games. Improved tool performance 3x. Parallelized deploy types to prevent code release blockers. Sped up deployment to AWS for test environments.
Automated Content Release Processes - Jenkins, Groovy, Bash
Sole programmer. Automated multi-stage content release processes to just one click in Slack. Monitored and notified stakeholders of content statistics (size, deploy times, failures).
Automated Performance Testing - Jenkins, CI/CD, InfluxDB, Grafana
Sole programmer. Automated game performance testing. Made visualizations and tools for warning about performance changes across commits and branches. Monitored both branch status and resource changes.
Development Status Monitoring Site - Go, REST, GitHub
Sole programmer. Made an internal development monitoring site to monitor development environment statuses, manage releases, and automate internal and external release notes.
Profanity Filter API - Python, REST
Sole programmer. Improved a basic system used by comments, chat, and asset naming. Mitigated censor avoidance tricks and added filters for personal information and age inappropriate topics.
Art pipeline tools for Load Roll Die
Godot Mod Loader - Godot, GDScript, GLSL, JSON					      2023-2025
Sole programmer. Implemented mod support for Godot project that allows mods to modify other mods. System loads assets (textures, models, content, scripts, etc.) at runtime and supports swapping mods without restarting. Used in a train layout building game that is under development.
Developed Unofficial Tools for picoCAD File Editing 1, 2 - Python, C#, Unity		 2021-Present
Reverse engineered file format. Made tools for mesh editing, UV unwrapping, model optimization, file management, 3D texture painting. Set up CI/CD builds. 2k+ downloads.
