## Framebuffer

Notes
* The [Viewport](https://docs.godotengine.org/en/stable/classes/class_viewport.html) usually uses a framebuffer, except you set [viewport_set_render_direct_to_screen](https://docs.godotengine.org/en/stable/classes/class_renderingserver.html#class-renderingserver-method-viewport-set-render-direct-to-screen) to `true`. ([source](https://docs.godotengine.org/en/stable/classes/class_renderingserver.html#class-renderingserver-method-viewport-attach-to-screen))
* [FramebufferCache](https://github.com/godotengine/godot/pull/63951)


## Depth Buffer (Z-buffer)

> Godot 3D uses the depth buffer for sorting objects. Reflected in the camera's **Near** and **Far** properties.

Notes
* Buffer size is _24-bit_ (desktop) or in some cases _16-bit_ (some mobile devices). Objects with the same buffer value start Z-fighting (ie. flickering occurs while the camera position/rotation changes). The depth buffer precision can be increased by _increasing_ the Camera's **Near** property value (set it too high and players can see through nearby geometry). And keep **Far** as low as possible. ([source](https://docs.godotengine.org/en/latest/tutorials/3d/3d_rendering_limitations.html#depth-buffer-precision))


## Fog Froxel Buffer

[FogVolume](https://docs.godotengine.org/en/latest/classes/class_fogvolume.html): _add_ or _remove_ **local fog**
* only have a visible effect if [Environment.volumetric_fog_enabled](https://docs.godotengine.org/en/latest/classes/class_environment.html#class-environment-property-volumetric-fog-enabled) is true. If you don't want fog to be globally visible (but only within FogVolume nodes), set [Environment.volumetric_fog_density](https://docs.godotengine.org/en/latest/classes/class_environment.html#class-environment-property-volumetric-fog-density) to 0.0.
* [RenderingServer.FogVolumeShape](https://docs.godotengine.org/en/latest/classes/class_renderingserver.html#enum-renderingserver-fogvolumeshape): FOG_VOLUME_SHAPE_ELLIPSOID, FOG_VOLUME_SHAPE_CONE, FOG_VOLUME_SHAPE_CYLINDER, FOG_VOLUME_SHAPE_BOX or FOG_VOLUME_SHAPE_WORLD.

Material & Shader
* [FogMaterial](https://docs.godotengine.org/en/latest/classes/class_fogmaterial.html) for standardized local fog
* [Fog shader](https://docs.godotengine.org/en/latest/tutorials/shaders/shader_reference/fog_shader.html) using the `fog()` processor function (like _vertex()_, _fragment()_ & _light()_) for custom fogs

Can be controlled/modified to some extend in [Environment](https://docs.godotengine.org/en/latest/classes/class_environment.html), [ProjectSettings](https://docs.godotengine.org/en/latest/classes/class_projectsettings.html) and [RenderingServer](https://docs.godotengine.org/en/latest/classes/class_renderingserver.html):
* can be affected by `VoxelGI` and `SDFGI` using [Environment.volumetric_fog_gi_inject](https://docs.godotengine.org/en/latest/classes/class_environment.html#class-environment-property-volumetric-fog-gi-inject)
* can be affected by `Sky` using [Environment.volumetric_fog_sky_affect](https://docs.godotengine.org/en/latest/classes/class_environment.html#class-environment-property-volumetric-fog-sky-affect)
* supports temporal reprojection
* [ProjectSettings/.../use_filter](https://docs.godotengine.org/en/latest/classes/class_projectsettings.html#class-projectsettings-property-rendering-environment-volumetric-fog-use-filter) for controlling blur/detail to fix harsh edges and aliasing artifacts
* [ProjectSettings/.../volume_depth](https://docs.godotengine.org/en/latest/classes/class_projectsettings.html#class-projectsettings-property-rendering-environment-volumetric-fog-volume-depth)
* [ProjectSettings/.../volume_size](https://docs.godotengine.org/en/latest/classes/class_projectsettings.html#class-projectsettings-property-rendering-environment-volumetric-fog-volume-size) for controlling detail
* Override the `ProjectSettings` using the `RenderingServer` functions
  * [environment_set_volumetric_fog_filter_active](https://docs.godotengine.org/en/latest/classes/class_renderingserver.html#class-renderingserver-method-environment-set-volumetric-fog-filter-active)
  * [environment_set_volumetric_fog_volume_size](https://docs.godotengine.org/en/latest/classes/class_renderingserver.html#class-renderingserver-method-environment-set-volumetric-fog-volume-size)


## Stencil Buffer

> is a per pixel buffer (integer values) representation of shapes

Common Use Cases:
* masking effects
  * Removing silhouettes of some objects (clipping them out of geometry using masks) ([source](https://github.com/godotengine/godot/issues/23721#issuecomment-439002500)); [example](https://youtu.be/9pWFCji7StY?t=151)
* fake lighting (like in [Zelda Wind Waker](http://simonschreibt.de/gat/zelda-wind-waker-hyrule-travel-guide/#shadowtheater)).
* [Shadow volumes](https://en.wikipedia.org/wiki/Shadow_volume)
* Planar Reflections
* Portals

Uncommon Use Cases which "[…] can be done much better adding custom g-buffer support" ([source](https://github.com/godotengine/godot/issues/23721#issuecomment-439246960))
* Outline
* Silhouette / X-Ray

Outline and Silhoutte can be achieved using depth functions:
* https://github.com/godotengine/godot/pull/73527
* https://github.com/godotengine/godot-proposals/issues/1298


Apparently, "Godot 4.0 seems to have a working stencil buffer, but stencil operations are currently not exposed through the Material APIs." ([source](https://github.com/alfredbaudisch/GodotShaderCollection/issues/3#issuecomment-1498231857))


## G-Buffer (Geometry Buffer)

> is a collection buffers in a texture representation

Currently not implemented in Godot. There's a [Proposal](https://github.com/godotengine/godot-proposals/issues/798) and [Pull Request](https://github.com/godotengine/godot/pull/38926) for Godot 3, but nothing in production.

Planned for Vulkan renderer ([source](https://github.com/godotengine/godot/issues/23721#issuecomment-439246960)).
