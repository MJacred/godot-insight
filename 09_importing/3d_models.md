# Blender to Godot

## Meshes

examples
* https://models.spriters-resource.com/


## Baking

Addons
* Bystedts Blender Baker
* xNormal

## Rigging

learned blender rigging from outdated videos? here is the bridge: https://www.youtube.com/watch?v=DkhW50q0uAg

tips
* helper add-ons on problems: https://www.youtube.com/watch?v=FTTNXtPi0hY
  * Action Constraint Builder, Animation Snapper Pro (fixing accidental feet floatings or body parts moving away from each other), Vertex Skin Weights (really good for fixing eyelids), Delta Shapekeys, VeeDynamics
* [fixing bad deforms](https://www.youtube.com/watch?v=n_waiAcd8xY)

rigging learning path
https://www.youtube.com/watch?v=w8J_GnBYE8o&list=PLdcL5aF8ZcJsSWrFwmLsQvCIKisuyyMnU
should contain: [face rigging](https://www.youtube.com/watch?v=gsS0qRztOPg)
more: https://www.youtube.com/watch?v=LJLkgSmmNFs

https://superhivemarket.com/products/voxel-heat-diffuse-skinning

Auto-Rig Pro
* [General tips](https://www.youtube.com/watch?v=PDRAH75ci7k)
* [Face](https://www.youtube.com/watch?v=hxWxhNNp1r4)

teacher 1:
* https://www.youtube.com/watch?v=sfVFdQC9_O4&list=PL1XwjHBOypY-bnvwbv-8vkeAeDUZtUm_x

teacher 2:
https://www.youtube.com/watch?v=2GQwAM5Wodk&list=PLdcL5aF8ZcJvCyqWeCBYVGKbQgrQngen3

* https://www.youtube.com/watch?v=iZBx1I7vmQ0
  * https://toshicg.gumroad.com/l/game_rig_tools

alternative add-on: https://github.com/vini-guerrero/Godot_Game_Tools


### Q&A

**Q**: auto generate the Vertex Groups for each bone AFTER parenting the armature? It's kinda tedious to type out all those bone names  
**A**: CTRL+P > Armature Deform > With Empty Groups even after it's already parented and it will generate vertex groups for all bones WITHOUT erasing weights on existing groups

## Animation

tips and tricks: https://www.youtube.com/watch?v=fV8xIP480qk&list=PLgKCjZ2WsVLQ3AFFCkbSXKhsvH1Z7WFXn&index=2

* https://www.youtube.com/watch?v=DeJoc4iCWQo

## Export

https://toshicg.gumroad.com/l/game_rig_tools?a=477128051
post-generation scripts: https://www.youtube.com/watch?v=raC9aWEsEoE
tutorial: https://www.youtube.com/watch?v=iZBx1I7vmQ0

what do we need
* _only_ **deform bones**: control bones need to be removed
* bones that are _not_ squashed or stretched
