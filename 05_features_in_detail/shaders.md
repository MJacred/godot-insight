https://twitter.com/john_clayjohn/status/1556757286898597894


---

https://twitter.com/wojtekpil/status/1551330663260950535

optimize custom shaders specifically in shadow pass? I draw a lot of "fluffy" vegetation on screen and its killing my frame time

I know about this proposal but I am curious if there is something I can do right now: https://github.com/godotengine/godot-proposals/issues/4443 Is using two Mesh Instances (one for shadows only) the only option now?

I am not exactly sure what the shader is, but shader compilation does unused code removal. The fragment shader is doing literally no work during shadow pass unless you use things like discard.

During shadow pass, if you look at a GPU profiler, you will notice that its just mostly ROP work (the fixed function part of the GPU) its just slow, so if you have an AMD GPU Godot will try to do other things at the same time while shadow renders, like GI computation.

That might be an issue actually. I am creating my vegetation by using a sphere, and rotating each quad in mesh towards camera (billboard). I am using ALPHA_SCISSOR_THRESHOLD which I assume is equal to discard. Probably it creates a lot of overdraw.

Also, to be clear, this is not my "real" performance. Frame rate is severely hurt probably due to nvidia drivers bug on linux with external monitors. This is a screenshot from builtin one:

https://www.youtube.com/watch?v=1ho6tbxGt4c

## Basics

* [Depth Texture explained](https://www.youtube.com/watch?v=wyGWuGQO63Y)


## Color

The color picker/texture is in srgb space. By default ParticlesProcessMaterial uses the `source_color` hint which tells the GPU to do an automatic conversion to linear when sampling. So the actual value in the shader is in linear space. It stays in linear space unless you write code yourself to convert it
> clayjohn


### Vertex Color

Blender (3.3.0) Vertex Color (and alpha) Painting and FBX export to Unity: https://www.youtube.com/watch?v=u6wAwzBnaGw

https://www.youtube.com/watch?v=jqV5wYV_G7A&t=1118s

https://godotshaders.com/shader/vertex-color-rgb-material-blender/

https://github.com/godotengine/godot/issues?q=is%3Aissue%20state%3Aopen%20vertex%20colors&page=1
* https://github.com/godotengine/godot/issues/103475
* https://github.com/godotengine/godot/issues/87486
* https://github.com/godotengine/godot/issues/60027
* https://github.com/godotengine/godot/issues/97953
* 

## Worley / Voronoi noise

https://www.youtube.com/watch?v=E0BBJMkbskU
https://godotshaders.com/snippet/voronoi/


## Combining Normal Maps

https://blender.stackexchange.com/questions/38298/how-to-combine-two-normal-maps



## Stochastic Texturing

https://www.youtube.com/watch?v=YzmnpTjit-c
https://www.youtube.com/watch?v=ssrJGxMtssE

