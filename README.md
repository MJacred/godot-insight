# Intro & Motivation

This repository contains informations about the [Godot Game Engine](https://github.com/godotengine/godot).

Sometimes, there's info on the Engine that currently has no place on the official github repositories or the web page, e.g. documentation on unexposed classes or too technical information.

My personal interest lies in 3D, so that will be my main focus. If you want to share about 2D, please see the CONTRIBUTING guidelines and open an issue or pull request!


# Disclaimer

The information in this reposotory may be partially or completely incorrect. Some info is from hearing, vague memory or reading and misunderstanding code. In case you find a mistake, you can open an Issue and state corrections (with source that backs your claim).


# Godot 4 - Engine Feature Overview

**NOTE: this list is still very much work in progress**

Legend
* :heavy_check_mark: - yes
* :no_entry_sign: - no
* :lemon: - yes, but but as addon
  * listed in the **Workaround** column
  * lists the most well-known one
* :jack_o_lantern: - yes, but…
  * feature might have serious bugs or lack of certain cases (i.e. sub features missing), depending on your concrete needs


| Feature                       | Implemented        | Reference (more info)                                                 | Workaround / Alternative                                                             |
| ----------------------------- |:------------------:| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------|
| **Internationalization**      |                    |                                                                       |                                                                                      |
| gettext                       | :heavy_check_mark: |                                                                       |                                                                                      |
| text in left-to-right         | :heavy_check_mark: |                                                                       |                                                                                      |
| text in right-to-left         | :heavy_check_mark: |                                                                       |                                                                                      |
| **Supported Platforms**       |                    |                                                                       |                                                                                      |
| Desktop                       | :heavy_check_mark: | Windows, macOS, Linux, UWP, and *BSD                                  |                                                                                      |
| Consoles                      | :heavy_check_mark: | [Insight](99_export/consoles.md)                                      |                                                                                      |
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
| **Export**                    |                    |                                                                       |                                                                                      |
| **Color**                     |                    |                                                                       |                                                                                      |
| Color Profile Managment (ICC) | :no_entry_sign:    | [proposal](https://github.com/godotengine/godot-proposals/issues/903) |                                                                                      |
| sRGB                          | :heavy_check_mark: |                                                                       |                                                                                      |
| **Images & Textures**         |                    |                                                                       |                                                                                      |
| Anisotropic                   | :heavy_check_mark: |                                                                       |                                                                                      |
| Mipmaps                       | :heavy_check_mark: |                                                                       |                                                                                      |
| Svg                           | :heavy_check_mark: |                                                                       |                                                                                      |
| Channel Packing               | :lemon:            |                                                                       | [Zylann's Channel Packer](https://github.com/Zylann/godot_channel_packer_plugin)     |
| Generate layered Textures     | :heavy_check_mark: |                                                                       |                                                                                      |
| Generate Texture Atlas        | :heavy_check_mark: |                                                                       |                                                                                      |
| **Shaders**                   |                    |                                                                       |                                                                                      |
| Language: based on GLSL       | :heavy_check_mark: |                                                                       |                                                                                      |
| Physically-based rendering    | :heavy_check_mark: |                                                                       |                                                                                      |
| Visual Shader Node System     | :heavy_check_mark: |                                                                       |                                                                                      |
| **Lighting**                  |                    |                                                                       |                                                                                      |
| Directional Light             | :heavy_check_mark: |                                                                       |                                                                                      |
| Spot Light                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Omni Light                    | :heavy_check_mark: |                                                                       |                                                                                      |
| Area Light                    | :no_entry_sign:    |                                                                       |                                                                                      |
| Global Illumination           | :heavy_check_mark: |                                                                       |                                                                                      |
| 2D lights and normal maps     | :heavy_check_mark: |                                                                       |                                                                                      |
| **3D Systems**                |                    |                                                                       |                                                                                      |
| Heightmap Terrain             | :lemon:            |                                                                       | [Zylann's heightmap terrain addon](https://github.com/Zylann/godot_heightmap_plugin) |
| Waterway System               | :lemon:            |                                                                       | [Arnklit's Waterways](https://github.com/Arnklit/Waterways)                          |
| Navigation                    | :jack_o_lantern:   | [Insight](03_godot-4/navigation.md)                                   | (see insight)                                                                        |
| **2D Systems**                |                    |                                                                       |                                                                                      |
| Tile map editor               | :heavy_check_mark: |                                                                       |                                                                                      |
| **Physics**                   |                    |                                                                       |                                                                                      |
| **Screen**                    |                    |                                                                       |                                                                                      |
| Mid- and Post-processing      | :heavy_check_mark: |                                                                       |                                                                                      |
| **UI**                        |                    |                                                                       |                                                                                      |
| Theming                       | :heavy_check_mark: |                                                                       |                                                                                      |
| **Animation**                 |                    |                                                                       |                                                                                      |
| Inverse Kinematics            | :heavy_check_mark: |                                                                       |                                                                                      |
| **Code**                      |                    |                                                                       |                                                                                      |
| Scripting: GDScript           | :heavy_check_mark: |                                                                       |                                                                                      |
| Language bindings             | :heavy_check_mark: | via GDExtension: C++ (17), C# (8.0), Rust, etc.                       |                                                                                      |
| **Miscellaneous**             |                    |                                                                       |                                                                                      |
| Extend Engine                 | :heavy_check_mark: | Godot Modules                                                         |                                                                                      |
| Performance Profiler          | :heavy_check_mark: |                                                                       |                                                                                      |
| Network Profiler              | :heavy_check_mark: |                                                                       |                                                                                      |
| Version Control               | :heavy_check_mark: |                                                                       |                                                                                      |


# Awesome Resources

* https://github.com/godotengine/awesome-godot 
* https://project-awesome.org/godotengine/awesome-godot
* https://github.com/hto/awesome-godot
* https://godotshaders.com/


# Games and Software made with Godot

https://godotengine.org/showcase

