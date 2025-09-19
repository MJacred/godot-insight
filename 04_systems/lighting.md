## Light Projectors

> supported in spot and omni lights

* https://github.com/godotengine/godot/pull/37887


## Shadowmasking

https://github.com/godotengine/godot-proposals/issues/2354


## Shadows

https://github.com/godotengine/godot-proposals/issues/12248
https://github.com/godotengine/godot-proposals/issues/4635
https://github.com/godotengine/godot-proposals/issues/7590


## Shadow Volumes (Stencil Shadows)

https://en.wikipedia.org/wiki/Shadow_volume

Can be implemented by 3rd parties, once stencil buffer is implemented (source [1](https://github.com/godotengine/godot-proposals/issues/1930#issuecomment-930379057), [2](https://github.com/godotengine/godot-proposals/issues/3373#issuecomment-932313022)).


## Global Illumination

GI happens in a separate render pass
(forward if using MSAA, deferred if not using MSAA)
this allows rendering it at half resolution if desired, to improve performance
> Calinou


## Research

https://github.com/godotengine/godot/issues/56033
