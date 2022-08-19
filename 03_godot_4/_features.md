
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
* Hierarchies displayed below do not necessarily reflect 100% actual class inheritance. In some cases, like Mid- and Post-processing, it does not represent classes :bookmark:, but rather components :package:.
* :ghost: deprecated / will be deleted


| Feature                        | Implemented        | Reference (more info)                                                 | Workaround / Alternative                                                             |
| ------------------------------ |:------------------:| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------|
| **Internationalization**       |                    |                                                                       |                                                                                      |
| gettext                        | :heavy_check_mark: |                                                                       |                                                                                      |
| **Fonts**                      |                    |                                                                       |                                                                                      |
| .ttf                           | :heavy_check_mark: |                                                                       |                                                                                      |
| .woff2                         | :heavy_check_mark: |                                                                       |                                                                                      |
| text in left-to-right          | :heavy_check_mark: |                                                                       |                                                                                      |
| text in right-to-left          | :heavy_check_mark: |                                                                       |                                                                                      |
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
| Vulkan                         | :heavy_check_mark: |                                                                       |                                                                                      |
| OpenGL                         | :heavy_check_mark: |                                                                       |                                                                                      |
| Direct3D 12                    | :heavy_check_mark: |                                                                       |                                                                                      |
| **Import**                     |                    |                                                                       |                                                                                      |
| gltf2                          | :heavy_check_mark: |                                                                       |                                                                                      |
| fbx                            | :heavy_check_mark: |                                                                       |                                                                                      |
| blender                        | :heavy_check_mark: |                                                                       |                                                                                      |
| material                       | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D skeleton / rig              | :heavy_check_mark: |                                                                       |                                                                                      |
| 3D animation                   | :heavy_check_mark: |                                                                       |                                                                                      |
| **Export**                     |                    |                                                                       |                                                                                      |
| gltf2                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **Color**                      |                    |                                                                       |                                                                                      |
| Color Profile Managment (ICC)  | :no_entry_sign:    | [proposal](https://github.com/godotengine/godot-proposals/issues/903) |                                                                                      |
| sRGB                           | :heavy_check_mark: |                                                                       |                                                                                      |
| **Images & Textures**          |                    |                                                                       |                                                                                      |
| Anisotropic                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Mipmaps                        | :heavy_check_mark: |                                                                       |                                                                                      |
| Svg                            | :heavy_check_mark: |                                                                       |                                                                                      |
| Channel Packing                | :lemon:            |                                                                       | [Zylann's Channel Packer](https://github.com/Zylann/godot_channel_packer_plugin)     |
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
| ├─ ProxyTexture                | :ghost:            | [Insight](../01_internal_classes/textures/proxytexture.md)            |                                                                                      |
| └─ ViewportTexture             | :heavy_check_mark: |                                                                       |                                                                                      |
| **Materials & Shaders**        |                    |                                                                       |                                                                                      |
| Language: based on GLSL        | :heavy_check_mark: |                                                                       |                                                                                      |
| Physically-based rendering     | :heavy_check_mark: |                                                                       |                                                                                      |
| Visual Shader Node System      | :heavy_check_mark: |                                                                       |                                                                                      |
| Decals                         | :heavy_check_mark: |                                                                       |                                                                                      |
| Volumetric Fog                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Reflection Probes              | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Mesh**                    |                    |                                                                       |                                                                                      |
| Shape keys / Blend Shapes      | :heavy_check_mark: | [Insight](../05_features_in_detail/blend_shapes.md)                   |                                                                                      |
| **Lighting**                   |                    |                                                                       |                                                                                      |
| Directional Light              | :heavy_check_mark: |                                                                       |                                                                                      |
| Spot Light                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Omni Light                     | :heavy_check_mark: |                                                                       |                                                                                      |
| Area Light                     | :no_entry_sign:    |                                                                       |                                                                                      |
| Bake lighting                  | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D lights and normal maps      | :heavy_check_mark: |                                                                       |                                                                                      |
| **Audio**                      |                    |                                                                       |                                                                                      |
| 3D                             | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D                             | :heavy_check_mark: |                                                                       |                                                                                      |
| Bus Management                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Audio Effects                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Midi                           | :heavy_check_mark: |                                                                       |                                                                                      |
| .wav                           | :heavy_check_mark: |                                                                       |                                                                                      |
| .ogg                           | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Systems**                 |                    |                                                                       |                                                                                      |
| Heightmap Terrain              | :lemon:            |                                                                       | [Zylann's heightmap terrain addon](https://github.com/Zylann/godot_heightmap_plugin) |
| Waterway System                | :lemon:            |                                                                       | [Arnklit's Waterways](https://github.com/Arnklit/Waterways)                          |
| Navigation                     | :jack_o_lantern:   | [Insight](../04_systems/navigation.md)                                | (see insight)                                                                        |
| **2D Systems**                 |                    |                                                                       |                                                                                      |
| Tile map editor                | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Physics**                 |                    |                                                                       |                                                                                      |
| Custom Engine                  | :heavy_check_mark: |                                                                       |                                                                                      |
| Bullet Engine                  | :lemon:            |                                                                       |                                                                                      |
| Static Body                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Dynamic Bodies                 | :bookmark:         |                                                                       |                                                                                      |
| ├─ Kinematic Body              | :heavy_check_mark: | called CharacterBody3D in Godot                                       |                                                                                      |
| └─ Ridid Body                  | :heavy_check_mark: | called RigidDynamicBody3D in Godot                                    |                                                                                      |
| PhysicalBone3D                 | :heavy_check_mark: |                                                                       |                                                                                      |
| SoftDynamicBody3D              | :heavy_check_mark: |                                                                       |                                                                                      |
| Area3D                         | :heavy_check_mark: |                                                                       |                                                                                      |
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
| ├─ HingeJoint3D                | :heavy_check_mark: |                                                                       |                                                                                      |
| └─ PinJoint2D                  | :heavy_check_mark: |                                                                       |                                                                                      |
| **Camera**                     |                    |                                                                       |                                                                                      |
| View Frustum Culling           | :heavy_check_mark: |                                                                       |                                                                                      |
| Automatic Occlusion Culling    | :no_entry_sign:    |                                                                       |                                                                                      |
| Portal Occlusion Culling       | :heavy_check_mark: |                                                                       |                                                                                      |
| HLOD                           | :heavy_check_mark: |                                                                       |                                                                                      |
| Mesh                           | :heavy_check_mark: |                                                                       |                                                                                      |
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
| ├─ Global Illumination         | :heavy_check_mark: | Signed Distance Field Global Illumination (SDFGI)                     |                                                                                      |
| ├─ Sky                         | :heavy_check_mark: |                                                                       |                                                                                      |
| ├─ Indirect Lighting           | :heavy_check_mark: | Screen-Space Indirect Lighting                                        |                                                                                      |
| └─ Reflections                 | :heavy_check_mark: | Screen-Space Reflections                                              |                                                                                      |
| **UI**                         |                    |                                                                       |                                                                                      |
| Theming                        | :heavy_check_mark: |                                                                       |                                                                                      |
| **Animation**                  |                    |                                                                       |                                                                                      |
| Vertex Animation               | :heavy_check_mark: |                                                                       |                                                                                      |
| Blend shape Animation          | :heavy_check_mark: |                                                                       |                                                                                      |
| **Particle System**            |                    | [Insight](../04_systems/particles.md)                                 |                                                                                      |
| Attractors                     | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| Manual emission                | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/41810)                 |                                                                                      |
| Subemitters                    | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/41810)                 |                                                                                      |
| Collision                      | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| Baked SDF Collision            | :heavy_check_mark: | [PR](https://github.com/godotengine/godot/pull/42628)                 |                                                                                      |
| **Code**                       |                    |                                                                       |                                                                                      |
| Scripting: GDScript            | :heavy_check_mark: |                                                                       |                                                                                      |
| Language bindings              | :heavy_check_mark: | via GDExtension: C++ (17), C# (8.0), Rust, etc.                       |                                                                                      |
| **Networking**                 |                    |                                                                       |                                                                                      |
| **Miscellaneous**              |                    |                                                                       |                                                                                      |
| Multithreading                 | :heavy_check_mark: |                                                                       |                                                                                      |
| Octahedral Impostors           | :lemon:            |                                                                       | [wojtekpil's impostors](https://github.com/wojtekpil/Godot-Octahedral-Impostors)     |
| Extend Engine                  | :heavy_check_mark: | Godot Modules                                                         |                                                                                      |
| Performance Profiler           | :heavy_check_mark: |                                                                       |                                                                                      |
| Network Profiler               | :heavy_check_mark: |                                                                       |                                                                                      |
| Version Control                | :heavy_check_mark: |                                                                       |                                                                                      |
| 64-bit (for large world)       | :heavy_check_mark: | https://gist.github.com/Calinou/ceb6d24b16d85bc47f9c5de9c9ae341d      |                                                                                      |
| Directed Graph Dialogsystem    | :lemon:            |                                                                       | [coppolaemilio's dialogic](https://github.com/coppolaemilio/dialogic)                |
| Timer                          | :heavy_check_mark: |                                                                       |                                                                                      |
| Tweening                       | :heavy_check_mark: |                                                                       |                                                                                      |
