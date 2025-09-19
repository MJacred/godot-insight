
https://godotengine.org/asset-library/asset/2785
A tool for testing joypad input and generating controller mapping strings.

See documentation: https://docs.godotengine.org/en/latest/tutorials/inputs/controllers_gamepads_joysticks.html

If any key mapping is incorrect or the values are abnormal, please provide feedback. Missing key codes are welcome to be added too. You can use the evtest/evemu-record command line tool to get the specific key code.

https://docs.kernel.org/input/gamepad.html#linux-gamepad-specification
https://docs.kernel.org/input/event-codes.html#input-event-codes

https://github.com/godotengine/godot/pull/95486


_Keycode Mapping Table_

FIXME: 2 column table START

[JoyButton](https://github.com/godotengine/godot/blob/06fbc8395b3c0ec6fa38588caea2ee94837f7b97/core/input/input_enums.h#L62-L84) / [JoyAxis](https://github.com/godotengine/godot/blob/06fbc8395b3c0ec6fa38588caea2ee94837f7b97/core/input/input_enums.h#L50C12-L57) 	[input-event-codes.h](https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h)
`JoyButton.A` 	`BTN_A`[1](#user-content-fn-1-a8b3960b80778c2bd04b6b7e40fa11c1) (`BTN_SOUTH`/`BTN_GAMEPAD`[2](#user-content-fn-2-a8b3960b80778c2bd04b6b7e40fa11c1))
`JoyButton.B` 	`BTN_B`[1](#user-content-fn-1-a8b3960b80778c2bd04b6b7e40fa11c1) (`BTN_EAST`)
`JoyButton.X` 	`BTN_X`[1](#user-content-fn-1-a8b3960b80778c2bd04b6b7e40fa11c1) (`BTN_NORTH`)
`JoyButton.Y` 	`BTN_Y`[1](#user-content-fn-1-a8b3960b80778c2bd04b6b7e40fa11c1) (`BTN_WEST`)
`JoyButton.BACK` 	`BTN_SELECT`
`JoyButton.GUIDE` 	`BTN_MODE`
`JoyButton.START` 	`BTN_START`
`JoyButton.LEFT_STICK` 	`BTN_THUMBL`
`JoyButton.RIGHT_STICK` 	`BTN_THUMBR`
`JoyButton.LEFT_SHOULDER` 	`BTN_TL`
`JoyButton.RIGHT_SHOULDER` 	`BTN_TR`
`JoyButton.DPAD_UP` 	`BTN_DPAD_UP`[3](#user-content-fn-3-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.DPAD_DOWN` 	`BTN_DPAD_DOWN`[3](#user-content-fn-3-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.DPAD_LEFT` 	`BTN_DPAD_LEFT`[4](#user-content-fn-3.1-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.DPAD_RIGHT` 	`BTN_DPAD_RIGHT`[4](#user-content-fn-3.1-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.MISC1`[5](#user-content-fn-6-a8b3960b80778c2bd04b6b7e40fa11c1) 	`KEY_RECORD` / `BTN_Z`[6](#user-content-fn-4-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.PADDLE1` 	[7](#user-content-fn-5-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.PADDLE2` 	[7](#user-content-fn-5-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.PADDLE3` 	[7](#user-content-fn-5-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.PADDLE4` 	[7](#user-content-fn-5-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyButton.TOUCHPAD` 	[7](#user-content-fn-5-a8b3960b80778c2bd04b6b7e40fa11c1)
`JoyAxis.LEFT_X` 	`ABS_X` 
`JoyAxis.LEFT_Y` 	`ABS_Y`
`JoyAxis.RIGHT_X`[5](#user-content-fn-6-a8b3960b80778c2bd04b6b7e40fa11c1) 	`ABS_RX` / `ABS_Z`
`JoyAxis.RIGHT_Y`[5](#user-content-fn-6-a8b3960b80778c2bd04b6b7e40fa11c1) 	`ABS_RY` / `ABS_RZ`
`JoyAxis.TRIGGER_LEFT`[5](#user-content-fn-6-a8b3960b80778c2bd04b6b7e40fa11c1) 	`ABS_BRAKE` / `ABS_Z` / `BTN_TL2`
`JoyAxis.TRIGGER_RIGHT`[5](#user-content-fn-6-a8b3960b80778c2bd04b6b7e40fa11c1) 	`ABS_GAS` / `ABS_RZ` / `BTN_TR2`

FIXME: 2 column table END

**Note**: If the final mapped keycodes have no corresponding buttons/analog-sticks, they will be mapped to `-1`( `JoyButton.INVALID`/ `JoyAxis.INVALID`).
## Footnotes

    1. While the _Linux Gamepad Specification_ recommends reporting events based on physical position, I don't have a gamepad that follows this convention. Perhaps reporting events by corresponding label name is a de facto convention. [↩](#user-content-fnref-1-a8b3960b80778c2bd04b6b7e40fa11c1) [↩2](#user-content-fnref-1-2-a8b3960b80778c2bd04b6b7e40fa11c1) [↩3](#user-content-fnref-1-3-a8b3960b80778c2bd04b6b7e40fa11c1) [↩4](#user-content-fnref-1-4-a8b3960b80778c2bd04b6b7e40fa11c1)

    2. `BTN_GAMEPAD` is used to identify a gamepad. See the [Detection](https://docs.kernel.org/input/gamepad.html#detection) section of the _Linux Gamepad Specification_. Devices without this keycode will not be automatically mapped. [↩](#user-content-fnref-2-a8b3960b80778c2bd04b6b7e40fa11c1)

    3. ~These are the key codes for the digital buttons. But godot will automatically map the key codes (`ABS_HAT0X`/`ABS_HAT0Y`) for the analog buttons to these `JoyButton`s, and godot doesn't seem to detect the `EV_SYN` event, so a gamepad that reports both digital and analog buttons on one button might be a bit of a hassle. I don't have such a device.~ Only mapped if `ABS_HAT0Y` is not present. [↩](#user-content-fnref-3-a8b3960b80778c2bd04b6b7e40fa11c1) [↩2](#user-content-fnref-3-2-a8b3960b80778c2bd04b6b7e40fa11c1)

    4. Only mapped if `ABS_HAT0X` is not present. [↩](#user-content-fnref-3.1-a8b3960b80778c2bd04b6b7e40fa11c1) [↩2](#user-content-fnref-3.1-2-a8b3960b80778c2bd04b6b7e40fa11c1)

    5. There does not seem to be a consistent convention for these axes. If the analog-sticks that issue the key code do not exist, they will try the next. [↩](#user-content-fnref-6-a8b3960b80778c2bd04b6b7e40fa11c1) [↩2](#user-content-fnref-6-2-a8b3960b80778c2bd04b6b7e40fa11c1) [↩3](#user-content-fnref-6-3-a8b3960b80778c2bd04b6b7e40fa11c1) [↩4](#user-content-fnref-6-4-a8b3960b80778c2bd04b6b7e40fa11c1) [↩5](#user-content-fnref-6-5-a8b3960b80778c2bd04b6b7e40fa11c1)

    6. Not sure, there is only one unofficial device. [↩](#user-content-fnref-4-a8b3960b80778c2bd04b6b7e40fa11c1)

    7. There is no such gamepad around me. But in order to prevent unexpected behavior, they are eventually mapped to `JoyButton.INVALID`. [↩](#user-content-fnref-5-a8b3960b80778c2bd04b6b7e40fa11c1) [↩2](#user-content-fnref-5-2-a8b3960b80778c2bd04b6b7e40fa11c1) [↩3](#user-content-fnref-5-3-a8b3960b80778c2bd04b6b7e40fa11c1) [↩4](#user-content-fnref-5-4-a8b3960b80778c2bd04b6b7e40fa11c1) [↩5](#user-content-fnref-5-5-a8b3960b80778c2bd04b6b7e40fa11c1)






```
$ evtest
No device specified, trying to scan all of /dev/input/event*
Not running as root, no devices may be available.
Available devices:
/dev/input/event24:	Zikway HID gamepad
Select the device event number [0-24]: 24
Input driver version is 1.0.1
Input device ID: bus 0x3 vendor 0x3537 product 0x1041 version 0x111
Input device name: "Zikway HID gamepad"
Supported events:
  Event type 0 (EV_SYN)
  Event type 1 (EV_KEY)
    Event code 114 (KEY_VOLUMEDOWN)
    Event code 115 (KEY_VOLUMEUP)
    Event code 116 (KEY_POWER)
    Event code 304 (BTN_SOUTH)
    Event code 305 (BTN_EAST)
    Event code 306 (BTN_C)
    Event code 307 (BTN_NORTH)
    Event code 308 (BTN_WEST)
    Event code 309 (BTN_Z)
    Event code 310 (BTN_TL)
    Event code 311 (BTN_TR)
    Event code 312 (BTN_TL2)
    Event code 313 (BTN_TR2)
    Event code 314 (BTN_SELECT)
    Event code 315 (BTN_START)
    Event code 316 (BTN_MODE)
    Event code 317 (BTN_THUMBL)
    Event code 318 (BTN_THUMBR)
    Event code 319 (?)
  Event type 3 (EV_ABS)
    Event code 0 (ABS_X)
      Value    128
      Min        0
      Max      255
      Flat      15
    Event code 1 (ABS_Y)
      Value    128
      Min        0
      Max      255
      Flat      15
    Event code 2 (ABS_Z)
      Value    128
      Min        0
      Max      255
      Flat      15
    Event code 5 (ABS_RZ)
      Value    128
      Min        0
      Max      255
      Flat      15
    Event code 9 (ABS_GAS)
      Value      0
      Min        0
      Max      255
      Flat      15
    Event code 10 (ABS_BRAKE)
      Value      0
      Min        0
      Max      255
      Flat      15
    Event code 16 (ABS_HAT0X)
      Value      0
      Min       -1
      Max        1
    Event code 17 (ABS_HAT0Y)
      Value      0
      Min       -1
      Max        1
  Event type 4 (EV_MSC)
    Event code 4 (MSC_SCAN)
Properties:
Testing ... (interrupt to exit)
Event: time 1723648532.480617, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90001
Event: time 1723648532.480617, type 1 (EV_KEY), code 304 (BTN_SOUTH), value 1
Event: time 1723648532.480617, -------------- SYN_REPORT ------------
Event: time 1723648532.580616, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90001
Event: time 1723648532.580616, type 1 (EV_KEY), code 304 (BTN_SOUTH), value 0
Event: time 1723648532.580616, -------------- SYN_REPORT ------------
Event: time 1723648533.956625, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90002
Event: time 1723648533.956625, type 1 (EV_KEY), code 305 (BTN_EAST), value 1
Event: time 1723648533.956625, -------------- SYN_REPORT ------------
Event: time 1723648534.078621, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90002
Event: time 1723648534.078621, type 1 (EV_KEY), code 305 (BTN_EAST), value 0
Event: time 1723648534.078621, -------------- SYN_REPORT ------------
Event: time 1723648534.761624, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90004
Event: time 1723648534.761624, type 1 (EV_KEY), code 307 (BTN_NORTH), value 1
Event: time 1723648534.761624, -------------- SYN_REPORT ------------
Event: time 1723648534.878625, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90004
Event: time 1723648534.878625, type 1 (EV_KEY), code 307 (BTN_NORTH), value 0
Event: time 1723648534.878625, -------------- SYN_REPORT ------------
```


Note that if you get rid of mappings by blanking out gamecontrollerdb.txt and godotcontrollerdb.txt, gamepad names will be different for some controllers and mapping will be incorrect for the DualSense and Switch Pro controllers (square/triangle and Y/X are inverted, but everything else is OK).



First of all, this PR is only for devices that are recognized as gamepads by Linux. The device needs to be able to send BTN_SOUTH events.

Although Godot doesn't differentiate between gamepads and joysticks, there is still a distinction here.

"Joy-Con (R)" and "Xbox 360 Wireless Receiver (XBOX)" meet this condition. This PR will work.

This "USB Gamepad" would be recognized as a Joystick by Linux (BTN_TRIGGER).

Joy-Con (L) Non-gamepad not working is expected.
