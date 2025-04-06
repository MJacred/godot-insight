## Features that are not live YET

Sometimes it might make sense to make a custom build of Godot which includes Pull Requests that are basically done, but some final approval or review is missing. And this last step can take months, or longer.

> Disclaimer: Once those PRs are merged, it can happen that they needed to introduce some breaking change and you'll have to redo some things.  
> Apart from making sure to use a version control system (e.g. Git), make sure to additionally store your assets (image/audio/heightmap/3D model/animation/etc. files) somewhere outside of the project. Just in case some weird glitch happens and Godot modifies and overwrites your original file and that leaves it broken/destroyed. Of course, the duplicate asset only make sense if you merge Pull Requests that touch those things.

It's a good idea to check certain things first, before merging:
* check the description and latest comments first: maybe some crash still lingers
* do the Github checks succeed? At least the ones on your platform should succeed
* make a new git branch (or whatever version control system you use) for your project and test the changes there first


These are the biggest features that you might want to use:
* Import: [Fix FBX runtime import](https://github.com/godotengine/godot/pull/96059) :red_circle: **salvageable**
* Input
  * [Use SDL for joypad input on Linux](https://github.com/godotengine/godot/pull/87925)
    * With some adjustments, this can run on Windows and macos as well
  * [Improve gamepad support on Linux](https://github.com/godotengine/godot/pull/95486)
* Navigation
  * [Add support for holes when triangulating HeightMapShape3D for navigation](https://github.com/godotengine/godot/pull/102215)
* Particles
  * 3D only: [Add Particle System emission shapes gizmo](https://github.com/godotengine/godot/pull/86902)
* Rendering
  * [Add Tony McMapface as a tonemapping mode](https://github.com/godotengine/godot/pull/97095)
    * Why? "[…] Linear, Reinhard and Filmic suffer from oversaturated bright lights, whereas ACES steers bright blues towards purple and can significantly darken scenes"
    * Current state (end of September 2024): https://github.com/godotengine/godot/pull/97095#issuecomment-2379582976
* Shaders:
  * [Add stencil support to spatial materials](https://github.com/godotengine/godot/pull/80710)
    * state: `Rebased for 4.4. Seems to work fine with the new render graph changes, but I'm not sure how to thoroughly test that.` ([2025-02-27](https://github.com/godotengine/godot/pull/80710#issuecomment-2687981855))
    * requires fine-grained control over rendering order; potentially only merged afer the compositor is done
    * demo: https://github.com/apples/godot-stencil-demo
  * [Add support for depth function in spatial materials](https://github.com/godotengine/godot/pull/73527)
    * state: `Similarly to the stencil PR, we need to first come up with a design for opaque multipass.` ([2023-10-11](https://github.com/godotengine/godot/pull/73527#issuecomment-1757689514))
  * [Custom shader templates](https://github.com/godotengine/godot/pull/94427)
* Vector:
  * [Add move_toward_smooth helper](https://github.com/godotengine/godot/pull/92236)
  * [Add rotate_toward to Vector2, Vector3, Basis and Quaternion](https://github.com/godotengine/godot/pull/82926)


## Features I'm pretty sure make it in time for version 4.5

> … based on gut feeling after reading latest comments / activity. Let's see how the predictions turn out…

* Accessibility
  * [Implement screen reader support using AccessKit library](https://github.com/godotengine/godot/pull/76829)
* Animation
  * [Implement a Texture2D to support Lottie animation](https://github.com/godotengine/godot/pull/91580)
* Navigation
  * [Add NavigationArea3D and navigation layers cost map](https://github.com/godotengine/godot/pull/102769)
  * [Add navigation path query parameter limits](https://github.com/godotengine/godot/pull/102767)
* Physics
  * [Add PhysicsServer2/3D::space_step() to step physics simulation manually](https://github.com/godotengine/godot/pull/76462)
* Rendering
  * [Implement FXAA 3.11](https://github.com/godotengine/godot/pull/89582)
  * [[Windows] Support output to HDR monitors](https://github.com/godotengine/godot/pull/94496)
* Shaders
  * [Add post light function for forward pipelines (useful in toon shading)](https://github.com/godotengine/godot/pull/102708)
* Workflow:
  * [Allow creating GDExtension plugins from inside the Godot editor](https://github.com/godotengine/godot/pull/90979)
  * [Migeran LibGodot Feature](https://github.com/godotengine/godot/pull/90510)
    * Control Godot Engine from a host application
    * Use it for automation of development tasks


## Features that went live

starting Godot 4.4
* Resources: [Universalize UID support in all resource types](https://github.com/godotengine/godot/pull/97352)
* Curve: [Extend Curve to allow for domains outside of [0, 1]](https://github.com/godotengine/godot/pull/67857)
* Particles
  * [Implement particle seek request and seed options](https://github.com/godotengine/godot/pull/92089)
* Rendering
  * [Implement LightmapGI shadowmasks](https://github.com/godotengine/godot/pull/85653)
  * [Add AgX tonemapper option to Environment](https://github.com/godotengine/godot/pull/87260)
