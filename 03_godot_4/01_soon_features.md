## Features that are not live YET

Sometimes it might make sense to make a custom build of Godot which includes Pull Requests that are basically done, but some final approval or review is missing. And this last step can take months, or longer.

> Disclaimer: Once those PRs are merged, it can happen that they needed to introduce some breaking change and you'll have to redo some things.  
> Apart from making sure to use a version control system (e.g. Git), make sure to additionally store your assets (image/audio/heightmap/3D model/animation/etc. files) somewhere outside of the project. Just in case some weird glitch happens and Godot modifies and overwrites your original file and that leaves it broken/destroyed. Of course, the duplicate asset only make sense if you merge Pull Requests that touch those things.

It's a good idea to check certain things first, before merging:
* check the description and latest comments first: maybe some crash still lingers
* do the Github checks succeed? At least the ones on your platform should succeed
* make a new git branch (or whatever version control system you use) for your project and test the changes there first


These are the biggest features that you might want to use:
* 3D editing
  * snapping
    * [Fix "Use snap (Y)" inconsistent behavior](https://github.com/godotengine/godot/pull/91587)
  * [Add "Follow Selection" in the 3D editor by using Center Selection twice](https://github.com/godotengine/godot/pull/99499)
* Animation
  * [Implement a Texture2D to support Lottie animation](https://github.com/godotengine/godot/pull/91580)
* Input
  * [Improve gamepad support on Linux](https://github.com/godotengine/godot/pull/95486)
* Navigation
  * [Add NavigationArea3D and navigation layers cost map](https://github.com/godotengine/godot/pull/102769)
  * [Add support for holes when triangulating HeightMapShape3D for navigation](https://github.com/godotengine/godot/pull/102215)
    *  Current state (end of March 2025): Changes requested.
* Particles
  * 3D only: [Add Particle System emission shapes gizmo](https://github.com/godotengine/godot/pull/86902)
* Physics
  * [Add PhysicsServer2/3D::space_step() to step physics simulation manually](https://github.com/godotengine/godot/pull/76462)
* Rendering
  * [Add Tony McMapface as a tonemapping mode](https://github.com/godotengine/godot/pull/97095)
    * Why? "[…] Linear, Reinhard and Filmic suffer from oversaturated bright lights, whereas ACES steers bright blues towards purple and can significantly darken scenes"
    * Current state (December 2024): [requesting a re-design](https://github.com/godotengine/godot/pull/97095#issuecomment-2554522539), there were more commits since, but no new comment on the requested re-design
* Shaders:
  * [Custom shader templates](https://github.com/godotengine/godot/pull/94427)
* Vector:
  * [Add move_toward_smooth helper](https://github.com/godotengine/godot/pull/92236)
  * [Add rotate_toward to Vector2, Vector3, Basis and Quaternion](https://github.com/godotengine/godot/pull/82926)


## Features I'm pretty sure make it in time for version 4.5

> The feature freeze for 4.5 is just around the corner. And all of these could make it. Or none.

* 3D editing
  * [Add option for Path3D to snap to colliders](https://github.com/godotengine/godot/pull/102085)
  * [Expose 3D editor snap settings to EditorInterface](https://github.com/godotengine/godot/pull/103608)
    * talking about grid snapping
* Input
  * [Add support for SDL3 joystick input driver for Windows, Linux and MacOS](https://github.com/godotengine/godot/pull/106218)
* Navigation
  * [Add navigation path query parameter limits](https://github.com/godotengine/godot/pull/102767)
* Rendering
  * [[Windows] Support output to HDR monitors](https://github.com/godotengine/godot/pull/94496)
* Shaders:
  * [Add stencil support to spatial materials](https://github.com/godotengine/godot/pull/80710)
    * demo: https://github.com/apples/godot-stencil-demo
  * [Add support for depth function in spatial materials](https://github.com/godotengine/godot/pull/73527)
    * state: if merged, then probably together with the stencil PR
* Workflow:
  * [Allow creating GDExtension plugins from inside the Godot editor](https://github.com/godotengine/godot/pull/90979)
  * [Migeran LibGodot Feature](https://github.com/godotengine/godot/pull/90510)
    * Control Godot Engine from a host application
    * Use it for automation of development tasks
    * state: "cicd tests don't work for all platforms"


## Features that went live

starting Godot 4.5
* Accessibility
  * [Implement screen reader support using AccessKit library](https://github.com/godotengine/godot/pull/76829)
* Import
  * [Fix fbx runtime import not generating meshes properly](https://github.com/godotengine/godot/pull/105787)
* Rendering
  * [Implement FXAA 3.11](https://github.com/godotengine/godot/pull/89582)
  * [Add SMAA 1x to screenspace AA options](https://github.com/godotengine/godot/pull/102330)

starting Godot 4.4
* Resources: [Universalize UID support in all resource types](https://github.com/godotengine/godot/pull/97352)
* Curve: [Extend Curve to allow for domains outside of [0, 1]](https://github.com/godotengine/godot/pull/67857)
* Particles
  * [Implement particle seek request and seed options](https://github.com/godotengine/godot/pull/92089)
* Rendering
  * [Implement LightmapGI shadowmasks](https://github.com/godotengine/godot/pull/85653)
  * [Add AgX tonemapper option to Environment](https://github.com/godotengine/godot/pull/87260)


## Salvagable Features

* Import
  * [Fix FBX runtime import](https://github.com/godotengine/godot/pull/96059)
* Shaders
  * [Add post light function for forward pipelines (useful in toon shading)](https://github.com/godotengine/godot/pull/102708)
