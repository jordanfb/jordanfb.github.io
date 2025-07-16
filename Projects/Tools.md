---
title: Tools
layout: default
tabs: main
---

During my time at <a href="https://controlzee.com" target="_blank">ControlZee</a> I developed and was in charge of a large number of internal tools for the company. These included asset management and modification tools, automated performance testing pipelines, status monitoring sites and more. In my spare time I've developed tools with 2k+ downloads as well as tools for my own game projects. Here are summaries and writeups of some of the ones that stand out to me!


Link|Project|Stack|Year
----|-----|----|----
[Asset Management Tool](#asset-management-toolautomated-content-releases)|ControlZee|Python, Go, AWS, MongoDB|2022-2025
[Automated Content Releases](#asset-management-toolautomated-content-releases)|ControlZee|Jenkins, Groovy, Bash, CI/CD|2022-2025
[Automated Performance Testing](#automated-performance-testing)|ControlZee|Jenkins, CI/CD, InfluxDB, Grafana|2024-2025
[Development Status Monitoring Site](#development-status-monitoring-site)|ControlZee|Go, REST, GitHub|2023-2025
[Godot Mod Loader](#godot-mod-loader)|Portable Train Game|GDScript, Godot|2024-2025
[Godot Non-Destructive Terrain Editing Tools](#non-destructive-terrain-editing-tool)|Portable Train Game|GDScript, GLSL, Godot|2024-2025
[Floating Origin GPU Particle Grass](#floating-origin-gpu-particle-grass)|Portable Train Game|GDScript, GLSL, Godot|2024-2025
[Unofficial picoCAD Modeling Tools](#unofficial-tools-for-picocad-file-editing)|picoCAD|Python, C#, Unity|2022-2024


Tools developed for editing <a href="https://johanpeitz.itch.io/picocad" target=null>picoCAD</a> files, creator of both tools, implemented everything except for slight UI tweaks by Gokhan Solak
![picoCADPainter](https://jordanfb.github.io/Images/picoCADPainterScreenshot.png)
![picoCADToolkit](https://jordanfb.github.io/Images/picoCADToolkitScreenshot.png)


# Asset Management Tool/Automated Content Releases
Python, Go, AWS, MongoDB
Jenkins, Groovy, Bash, CI/CD

The writeup of everything I did with ControlZee's asset management tools would take far too long to read, but I do want to highlight the broad strokes of optimization and automation that I implemented to improve the tool.

ControlZee's platform <a href="https://dotbigbang.com" target=null>dot big bang</a> created and used assets in multiple non-standard formats and presented a challenge to ensure they were delivered and optimized effectively. I inherited sole ownerhip of the tool used to manage all their assets including games themselves, scripts, models, terrain, animations, sounds and more. Its primary role was to move assets between our various internal development environments and to the production environment but it accreted a large number of modes to handle asset manipulation as well -- changing ownership between platform users, remapping dependency trees, migrating and optimizing assets, reporting sizes and the like.

Aside from all the things ownership entails (adding support for new asset types, updating the stack, adding new features, improving documentation, etc.) early on I started with improving the performance. Improving the performance for such a monolithic tool isn't always easy. Best and worst case scenarios can have very little to do with each other, but the vast majority of the use cases of the tools started by loading assets. By reducing what we loaded for each mode to its bare minimum I was able to vastly improve performance in most use cases while still ensuring the asset structure remained sound. To speed up worst case performance I parallelized our loading using thorough benchmarking and testing. Speed has a user experience component as well -- previously the tool would freeze while loading and give no indication of progress. I added loading bars with time estimates that ensured users weren't trapped in limbo. Next I improved the performance when the tool deployed assets to environments. The company pipelines started out with content deploys locked to code ones, requiring that the state of content on an environment fully matched what the tool expected before it would permit any changes. While it ensured environments were up to date, it was a major blocker when deploying to the development environments we used to work on first party content, as our global development team meant we had someone changing assets at most times of the day. I modified our Jenkins pipelines be smarter when it deployed to content focused environments, allowing deploys if the asset changes in the repository wouldn't collide with any changes made on the environment. With smoother code deploys it meant that it was easier for our code and content teams to work and ship in parallel.

As a critical part of our infrastructure this tool was run often during our content develpers' day to day work. My performance improvements helped, but the tool itself still required a larger context shift away from flow. Later on in my time at ControlZee I automated a large portion of our regular tool runs into Jenkins pipeline jobs that could run nightly or at the click of a button in Slack. The games and assets under development had several different final owners and varying permission schemas that I needed to enforce while still allowing for easy customization as the projects evolved. I solved this with a parameterized pipeline that I was able to initiate with the Jenkins API and easy URL generation that encapsulated the necessary behaviors into stateless requests. With a little Slack formatting it was easy for Jenkins to post automated reports alongside buttons to initiate update jobs whenever necessary.

While I could never remove the need for this tool entirely, I did my best to make it as efficient and unobtrusive as possible. It also ended up helping me in the end! I ended up taking on a large portion of the work coordinating our content releases to production and ensuring that the deploys were complete and ready for release, using the asset management tool all along the way.




# Automated Performance Testing
Jenkins, CI/CD, InfluxDB, Grafana, Terraform, Docker

At times ControlZee had upwards of seven internal developers building games and assets on the platform, and we worked with several external teams as well. We wanted to make sure that our games ran smoothly as we pushed the cutting edge of what our engine could do, as well as that any engine changes themselves resulted in consistent performance gains across our first party games and a variety of test games. I implemented an automated performance testing system that visualized and warned about performance changes across commits and asset changes. The system ingested a variety of datapoints into InfluxDB and visualized it in Grafana.




# Development Status Monitoring Site
Go, REST, HTML, CSS, GitHub API, Jenkins API

I developed an internal monitoring site to view the statuses of our environments, manage releases, and automate generating internal and external release notes. While a relatively simple project overall it did allow me to make a nice staging ground to let team members interact with Jenkins without needing to directly use Jenkins.




# Profanity Filter API
Python, REST

I improved a basic API that the platform used to censor bad words in comments, chat, and asset naming. There are three main spots that I improved. The first issue I mitigated was that users were using some tricks to avoid the basic filters we had in place. I implemented a system that could support running multiple filtering methods at differing levels of strictness on the text to find hidden issues. The second issue I fixed was that our original system didn't support emoji or more complicated Unicode characters. Lastly I added additional smarter filtering that checked for people sharing personal information and made sure to filter that out. While censoring messages is a constant back and forth I certainly moved the needle in the correct direction.




# Godot Mod Loader
Godot, GDScript, JSON

One of the greatest challenges in mod support is how to handle multiple mods that change the same fields or even other mods. I implemented a mod loading system for my <a href="/Projects/PortableTrainGame">train project</a> that supported exactly that. Rather than treating game content as self contained objects, I designed the JSON structures to function more like database operations. That way mods are able to not only create assets and callbacks but modifying existing ones. On top of that, implementing the ability to load assets at runtime (textures, models, data, scripts, etc.), to load loading scripts that can then load totally new asset types, and the ability to hotswap mods without restarting the game created an impressively moddable project that I am extremely proud of.

A section of the mod config describing a locomotive in the game:

<img src="/Images/mod_loader_json.png" width="75%" alt="Screenshot of mod config describing the configuration of a locomotive" /> <br>




# Non-Destructive Terrain Editing Tool
Godot, GDScript, GLSL

I strongly believe trains should have tunnels to drive through, so I knew it was essential to build a fully featured terrain system for my train project. At the same time I'm also of the opinion that standard heightmap painting tools are a cumbersome solution to the problem of both building mountains and naturally blending in structures and other objects into terrain. <a href="https://www.youtube.com/watch?v=YOtDVv5-0A4" target=null>The developers of Alba</a> mentioned they built an editor tool to generate terrain meshes out of component shapes. Inspired, I decided to take it a step or two further and non-destructively combine painted heightmaps and splatmaps with generated meshes that indicate both heightmap height, blend strength, and even splat map material at runtime. This allows player placed structures such as train tracks and buildings as well as larger terrain editing objects to all combine together smoothly at runtime without the need to hand paint each individual structure.

Train tracks adjusting heightmap and splatmap automatically on a test map:

<img src="/Images/portable_train_overview.png" width="75%" alt="Train tracks building up supporting terrain underneath them" /> <br>




# Floating Origin GPU Particle Grass
Godot, GLSL, GDScript

Since the train project's target device is a Steam Deck I can't count on the same level of performance as my desktop GPU. Consequently I was struggling to render my desired density of grass spread across the terrain even using classic systems like batched rendering. Inspired partially by <a href="https://gdcvault.com/play/1027214/Advanced-Graphics-Summit-Procedural-Grass" target=null>Ghost of Tsushima's</a> grass sytem I implemented a floating origin grass system that blends the best of both worlds combining high density nearby grass while avoiding excessive overdraw at further distances. Sampling my terrain splat map also allowed me to customize the grass parameters per material and to automatically prevent spawning grass inside objects. Combined with random offsets based on world coordinate and fading out at a distance, the grass is able to seamlessly stay focused on the player without any pop-in, even on a lower end device.

Grass automatically accounting for the splat map material:

<img src="/Images/portable_train_grass.png" width="75%" alt="Grass system in Godot accounting for terrain material" /> <br>




# Blender to Godot Foliage Pipeline
Blender, Python, Blender API, Godot, GLSL, GDScript

I implemented a Python script in Blender to "fluff up" foliage and to store additional animation data in the vertex colors of meshes. Then I implemented the Godot side of the equation creating the shaders in the Godot shader language to display the end result. The resulting foliage blows in the wind in game and blends with lower detailed LOD meshes at a distance to smooth the transition to static meshes.

Blender view of customized tree vertex colors:

<img src="/Images/blender_tree_vert_pos.png" width="75%" alt="Tree in Blender using vertex colors to cue animation" /> <br>

Breeze blowing a tree in engine:

<img src="/Images/godot_tree_anim_optimization.gif" width="75%" alt="Tree in Godot rustling with the wind" /> <br>




# Godot Rocket Simulation
Godot, GLSL, GDScript

Inspired by <a href="https://www.gamedeveloper.com/programming/aerodynamics-of-just-cause-4" target=null>this article</a> on the aerodynamics systems of Just Cause 4 I decided to implement a similar system in Godot. Using a subviewport and custom shaders I approximated rocket aerodynamics in realtime. Since I was checking the visual profile of the object I was then able to customize the model at runtime to simulate control surfaces and play around with alternative rocket designs.

Model rocket just about to launch (aerodynamic profile shown in the bottom left):

<img src="/Images/rocket_simulation.png" width="75%" alt="A screenshot of a model rocket model in a park about to launch" /> <br>




# Unofficial Tools for picoCAD File Editing
Python, Unity, C#

<a href="https://johanpeitz.itch.io/picocad" target=null>picoCAD</a> is a 3D modeling software made in PICO-8, a fantasy game console. It provides a creative tool to 3D model in a less imposing manner than Blender or Maya, but its limitations also mean that its users were looking for side tools to help them push the boundries as well as beginner friendly ways to achieve sometimes imposing tasks such as texture unwrapping and texturing.

I developed two tools to help the community. <a href="https://github.com/jordanfb/PicoCADToolKit" target=null>One is a general toolkit</a> written in Python using tkinter that implements features such as automatic UV unwrapping, mesh manipulation, color remapping, file splitting, combining, optimization and more. The toolkit is open source and has had contributions from members of the community implementing several UI improvements and tool features. <a href="https://jordanfb.itch.io/picocad-painter" target=null>The second tool</a> I developed is a texturing tool along the lines of Substance Painter that lets users texture their models by painting directly on the models themselves. Implemented in Unity it's far more visually complicated but unfortunately uses some paid UI assets so I've been unable to open source it.

This has been a very fun ongoing project. I started out by reverse engineering the file format and it expanded into much more than that. One of the core limitations I've set for these tools is that I don't want to implement face or vertex specific manipulations - I don't want to step on the toes of picoCAD. While that has limited some of the features that I've been able to implement that has also resulted in a nice parallel evolution of the tools. With over two thousand downloads all I can say is that I'm happy to have helped and excited to make some tools for picoCAD 2 which is being developed as of 2025.

picoCAD Painter:
![picoCAD Painter](https://jordanfb.github.io/Images/picoCADPainterScreenshot.png)
picoCAD ToolKit:
![picoCAD Toolkit](https://jordanfb.github.io/Images/picoCADToolkitScreenshot.png)




# Art and Pipeline Tools for Load Roll Die
Unity, C#, GitHub Actions

Perhaps unsurprisingly for a developer with as many tools projects as I have, I implemented several tools during the development of <a href="/Projects/LoadRollDie">Load Roll Die</a>. I started with some continuous delivery tools implemented in Github Actions to build the game for my target platforms and to deploy it to my playtesters on my itch.io page. I further implemented asset management tools to automatically generate necessary data types as well as to ensure my art and content assets were linked correctly. Last but not least I worked on several art tools to speed up art asset generation. The most obvious of those tools was a mesh generation tool that would build and UV unwrap the meshes for characters and locations on the overworld map given a texture with the cardboard outline shape hidden in the alpha channel. Totally removing the need to open mesh editing software at all, the tool sped up art iteration and production speed immensely which was essential for the solo project.

Load Roll Die overworld map:
![Load Roll Die Map](https://jordanfaasbush.com/Images/livelyMapVideoTrimmedGifOptimized.gif)


# SteamSeer
Python, Flask, Docker, PostgreSQL, Nginx, Steam Store API

I developed <a href="https://steamseer.toothpike.com" target=null>a site</a> to help game developers get a better sense of the market. A recreation (with permission) of the concepts behind the site Steam Prophet by Lars Ducet, users predict how many reviews a game on Steam will earn in the first four weeks after release and the site tracks the results and reports back. With the goal of making it as accessible as possible the site has user accounts for cross device and longer term save data but also supports saving predictions directly in your browser for anonymous use.
