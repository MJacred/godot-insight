## Folder structure

### Addon

Example setup:
```
my-repository/
├─ godot-cpp/
├─ project/
│  ├─ addons/
│    ├─ my_addon/
│      ├─ bin/
│      ├─ resources/
│      ├─ src/
│      ├─ plugin.cfg
│      ├─ my_addon.gdextension
├─ src/
│  ├─ register_types.cpp
│  ├─ register_types.h
├─ README.md
```

folders explained
* `godot-cpp`: the git submodule required for GDExtension
* `src` on top-level: where you put your `register_types.*` files and everything else that's needed when running the exported game
* `project`: everything that's required to use the addon while using the editor
  * `addons/my_addon`
    * `bin`: this folder and its content are generated automatically (using your C++ files in `src` from top-level)
    * `resources`: where your image/audio/localization files are placed; you can add more sub-folders to separate them
    * `src`: GDScript files to control the UI elements and other editor-related things that don't require the speed of C++


files explained
* `my_addon.gdextension`: holds your extension's…
  * configuration: your icon path, minimum required Godot version, the `entry_symbol`
    * the entry symbol is what you wrote in your `register_types.cpp` file after `GDExtensionBool GDE_EXPORT` at the start of the `extern "C"` scope
  * library paths: where your C++ code compiled libraries are stored (`.so`, `.dll`, etc. files), per platform
    * they are placed automatically in `"res://addons/my_addon/bin`
* `register_types.cpp`
  * you only need to register the classes that are in the top-level `src` folder


## Setup

```
# in your terminal, change directory to your project

# get godot-cpp by banch
git submodule add -b 4.4 https://github.com/godotengine/godot-cpp.git godot-cpp

# init
git submodule update --init
```

Your IDE will not find the godot-cpp class files, therefore you need to setup your IDE accordingly:  
https://docs.godotengine.org/en/latest/contributing/development/configuring_an_ide/index.html#toc-devel-configuring-an-ide

## File content structure

Usually, put your header code into Godot's namespace (especially when creating custom Nodes):
```cpp
namespace godot {
    // Your code.
}
```
Using Godot's namespace in the class body files (and header file, if you don't define it in `namespace godot`. It will shorten your code considerably:
```cpp
using namespace godot;
```


Include names
```cpp
// File names are split by an '_' on each upper case in the CamelCase-formatted class name:
#include <godot_cpp/classes/editor_plugin.hpp> // EditorPlugin.
#include <godot_cpp/classes/static_body3d.hpp> // StaticBody3D.
#include <godot_cpp/classes/sub_viewport.hpp> // SubViewport.

// The suffixes "2D" and "3D" don't warrant a split:
#include <godot_cpp/classes/camera3d.hpp>

// All classes are in the path <godot_cpp/classes/..>. Helpers are not:
#include <godot_cpp/godot.hpp>
#include <gdextension_interface.h>

// Variants have their own path:
#include <godot_cpp/variant/dictionary.hpp>
#include <godot_cpp/variant/string.hpp>
#include <godot_cpp/variant/string_name.hpp>
#include <godot_cpp/variant/variant.hpp>

// Templates also have their own path:
#include <godot_cpp/templates/list.hpp>
#include <godot_cpp/templates/local_vector.hpp>
#include <godot_cpp/templates/sort_array.hpp>
```

Minimal header file:
```cpp
// myclass.h or myclass.hpp
class MyClass : public Object {
	GDCLASS(MyClass, Object);

	static MyClass *singleton; // If you want to use this class as a singleton.

protected:
	static void _bind_methods();

public:
	static MyClass *get_singleton(); // If you want to use this class as a singleton.

	MyClass();
	~MyClass();

    void do_something();
};
```

Minimal class file:
```cpp
// myclass.cpp
#include "myclass.h"

#include <godot_cpp/core/class_db.hpp>

using namespace godot;

MyClass *MyClass::singleton = nullptr;

void MyClass::_bind_methods() {
	ClassDB::bind_method(D_METHOD("do_something", "label"), &MyClass::do_something);
}

MyClass *MyClass::get_singleton() {
    // If you want to use this class as a singleton.
	return singleton;
}

MyClass::MyClass() {
    // If you want to use this class as a singleton.
	ERR_FAIL_COND(singleton != nullptr);
	singleton = this;
}

MyClass::~MyClass() {
    // If you want to use this class as a singleton.
	ERR_FAIL_COND(singleton != this);
	singleton = nullptr;
}

void MyClass::do_something(const Ref<Label> &p_label) {
    // …
}
```

## Quick tips

Define C++ macros for Godot's singletons in a default header file, e.g.:
```cpp
#define RS RenderingServer::get_singleton()
#define IS_EDITOR Engine::get_singleton()->is_editor_hint()
```


Getting pointers from Godot: You get `Ref<SomeClass>`
```cpp
// As parameter:
void MyClass::do(const Ref<Image> &p_image) {}

// As part of function body:
Ref<Image> &image;
// If you don't read from Godot, but rather want to create one on your own, you need to instantiate it:
image.instantiate();
```


Casting generic node to the one you need:
```cpp
TypedArray<Node> controls = my_node->find_children("*", "Control");

for (int i = 0; i < controls.size(); i++) {
	Control *control = Object::cast_to<Control>(controls[i]);
	// etc.
}
```


Callable: https://github.com/godotengine/godot-cpp/pull/1155
```cpp

```

Calling GDScript from within GDExtension
```cpp
// You'll need to reference an [`Object`](https://docs.godotengine.org/en/stable/classes/class_object.html), or one of its sub-classes.
// `Object` has methods such as `get()`/`set()` for reading/writing properties, and `call()` to call methods, and `connect()` for listening to signals.
```


Mass operations
```cpp
// make use of:
PackedVector3Array
PackedInt32Array
// etc.
```


## SConstruct

Good examples
* https://github.com/TokisanGames/Terrain3D/blob/main/SConstruct


## Notes

https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension/what_is_gdextension.html
hot reloading: https://github.com/godotengine/godot/pull/80284
