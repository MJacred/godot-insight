# Graphics

UX / Assist

UI scale
```gdscript
# change the viewport size, based on our setting in the project settings:
var viewport_start_size := Vector2(
	ProjectSettings.get_setting(&"display/window/size/viewport_width"),
	ProjectSettings.get_setting(&"display/window/size/viewport_height")
)
viewport_start_size *= x # 0.5 (bigger (200%) <= x <= 1.5 (smaller: 66%)
get_tree().root.set_content_scale_size(viewport_start_size)
```


Resolution scale / 3D viewport resolution (**Performance**)
```gdscript
# scaling_3d_scale: Resolution scales below 1.0 can be used to speed up rendering, at the cost of a blurrier final image and more aliasing. Resolution scales above 1.0 can be used for supersample antialiasing (SSAA) - only available for bilinear scaling. This will provide antialiasing at a very high performance cost, and is not recommended for most use cases.
var viewport_render_size = get_viewport().size * get_viewport().scaling_3d_scale
"3D viewport resolution: %d × %d (%d%%)" \
			% [viewport_render_size.x, viewport_render_size.y, round(get_viewport().scaling_3d_scale * 100)]
# When using FSR upscaling, AMD recommends exposing the following values as preset options to users "Ultra Quality: 0.77", "Quality: 0.67", "Balanced: 0.59", "Performance: 0.5" instead of exposing the entire scale.
# If FSR2 is used: A value of 1.0 will use FSR2 at native resolution as a TAA solution.
```

sources
* https://github.com/godotengine/godot-demo-projects/tree/master/3d/graphics_settings

Video settings:

    .
    Display filter (bilinear or AMD FidelityFX Super Resolution 1.0).
    Fullscreen.
    V-Sync (traditional and adaptive).
    Anti-aliasing (MSAA and FXAA).
    Camera field of view.

Effect settings:

    Signed distance field global illumination (SDFGI).
    Bloom (glow).
    Screen-space ambient occlusion (SSAO).
    Screen-Space reflections (SSR).
    Screen-space indirect lighting (SSIL).
    Volumetric fog.
    Screen adjustments: brightness, contrast, saturation.

