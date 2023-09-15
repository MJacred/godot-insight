# What is Godot 4.0?

Much cose was rewritten in 4.0, not just rendering:
- Core Engine (including porting to C++17)
- Physics
  - GodotPhysics, which experienced a revival, will be replaced with [Jolt](https://github.com/godot-jolt/godot-jolt).
- GDScript
- Native Extensions
- Asset Workflow
- Networking
- 3D Editor
- Tile Editor
- and many more

To aid the migration from Godot 3 to Godot 4, a [converter](https://github.com/godotengine/godot/pull/51950) was written. And it was written about if you should upgrade and how: https://docs.godotengine.org/en/stable/tutorials/migrating/upgrading_to_godot_4.html  
If you depend on looping audio files, where the loop offest is `> 0` seconds, then I currently cannot recommend Godot 4, see [here](https://github.com/godotengine/godot/issues/64775). While a [PR](https://github.com/godotengine/godot/pull/80452) is in the making, this might take a while (starting 2024 at the earliest, possibly middle 2024).


# Changes to Godot 3: Vanished Nodes and Classes

* [ClippedCamera](https://github.com/godotengine/godot/pull/53354)
* [InterpolatedCamera](https://github.com/godotengine/godot/pull/42113)
  * lives on as an [add-on](https://github.com/godot-extended-libraries/godot-interpolated-camera3d)
* [QuadMesh](https://github.com/godotengine/godot/pull/64801) was merged with PlaneMesh
  * PlaneMesh has skewed planes in the viewport when Billboard particle mode turned on in the materials
    * [Fix](https://github.com/godotengine/godot/issues/65186): set the orientation property of the PlaneMesh to FACE_Z then it will behave the same as QuadMesh did
    * Other issues with the texture are probably due to an Octahedral compression issue. To fix it, reimport your assets.
* [ProxyTexture](https://github.com/godotengine/godot/pull/73492).
* [ProximityGroup](https://docs.godotengine.org/en/stable/classes/class_proximitygroup.html)
