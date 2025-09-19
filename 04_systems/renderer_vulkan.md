integrated GPU doesn't fully support Vulkan. (Intel IGPs only fully support Vulkan from Broadwell onwards.)

Intel Haswell IGP, since it claims to support Vulkan but is unusable in practice.

[source](https://github.com/godotengine/godot/issues/42348#issuecomment-699527916)



You can change the PRIME GPU in the NVIDIA Control Panel, but this requires you to log out and back in to make the change effective. Once you do this, the dedicated GPU will be used for everything, which decreases battery life significantly.

Unfortunately, NVIDIA Optimus on Linux has never been as seamless as it is on Windows (where it's still far from ideal).


[source](https://github.com/godotengine/godot/issues/42348#issuecomment-699544499)



If you have several displays and some of them are connected to the motherboard's video outputs, they will use the IGP instead of the dedicated graphics card.

[source](https://github.com/godotengine/godot/issues/42348#issuecomment-699545825)

