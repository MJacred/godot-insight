# NavigationServer

> Godot uses [recastnavigation](https://github.com/recastnavigation/recastnavigation)'s **Recast** for creating the NavigationMesh.
> But Godot uses RVO2 for the **Detour**.


## Missing Features that might trouble you


### Cannot mark areas on NavigationRegion(s) as water, ground, etc.

[NavigationRegion3D] has a cost for entering and travelling within.  
And in this Node, you have your NavigationMesh, which is constructed automatically using Meshes in your scene - i.e. there's a certain lack of control.

In some cases you might want to have perfect control over certain Areas, which have special travelling costs.

Godot's NavigationServer does not support this (see [proposal](https://github.com/godotengine/godot-proposals/issues/5116))

[recastnavigation] has this feature out-of-the-box: https://github.com/recastnavigation/recastnavigation/pull/260


### Select an obstacle or an agent as a target

**sources**:
* https://github.com/godotengine/godot-proposals/issues/3812
* https://github.com/godotengine/godot-proposals/issues/4522


**Workaround**:  
But this can be worked around using custom code updating the target position and [NavigationAgent2D/3D.set_path_desired_distance()](https://github.com/godotengine/godot/pull/62181)


### Agents can exit the navigation mesh

**source**: https://github.com/godotengine/godot-proposals/issues/1966


**Workaround**:  
Hinted possible workaround: https://github.com/godotengine/godot-proposals/issues/1966#issuecomment-1014019912


# Alternative to NavigationServer

https://github.com/TheSHEEEP/godotdetour

but
* only works for Godot 3.x
* no editor integration, do all by code
* NavigationMesh generation is currently limited to one [Mesh] (see [here](https://github.com/TheSHEEEP/godotdetour/issues/5))

possible usage in editor
* create NavigationMesh using NavigationServer, export as Mesh, feed into godotdetour
