## File content structure

Using Godot's namespace will shorten your code considerably:
```cpp
using namespace godot;
```
Alternatively, put your code into Godot's namespace:
```cpp
namespace godot {
    // Your code.
}
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
