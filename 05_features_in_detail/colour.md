https://www.youtube.com/watch?v=uMheYNOdl5c

gradient + gradienttexture2d

b5dfce
60989d
3e5560
252b2e


interpolation
* mode: cubic
* color space: oklab

fill
* radial


todo
* https://github.com/godotengine/godot/pull/107249
* https://github.com/godotengine/godot/pull/103583

## HDR

texture LUT for tonemapping only works with SDR output and is fundamentally incompatible with the EDR paradigm.
perform custom tonemapping with a custom compositor?
[source](https://github.com/godotengine/godot-proposals/issues/12728#issuecomment-3045364563)


Only the Linear and Reinhard tonemappers extend into the additional headroom of the display. This is due to the Flimic ACES, and AgX tonemappers being designed for SDR displays. Updating them to output to an HDR range is not trivial. AgX will be extended into HDR in a future PR.
HDR output requires viewports to enable hdr_2d to output the additional dynamic range needed for HDR displays. Blending, glow, color correction, brightness, contrast, and saturation adjustments will look different when hdr_2d is enabled. hdr_2d will automatically be enabled for windows that turn on HDR output, which may result in a change in appearance of the scene. It's advised that applications planning to support HDR output create their content with hdr_2d enabled, even when HDR output is disabled.
[source](https://github.com/godotengine/godot/pull/94496)

Importantly, Flimic and ACES tonemappers cannot support HDR output. This is because the white parameter of these tonemappers was designed exclusively for SDR and this style of white parameter cannot work with HDR and variable Extended Dynamic Range (EDR).

Compatible tonemappers:
* AgX
  * some parameters (specifically `contrast`) can be used to make it appear similar to ACES, but with correct HDR output support.
  * downsides:
    * desaturate colours to white as they become very bright
* **Adjustable** tonemapper
  * https://allenwp.com/blog/2025/05/29/allenwp-tonemapping-curve/
  * It's configuration parameters match AgX and it can be configured to appear similar to Filmic in SDR.
  * strengths
    * Unlike AgX, this tonemapper does not desaturate colours to white as they become very bright
    * applies the tone curve directly in the linear sRGB working colour space, just like Filmic does.


[source](https://github.com/godotengine/godot/pull/106696)
