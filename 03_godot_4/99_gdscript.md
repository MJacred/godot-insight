## Advantages and Disadvantages

Advantages
* no build times, instant run
* hot reload changes in the running game
* engine-specific features: Node path syntax, onready, connection visualization in IDE, preload keyword, etc.
* code autocomplete can complete game data (node paths, file paths, animation names in a player, live data in objects, etc).
* receives drop events from editor (node paths, file paths, properties, etc.)
* docs built-in in code editor and inspector
* [static typing](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/static_typing.html) (though it's optional)
* you extend GDScript with C#/C++

Disadvantages
* not strictly strongly typed

Should not be confused with UnityScript or Python.

source: https://twitter.com/reduzio/status/1702582789823226206


## Cheatsheet

source: https://github.com/godotengine/godot/issues/90513

```gdscript
var node: Node
if node: # `is_instance_valid(node)` is the same.
    # `node` is a valid object.
else:
    # `node` is a freed object or `null`.


# To check if a variable has ever been assigned, I would recommend using a separate flag rather than distinguishing between null and freed object:

var _node: Node
var _initialized: bool

func set_node(node: Node) -> void:
    _node = node
    _initialized = true

func do_something() -> void:
    if _node:
        # Valid.
    elif _initialized:
        # Cleanup. 
```

```
Stringification
---------------
str(value)                      <null>         <Object#null>  <Freed Object>
var_to_str(value)               null           null           null

Booleanization
--------------
true if value else false        false          false          false
true if not value else false    true           true           true
is_instance_valid(value)        false          false          false

Comparison
----------
value == null                   true           true           false
value == other_obj_null         true           true           false
value == other_freed            false          false          false

Strict Comparison
-----------------
is_same(value, null)            true           false          false
is_same(value, other_obj_null)  false          true           false
is_same(value, other_freed)     false          false          false

Type Checking
-------------
typeof(value) == TYPE_NIL       true           false          false
typeof(value) == TYPE_OBJECT    false          true           true
value is Object                 false          false          Error
```