# Navigation

> Godot uses [recastnavigation](https://github.com/recastnavigation/recastnavigation)'s **Recast** for creating the NavigationMesh.
> But Godot uses RVO2 for the **Detour**.

The navigation system is still experimental and thus experiences rapid changes.
See the [roadmap](https://github.com/godotengine/godot/issues/73566) for more info on its state.


## Basics

* runs in the main thread by default, though multi-threading is supported by creating a `Thread` and call the NavigationServer2D/3D from within
* there is no built-in time slicing queries, that has to be done by the user at the moment
* Godot uses navigation meshes, no tiling (and therefore no hierarchical optimization). Though you can chunk navmeshes (see NavigationRegion2D/3D) and align them like tiles
  * navigation is based on the mesh's vertices (aka edge point), therefore odd vertex placement can create odd (unnatural) paths (see `Holes` and `Navigation Areas`)
* algorithms:
  * AStar: takes the first path that "connects" to the target, not necessarily the optimal path
    * travels from closest edge point to closest edge point in the direction of the target, [potentially going through navigation mesh holes](https://github.com/godotengine/godot/issues/105705#issuecomment-2952433509)
  * Dijkstra: [is not available yet](https://github.com/godotengine/godot/pull/64326#issuecomment-1213323898)
* As the path result (including its waypoints) relies on the used navigation mesh, there might be some odd results. Especially when you use `Navigation Areas` (for more info, read that chapter).  
  Using `simplify_path` in combination with `simplify_epsilon` on your agents may help you create a more natural flowing path.



## Features

### Navigation Agent

To calculate a path for an NavigationAgent*, you need only to call `set_target_position()`.

Each physics step, you can call `get_next_path_position()` _once_ to know where your agent should go to.  
To get the agent actually moving, you need to code the logic.

When can/should you call `get_next_path_position()` more than once?
* rollbacks (because sth. unexpected happened when you tried to move your agent after getting the next position)


Reaching a desired destination (or a waypoint towards the destination) is not always possible, therefore
* compare to `get_final_position()` if the targeted position is reachable. Or use `is_target_reachable()`, which also takes `target_desired_distance` into consideration.
* `set_path_desired_distance()` aids you in reaching your target; examples
  * some NPC (or even a player) walks and stops ontop a waypoint. The path is blocked, for now. Or worse: an item is gladed there, forever.
  * if the value is too high, the agent may leave the navigation mesh
    * and become lost
    * or smash into a corner, while trying to walk around it. A big enough agent radius used to calculate the navigation mesh helps here.
* `set_target_desired_distance()` acts the same as `set_path_desired_distance()`, but allows you to handle a different use case
  * imagine you set the target position to some chest, or NPC. Usually, they have collisions that prevent others from walking right through them. So stopping beforehand, e.g. 1 world unit, is not that bad of an idea.
  * the distinction to `path_desired_distance` really only matters when your target is really big, but you don't want to mess up how waypoints are handled. In this case, set it to a higher value than `path_desired_distance`. Or if your target is really "small", say infront of a shop, which usually has no collision set, use a smaller value than `path_desired_distance`.
  * once it is reached, the `target_reached` signal is emitted


#### Path calculation and Signals

Mind that many method calls on an agent re-trigger pathfinding; examples:
* `set_navigation_layers()` & `set_navigation_layer_value`
* `set_navigation_map()`
* `get_next_path_position()` (only when necessary)
* crossing the threshold set by `set_path_max_distance()`. For more info, see chapter `Avoidance`

You get notified about this in the `path_changed` signal.

Re-calculating a path can re-trigger some signals you already handled.  
Therefore, calling these methods while you're handling an agent signal, e.g. `waypoint_reached`, will cause an infinite recursion.

**target_reached** vs **navigation_finished**  
The `navigation_finished` signal is emitted once the path is "done". As mentioned before, whether your agent reached the target or only the closest possible navigation point are two entirely different things.  
Note: this signal and `target_reached` are not guaranteed to be emitted in any specific order. A deferred update, for example, can help you to truly handle "target reached".  
Once this signal is emitted, pathfinding won't be tre-triggered, until you call `set_target_position()` again.


### Avoidance

Handle avoidance correctly
* https://github.com/godotengine/godot-proposals/issues/5013#issuecomment-1199982413


#### Obstacles

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


