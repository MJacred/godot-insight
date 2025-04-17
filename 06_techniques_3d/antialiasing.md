Cost/Quality: None < FXAA or SMAA < MSAA < TAA < FSR2 / MetalFX (Spatial / Temporal) on macOS < SSAA


* FXAA
  * not available in Compatibility renderer
  * looks decent on 1440p and higher
  * works well with cel-shaded, and similar, art styles
  * the added blurriness makes it hard to use on 1080p and below
    * reduce a bit of blurriness (while introducing aliasing) by [sharpening](https://github.com/godotengine/godot/pull/47401)
      * [not recommended for VR](https://github.com/godotengine/godot/pull/47401#issuecomment-2465407613)
* SMAA 1x (coming Godot 4.5)
  * less blurriness than FXAA
  * not available in Compatibility renderer
  * is an alternative to FXAA, and therefore cannot be combined
  * does _not_ use temporal antialiasing
  * modern games are often full of geometric and specular detail that SMAA can't do anything about
  * use this, if you can't afford MSAA, but find FXAA too blurry
  * combine with TAA to maximaize antialiasing, though it adds some blurriness and is fairly expensive
    * using just FSR2 might actually result in a better result and is [cheaper](https://github.com/godotengine/godot/issues/61905)
* MSAA
  * quite expensive, when used with GIProbe or SDFGI
  * doesn't smooth out aliasing that originates from fragment shaders (such as specular aliasing)
  * recommended additions:
    * [alpha antialiasing](https://docs.godotengine.org/en/stable/tutorials/3d/standard_material_3d.html#doc-standard-material-3d-alpha-antialiasing)
    * [Screen-space roughness limiter](https://docs.godotengine.org/en/stable/tutorials/3d/3d_antialiasing.html#screen-space-roughness-limiter)
* TAA
  * uses temporal antialiasing
  * handles specular and shader-induced aliasing much better than SMAA
  * [is currently not very performant](https://github.com/godotengine/godot/issues/61905)
* FSR2
  * uses temporal antialiasing
  * at native resolution: provides high-quality antialiasing with a sharper result compared to native TAA
  * high cost
* SSAA
  * antialiases edges, transparency and specular aliasing at the same time, without introducing potential ghosting artifacts
  * extremely high cost


special mentions
* Screen-space roughness limiter (turned on by default):
  * reduces specular aliasing
  * works best on detailed geometry



about temporal antialiasing
* works wonders against specular aliasing (used in games with realistic graphics)
* produces by design ghosting artifacts
* does not mesh well with [terrains](https://github.com/TokisanGames/Terrain3D/issues/302) currently, though FSR2 does a better job than TAA. Might be fixed in [Godot 4.5](https://github.com/godotengine/godot/pull/105437).


Sources
* https://docs.godotengine.org/en/latest/tutorials/3d/3d_antialiasing.html
* https://github.com/godotengine/godot-proposals/issues/2779

