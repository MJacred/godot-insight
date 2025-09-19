* you only load some section of the game at runtime not all of it
* Crowd npcs in far distance are just static mesh with vertex animation baked in texture. Not full and high polygon skinned mesh all over the place.
* hlod
* multimesh
* load points

## Double Precision

When compiling Godot using double precission, the main problem is that shaders don't support 64bits doubles well, and using 32 bits cause jitter when camera is far away from the origin (due to model * view multiplication). This is a nice solution:
http://andrewthall.org/papers/df64_qf128.pdf

To clarify, when possible, one would compute the modelview on the CPU side (and this is the case of OpenGL and the mobile renderer), but more advanced renderers do almost everything on GPU so this is not always possible.

via nature of floating point you can represent a bigger number having two floats via compliemtary mantissa and exponent. For this specific case, only addition is needed.

learn floating point arithmetic

Metal is one of the biggest reasons why we want to go this route, as it does not support doubles.

I vaguely remeber doing this on the cpu when learning OpenGL. If I'm right it's the projection of the to be rendered object into the viewers coordinate system. Could this be done with CUDA or OpenCL? Maybe the renderer could accept some 64 bit data lying in GPU memory as input?
I think those nowadays support doubles, this is more for the sake of portability.

Camera-relative worldspace is a good partial solution to the problem. Each frame on the CPU, subtract camera position from your model matrices (inlcuding lights, refl probes, etc), and then zero out translation in your view matrix. (Unity HDRP does this)
If doing this for every model matrix is too expensive cpu-wise, you can also do it in the shader, which works fairly well.

Apart from that, another good solution is recentering the world origin when the camera gets far away. (I think Unreal does this) You can also combine both.

So does HDRP handle being far from the origin much better than built in?

In theory, yes. However, it could be better, as for model matrices, it still does all the work in the shader with single precision floats. Ideally you'd do it all on the CPU with double precision, then convert back to single for the GPU, but SRP/HDRP doesn't allow this yet.


## Heap

source: https://twitter.com/FilmicWorlds/status/1562090212225716224

One of the fascinating things for me at Naughty Dog as a junior programmer was that allocating memory was forbidden. Literally everything was fixed size with several heaps (like the level heap). Per-frame allocations were allowed as an arena allocator that was wiped each frame.

When Uncharted 2 was near ship, I had to ask for 1k of memory. @cgyrling
 looked at me and said "well, we have 2k left, so take it". That's how tight memory was.
 
 If you go beyond the limits, then you crash. But at least you know exactly which limit you breached. On a console, if you allocate and free often, you can still run out (and crash) from fragmentation, even if you keep under budget and don't leak memory.
 
 In practice, it makes writing a new feature harder because you have to remove all allocations. But it makes shipping it easier because you don't get surprised by nasty fragmentation/leaking bugs later.
 
 What are the types of things that would be allocated into an arena alloc.? As opposed to just using the stack? Is it just easier to deal with "transient" pointers throughout the engine?
 
 A really common one is debug text you want rendered to the screen. The string will likely need to be allocated by a function that will not be on the stack when the rendering code runs. But once rendered, you no longer need that string.
 
 Likewise, while working on ps2 titles. PS3 was a relief. Game data for all AI was a binary image we streamed in and fixed up pointers on. Object allocations were fixed size and came out of free lists.



