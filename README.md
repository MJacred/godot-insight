# Intro & Motivation

This repository contains informations about the [Godot Game Engine](https://github.com/godotengine/godot).

Sometimes, there's info on the Engine that currently has no place on the official github repositories or the web page, e.g. documentation on unexposed classes or information that's too technical. Consequently, this is no replacement for Godot's official documentation, but more of an addition that may or may not interest new or most Godot users.

My personal interest lies in 3D, so that will be my main focus. If you want to share about 2D, please see the CONTRIBUTING guidelines and open an issue or pull request!


# Disclaimer

The information in this reposotory aims to be as accurate as possible, therefore a source is usually added. In case you find a mistake, you can open an Issue and state corrections (with source that supports your correction).


# Who is this for?

* people interested in the technical implementation details of
  * [internal classes](01_internal_classes/)
  * [exposed classes](02_classes/)
* those who want to learn about Godot's various [systems](04_systems/) and [features](03_godot_4/00_features.md) in as much [detail](05_features_in_detail/) as possible
  * also listing features Godot does not have, but other Engines, or features added via addons (which is often desired, as Godot tries to stay small in code size)
* everyday users interested in
  * background info and references for most common features in [2D](07_techniques_2d/) and [3D](06_techniques_3d/) users implement all the time, e.g. a 3D Third Person Character Controller, moving platforms, etc.
  * how to [structure](08_project_architecture/) your project
  * [importing](09_importing/) various files
  * improving [performance](10_performance/)
  * finishing and [exporting](99_export/) a game

Information that is polished and fits into an official Godot space (API documentation, tutorial, etc.), will be contributed to said official space.


# Godot 4

Initially, Godot 4 was only about a new renderer (Vulkan), but feedback made clear that a fundamental change was in order ([source](https://www.reddit.com/r/godot/comments/on0hzn/comment/h5pgoi5/?utm_source=reddit&utm_medium=web2x&context=3)). Therefore, Godot 4 is mostly a complete code/system restructure, to allow more modern features to be implemented more easily and consequently more quickly.  
Fundamental changes always break compatibility with a previous version. So, identifying desired new features (and changes of existing ones) helps with deciding on the target architecture. Thus, adding as many needed compatibility breaking changes in one fell swoop is strongly desired. And because changing the code structure always takes a long time, new features find their way in. Which in turn makes Godot 4 also more feature rich.  
To make things easier for existing projects, there's a converter tool in the making, helping developers to migrate their projects from Godot 3 to 4. More info on this [here](https://docs.godotengine.org/en/stable/tutorials/migrating/upgrading_to_godot_4.html).  

Read about Godot 4 Features [here](03_godot_4/00_features.md).


# Awesome Resources

**Starter Pack**
* Features: https://docs.godotengine.org/en/stable/about/list_of_features.html
* Features 2: https://github.com/MJacred/godot-insight/blob/main/03_godot_4/00_features.md
* Custom Godot Build: https://github.com/MJacred/godot-insight/blob/main/03_godot_4/01_soon_features.md
* Tutorials: https://docs.godotengine.org/en/latest/community/tutorials.html
* Communities: https://godotengine.org/community/
* Plugins: https://godotengine.org/asset-library/asset

Other
* https://github.com/godotengine/awesome-godot 
* https://project-awesome.org/godotengine/awesome-godot
* https://github.com/hto/awesome-godot
* https://godotshaders.com/


# Games and Software made with Godot

* https://godotengine.org/showcase
* "Made In Godot" by StayAtHomeDev on [YouTube](https://www.youtube.com/watch?v=ZrtK_3PfLsU&list=PLEHvj4yeNfeHArSU6U2a715ssJYYCnKCg).
