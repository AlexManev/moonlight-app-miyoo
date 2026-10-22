# Patches

## sdl2_miyoo-gamepad-mode.patch

Source for the gamepad support in `moonlight/lib/libSDL2-2.0.so.0`.

It applies to [XK9274/sdl2_miyoo](https://github.com/XK9274/sdl2_miyoo/tree/moonlight) at commit
`50fc717` ("Move moonlight SDL2 to this repo"), which is the commit the v1.5.1 library was built from.

```sh
git clone -b moonlight https://github.com/XK9274/sdl2_miyoo
cd sdl2_miyoo
git checkout 50fc717
git am /path/to/sdl2_miyoo-gamepad-mode.patch
```

Then build it with the Miyoo Mini toolchain as usual and copy the resulting
`libSDL2-2.0.so.0` to `moonlight/lib/`. The moonlight binary itself does not need to be rebuilt:
it already forwards SDL game controllers to the host.

What the patch does:

- `src/joystick/mmiyoo/SDL_joystick_mmiyoo.c`: the joystick driver was a stub. It now reports the
  Miyoo keypad as an Xbox style controller (with a built-in game controller mapping) while gamepad
  mode is active.
- `src/video/mmiyoo/SDL_event_mmiyoo.c`: adds a gamepad mode next to the keypad and mouse modes.
  In gamepad mode the buttons are no longer sent as keyboard keys. The mode switches on when the
  app opens the controller (moonlight does this when a stream starts) and `Select + X` toggles it.
- `src/cfg/SDL_moonlightcfg_mmiyoo.c`: reads the `gamepad` block from `config/settings.json`.
