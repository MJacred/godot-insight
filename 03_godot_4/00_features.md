
# Godot 4 - Engine Feature Overview

> see official [feature list](https://docs.godotengine.org/en/latest/about/list_of_features.html). The following list aims to be oriented around API classes, whenever possible/feasible.

**NOTE: this list is still very much work in progress**

Legend
* :heavy_check_mark: - yes
* :no_entry_sign: - no
* :lemon: - yes, but as addon
  * listed in the **Workaround** column
  * lists the most well-known one
    * addons only available in Godot 3 are given hints; those without a hint, have a version for Godot 4 in the works
* :jack_o_lantern: - yes, but…
  * feature might have serious bugs or lack of certain cases (i.e. sub features missing), depending on your concrete needs
* Hierarchies displayed below do not necessarily reflect 100% actual class inheritance. In some cases, like Mid- and Post-processing, it does not represent classes :bookmark:, but rather components :package:.
* :ghost: deprecated / will be deleted
* :question: proposed, but not sure if needed


| Feature                        | Implemented        | Reference (more info)                                                 | Workaround / Alternative                                                             |
| ------------------------------ |:------------------:| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------|
| **Internationalization**       |                    |                                                                       |                                                                                      |
| gettext                        | :heavy_check_mark: | [Tutorial](https://docs.godotengine.org/en/latest/tutorials/i18n/localization_using_gettext.html) |                                                          |
| CSV                            | :heavy_check_mark: | [Tutorial](https://docs.godotengine.org/en/latest/tutorials/assets_pipeline/importing_translations.html) |                                                   |
| **Accessibility**              |                    |                                                                       |                                                                                      |
| Text-To-Speech (TTS)           | :heavy_check_mark: | on Android, iOS, HTML5, Linux, macOS, and Windows                     |                                                                                      |
| Speech-To-Text (STT)           | :no_entry_sign:    | [Proposal](https://github.com/godotengine/godot-proposals/issues/983) |                                                                                      |
| Screen Reader support          | :no_entry_sign:    | [Issue](https://github.com/godotengine/godot/issues/58074)            |                                                                                      |
| Colorblindness simulator       | :lemon:            | Godot 3                                                               | [Paul's Colorblindness simulator](https://github.com/paulloz/godot-colorblindness)   |
| **Fonts**                      |                    |                                                                       |                                                                                      |
| File Formats                   | :heavy_check_mark: | .ttf, .otf, .woff1, .woff2                                            |                                                                                      |
| Font rendering                 | :heavy_check_mark: | [News](https://godotengine.org/article/dev-snapshot-godot-4-0-alpha-1): Latin and Cyrillic characters, Arabic scripts, logograms of East Asian languages |   |
| text in left-to-right          | :heavy_check_mark: |                                                                       |                                                                                      |
| text in right-to-left          | :heavy_check_mark: |                                                                       |                                                                                      |
| FreeType sub-pixel anti-aliasing | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/64422)               |                                                                                      |
| **Supported Platforms**        |                    |                                                                       |                                                                                      |
| Desktop                        | :heavy_check_mark: | Windows, macOS, Linux, UWP, and *BSD                                  |                                                                                      |
| Consoles                       | :heavy_check_mark: | [Insight](../99_export/consoles.md)                                   |                                                                                      |
| Web                            | :heavy_check_mark: | HTML5 and WebAssembly                                                 |                                                                                      |
| Mobile                         | :heavy_check_mark: | iOS and Android                                                       |                                                                                      |
| **XR**                         |                    |                                                                       |                                                                                      |
| headsets                       | :heavy_check_mark: | Meta Quest, Valve Index, HTC Vive, Oculus Rift, etc.                  |                                                                                      |
| OpenXR                         | :heavy_check_mark: |                                                                       |                                                                                      |
| WebXR                          | :heavy_check_mark: |                                                                       |                                                                                      |
| ARKit                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D graphics APIs**           |                    |                                                                       |                                                                                      |
| Vulkan                         | :heavy_check_mark: | 2 backend versions: Clustered and Mobile                              |                                                                                      |
| GLES3                          | :heavy_check_mark: | [subset of OpenGLES3](https://github.com/godotengine/godot/pull/54307); [background info](https://godotengine.org/article/about-godot4-vulkan-gles3-and-gles2) |  |
| Direct3D 12                    | :heavy_check_mark: |                                                                       |                                                                                      |
| **Import**                     |                    |                                                                       |                                                                                      |
| 3D scenes                      | :package:          |                                                                       |                                                                                      |
| ├─ glTF 2.0                    | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Blender                     | :heavy_check_mark: | via ESCN                                                              |                                                                                      |
| ├─ FBX                         | :heavy_check_mark: | static meshes only; can be converted to glTF 2.0 on import            |                                                                                      |
| ├─ Wavefront OBJ               | :heavy_check_mark: | static meshes only                                                    |                                                                                      |
| └─ Collada                     | :jack_o_lantern:   |                                                                       |                                                                                      |
| Import glTF at runtime         | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/52541)                 |                                                                                      |
| material                       | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D skeleton / rig              | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D animation                   | :heavy_check_mark: |                                                                       |                                                                                      |
| **Export**                     |                    |                                                                       |                                                                                      |
| glTF 2.0                       | :heavy_check_mark: |                                                                       |                                                                                      |
| **Color**                      |                    |                                                                       |                                                                                      |
| Color Profile Managment (ICC)  | :no_entry_sign:    | [Proposal](https://github.com/godotengine/godot-proposals/issues/903) |                                                                                      |
| sRGB                           | :heavy_check_mark: |                                                                       |                                                                                      |
| **Images & Textures**          |                    |                                                                       |                                                                                      |
| Anisotropic                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Mipmaps                        | :heavy_check_mark: |                                                                       |                                                                                      |
| Svg                            | :heavy_check_mark: |                                                                       |                                                                                      |
| Channel Packing                | :lemon:            | Godot 3                                                               | [Zylann's Channel Packer](https://github.com/Zylann/godot_channel_packer_plugin)     |
| Image compressions             | :heavy_check_mark: |                                                                       |                                                                                      |
| TextureLayered                 | :bookmark:         | ↓ Generatable in Editor; has compressed and placeholder counterparts  |                                                                                      |
| ├─ Texture2DArray              | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Cubemap                     | :heavy_check_mark: |                                                                       |                                                                                      |
| └─ CubemapArray                | :heavy_check_mark: |                                                                       |                                                                                      |
| ImageTexture3D                 | :heavy_check_mark: | Generatable in Editor; has compressed and placeholder counterparts    |                                                                                      |
| Texture2D                      | :bookmark:         | ↓ Generatable in Editor; has compressed and placeholder counterparts  |                                                                                      |
| ├─ AnimatedTexture             | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ AtlasTexture                | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ CameraTexture               | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ CanvasTexture               | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ CurveTexture                | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ GradientTexture1D           | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ CurveXYZTexture             | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ GradientTexture2D           | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ ImageTexture                | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ MeshTexture                 | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ NoiseTexture                | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ PlaceholderTexture2D        | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ PortableCompressedTexture2D | :heavy_check_mark: | generatable by code                                                   |                                                                                      |
| └─ ViewportTexture             | :heavy_check_mark: |                                                                       |                                                                                      |
| **Materials & Shaders**        |                    |                                                                       |                                                                                      |
| Language: based on GLSL        | :heavy_check_mark: |                                                                       |                                                                                      |
| Physically-based rendering     | :heavy_check_mark: |                                                                       |                                                                                      |
| Visual Shader Node System      | :heavy_check_mark: |                                                                       |                                                                                      |
| Procedural PBR Materials       | :lemon:            | can also texture paint on 3D models                                   | [Material Maker - by Rodz Labs](https://github.com/RodZill4/material-maker)          |
| Decals                         | :heavy_check_mark: |                                                                       |                                                                                      |
| Volumetric Fog                 | :heavy_check_mark: | global ([PR](https://github.com/godotengine/godot/pull/41213)) or local as an area (fog volume; [PR](https://github.com/godotengine/godot/pull/53353)) |     |
| GPU lightmapper                | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/38386)                 |                                                                                      |
| CPU lightmapper                | :heavy_check_mark: |                                                                       |                                                                                      |
| Reflection Probes              | :heavy_check_mark: |                                                                       |                                                                                      |
| Stencil Buffer                 | :no_entry_sign:    | [Proposal](https://github.com/godotengine/godot-proposals/issues/3373) |                                                                                     |
| Screen Texture                 | :heavy_check_mark: | [Tutorial](https://docs.godotengine.org/en/stable/tutorials/shaders/screen-reading_shaders.html) |                                                           |
| Depth Texture                  | :heavy_check_mark: | [Tutorial](https://docs.godotengine.org/en/stable/tutorials/shaders/screen-reading_shaders.html) |                                                           |
| **3D Mesh**                    |                    |                                                                       |                                                                                      |
| Shape keys / Blend Shapes      | :heavy_check_mark: | [Insight](../05_features_in_detail/blend_shapes.md)                   |                                                                                      |
| Mesh Slicing                   | :lemon:            | Godot 3.2.1 (Godot C++ module, not a plugin) ([broken port to Godot 4](https://github.com/pooroligarch/godot-slicer)) | [CJ's Godot-Slicer](https://github.com/cj-dimaggio/godot-slicer) |
| **Lighting**                   |                    |                                                                       |                                                                                      |
| Directional Light              | :heavy_check_mark: |                                                                       |                                                                                      |
| Spot Light                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Point Light                    | :heavy_check_mark: | Godot calls it OmniLight3D                                            |                                                                                      |
| Area Light                     | :no_entry_sign:    |                                                                       |                                                                                      |
| Bake lighting                  | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D lights and normal maps      | :heavy_check_mark: |                                                                       |                                                                                      |
| **Audio**                      |                    |                                                                       |                                                                                      |
| 3D                             | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D                             | :heavy_check_mark: |                                                                       |                                                                                      |
| Bus Management                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Polyphony                      | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/52237)                 |                                                                                      |
| Audio Effects                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Midi Input                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Midi Output                    | :no_entry_sign:    |                                                                       |                                                                                      |
| File Support                   | :heavy_check_mark: | .wav, .ogg (Ogg Vorbis), .mp3                                         |                                                                                      |
| APIs                           | :package:          |                                                                       |                                                                                      |
| ├─ Windows                     | :heavy_check_mark: | WASAPI                                                                |                                                                                      |
| ├─ macOS                       | :heavy_check_mark: | CoreAudio                                                             |                                                                                      |
| ├─ Linux/BSD                   | :heavy_check_mark: | PulseAudio/ALSA/sndio                                                 |                                                                                      |
| ├─ Nintendo Switch             | :heavy_check_mark: | should be PulseAudio/ALSA/sndio (operating system is based on BSD)    |                                                                                      |
| ├─ PlayStation 4               | :heavy_check_mark: | should be PulseAudio/ALSA/sndio, (operating system is based on BSD)   |                                                                                      |
| ├─ PlayStation 5               | :heavy_check_mark: | should be PulseAudio/ALSA/sndio, (should still be based on BSD)       |                                                                                      |
| ├─ Xbox Series X/S             | :heavy_check_mark: | WASAPI                                                                |                                                                                      |
| ├─ iOS                         | :heavy_check_mark: | CoreAudio                                                             |                                                                                      |
| └─ Android                     | :heavy_check_mark: | probably PulseAudio/ALSA (as Android is based on Linux)               |                                                                                      |
| **3D Systems**                 |                    |                                                                       |                                                                                      |
| GridMap                        | :heavy_check_mark: | including Editor utilities                                            |                                                                                      |
| MeshLibrary                    | :heavy_check_mark: | only for GridMap                                                      |                                                                                      |
| Heightmap Terrain              | :lemon: :jack_o_lantern: | Godot 3.3 <= x < 4                                              | [Zylann's heightmap terrain addon](https://github.com/Zylann/godot_heightmap_plugin) |
| Waterway System                | :lemon:            |                                                                       | [Arnklit's Waterways](https://github.com/Arnklit/Waterways)                          |
| Navigation                     | :jack_o_lantern:   | [Insight](../04_systems/navigation.md), [News](https://godotengine.org/article/navigation-server-godot-4-0) | (see insight)                                  |
| Scattering Objects in Area     | :lemon:            |                                                                       | [HungryProton's Scatter](https://github.com/HungryProton/scatter)                    |
| Placing Objects indiviually    | :lemon:            | Godot 3                                                               | [Zylann's Scene scattering tool](https://github.com/Zylann/godot_scatter_plugin)     |
| Painting Objects on surfaces   | :lemon:            | Godot 3.5; HLOD via octree spatial partitioning; for Singleplayer Games; probably next in Godot 4.1 | [Dreadpon's Spatial Gardener](https://github.com/dreadpon/godot_spatial_gardener) |
| Procedural 3D Model Generation | :lemon:            | Godot 3                                                               | [3D Generator by ProtonGraph](https://github.com/protongraph/protongraph)            |
| Vertex Painting                | :lemon:            | Godot 3                                                               | [tomanski's Vertex Painter](https://github.com/tomankirilov/VPainter)                |
| Texture Painting               | :lemon:            | Godot 3.4                                                             | [Edouard's Mesh Painter](https://github.com/StrayEddy/GodotPlugin-MeshPainter)       |
| **2D Systems**                 |                    |                                                                       |                                                                                      |
| TileMap                        | :heavy_check_mark: | including Editor utilities                                            |                                                                                      |
| **3D Physics**                 |                    |                                                                       |                                                                                      |
| Custom Engine                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Bullet Engine                  | :lemon:            | Box, Capsule, Cylinder, Heightmap, Plane, Ray, Sphere, Concave Polygon, Convex Polygon |                                                                     |
| Collision Shapes               | :heavy_check_mark: | Box, Capsule, [Cylinder](https://github.com/godotengine/godot/pull/45854), [Heightmap](https://github.com/godotengine/godot/pull/47347), Plane, Ray, Sphere, Concave Polygon, Convex Polygon |  |
| Static Body                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Dynamic Bodies                 | :bookmark:         |                                                                       |                                                                                      |
| ├─ Kinematic Body              | :heavy_check_mark: | called CharacterBody3D in Godot                                       |                                                                                      |
| └─ Ridid Body                  | :heavy_check_mark: | called RigidDynamicBody3D in Godot                                    |                                                                                      |
| ........└─ VehicleBody3D       | :jack_o_lantern:   | uses arcade physics, not realistic 3D vehicle physics                 |                                                                                      |
| PhysicalBone3D                 | :heavy_check_mark: | [Ragdolls](https://docs.godotengine.org/en/stable/tutorials/physics/ragdoll_system.html) |                                                                   |
| SoftDynamicBody3D              | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/46937)                 |                                                                                      |
| Area3D                         | :heavy_check_mark: | detects bodies and areas entering and leaving                         |                                                                                      |
| Inverse Kinematics             | :heavy_check_mark: |                                                                       |                                                                                      |
| Joint3D                        | :bookmark:         |                                                                       |                                                                                      |
| ├─ ConeTwistJoint3D            | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Generic6DOFJoint3D          | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ HingeJoint3D                | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ PinJoint3D                  | :heavy_check_mark: |                                                                       |                                                                                      |
| └─ SliderJoint3D               | :heavy_check_mark: |                                                                       |                                                                                      |
| **2D Physics**                 |                    |                                                                       |                                                                                      |
| Custom Engine                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Bullet Engine                  | :lemon:            |                                                                       |                                                                                      |
| Joint2D                        | :bookmark:         |                                                                       |                                                                                      |
| ├─ DampedSpringJoint2D         | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ GrooveJoint2D               | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ HingeJoint2D                | :question:         | [Proposal with workaround notes](https://github.com/godotengine/godot-proposals/issues/1033) |                                                               |
| └─ PinJoint2D                  | :heavy_check_mark: |                                                                       |                                                                                      |
| **Camera**                     |                    |                                                                       |                                                                                      |
| View Frustum Culling           | :heavy_check_mark: | controlled via the Camera's near and far plane                        |                                                                                      |
| Automatic Occlusion Culling    | :no_entry_sign:    |                                                                       |                                                                                      |
| Baked Occlusion Culling        | :heavy_check_mark: | for static meshes, using Occluders ([PR](https://github.com/godotengine/godot/pull/48050), [intro](https://www.youtube.com/watch?v=hkE0laiXTFQ)) |           |
| Portal Occlusion Culling       | :heavy_check_mark: |                                                                       |                                                                                      |
| manual HLOD                    | :heavy_check_mark: | [manual HLOD using visibility ranges](https://github.com/godotengine/godot/pull/48847) |                                                                     |
| automatic Mesh LOD             | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/44468)                 |                                                                                      |
| **Screen**                     |                    |                                                                       |                                                                                      |
| Mid- and Post-processing       | :package:          |                                                                       |                                                                                      |
| ├─ Color Adjustment            | :heavy_check_mark: | Brightness, Saturation, Contrast, Color Correction (via Texture)      |                                                                                      |
| ├─ Ambient Light               | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Ambient Occlusion           | :heavy_check_mark: | Screen-Space Ambient Occlusion                                        |                                                                                      |
| ├─ Tonemapping                 | :heavy_check_mark: | modes (Linear, Reinhardt, Filmic, ACES), exposure, whitepoint         |                                                                                      |
| ........└─ auto exposure       | :heavy_check_mark: | luminance, intensity, speed                                           |                                                                                      |
| ├─ Height Fog                  | :heavy_check_mark: | aerial perspective, density, color, brightness, sun piercing          |                                                                                      |
| ├─ Glow                        | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Boom                        | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Global Illumination         | :heavy_check_mark: | Signed Distance Field Global Illumination (SDFGI) or voxel-based (VoxelGI) or baked lightmap (LightmapGI) |                                                  |
| ├─ Sky                         | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Indirect Lighting           | :heavy_check_mark: | Screen-Space Indirect Lighting                                        |                                                                                      |
| └─ Reflections                 | :heavy_check_mark: | Screen-Space Reflections                                              |                                                                                      |
| **UI**                         |                    |                                                                       |                                                                                      |
| Theming                        | :heavy_check_mark: |                                                                       |                                                                                      |
| **Animation**                  |                    |                                                                       |                                                                                      |
| Vertex Animation               | :heavy_check_mark: |                                                                       |                                                                                      |
| Blend shape Animation          | :heavy_check_mark: |                                                                       |                                                                                      |
| Animation Retargeting          | :heavy_check_mark: |                                                                       |                                                                                      |
| Animation Compression          | :heavy_check_mark: | [News](https://godotengine.org/article/animation-data-redesign-40)    |                                                                                      |
| **Particle System**            |                    | [Insight](../04_systems/particles.md)                                 |                                                                                      |
| Attractors                     | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| Trails                         | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/48242)                 |                                                                                      |
| Manual emission                | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/41810)                 |                                                                                      |
| Subemitters                    | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/41810)                 |                                                                                      |
| Collision                      | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| Baked SDF Collision            | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| Effekseer Support              | :lemon:            | Godot 3, [Godot 4 is planned](https://github.com/effekseer/EffekseerForGodot4) | [by Effekseer](https://github.com/effekseer/EffekseerForGodot3)             |
| **Code**                       |                    |                                                                       |                                                                                      |
| Scripting: GDScript            | :heavy_check_mark: |                                                                       |                                                                                      |
| Language bindings              | :heavy_check_mark: | via GDExtension: C++ (17), C# (8.0 Mono; .NET 6), Rust, etc.          |                                                                                      |
| **Networking**                 |                    |                                                                       |                                                                                      |
| Multiplayer                    | :heavy_check_mark: | DNS, HTTP, TCP, UDP, ENet, Websockets                                 |                                                                                      |
| Remote Procedure Call (RPC)    | :heavy_check_mark: |                                                                       |                                                                                      |
| Peer-To-Peer (P2P)             | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/50710)                 |                                                                                      |
| **Cinematics**                 |                    |                                                                       |                                                                                      |
| MovieWriter                    | :heavy_check_mark: | record **non-real-time** video footage                                |                                                                                      |
| **Miscellaneous**              |                    |                                                                       |                                                                                      |
| Engine headless mode           | :heavy_check_mark: | for multiplayer server hosting, CI/CD, and more                       |                                                                                      |
| Multithreading                 | :heavy_check_mark: |                                                                       |                                                                                      |
| WorkerThreadPool               | :heavy_check_mark: |                                                                       |                                                                                      |
| Asset streaming from disk      | :no_entry_sign:    | [hopefully with Godot 4.1](https://twitter.com/reduzio/status/1565957843475480576) |                                                                         |
| Octahedral Impostors           | :lemon:            | Godot 3, [Godot 4 is planned](https://github.com/wojtekpil/Godot-Octahedral-Impostors/issues/19) | [wojtekpil's impostors](https://github.com/wojtekpil/Godot-Octahedral-Impostors) |
| Extend Engine                  | :heavy_check_mark: | Godot Modules                                                         |                                                                                      |
| Performance Profiler           | :heavy_check_mark: |                                                                       |                                                                                      |
| Network Profiler               | :heavy_check_mark: |                                                                       |                                                                                      |
| Version Control                | :heavy_check_mark: |                                                                       |                                                                                      |
| 64-bit (for large world)       | :heavy_check_mark: | https://gist.github.com/Calinou/ceb6d24b16d85bc47f9c5de9c9ae341d      |                                                                                      |
| Directed Graph Dialogsystem    | :lemon:            |                                                                       | [coppolaemilio's dialogic](https://github.com/coppolaemilio/dialogic)                |
| Timer                          | :heavy_check_mark: |                                                                       |                                                                                      |
| Tweening                       | :heavy_check_mark: |                                                                       |                                                                                      |

