# proper way?

requires: https://github.com/godotengine/godot-proposals/issues/1298


# alternatives

https://www.reddit.com/r/godot/comments/u2yssh/today_i_learned_how_to_do_character_silhouette/


    assign standart Spatial Material to your object

    go to Flags

    enable Unshaded

    enable No Depth Test

    go to Albedo and set Color that you wish to see through wall

    click on Next Pass slot

    assign new Spatial Material to that slot

    set Render Priority to 1

    go to Flags

    enable Transparent

    go to Parameters

    set Depth Draw Mode to Opaque Pre-Pass (otherwise you don't get shadows)


ooooor


    Disable depth-test for the character

    Render it after the environment (with priority I assume)

    Pass the depth values from vertex shader to the fragment shader

    Read from the depth buffer and compare the values

    Depending on the comparison draw the first or the second material (also write the depth values to the depth buffer if necessary).


https://github.com/godotengine/godot-proposals/issues/496

see: https://github.com/GDQuest/godot-shaders/tree/master/godot/Shaders
