## Features that are not live YET

Sometimes it might make sense to make a custom build of Godot which includes Pull Requests that are basically done, but some final approval or review is missing. And this last step can take months, or longer.

> Disclaimer: Once those PRs are merged, it can happen that they needed to introduce some breaking change and you'll have to redo some things.  
> Apart from making sure to use a version control system (e.g. Git), make sure to additionally store your assets (image/audio/heightmap/3D model/animation/etc. files) somewhere outside of the project. Just in case some weird glitch happens and Godot modifies and overwrites your original file and that leaves it broken/destroyed. Of course, the duplicate asset only make sense if you merge Pull Requests that touch those things.  

It's a good idea to check certain things first, before merging:
* check the description and latest comments first: maybe some crash still lingers
* do the Github checks succeed? At least the ones on your platform should succeed
* make a new git branch (or whatever version control system you use) for your project and test the changes there first


These are the biggest features that you might want to use:
* Workflow:
  * [Allow creating GDExtension plugins from inside the Godot editor](https://github.com/godotengine/godot/pull/90979)
  * [Migeran LibGodot Feature](https://github.com/godotengine/godot/pull/90510)
    * Control Godot Engine from a host application
    * Use it for automation of development tasks
* Import: [Fix FBX runtime import](https://github.com/godotengine/godot/pull/96059) :red_circle: **salvageable**
* Gamepad/Joypad: [Use SDL for joypad input on Linux](https://github.com/godotengine/godot/pull/87925)
  * With some adjustments, this can run on Windows and macos as well
* Vector: [Add move_toward_smooth helper](https://github.com/godotengine/godot/pull/92236)
* Particles
  * 3D only: [Add Particle System emission shapes gizmo](https://github.com/godotengine/godot/pull/86902)
* Shaders: [Add stencil support to spatial materials](https://github.com/godotengine/godot/pull/80710)
* Rendering
  * [Implement FXAA 3.11](https://github.com/godotengine/godot/pull/89582)
  * [Add Tony McMapface as a tonemapping mode](https://github.com/godotengine/godot/pull/97095)
    * Why? "[…] Linear, Reinhard and Filmic suffer from oversaturated bright lights, whereas ACES steers bright blues towards purple and can significantly darken scenes"
    * Current state (end of September 2024): https://github.com/godotengine/godot/pull/97095#issuecomment-2379582976
* Animation: [Implement a Texture2D to support Lottie animation](https://github.com/godotengine/godot/pull/91580)
* Physics: [Add PhysicsServer2/3D::space_step() to step physics simulation manually](https://github.com/godotengine/godot/pull/76462)


## Features that went live

starting Godot 4.4
* Resources: [Universalize UID support in all resource types](https://github.com/godotengine/godot/pull/97352)
* Curve: [Extend Curve to allow for domains outside of [0, 1]](https://github.com/godotengine/godot/pull/67857)
* Particles
  * [Implement particle seek request and seed options](https://github.com/godotengine/godot/pull/92089)
* Rendering
  * [Implement LightmapGI shadowmasks](https://github.com/godotengine/godot/pull/85653)
  * [Add AgX and AgX Punchy tonemapper options to Environment](https://github.com/godotengine/godot/pull/87260)
