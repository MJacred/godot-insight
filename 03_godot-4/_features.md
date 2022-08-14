
# Godot 4 - Engine Feature Overview

**NOTE: this list is still very much work in progress**

Legend
* :heavy_check_mark: - yes
* :no_entry_sign: - no
* :lemon: - yes, but as addon
  * listed in the **Workaround** column
  * lists the most well-known one
* :jack_o_lantern: - yes, but…
  * feature might have serious bugs or lack of certain cases (i.e. sub features missing), depending on your concrete needs


| Feature                       | Implemented        | Reference (more info)                                                 | Workaround / Alternative                                                             |
| ----------------------------- |:------------------:| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------|
| **Internationalization**      |                    |                                                                       |                                                                                      |
| gettext                       | :heavy_check_mark: |                                                                       |                                                                                      |
| **Fonts**                     |                    |                                                                       |                                                                                      |
| .ttf                          | :heavy_check_mark: |                                                                       |                                                                                      |
| .woff2                        | :heavy_check_mark: |                                                                       |                                                                                      |
| text in left-to-right         | :heavy_check_mark: |                                                                       |                                                                                      |
| text in right-to-left         | :heavy_check_mark: |                                                                       |                                                                                      |
| **Supported Platforms**       |                    |                                                                       |                                                                                      |
| Desktop                       | :heavy_check_mark: | Windows, macOS, Linux, UWP, and *BSD                                  |                                                                                      |
| Consoles                      | :heavy_check_mark: | [Insight](../99_export/consoles.md)                                   |                                                                                      |
| Web                           | :heavy_check_mark: | HTML5 and WebAssembly                                                 |                                                                                      |
| Mobile                        | :heavy_check_mark: | iOS and Android                                                       |                                                                                      |
| **XR**                        |                    |                                                                       |                                                                                      |
| headsets                      | :heavy_check_mark: | Meta Quest, Valve Index, HTC Vive, Oculus Rift, etc.                  |                                                                                      |
| OpenXR                        | :heavy_check_mark: |                                                                       |                                                                                      |
| WebXR                         | :heavy_check_mark: |                                                                       |                                                                                      |
| ARKit                         | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D graphics APIs**          |                    |                                                                       |                                                                                      |
| Vulkan                        | :heavy_check_mark: |                                                                       |                                                                                      |
| OpenGL                        | :heavy_check_mark: |                                                                       |                                                                                      |
| Direct3D 12                   | :heavy_check_mark: |                                                                       |                                                                                      |
| **Import**                    |                    |                                                                       |                                                                                      |
| gltf2                         | :heavy_check_mark: |                                                                       |                                                                                      |
| fbx                           | :heavy_check_mark: |                                                                       |                                                                                      |
| blender                       | :heavy_check_mark: |                                                                       |                                                                                      |
| material                      | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D skeleton / rig             | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D animation                  | :heavy_check_mark: |                                                                       |                                                                                      |
| **Export**                    |                    |                                                                       |                                                                                      |
| gltf2                         | :heavy_check_mark: |                                                                       |                                                                                      |
| **Color**                     |                    |                                                                       |                                                                                      |
| Color Profile Managment (ICC) | :no_entry_sign:    | [proposal](https://github.com/godotengine/godot-proposals/issues/903) |                                                                                      |
| sRGB                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **Images & Textures**         |                    |                                                                       |                                                                                      |
| Anisotropic                   | :heavy_check_mark: |                                                                       |                                                                                      |
| Mipmaps                       | :heavy_check_mark: |                                                                       |                                                                                      |
| Svg                           | :heavy_check_mark: |                                                                       |                                                                                      |
| Channel Packing               | :lemon:            |                                                                       | [Zylann's Channel Packer](https://github.com/Zylann/godot_channel_packer_plugin)     |
| Generate layered Textures     | :heavy_check_mark: | Texture2DArray, Cubemap, CubemapArray                                 |                                                                                      |
| Generate Texture Atlas        | :heavy_check_mark: |                                                                       |                                                                                      |
| Texture3D                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Image compressions            | :heavy_check_mark: |                                                                       |                                                                                      |
| **Materials & Shaders**       |                    |                                                                       |                                                                                      |
| Language: based on GLSL       | :heavy_check_mark: |                                                                       |                                                                                      |
| Physically-based rendering    | :heavy_check_mark: |                                                                       |                                                                                      |
| Visual Shader Node System     | :heavy_check_mark: |                                                                       |                                                                                      |
| Decals                        | :heavy_check_mark: |                                                                       |                                                                                      |
| Volumetric Fog                | :heavy_check_mark: |                                                                       |                                                                                      |
| Reflection Probes             | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Mesh**                   |                    |                                                                       |                                                                                      |
| Shape keys / morph targes     | :heavy_check_mark: | Godot calls them "Blendshapes"                                        |                                                                                      |
| **Lighting**                  |                    |                                                                       |                                                                                      |
| Directional Light             | :heavy_check_mark: |                                                                       |                                                                                      |
| Spot Light                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Omni Light                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Area Light                    | :no_entry_sign:    |                                                                       |                                                                                      |
| Bake lighting                 | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D lights and normal maps     | :heavy_check_mark: |                                                                       |                                                                                      |
| **Audio**                     |                    |                                                                       |                                                                                      |
| 3D                            | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D                            | :heavy_check_mark: |                                                                       |                                                                                      |
| Bus Management                | :heavy_check_mark: |                                                                       |                                                                                      |
| Audio Effects                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Midi                          | :heavy_check_mark: |                                                                       |                                                                                      |
| .wav                          | :heavy_check_mark: |                                                                       |                                                                                      |
| .ogg                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Systems**                |                    |                                                                       |                                                                                      |
| Heightmap Terrain             | :lemon:            |                                                                       | [Zylann's heightmap terrain addon](https://github.com/Zylann/godot_heightmap_plugin) |
| Waterway System               | :lemon:            |                                                                       | [Arnklit's Waterways](https://github.com/Arnklit/Waterways)                          |
| Navigation                    | :jack_o_lantern:   | [Insight](../03_godot-4/navigation.md)                                | (see insight)                                                                        |
| **2D Systems**                |                    |                                                                       |                                                                                      |
| Tile map editor               | :heavy_check_mark: |                                                                       |                                                                                      |
| **Physics**                   |                    |                                                                       |                                                                                      |
| Custom Engine                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Bullet Engine                 | :lemon:            |                                                                       |                                                                                      |
| Static Body                   | :heavy_check_mark: |                                                                       |                                                                                      |
| Dynamic Body                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Soft Body                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Area                          | :heavy_check_mark: |                                                                       |                                                                                      |
| Inverse Kinematics            | :heavy_check_mark: |                                                                       |                                                                                      |
| Wobble Physics                | :heavy_check_mark: | [PR for Bullet](https://github.com/godotengine/godot/pull/64281)      |                                                                                      |
| **Camera**                    |                    |                                                                       |                                                                                      |
| View Frustum Culling          | :heavy_check_mark: |                                                                       |                                                                                      |
| Automatic Occlusion Culling   | :no_entry_sign:    |                                                                       |                                                                                      |
| Portal Occlusion Culling      | :heavy_check_mark: |                                                                       |                                                                                      |
| HLOD                          | :heavy_check_mark: |                                                                       |                                                                                      |
| Mesh                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **Screen**                    |                    |                                                                       |                                                                                      |
| Mid- and Post-processing      |                    |                                                                       |                                                                                      |
| ├─ Color Adjustment           | :heavy_check_mark: | Brightness, Saturation, Contrast, Color Correction (via Texture)      |                                                                                      |
| ├─ Ambient Light              | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Ambient Occlusion          | :heavy_check_mark: | Screen-Space Ambient Occlusion                                        |                                                                                      |
| ├─ Tonemapping                | :heavy_check_mark: | modes (Linear, Reinhardt, Filmic, ACES), exposure, whitepoint         |                                                                                      |
| ........└─ auto exposure      | :heavy_check_mark: | luminance, intensity, speed                                           |                                                                                      |
| ├─ Height Fog                 | :heavy_check_mark: | aerial perspective, density, color, brightness, sun piercing          |                                                                                      |
| ├─ Glow                       | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Boom                       | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Global Illumination        | :heavy_check_mark: | Signed Distance Field Global Illumination (SDFGI)                     |                                                                                      |
| ├─ Sky                        | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Indirect Lighting          | :heavy_check_mark: | Screen-Space Indirect Lighting                                        |                                                                                      |
| └─ Reflections                | :heavy_check_mark: | Screen-Space Reflections                                              |                                                                                      |
| **UI**                        |                    |                                                                       |                                                                                      |
| Theming                       | :heavy_check_mark: |                                                                       |                                                                                      |
| **Animation**                 |                    |                                                                       |                                                                                      |
| Vertex Animation              | :heavy_check_mark: |                                                                       |                                                                                      |
| Blendshape/Morph Animation    | :heavy_check_mark: |                                                                       |                                                                                      |
| **Particle System**           |                    |                                                                       |                                                                                      |
| Attractors                    | :heavy_check_mark: |                                                                       |                                                                                      |
| **Code**                      |                    |                                                                       |                                                                                      |
| Scripting: GDScript           | :heavy_check_mark: |                                                                       |                                                                                      |
| Language bindings             | :heavy_check_mark: | via GDExtension: C++ (17), C# (8.0), Rust, etc.                       |                                                                                      |
| **Networking**                |                    |                                                                       |                                                                                      |
| **Miscellaneous**             |                    |                                                                       |                                                                                      |
| Multithreading                | :heavy_check_mark: |                                                                       |                                                                                      |
| Octahedral Impostors          | :lemon:            |                                                                       | [wojtekpil's impostors](https://github.com/wojtekpil/Godot-Octahedral-Impostors)     |
| Extend Engine                 | :heavy_check_mark: | Godot Modules                                                         |                                                                                      |
| Performance Profiler          | :heavy_check_mark: |                                                                       |                                                                                      |
| Network Profiler              | :heavy_check_mark: |                                                                       |                                                                                      |
| Version Control               | :heavy_check_mark: |                                                                       |                                                                                      |
| 64-bit (for large world)      | :heavy_check_mark: | https://gist.github.com/Calinou/ceb6d24b16d85bc47f9c5de9c9ae341d      |                                                                                      |
| Directed Graph Dialogsystem   | :lemon:            |                                                                       | [coppolaemilio's dialogic](https://github.com/coppolaemilio/dialogic)                |
| Timer                         | :heavy_check_mark: |                                                                       |                                                                                      |
| Tweening                      | :heavy_check_mark: |                                                                       |                                                                                      |
