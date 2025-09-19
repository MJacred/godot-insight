https://twitter.com/reduzio/status/1401589065905061892

Lumen:
* High quality, mostly leak-free, supports dynamic (non-deformable) objects, high freq light.
* Complex objects need to be manually split in parts.
* Meshes need storage space on for high res SDF.
* Won't work with procedural geometry.
* Poor reflections.
* High end only.

SDFGI: 
* Reasonable quality, very thin walls can leak light, no high freq detail (need SSGI to this).
* No dynamic objs for now (likely 4.1).
* Instant-on, Just Works (tm).
* Great sharp (voxel) and rough reflections.
* Procedural is fine.
* Runs on old GPUs.

VoxelGI (4.0)
* Not great for realism, leaks light. Not open world (limited to areas).
* Fully dynamic object support, even deformable.
* Needs prebake of static objects (maybe not for 4.1).
* Good sharp (voxel) and rough reflections.
* Runs great even on old Intel IGP.

So, in short, I think Lumen is aimed at high end, and is significantly more laborious to use, but looks great. It will probably not have much reason to be when raytracing becomes more available, other than working with Nanite..

Oh the other hand Godot 4.0 provides high quality real-time GI that still can look very good, is much easier to use and is more complete (reflections, rough and sharp) 

Most importantly, its designed so anyone, even with an old GPU or IGP will still be able to enjoy your game.

It still needs to be optimized a bit more, but you should be able to run it on something like a 960 or 1050 at 60fps, and lower at 30fps. The VoxelGI (Ex GIProbe) in 4.0 has more optimizations so it runs on IGP now.


poor reflections?
Afaik its kind of like with regular raytracing. Rough reflections are complex because reflections dont move with the regular motion vectors in TAA. In Godot 4.0, SDFGI uses automatically placed probes and VoxelGI uses voxel cone tracing for those, so they look pretty good.


No dynamic objects means you can’t transform meshes and have the GI update, right? Why is that? And just to check, not deformable means meshes themselves cannot be modified?

Yes, deformable is often vertex anim, blend shape, skeleton, etc.

Wasn't this technique using some captures from different angles, similar to what lumen ended up doing (cards)? Or am I confused?

Kind of, in VoxelGI godot renders supersampled cards too, then resolves them to every 3D texture mipmap. As Voxel Cone Tracing supports transparecy, it allows for smooth object motion.

