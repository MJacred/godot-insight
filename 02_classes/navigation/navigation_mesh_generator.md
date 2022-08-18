# Backstory

Earlier, the geometry parsing was done using PhysicsServer/VisualServer, but both servers would stall when used with threads. ([source](https://github.com/Zylann/godot_heightmap_plugin/issues/335#issuecomment-1210870042))

Now, there's the `NavigationMeshGenerator::get_singleton()`.


# NavigationMeshGenerator

https://github.com/godotengine/godot/blob/master/modules/navigation/navigation_mesh_generator.cpp
