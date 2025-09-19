## Joypads

Godot relies in most cases on https://github.com/mdqinc/SDL_GameControllerDB to get the correct mapping from a technical code, which the joypad provides via a driver to Godot, to the abstract key names all joypads share.  
If there's no known mapping for joypad X, then Godot can still pass on a button press, but it might mix up some buttons.

If your joypad's firmware is outdated, it will also lead to incorrect mappings, [see the documentation](https://docs.godotengine.org/en/stable/tutorials/inputs/controllers_gamepads_joysticks.html#my-controller-has-incorrectly-mapped-buttons-or-axes).

To test which code belongs to which button/axis, use a helper tool such as [Godot joypads demo](https://godotengine.org/asset-library/asset/2785), [jstest-gtk](https://github.com/Grumbel/jstest-gtk/), or https://hardwaretester.com/gamepad. The former is preffered (and can be installed using the package manager on Linux).

The required mapping format looks like this:
`030000006d04000019c2000011010000,Logitech F710,a:b1,b:b2,back:b8,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,dpup:h0.1,leftshoulder:b4,leftstick:b10,lefttrigger:b6,leftx:a0,lefty:a1,rightshoulder:b5,rightstick:b11,righttrigger:b7,rightx:a2,righty:a3,start:b9,x:b0,y:b3,platform:Linux,`

Example: `leftshoulder:b4`  
`leftshoulder` is the abstract key name. And `b4` is the code the joypad sends.  
So if the player uses `b4` on their joypad, your computer knows that the `leftshoulder` is pressed.

After you have your mapping, make a pull request in https://github.com/mdqinc/SDL_GameControllerDB ([example](https://github.com/mdqinc/SDL_GameControllerDB/pull/809)).

And then make another pull request in Godot ([example](https://github.com/godotengine/godot/pull/99304)).


More infos on joypads: https://docs.godotengine.org/en/stable/tutorials/inputs/controllers_gamepads_joysticks.html
