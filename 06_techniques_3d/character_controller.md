# Miscellaneous

Background Info
* With Bullet physics, the center of mass is the RigidBody3D center. With GodotPhysics, the center of mass is the average of the [CollisionShape3D] centers.
* https://www.youtube.com/watch?v=Q_miIugqx3Q

Helpers
* unstuck your character: https://github.com/godotengine/godot/pull/64195
  * further improved with: https://github.com/godotengine/godot/pull/64343

Examples
* https://github.com/fabriceci/move_and_slide_4.0

Related Addons
* https://github.com/godot-extended-libraries/godot-interpolated-camera3d


# Camera

* first person: TODO
* third person: TODO


# Ledge

* Ledge hanging: https://www.youtube.com/watch?v=ScRydHpovzI
* Ledge moving: …


# Climbing

* https://www.youtube.com/watch?v=d_o5aCzxpuA


# Ladders

* https://www.youtube.com/watch?v=xeXULKMhAX4


# Footsteps

* reading material: https://www.youtube.com/watch?v=WhMfocT9l-o


# Swimming

* buoyancy: TODO


# Climbing Stairs - Shifty's Manifesto

If your character needs to be able to climb stairs, a capsule collider won't cut it [source](https://github.com/godotengine/godot-proposals/issues/2751). A cylinder is preferred. See [Shifty's Godot Character Movement Manifesto](character_controller_by_shifty.md), who describes a solution for Godot 3.  
See implementation example for Godot 3.5: https://github.com/mrezai/GodotStairs

In Godot 4, in theory, use
* RigidBody3D
    * `set_lock_rotation_enabled(true)` OR
        * sets body mode to PhysicsServer3D::BODY_MODE_DYNAMIC_LINEAR
    * `set_freeze_enabled(true)` + `set_freeze_mode(FreezeMode.FREEZE_MODE_KINEMATIC)`
        * but ignores `lock_rotation`
* then call and use custom integrator
    * `set_use_custom_integrator(true)`


# Issues

Issues Trackers
* [Discussion of CharacterBody behavior](https://github.com/godotengine/godot/issues/50732)
* [[TRACKER] Godot Physics 3D issues](https://github.com/godotengine/godot/issues/45333)

Issues
* see list in [moving platforms](moving_platforms.md)
* [KinematicBody3D penetrate each other](https://github.com/godotengine/godot-proposals/issues/2332)
