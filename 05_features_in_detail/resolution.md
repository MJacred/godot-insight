https://docs.godotengine.org/en/stable/tutorials/rendering/multiple_resolutions.html
[Implement native DPI scaling](https://github.com/godotengine/godot/pull/86022)
* [Implement SVGTexture auto-scalable with font oversampling](https://github.com/godotengine/godot/pull/105375)
https://en.wikipedia.org/wiki/1440p

https://github.com/mativizo/common-game-resolutions/
most common ratios are:
    16:9 at 89.43%
    16:10 at 5.16% (Steam Deck)
    21:9 at 2.51%
    4:3 at 0.95%



## Basics

Window and Viewports
* There's the viewport, that displays your game content.
* And there's the window, that contains the viewport and shows a frame around it, as well as a bar at the top to minimize, maximize and close the window.
* To note is: the root viewport is a Window instance! Therefore, all Window instances are viewports! The other viewports (SubViewports) are not windows.

Sizes in ProjectSettings
* Display → Window: viewport width/height are DESIGN sizes. Their interpretation depends on settings in ProjectSettings -> Stretch:
  * Mode: order of rendering Vs stretching, using the design size as a factor.
  * Aspect: where to grant extra space, or none.
  * Scale (runtime: Window.content_scale_factor): resizing 2D elements
    * 3D resolution scalig: This is done by using the root Viewport node only for 2D elements, then creating a Viewport node to display the 3D world and displaying it using a SubViewportContainer or TextureRect node. There will effectively be two viewports in the final project. One upside of using TextureRect over SubViewportContainer is that it allows enable linear filtering. This makes scaled 3D viewports look better in many cases. (this might be outdated?)  
    Or use `rendering/scaling_3d/scale` (runtime: [Viewport.scaling_3d_scale](https://docs.godotengine.org/en/stable/classes/class_viewport.html#class-viewport-property-scaling-3d-scale)).


## Demos

* https://github.com/godotengine/godot-demo-projects/tree/master/viewport/3d_scaling
* https://github.com/godotengine/godot-demo-projects/tree/master/3d/graphics_settings
* https://github.com/godotengine/godot-demo-projects/tree/master/gui/multiple_resolutions

## 2D and 3D scaling

Approach
* get DPI of the active screen
* scale accordingly
  * 2D elements, using `Window.content_scale_factor`
    * affects `SubViewportContainer` (if `SubViewportContainer.stretch` is true), but not the `SubViewport`'s stretch size (TODO: `SubViewport.size_2d_override` or `Viewport.get_stretch_transform()`?)
      * [fix for that](https://github.com/godotengine/godot/pull/102328)
  * 3D elements, using 

Display → Window: viewport width/height are DESIGN sizes  
can be changed at runtime, see `Window.set_content_scale_size`.

notes: https://github.com/godotengine/godot/issues/101137#issuecomment-2596063109

user-defined scale factor: ProjectSettings.(display/window/stretch/scale)

https://docs.godotengine.org/en/latest/classes/class_displayserver.html
```gdscript
DisplayServer.screen_get_scale
DisplayServer.window_get_size()
get_viewport().get_visible_rect().size
DisplayServer.has_feature(DisplayServer.FEATURE_HIDPI) # Windows, Linux (Wayland), macOS
```

Stretch Modes
* Disabled: 1 Scene Unit == 1 Pixel. No aspect ratio ensurance, no stretching, therefore: If the viewport/window gets smaller, things get hidden. If the viewport/window gets bigger, more is visible, potentially empty space.
  * useful if you want to handle the UI resizing yourelf, e.g. defining min and max viewport size thresholds yourself to scale on demand (best add a scale multiplier to future-proof this)
* Canvas Items: Scales images first, then renders them to the viewport.
  * good for: for all games but pixel art games (on desktop)
  * downsides:
    * might not be pixel perfect (by design)
    * [makes text blurry](https://github.com/godotengine/godot/issues/86563) ([workaround](https://github.com/godotengine/godot/issues/86563#issuecomment-3172696479))
* Viewport: Rendered to viewport, then scaled to screen.
  * good for: for pixel art games (on desktop); together with stretch scale mode to `integer`.
  * Can be controlled by calling and setting `get_viewport().size` (see also `DisplayServer.screen_get_size()`. Changing the viewport size also changes the window size, if windowed. Combine with `Scale` (increase the value to makes pixels look larger and lower the resolution of 2D elements). Can be changed at runtime using `get_tree().root.content_scale_factor` (see also [Window.content_scale_factor](https://docs.godotengine.org/en/stable/classes/class_window.html#class-window-property-content-scale-factor)). Is recommended over using `get_tree().root.set_content_scale_size` (see `Window.set_content_scale_size`).

Stretch Aspect
* Ignore: UI is auto-scaled, but also deformed.
* Keep: Always adds black bars to the top/bottom and sides, if the aspect would be broken because of the top-side-size-relations.
  * useful if you're certain of the target device's aspect ratio. Or want to guarantee the distances between your UI elements, if you anchored them to the viewport edges/corners. Or if you want to disallow changing the window size by clicking the window corner and dragging the mouse, and only allow changing the window/viewport size in the menu - where you can also offer alternate aspect ratios.
* Keep Width: The width you set will be respected (adding black bars on the left/right to fake it), but expands vertically and adds space at the bottom.
  * If you remember playing games with black bars on the sides. This is the logic used.
* Keep Height: Inverse of "Keep Width"
* Expand: Adds space at the sides or bottom, when the original aspect cannot be kept.

To adjust the 3D resolution rendering without affecting 2D/UI, change the `scaling_3d_scale` property. Nothing else. This works on mobile too and will give you most of the performance benefits of reducing the viewport resolution with the `viewport` stretch mode.

Viewport.get_stretch_transform # https://docs.godotengine.org/en/stable/classes/class_viewport.html#class-viewport-method-get-stretch-transform
`Viewport.get_stretch_transform()` returns the automatically computed 2D stretch transform. Combined with `Transform2D.get_scale()`, this is useful when using the `Canvas Items` stretch mode in a project.
* Divide Camera2D zoom to keep the size of the 2D game world identical regardless of the 2D scale factor (so that UI elements can still be scaled).
* Make _certain_ controls always drawn at 1:1 scale (e.g. for the crosshair in a FPS). This is done by dividing the Control node's scale by the scale factor.
equal to
```gdscript
var scale_factor: float = min(
		(float(get_viewport().size.x) / get_viewport().get_visible_rect().size.x),
		(float(get_viewport().size.y) / get_viewport().get_visible_rect().size.y)
)
```

A lot of modern pixel art games use a 640×360 viewport (using integer scaling) as it covers a lot of common screen resolutions with integer scaling (720p, 1080p, 1440p and 2160p are all covered).

1280×960 is a very large viewport size for a pixel art game, as it won’t be able to fit on the Steam Deck which is 1280×800.
https://forum.godotengine.org/t/how-to-get-scaling-to-work-properly-in-fullscreen/72894/5

check how a scaled viewport works with mouse position.
https://docs.godotengine.org/en/stable/tutorials/inputs/mouse_and_input_coordinates.html
If you're referring to split screen, you will need to offset the position returned by get_mouse_position() by the viewport's position (and perhaps its scale, if it's scaled).
```gdscript
var screen_mouse_position = get_viewport().get_mouse_position() # Get the mouse position on the screen
# Convert it to world coordinates
mouse_position = (get_viewport().get_screen_transform() * get_viewport().get_canvas_transform()).affine_inverse() * screen_mouse_position
```

Issues
* [SubViewport scaling not respecting override size for 3D](https://github.com/godotengine/godot/issues/72548)
* [SubViewportContainer doesn't adjust the stretch size of the SubViewport when stretch is true](https://github.com/godotengine/godot-proposals/issues/11680)
  * [WIP FIX](https://github.com/godotengine/godot/pull/102328)
* [Window content_scale_factor causes jitter in Control size](https://github.com/godotengine/godot/issues/102656)
  * [Fix](https://github.com/godotengine/godot/pull/102741): "This is done by allowing size_2d_override to be Size2 instead of Size2i. This change is _not propagated_ to SubViewport to keep compatibility for now."
* [Scaling artifacts when viewport is scaled up and texture filtering is disabled](https://github.com/godotengine/godot/issues/79726)
  * [WIP Fix](https://github.com/godotengine/godot/pull/93796)
  * potentially related: [Incorrect mouse event position y precision in maximized window with `content_scale_factor`](https://github.com/godotengine/godot/issues/94780)
* [Custom cursors don't scale the same as ui elements, when monitors are scaled](https://github.com/godotengine/godot/issues/97390)



Redesign of sizing
* https://github.com/godotengine/godot-proposals/issues/6221#issuecomment-2772310135
  * replacing
    * `Viewport.size`: size of the viewport texture
    * `Viewport.size_2d_override`: area-size of the viewport-texture, that should be used for display. The rest of the viewport-texture is not displayed
  * note about a SubViewport in a SubViewportContainer
    * `SubViewport.size`: determines the size of the viewport render target
    * `SubViewport.size_2d_override`: when enabled, affects the transform; it does not affect the size of the render target
    * `SubViewportContainer.stretch`, when enabled, forces the `SubViewport.size` to equal the size of the `SubViewportContainer`, scaled by the `SubViewportContainer.stretch_shrink` property, which defaults to 1 (it only affect child nodes of type `SubViewport`)
    * `SubViewportContainer.stretch_shrink` affects the `SubViewport` render target size, scaling it by the value of `stretch_shrink`


### hiDPI

https://docs.godotengine.org/en/stable/tutorials/rendering/multiple_resolutions.html#hidpi-support
really not on Linux?
https://github.com/godotengine/godot/issues/61236
https://github.com/godotengine/godot/issues/102512
https://github.com/godotengine/godot/pull/86943#issuecomment-2183141297
https://github.com/godotengine/godot/issues/56341
[Resolution issue with dual monitors with different DPI](https://github.com/godotengine/godot/issues/84948)

ProjectSettings:
display/window/dpi/allow_hidpi
allow hidpi option mainly affects how the image is scaled when the game window is resized. Without allow hdpi, Godot stretches the viewport, while with allow hdpi, godot renders at its ‘native’ resolution and lets the OS handle upscaling


```gdscript
DisplayServer.screen_get_scale
DisplayServer.screen_get_dpi # not really reliable
```
If hiDPI and in windowed mode, resize window.

There's no official threshold, but a typical HiDPI monitor has a DPI of at least 200

To increase the in-game visible area when the user resizes the window, you can change the camera zoom automatically when the window/viewport is resized. There’s a signal you can connect from the root viewport like [this](https://github.com/godotengine/godot-demo-projects/blob/master/viewport/3d_in_2d/3d_in_2d.gd#L10).

For non-games, where Stretch Modes is usually `disabled`, use Window.content_scale_factor for runtime changes.
And ProjectSettings.("display/window/stretch/scale", 1.0) for setup.


https://github.com/godotengine/godot-proposals/issues/7968#issuecomment-1782522897
things will just be smaller on a hiDPI display: 
get the screen scaling factor with DisplayServer.get_screen_scale() (Only on macOS so far #2661) or guess based on DisplayServer.get_screen_dpi(). Then apply this to Window.content_scale_factor and scale up the Window.size to match. This is cumbersome and prone to errors since you also need to take into account that the user might move a window to/from a hiDPI monitor and some things don't scale correctly (godotengine/godot#59882 (comment)). And I've noticed that if you increase the content_scale_factor and size at while the window is on screen the window will still seem to render at the old lower resolution and just scale up the viewport resulting in larger but blurry content.


https://github.com/godotengine/godot-proposals/issues/2661



### Split Screen

**problem**: `Canvas Items` stretch mode: the GUI scales up as the window size increases, but SubViewports in a SubViewportContainer don’t change in resolution (they pixilate with increased window size)

**solution**: resize the respective viewports in a script whenever the root viewport’s size changes

```gdscript
extends Node2D

@onready var viewport: SubViewport = $SubViewport
@onready var viewport_initial_size_y: float= viewport.size.y
@onready var viewport_sprite: Sprite2D = $ViewportSprite # If you render 3D to 2D, see linked example.

func _ready():
	get_viewport().size_changed.connect(_root_viewport_size_changed)


# Called when the root's viewport size changes (i.e. when the window is resized).
# This is done to handle multiple resolutions without losing quality.
func _root_viewport_size_changed():
    # The viewport is resized depending on the window height.
    # To compensate for the larger resolution, the viewport sprite is scaled down.
    var new_y: float = get_viewport().size.y
    viewport.size = Vector2.ONE * new_y
    viewport_sprite.scale = Vector2.ONE * viewport_initial_size_y / new_y
```


[up-to-date example](https://github.com/godotengine/godot-demo-projects/tree/master/viewport/3d_in_2d)

If you're referring to split screen, you will need to offset the position returned by get_mouse_position() by the viewport's position (and perhaps its scale, if it's scaled).
```gdscript
var screen_mouse_position = get_viewport().get_mouse_position() # Get the mouse position on the screen
# Convert it to world coordinates
mouse_position = (get_viewport().get_screen_transform() * get_viewport().get_canvas_transform()).affine_inverse() * screen_mouse_position
```

([source](https://forum.godotengine.org/t/godot-4-how-do-i-make-a-subviewport-that-supports-both-scaling-gui-and-multiple-resolutions/4110))


## Oversampling

### Font

Warning regarding editor: When zooming in the 2D editor, fonts are not re-rasterized as this could be extremely slow at high zoom levels. MSDF fonts however don't need to be re-rasterized to look crisp.


https://github.com/godotengine/godot/pull/104872
* per-viewport Font oversampling (required for automatic DPI scaling)
  * can reuse existing font data if possible (e.g, if you have 16 and 32 size fonts already in use, and oversampling of 2x, oversampled 16 font will reuse 32 font data) and won't reload all fonts
  * can be overridden per Font/CanvasItem object.

requires Viewport.oversampling == true AND one of these (see Viewport class docs):
* Window.content_scale_factor + scaling is enabled
* Viewport.oversampling_override > 0
* SubViewport.size_2d_override_stretch + SubViewport.size_2d_override
* Font.oversampling > 0

Issues
* [Text gets blurry when resizing the viewport with the `Canvas Items` stretch mode](https://github.com/godotengine/godot/issues/86563)
  * [workaround](https://github.com/godotengine/godot/issues/86563#issuecomment-3172696479)

Notes
* MSDF font: render text crisp at any resolution without requiring oversampling, as they are rasterized once, and a shader is used to interpolate smooth curves from the signed distance field data. This works well when changing Camera2D zoom.
  * when MSDF font rendering is enabled, font hinting and LCD subpixel layout settings are ignored ([source](https://github.com/godotengine/godot/issues/86563#issuecomment-1972080864))
  * If *not* using a custom font, this can be done by enabling **Default Font Multichannel Signed Distance Field** in the advanced Project Settings. If using a custom font, this can be done by checking the **Multichannel Signed Distance Field** property in the Import dock after selecting a font file in the FileSystem dock, then clickin **Reimport**. ([source](https://github.com/godotengine/godot/issues/69711#issuecomment-1341237691))
  * limitations: https://docs.godotengine.org/en/latest/tutorials/ui/gui_using_fonts.html#doc-using-fonts-msdfgdscript
DisplayServer.screen_get_scale

