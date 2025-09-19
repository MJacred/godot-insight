
# Issues

* [Workflow Issue Tracker](https://github.com/godotengine/godot/issues/63780)

# General

https://github.com/godotengine/godot-proposals/issues/7085

* One shot particles
  * The only thing one_shot does is uncheck emitting. That is the entire purpose of that checkbox
  * Once you emit a one shot particle system, you need to wait until the particle system stops processing before toggling emitting will restart the particles.
    * https://github.com/godotengine/godot-proposals/issues/7322
  * https://github.com/godotengine/godot/issues/93991
  * https://docs.godotengine.org/en/stable/classes/class_gpuparticles3d.html#class-gpuparticles3d-property-emitting
  * https://docs.godotengine.org/en/stable/classes/class_gpuparticles3d.html#class-gpuparticles3d-method-restart
    * clears existing particles immediately, wait for finished if you don't want that
  * https://github.com/uzkbwza/BurstParticles2D
* stop/pause particles: set speed_scale to 0 with a timer, else the particles won't be drawn
  * restart: If keep_seed is true, the current random seed will be preserved. Useful for seeking and
  * https://github.com/godotengine/godot/pull/92089
  * https://docs.godotengine.org/en/stable/classes/class_gpuparticles3d.html#class-gpuparticles3d-property-preprocess
  * https://docs.godotengine.org/en/stable/classes/class_gpuparticles3d.html#class-gpuparticles3d-method-request-particles-process


# 3D Particles


## On GPU



## On CPU



# 2D Particles




