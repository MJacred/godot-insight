## Portal-based occlusion culling

https://github.com/godotengine/godot/pull/46130
and more: https://github.com/godotengine/godot-proposals/issues/3814#issuecomment-1021355647

## Static Mesh Occluders

https://github.com/godotengine/godot/pull/48050


https://twitter.com/reduzio/status/1384677321156833283

Q: Does Occlusion Culling ensure that only objects visible by the camera are drawn, so it's faster?
A: No, that is Frustum Culling, Godot always supported this since it was open sourced.

Q: So, does Occlusion Culling prevent overdraw (drawing a pixel multiple times)?
A: No, that is prevented thanks to the depth prepass, Godot supported this since it was open sourced. Additionally mobile hardware has no overdraw due to TBDR architecture.

Q: Does occlusion culling in Godot run on the GPU?
A: No, it runs on the CPU. It does this to ensure objects are culled as early as possible, and so other objects (like lights, shadow redrawing, decals, refprobes, etc) dont process. Doing it on GPU is less of a win.

Q: What technique does Godot use for occlusion culling?
A: Depth buffer generated from Embree, then HZB,

Q: Embree? Is not raytracing slower than rastering?
A: No, on CPU and for this very purpose (lots of complex geometry on a low resolution buffer), raytracing is much faster. It allows to use your regular level geometry as occluders in most cases.

Q: Why no depth buffer reprojection?
A: It will eventually be an option, but it's not so precise (specially for interiors), and Embree is insanely fast at doing this.

Q: I don't understand any of that, so how do I use it?
A: You can tag meshes to also be occluders on import. 

Additionally you can tag meshes as _only_ occluders, if you want, then use simpler occluder geometry. Not a big win on desktop, but useful for mobile or slower hardware.

Q: Should I always use occlusion culling to improve performance?
A: No, it has a base cost. Use only for complex interiors (several rooms), cities, or other levels with can lead to large amount of objects (drawcalls and vertex count). Remember overdraw itself is not a problem.

Q: Will I able to debug and profile occlusion culling?
A: Yes the overdraw mode will show you when objects  culled disapper. Additionally, you will see a % of scene culled in your profiler.

Q: Can I use occlusion culling dynamically in procedural games, terrain, etc.
A: Yes, it works really well, but for if the level static objects change every frame it may not be so efficient (takes a while to rebuild).

Q: Are truly dynamic occluders supported?
A: Yes, but because they are drawn to the occlusion buffer using rastering, they should use relatively simple geometry (like boxes or very lowpoly meshes), and limited to not so many per scene (static can use millions of triangles).

Q: I am so excited about this feature that I will enable it anyway even if I don't need it. Does it have a cost?
A: If no occluders are present, there are no cost. If occlusers are present, you will have a slight base cost, that only becomes a win when many objects are culled.

Q: An occluded light will enlighten a visible object? An occluded object will emit indirect light to non occluded objects?
A: Technically if a light is occluded, the part of the object being lit by it will not be visible, so disabling it has no effect. Indirect light is not affected.

Q: So in a city scene there is some occluder property that has to be enabled for every single building?
A: You can make your entire scene occluders on import, or tag them manually, or generate them from the geometry when using the editor.
Q: "or generate them from the geometry" The more poligon that mesh has, the more performance his occluder will eat?
A: yes, as with Raytracing, this is logarithmic complexity, so for the most part, its ok. You can use regular level geometry for most cases and it will be fine. Maybe for things like bushes (lots of tris) you want to keep out as occluder or make a custom one (occluder only).

Q: will an occluded object still cast a long shadow that stretches beyond the occlusion buffer and thus should be visible on other objects/terrain?
A: Yes, shadow rendering is a separate, more optimized, pass.

