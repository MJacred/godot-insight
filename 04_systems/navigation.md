# NavigationServer

> Godot uses [recastnavigation](https://github.com/recastnavigation/recastnavigation)'s **Recast** for creating the NavigationMesh.
> But Godot uses RVO2 for the **Detour**.

TODO: https://github.com/godotengine/godot-proposals/labels/topic%3Anavigation


## Features

## Avoidance

Handle avoidance correctly
* https://github.com/godotengine/godot-proposals/issues/5013#issuecomment-1199982413


### Layers, Masks and Priority


Like visuals and pyhsics, avoidance has Layers and Masks as well. So if two (or more) groups are on collision course (provided their layers and masks match), they'll avoid each other. The ones with a higher priority won't avoid, though. The other ones will.  
([source](https://github.com/godotengine/godot/pull/69988))


### Obstacles

> [NavigationObstacle2D/3D] Affect the avoidance of agents by changing their velocity. They don't block paths as they don't change the path.


* "obstacles can be defined by vertices to create a hard do-not-cross obstacle edge. The vertices winding order decides if the agents are pushed out or in by the obstacle." ([source](https://github.com/godotengine/godot/pull/69988))
  * are still agents
  * but now you can define vertices, they are used instead of the radius
    * if the vertices are in clockwise order, the obstacle changes the incoming agent's velocity _towards_ itself. Otherwise it pushes their velocity away.
* before 4.1: These are actually agents, that just don't move. Therefore, they only have a height and radius. [source](https://github.com/godotengine/godot/blob/master/scene/3d/navigation_obstacle_3d.cpp)

dangers: agent might get stuck permanently ([source: Known Issues / Performance](https://github.com/godotengine/godot/pull/69988))
* "when [an agent is] surrounded by avoidance constrains", their path is reset.
* "target position is occupied by another avoidance agent or obstacle that the pathfinding is not aware of".
* "when navigation mesh edges and avoidance obstacle edges overlap": "create[s] situations with conflicting velocity interests".


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


### Select an obstacle or an agent as a target

> since Godot 4.1 [PR](https://github.com/godotengine/godot/pull/69988)


**Workaround for earlier versions**:  
Can be worked around using custom code to update the target position and [NavigationAgent2D/3D.set_path_desired_distance()](https://github.com/godotengine/godot/pull/62181)


## Troubleshooting for older Godot versions

before 4.1
* _Agents can exit the navigation mesh_
  * source: https://github.com/godotengine/godot-proposals/issues/1966
  * Hinted possible workaround: https://github.com/godotengine/godot-proposals/issues/1966#issuecomment-1014019912
* _Agents on different floors avoid each other_: Because the UP-axis was ignored until now, Agents mistakenly thought they were all on the same "floor" and therefore collide


## Problems and missing Features that might trouble you

### Pathfinding ignoring Navigation Cost

> Current Godot pathfinding without a custom server / module is hardcoded to AStar so you will not get good results with cost polygons and regions.  
> AStar is a notoriously wrong algorithm for a cost based path search as it tends to miss obvious shortest paths by searching to strong in the often wrong direction directly to the target and returning with the first path it finds even if it is a very bad path.  
> ([source](https://www.reddit.com/r/godot/comments/x10o2m/comment/imcgsm1/?utm_source=reddit&utm_medium=web2x&context=3))


### Cannot mark areas on NavigationRegion(s) as water, ground, etc.

[NavigationRegion3D](https://docs.godotengine.org/en/stable/classes/class_navigationregion3d.html) has a cost for entering and travelling within.  
And in this Node, you have your NavigationMesh, which is constructed automatically using Meshes in your scene - i.e. there's a certain lack of control. And as AStar is used, costs of Navigation Regions are not always considered (see `Pathfinding ignoring Navigation Cost`).

In some cases you might want to have perfect control over certain Areas, which have special travelling costs.

Godot's NavigationServer does not support this (see [proposal](https://github.com/godotengine/godot-proposals/issues/5116))

[recastnavigation] has this feature out-of-the-box: https://github.com/recastnavigation/recastnavigation/pull/260


## Performance

* limit pathfinds per physics tick: https://github.com/godotengine/godot/pull/62745
* avoidance obstacles: "The avoidance world is currently rebuilding all static obstacles when a single static obstacle is changed cause each obstacle holds ref to some of it's neighbors which can be costly at runtime"


# Alternative to NavigationServer

## Advanced Navigation Plugin for Godot

https://github.com/lampe-games/godot-advanced-navigation-plugin

but
* only works for Godot 3.x


## godotdetour

https://github.com/TheSHEEEP/godotdetour

but
* only works for Godot 3.x
* no editor integration, coding for everything required
* NavigationMesh generation is currently limited to one [Mesh] (see [here](https://github.com/TheSHEEEP/godotdetour/issues/5))

possible usage in editor
* create NavigationMesh using NavigationServer, export as Mesh, feed into godotdetour
