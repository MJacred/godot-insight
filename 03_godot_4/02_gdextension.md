## GDExtension

> GDExtension is a Godot-specific technology that lets the engine interact with native shared libraries at run-time, and _not_ a scripting language and has no relation to GDScript. You can use it to run native code without compiling it with the engine.  
> ([Source](https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension/what_is_gdextension.html))

Right now, creating a GDExtension is still a bit cumbersome. The instructions are noted [here](https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension/gdextension_cpp_example.html#doc-gdextension-cpp-example) and [here](https://github.com/godotengine/godot-cpp?tab=readme-ov-file#getting-started).  
This [GDExtension template](https://github.com/nathanfranke/gdextension) can get you started if you aim to create an addon in GDExtension.  
Though soon™, it will become easier and you can [create GDExtension plugins from inside the Godot editor](https://github.com/godotengine/godot/pull/90979).

Have a look at this [showcase](https://github.com/paddy-exe/GDExtensionSummator) on how a written GDExtension can look like and how to use it in GDScript.

If you want to call GDScript from within GDExtension, afaik, you'll have to take a reference to a [`Object`](https://docs.godotengine.org/en/stable/classes/class_object.html), or one of its sub-classes.  
`Object` has methods such as `get()` for reading properties, and `call` to call methods, and `connect` for listening to signals.
