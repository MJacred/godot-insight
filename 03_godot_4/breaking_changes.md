# Changes to Godot 3: Vanished Nodes and Classes

* [ClippedCamera](https://github.com/godotengine/godot/pull/53354)
* [InterpolatedCamera](https://github.com/godotengine/godot/pull/42113)
  * lives on as an [add-on](https://github.com/godot-extended-libraries/godot-interpolated-camera3d)
* [QuadMesh](https://github.com/godotengine/godot/pull/64801) was merged with PlaneMesh
  * PlaneMesh has skewed planes in the viewport when Billboard particle mode turned on in the materials
    * [Fix](https://github.com/godotengine/godot/issues/65186): set the orientation property of the PlaneMesh to FACE_Z then it will behave the same as QuadMesh did
    * Other issues with the texture are probably due to an Octahedral compression issue. To fix it, reimport your assets.
* [ProxyTexture](https://github.com/godotengine/godot/pull/64595) became unexposed
  * Was only used as a hack. It steadily loses its purpose and will be removed soon.
* [ProximityGroup](https://docs.godotengine.org/en/stable/classes/class_proximitygroup.html)
