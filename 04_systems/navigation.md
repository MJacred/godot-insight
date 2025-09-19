# Navigation

> Godot uses [recastnavigation](https://github.com/recastnavigation/recastnavigation)'s **Recast** for creating the NavigationMesh.
> But Godot uses RVO2 for the **Detour**.

The navigation system is still experimental and thus experiences rapid changes.
See the [roadmap](https://github.com/godotengine/godot/issues/73566) for more info on its state.

## Tutorials
* C# https://www.youtube.com/watch?v=df3_IvMEbHE
* GDScript: https://www.youtube.com/watch?v=NifXo5UDz_c


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
* crossing the threshold defined in `path_max_distance`: how far is an agent allowed to be pushed away from the current path segment?
  * A too low path_max_distance resets the pathfinding all the time when agents try to avoid and a too high time_horizon predicts a velocity consistent in that direction for x-seconds. The avoidance does not know that you will change this velocity on the next frame again. ([source](https://www.reddit.com/r/godot/comments/13o5br4/comment/jl5iv90/))


You get notified about this in the `path_changed` signal.

Re-calculating a path can re-trigger some signals you already handled.  
Therefore, calling these methods while you're handling an agent signal, e.g. `waypoint_reached`, will cause an infinite recursion.

**target_reached** vs **navigation_finished**  
The `navigation_finished` signal is emitted once the path is "done". As mentioned before, whether your agent reached the target or only the closest possible navigation point are two entirely different things.  
Note: this signal and `target_reached` are not guaranteed to be emitted in any specific order. A deferred update, for example, can help you to truly handle "target reached".  
Once this signal is emitted, pathfinding won't be tre-triggered, until you call `set_target_position()` again.


### Avoidance

> … is neither a part of pathfinding, nor a means to changing paths: this system is more like an evasion/dodge reflex.

What makes the avoidance so different from the above?  
`get_next_path_position()` only tells you where to go to in a static and void world. Avoidance fills this world with other agents, and obstacles.  
You tell it where you want to go to and which speed (i.e. the desired velocity), and it tells you what the safe version of your velocity _should be_.  
It basically goes like this:
* you connect some `agent_parent.callable()` to the `agent.velocity_computed` signal in `agent_parent._ready()`, and
* in `agent_parent.physics_process()` you call `agent.set_velocity()`
* your `agent_parent.callable()` receives this "safe" velocity, which
* you can pass to `CharacterBody.move_and_slide()`, or `RigidBody.linear_velocity` (though you shouldn't change `linear_velocity` too often…)


So, how do we configure our "safe" velocity using an agent's properties?
* first things first: enable it calling `set_avoidance_enabled(true)`
* knowing when to even think about avoiding: basically everybody who's too close for comfort. These are called "neighbors".
  * `neighbor_distance`: the distance threshold between agent positions, *ignoring* their dimensions, until they are considered neighbors
    * `max_neighbors`: Stop avoiding all agents who would be neighbors after having found x of them.
      * If `neighbor_distance` is too far and this value too low in comparison, you can miss out on closer neighbors
* call `set_velocity`:
  * the desired velocity based on `get_next_path_position()`, your agent's desired movement speed (not to be confused with `agent.max_speed`), gravity, etc.
* safe velocity calculation
  * using an **agent's avoidance shape** to know the bounds
    * `radius`: the agent's physical body radius. Is usually equal, or similar, to the agent radius you set for baking a particular `NavigationMesh`
      * **does not affect the avoidance threshold**, see `neighbor_distance`
    * `height`: works together with `radius` and the parent's UP position to create a cylinder body.
  * if `keep_y_velocity` is `true`, then the safe velocity will contain the velocity.y value you set in `set_velocity`.
    * Should be `true` on uneven ground, else you can set it to `false`.
  * the property `max_speed` clamps the safe velocity's magnitude.
  * increasing `time_horizon_agents` and `time_horizon_obstacles` (both in seconds), increases the agent's reaction time by decreasing the velocity's speed.
  * `use_3d_avoidance`: **this flag should be the same for all agents** that should interact with eachother, as they cannot avoid each other
    * the biggest difference to 2D avoidance: respects velocity on the UP-axis. Therefore, **it's suitable for 3D maneuvers in e.g. water and space**.
* the result: emittance of the "safe" velocity via signal `velocity_computed`

NOTES
* only call avoidance-related properties when it's needed: always check `if agent.avoidance_enabled`; also when receiving signal of `agent.velocity_computed`
* on teleportation
  * change the position of this node's parent, then call
  * `set_velocity()`, and
  * `set_velocity_forced()`


Navigation Obstacles
* are basically fake Agents.
* Use _either_ of the below:
  * `set_avoidance_enabled()`
  * `set_affect_navigation_mesh()`
    * This removes a circular shape from the navigation mesh. Therefore, avoidance should not be needed.
* Shape: circular, with [rectengular proposed](https://github.com/godotengine/godot-proposals/issues/12668)

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

### Holes


### Layers, Masks and Priority


Like visuals and pyhsics, avoidance has Layers and Masks as well. So if two (or more) groups are on collision course (provided their layers and masks match), they'll avoid each other. The ones with a higher priority won't avoid, though. The other ones will.  
([source](https://github.com/godotengine/godot/pull/69988))


#### Navigation Layers

who has layer info
* `NavigationAgent3D`
  * layers: the ones they are allowed to walk on
* `NavigationLayerCostMap`
  * assigned to: `NavigationAgent3D`
  * navigation cost per layer: if an Agent has a certain layer bit not toggled, that layer cost does not apply to them
* `NavigationArea3D`
  * used for: baking `NavigationMesh3D`
  * allows to:
    * creating sub regions in a `NavigationRegion3D`: overwrites the layer bits of the affected `NavigationRegion3D`, and thus can change the pathfinding, including blocking the path for Agents that don't have the required flag
    * boolean operation on `NavigationMesh3D`: if layer is 0, it carves out a hole as closely as possibly to the area's shape
    * requirements: at least 1 bit flag must be different from the `NavigationRegion3D` in order to work
    * side effects: if 2 or more Areas overlap, and their flags are equal, they will be merged
* `NavigationLink3D`:
  * used for: allows `NavigationAgent3D` to move from one Region, or one sub-region, to another

What meaning can you assign to one specific layer bit?
* fully blocking off certain areas:
  * examples
    * door
    * bridge
    * going onto weird paths
  * how can you do that: `NavigationLink3D`, `NavigationArea3D`, have different regions for different cases and re-assign agents between them
* separate areas, while not making them totally off-limits
  * examples
    * side walks and street
  * how can you do that: `NavigationArea3D`, have different regions for different cases and re-assign agents between them


#### Avoidance Layers

* not to be confused with navigation layers, these are just between Agents to not bump into each other


### Navigation Region

In general regions are partition units, most users do not use them like that which is its own problem, e.g. in Godot 4.5 they could already query filter for specific regions instead of throwing giant queries at the entire map.

most larger projects use chunk regions with box AABB due to the navmesh baking requiring that. So because the large worlds are already grid based it is easy for them to filter the queries to just chunks around a position.


### NavigationArea3D

performance criterias
* cell size (smaller is more expensive)
* shape type (cost order: Polygon > Cylinder > Box)


### Baking

control
* Bake Area: https://github.com/godotengine/godot/pull/62348
  * cherry-picked Godot 3.5
* Bake on import: `ImporterMesh::create_navigation_mesh()` ([source](https://github.com/godotengine/godot/blob/master/scene/resources/importer_mesh.cpp)), your mesh must have `-navmesh` suffix and the original mesh is discarded
* https://github.com/godotengine/godot/pull/70724


avoid vertex overlap. may create holes


If you are using the MeshInstance convert button all you are doing is copying the visual mesh data into a navigation mesh resource. This option has no check if the visual mesh polygon data that you copy makes any sense for navigation or pathfinding.

If you are using the NavigationRegion bake button you are actually parsing the SceneTree nodes as source geometry. Then you bake from that source geometry a new navigation mesh according to the NavigationMesh settings.

You properly just have NavigationMesh bake settings that allow no navigation mesh, e.g. a too small plane mesh as the only source geometry and a too large agent radius.



Different NavigationRegions merge / connect navigation mesh polygons in two ways.

When the two vertices of an edge overlap exactly (withing the navigation map cell size grid) the navigation mesh polygon edges are merged by vertex. This is the quickest merge with the best quality for pathfinding.

When edges can not merge by vertex but are still withing edge connection margin distance and a near identical angle they are merged with an edge connection. This is a costly operation cause the NavigationServer needs to compare all free and unmerged edges by distance and angle. This edge connections also exists only virtual which means that any point query aimed at this connection gap will still snap to the closest "real" polygon edge.

The gist is, if you can merge all your navigation mesh edges by vertex and nothing by edge connection.




You properly want to switch to using a navigation mesh and NavigationServer if you want "more points for more detailed movement".

The entire "point" (fun intended) of AStar classes is point based navigation restricted to distinct positions while the NavigationServer uses navigation mesh based navigation that defines an entire area for movement instead of just points.

If you don't want a path to go somewhere due to e.g. a placed object you need to change the navigation mesh, No real way around that but you also would need to disable a point in the AStar graph cause how else should the pathfinding know about such object. You can restrict the bake area with the NavigationMesh baking AABB and also limit your source geometry with node groups to make the baking very fast for small areas at runtime.


Added very recently an entire documentation page dedicated to the navigation performance topic.

https://docs.godotengine.org/en/stable/tutorials/navigation/navigation_optimizing_performance.html

The NavigationServer synchronizes per navigation map. If the map has seen no changes it will not update and cost runtime performance. If anything changed for the navigation regions or navigation meshes it will update a navigation map in full. While there are many performance improvements coming this is the situation now. Knowing this if you really reach a performance ceiling with navigation map updates that you can't seem to solve otherwise there is always the option to slice your game world into different navigation maps and switch the agents between them to better control the update workload at runtime.



doors
> Use the navigation layers on the navigation regions. Have the "door" navigation mesh on a NavigationRegion3D with a special navigation layer that is basically the "key" or the "open/closed" state to use the door.
> https://docs.godotengine.org/en/latest/tutorials/navigation/navigation_different_actor_area_access.html
> This way whenever an agent queries a new navigation path, as long as they have that bit enabled to use that door, they will use the navigation mesh part of that door to go through it. Mind you that if an agent already has a navigation path to follow while you change the bitmask on the "door" it will not auto-update the path for the agent so take care of that situation.
> [source](https://www.reddit.com/r/godot/comments/13jyytq/comment/jkhlckx/)


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
Can be worked around using custom code to update the target position and [NavigationAgent2D/3D.set_path_desired_distance()](https://github.com/godotengine/godot/pull/62181).  
Beware: `path_desired_distance` represents whole 3D world units (i.e. meters)


You can store the current navigation path target position and in your _process() check the distance between that position and your player.

If the player has moved to far away from that position because the distance is too large you query a new navigation path with the current player position.

This avoids the usual complications mentioned in your old post that come with querying a new navigation path every single frame.


## Troubleshooting

### In General

* physics: an Agent's parent node should not be a physics node. Use a normal Node2d/3D, or use a subnode that is toplevel and follow that one.
  * in short: physics (incl. interpolation) does not play nice with pathfinding and avoidance


### For older Godot versions

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


### Editor

Don'ts while baking
* editing the nodes involved in the baking
* switch scenes (because of background actions of the editor)



## Performance

### Baking

ReCast spans a giant voxel grid over your geometry and if you remove or lower nearly every setting that stands for detail you get low or no quality.

If you dont use Watershed you have no distance field so verticality is already at the lower detail end. That alone might be compensated with a better cell resolution or lower error margins but pair everything together like in this project and it can not work. Dial at least 1 or 2 of those parameters to something more reasonable and the issue will disappear.


### Other

* limit pathfinds per physics tick: https://github.com/godotengine/godot/pull/62745
* avoidance obstacles: "The avoidance world is currently rebuilding all static obstacles when a single static obstacle is changed cause each obstacle holds ref to some of it's neighbors which can be costly at runtime"
* https://github.com/godotengine/godot/pull/102766
* https://github.com/godotengine/godot/pull/102767
* https://github.com/godotengine/godot/pull/106670
  * enabled by default
  * project settings: `navigation/world/region_use_async_iterations`
  * NavigationServer2D/3D: `region_set_use_async_iterations(region, enabled)`
  * navigation links are still synced on the main thread (their update cost is minimal)
  * can cause additional delay on navmesh and navigation map changes, but prevents from stalling the main thread
  * use the `map_changed` signal to know when a map has changed, or poll the `map_get_iteration_id()`.
  * async region update there is no signal but the `region_get_iteration_id()` can be polled to know when a region iteration has finished
    * Note: just because a region iteration has finished does not mean that a map iteration has also finished that includes that region
