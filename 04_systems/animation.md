# General

## Intro

https://docs.godotengine.org/en/stable/tutorials/animation/introduction.html


## Direction

https://github.com/godotengine/godot-proposals/issues/6198
* Blender has +Y as FORWARD, glTF changes this to +Z and Godot expects -Z
  * Fix to change orientation on import: https://github.com/godotengine/godot/pull/72753


## Recording

* https://github.com/godotengine/godot/pull/77395


## RESET tracks

https://docs.godotengine.org/en/stable/tutorials/animation/introduction.html#using-reset-tracks

Bugs
* [ ] https://github.com/godotengine/godot/issues/56387


# Features - 3D


## Root Motion

* Intro: https://www.youtube.com/watch?v=j7XZ3Q8JNfM
* Docs: https://docs.godotengine.org/en/stable/tutorials/animation/animation_tree.html#root-motion
* Godot:
  * https://www.youtube.com/watch?v=fq0hR2tIsRk
  * https://www.youtube.com/watch?v=3Hk9ljcS1Ro


## Additive / Layered Animation

It's important to undestand **rest** (pose) first:
* [Docs](https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/escn_exporter/skeleton.html#rest-bone)



> The mainly need for Add2 is […] for inertial transitions such as adding a knockback animation to the original animation, or swinging the arm up when switching weapons, which would be unnatural with Blend2.
> ([source](https://github.com/godotengine/godot/pull/76310#issuecomment-1518479340))


> If you want to use NodeAdd2 to blend animation, the difference from rest [pose] will be added unless the animation of the "in" port is rest [pose].  
> This can make the animation too exaggerated, so add a NodeSub2 to cancel the extra pose in advance and extract the delta animation.  
> ([source](https://github.com/godotengine/godot/pull/76616))


AnimationNodeAdd2 + [AnimationNodeSub2](https://github.com/godotengine/godot/pull/76616)
```
// A: Base Animation
// B: Second Animation on top
// C: Reference Pose to fix exaggerations (usually equals A)
A + (B - C)
```


![Additive vs Blend](https://user-images.githubusercontent.com/61938263/235358998-9fca1c28-84fd-44aa-b80b-dd775c304697.png)


Reading resources
* https://github.com/godotengine/godot-proposals/issues/2700
* https://github.com/godotengine/godot/pull/76310


**Theres is no AnimationNodeSub2 in Godot 3, why?**  
> […] in 3.x, it should not be necessary, at least in the case of importing a model with rest [pose].  
> ([source](https://github.com/godotengine/godot/pull/76310#issuecomment-1517869077))

```
[Godot 3]
Bone Pose Initial Value = Transform(Quat(0,0,0,1), Vector3(0,0,0))
Visible Bone Pose = BoneRest * BonePose

[Godot 4]
Bone Pose Initial Value = BoneRest 
Visible Bone Pose = BonePose
```


> […] in Godot 3, if the skeleton bones have data that can be read as BoneRests, Godot 3 glTF importer subtracts the rests from the animation data and imports the processed values as BonePoses.
> [This means] all poses value are relative to the default pose (e.g. T Pose). ([source](https://github.com/godotengine/godot-proposals/issues/2700#issuecomment-837325712))

```
[Godot 3]
Godot BonePose = glTF BoneRest^(-1) * glTF AnimationData

[Godot 4]
Godot BonePose = glTF AnimationData
```

> This means that Godot 3.x glTF importer has already done the work that Sub2 should have done: the Bone Pose […] is already the result of the Rest [Pose] subtracted  
> [And] Godot 4 does not consider Rest [Pose] on import, but does consider Rest on blending process.


Note that exports from Maya `do not have a rest (they have a T-pose separate from the rest)` so it would need a Sub2 operation. ([source](https://github.com/godotengine/godot/pull/76310#issuecomment-1517869077))  
Sub2 can only come to Godot 3 after it arrived in Godot 4 ([source](https://github.com/godotengine/godot/pull/76310#issuecomment-1520392801)).


## Blending Animation

> Quaternion blending, and I have avoided the problem of slerp() with initial Quaternion by referring to the Reset track or bone_rest. In character models, it is almost rare to animate a joint more than 180 degrees, so this implementation is generally fine.  
> The problem may occur when the object is rotated more than 180 degrees, but I am not sure if blending should be used in such a case.
> ([source](https://github.com/godotengine/godot/pull/57675#issuecomment-1030709158))

> propeller rotation should be controlled by scripts rather than animations, and for cases like turrets that always face the target, I think IK or any script should be used instead of BlendSpace2D.
> ([source](https://github.com/godotengine/godot/pull/57675#issuecomment-1030749214))


Bugs
* https://github.com/godotengine/godot/issues/73537
  * [x] [AnimationPlayer blending and AnimationTree blending are inconsistent](https://github.com/godotengine/godot/issues/70207), fixed in [Godot 4.2](https://github.com/godotengine/godot/pull/80813)
  * [ ] [AnimationPlayer can't queue the same animations](https://github.com/godotengine/godot/issues/72484)


## Animation Retargeting

* [Proposal](https://github.com/godotengine/godot-proposals/issues/4510)
* [PR](https://github.com/godotengine/godot/pull/63854)
