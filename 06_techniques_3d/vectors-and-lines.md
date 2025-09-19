* stuff (ray_triangle_intersection, selection_in_3d, etc.): https://github.com/JohnMeadow1/GodotGeometryElements


## Transform

Tescribes position (`origin`) in a 3D vector, rotation and scale (`basis`) in a 3 3D vectors. The basis is in the object's local space.

```gdscript
# position
mesh_instance.global_transform.origin = Vector3(…)

# direction: is a Vector3, can never be a matrix (i.e. basis)
# direction per axis:
mesh_instance.global_transform.basis.x
mesh_instance.global_transform.basis.y
mesh_instance.global_transform.basis.z

# rotation angles on all 3 axes in radians
mesh_instance.global_transform.rotation

# scale: the magnitude of the vectors in basis
# scale on y: y-axis on basis.y
# scale on x: x- and z-axis uniformly on basis.x
# scale on z: x- and z-axis uniformly on basis.z
```


