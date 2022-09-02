# NavigationServer

> Godot uses [recastnavigation](https://github.com/recastnavigation/recastnavigation)'s **Recast** for creating the NavigationMesh.
> But Godot uses RVO2 for the **Detour**.

TODO: https://github.com/godotengine/godot-proposals/labels/topic%3Anavigation


## Features

### Obstacles

* These are actually agents, that just don't move. Therefore, they only have a height and radius. [source](https://github.com/godotengine/godot/blob/master/scene/3d/navigation_obstacle_3d.cpp)


### Baking

control
* Bake Area: https://github.com/godotengine/godot/pull/62348
  * cherry-picked Godot 3.5
* Bake on import: `ImporterMesh::create_navigation_mesh()` ([source](https://github.com/godotengine/godot/blob/master/scene/resources/importer_mesh.cpp)), your mesh must have `-navmesh` suffix and the original mesh is discarded


types
* heightmap and GridMap: https://github.com/godotengine/godot/pull/63932
  * cherry-picked Godot 3.6


### Navigation (jump) links

> Navigation links allow you to inform the navigation system about routes that don't involve traveling the surface of the navigation mesh. This can include things like jumping off surfaces, teleporters, rappelling points, or other game specific activities.  
> ([source](https://github.com/godotengine/godot/pull/63479))


**NOTE**: There's no signal when you enter the navigation link, therefore you need to build your own system. Usually with Areas ([example](https://www.youtube.com/watch?v=xeXULKMhAX4))


## Problems and missing Features that might trouble you

### Pathfinding ignoring Navigation Cost

> Current Godot pathfinding without a custom server / module is hardcoded to AStar so you will not get good results with cost polygons and regions.  
> AStar is a notoriously wrong algorithm for a cost based path search as it tends to miss obvious shortest paths by searching to strong in the often wrong direction directly to the target and returning with the first path it finds even if it is a very bad path.  
> ([source](https://www.reddit.com/r/godot/comments/x10o2m/comment/imcgsm1/?utm_source=reddit&utm_medium=web2x&context=3))


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


## Performance

wip
* limit pathfinds per physics tick: https://github.com/godotengine/godot/pull/62745


# Alternative to NavigationServer

https://github.com/TheSHEEEP/godotdetour

but
* only works for Godot 3.x
* no editor integration, do all by code
* NavigationMesh generation is currently limited to one [Mesh] (see [here](https://github.com/TheSHEEEP/godotdetour/issues/5))

possible usage in editor
* create NavigationMesh using NavigationServer, export as Mesh, feed into godotdetour
