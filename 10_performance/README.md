* https://docs.godotengine.org/en/latest/development/cpp/using_cpp_profilers.html
* https://github.com/godotengine/godot/issues/10970
* https://www.gdquest.com/tutorial/godot/3d/optimization-3d/
* https://docs.godotengine.org/en/stable/tutorials/performance/gpu_optimization.html
* https://docs.godotengine.org/en/stable/tutorials/performance/optimizing_3d_performance.html
* http://wiki.polycount.com/wiki/Polycount
* channel packing: https://github.com/Zylann/godot_channel_packer_plugin
* https://github.com/godotengine/godot/pull/58512

https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_images.html

https://ramatak.com/2023/04/28/boosting-performance-by-rendering-3d-at-a-lower-resolution/
* https://web.archive.org/web/20230522195144/https://ramatak.com/2023/04/28/boosting-performance-by-rendering-3d-at-a-lower-resolution/

best check out your shaders and logic in a bigger scene: https://github.com/Calinou/game-maps-obj


## Testing

When comparing performance, it's recommended to compare milliseconds per frame rather than frames per second. This avoids skewing results at very high framerates where some differences may seem large, but are actually rather small in the grand scheme of things. The formula to calculate mspf is 1000.0/fps.


## Viewport

Resolution
* Set Stretch Mode to `Viewport` in the project settings, and lower resolution at runtime using `get_tree().root.content_scale_factor` (see also [Window.content_scale_factor](https://docs.godotengine.org/en/stable/classes/class_window.html#class-window-property-content-scale-factor)). Increase the value to lower the resolution.


## scene

A tip: If you enable sub-threads, large scenes with many images and dependencies can load up to ten times faster!

ResourceLoader.load_threaded_request("res://", "", true);
var scene = ResourceLoader.load_threaded_get("res://");

### multithreading

This is probably a good example of how Godot is just  designed different to most other game engines. In most existing engines, if you want to start taking advantage of multiple cores to optimize your game logic, you have basically two choices: 

The first one is to use Jobs. Jobs is basically using threads yourself but sugar-coated with things like dependencies. It is still a very low level solution, requires large rewrite, and does not solve most of the thread-related problems such as race conditions, IPC..

The second one is to use ECS. This solves more problems for you, but its still considerably more complex to use and adapt your game logic to and, if your game requires dozens or hundreds of complex entities (not [tens of]thousands), the DO nature is not really an advantage..

But due to how you make games in Godot, most of the logic runs intrinsically inside sub-scenes. Within a single scene you will:
-Do kinematic physics
-Do pathfinding
-Run animations
-Update skeletons
-etc
Most of this is data/logic flowing within the scene, with little outreach..

So, when you have lots of those scenes, how do you optimize? Just tag them to run parallel in threads and that's it. Elegant and far simpler! Instant performance without having to rewrite your game or use much more complex features.

And the way its implemented, its _very_ difficult for you to mess up because the idea is that accessing outside your valid API surface will trigger an error to let you know you are doing a mistake, without having to worry about hard to debug deadlocks or race conditions.


## GPU Instancing

* MultiMesh3D
* you also get them for free, if the MeshInstances are the same. grouped by proximity. e.g. when importing geonode instances from Blender. Objects are batched by their location. Moving an object away from its copies puts it in a separate batch, increasing the number of draw calls by 1. Although it's hard to say what the algorithm is, sometimes it's 70 meters, sometimes it's 300, but I'm too lazy to research this, I'm just glad the magic is happening. A different material is a split reason


## shaders

https://docs.godotengine.org/en/latest/tutorials/3d/variable_rate_shading.html

 Does anyone know what the docs means by "However, high amounts of VGPRs (which can be caused by having too many branches) can still slow down shader execution significantly."? (link: https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shader_preprocessor.html). I am a bit confused since VGPR seems to be the shorthand for vector registers, and I don't quite understand why the high amounts could slow down shaders. Thanks.
12:52 AM
indeed
Basically, if you have branches, the GPUs must allocate the maximum possible number of registers across all branches, since it has no way of knowing if execution will take the program to those branches
hence, the GPU may limit the number of parallel threads on a core due to not having enough registers to cover the worst possible case of each thread
so let's say you have
... some shader code .... if (some_uniform) { expensive code that takes 32 registers;} ... some shader code ..., even if some_uniform is false, your program will take 32 extra VGPRs even if you never go into that branch. The GPU may have 1024 registers, and now perhaps instead of having 16 parallel executions (taking 64 VGPRs each), it now can only have 12 or so (taking 96 VGPRs each).



frame debugging tools
* RenderDoc


### Load Times

Godot 4
* https://github.com/godotengine/godot/pull/49050/files#diff-fde7b128f969876f24670dfbe1a65bcd906ea226b337cbad5467d73738b6e80b
* https://github.com/godotengine/godot/issues/43351
* https://github.com/godotengine/godot-proposals/issues/4754

Godot 3
* https://github.com/godotengine/godot/pull/53411
* https://github.com/godotengine/godot/pull/56366
* https://github.com/outfrost/ld47/issues/21
* https://github.com/imjp94/gd-shader-cache
* https://www.reddit.com/r/godot/comments/osx0f6/my_very_comprehensive_shader_cache_solution/
* https://www.youtube.com/watch?v=kbDj9V2MZvw

## Misc


### Squeezing - micro things

if you have your own fonts, you can clear the cache of system fonts:
TextServer.font_clear_system_fallback_cache


###

https://itch.io/t/2885012/tips-for-reducing-the-size-of-your-build

If you’ve ever tried to upload a larger game to itch.io, you may have noticed that we have a default file size limit, and you need to get approval to upload larger files. Larger files bring about many costs - not only do they entail higher bandwidth and storage expenses, but they can also make your game less accessible to players due to extended download times.

Before we grant an increased upload size for your game, we ask that you share how you’ve used best practices to ensure that you’ve optimized the build and assets appropriately.

This guide includes some things to look out for. If you have any tips to add, please leave a reply below, and we’ll update the guide accordingly.
Identify and Review Largest Assets

Often, a handful of assets contribute disproportionately to the build size.

    Identify large assets: Some game engines include a profiler tool to identify the largest assets in your build. Otherwise, use your file manager and sort by size.
    Review usage: Check whether these large assets are vital for your game. If an asset is not essential or is used infrequently, consider removing it or replacing it with a smaller asset. For example, a model of a rock that appears in the background of a cutscene should not be larger than your main player’s model.
    Optimize assets: If the large assets are necessary, ensure they are optimized and compressed
    Be weary of Asset Store resources: Files obtained from Asset Stores may include substantially more details than you need. Review assets you’ve imported individually to ensure they meet your size requirements

Use Compressed Formats Always, Use “Lossy” Formats When Possible

Lossless file formats, while providing high-quality assets, can significantly increase your game’s build size. Lossy compression formats can substantially decrease the size while maintaining a high level of quality. Unless explicitly needed for your application, avoid using lossless formats.

    Images: Use JPEG, WebP or PNG for images instead of formats like BMP. Vector image formats, like SVG, typically compress well, but ensure that they do not have too many details for the resolution they are rendered at. Ensure that you don’t have raster data embedded into vector formats that is not compressed.
        Resolution: Avoid overly large images. Consider the resolution the image will be displayed at. For example, including an 8k backgrop image for a game that targets a 1080p resolution could be a waste of resources.
    Audio: Formats like MP3 or Ogg Vorbis can greatly reduce file sizes compared to WAV or FLAC.
    Video: Use codecs like H.264 or H.265 for video files. Ensure you’ve selected an appropriate bit rate or encoding quality to balance both size and visual quality.

Always balance quality and size. Over-compression can result in a poor experience due to noticeably lower quality assets.
3D projects
Optimizing Textures

    Compression: Use compressed texture formats. For instance, DXT (DirectX Texture Compression) formats can significantly reduce file size. Keep in mind that the appropriate compression format depends on the target platform of the game.
        Unity Reducing the size of your build
        Unity Texture Compression formats
        Unreal Engine Reducing Packaged Game Size
        Godot Importing Images
    Resolution: Scale down the resolution of textures, especially for mobile or web games. A high-resolution texture on a small screen doesn’t significantly improve visual quality.
    Reuse and Recycle: Try to use texture atlases and UV mapping to make the most out of a single texture. Instead of using unique textures for each model, see if you can reuse existing textures creatively.

Reducing 3D Model Sizes

3D models can be large, especially when they are highly detailed.

    Polygon Reduction: Use tools to reduce the polygon count of your models. Many 3D modeling tools like Blender or 3ds Max have built-in decimation tools. Consider creating LODs for highly detailed models, especially if they are rendered only in the distance.

Other Optimizations

    Dead Code and Unused Assets: Over time, your game might accumulate code and assets that are no longer in use. Regularly review and clean up your project to remove these.
    Duplicate Assets: Ensure code and data that use the same image or model reference a single copy of it, instead of a duplicate for each object or class you have defined.
    Procedural Generation: If it fits your game, consider using procedural generation for some assets. This can massively reduce the size of your game, but it also has some unique challenges. As a simple example, instead of having copies of the same image in different colors, perform the color manipulation in code with a shader. More advanced examples could include generating entire levels, models, images, and audio files at runtime within the game’s code

Reply
redonihunter32 days ago

PNG is a lossless format. It is compressed and heaps better than BMP, but it has its uses for small or exact assets like ui elements and not for photorealistic renders or actual photos. If you transform a png of a rendered master image  into jpg you can go down orders of magnitude in file size. While you can go down like x3 or x4 from BMP to PNG, you can go down another x5 to x20 from PNG to JPG.

For jpg specifically, just try it out. Make a row of sample pictures of your content and since the compression in jpg is given as a number up to 100, just make a 100, a 95, a 90 ... and so on. Then mix in your original and see for yourself how small you can go, until you notice and how far you can go even further till you care. jpg is *very* good at compressing natural looking stuff or stuff that was made to look like it might be natural. Pixelart and comics/anime are unnatural in this context. (note, 100 is not lossless, but even this placebo setting can be half as big as a png)

WebP is available in lossless and lossy, so depending on the content of the image, the same rules applay. Pixel heroes -> lossless. Photorealistically rendered heroes -> lossy.

In other words, there do be reasons, why your digital camera stores those ZillionMegalPixel Pictures in jpg. That is the format you want for stuff that looks like real things. For stuff that looks like my little pony animation with big same color areas and sharp lines you might get away with lossless compression, as this compresses fantastical and you do not want to be fuzzy around the sharp edges, and for pixel graphics, you want to keep the pixels intact, so lossy is bad for those.

Just try out with different samples of your images what is efficient.
