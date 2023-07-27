# AngloDox Keyboard

This is yet another split keyboard with the following features:

- Per-key RGB LEDs
- Separate function row
- RC-circuit debouncing for each key
- Kailh hot-swap sockets
- RJ11 connectors for the halves
- Key polling via shift register instead of matrix

And entirely manufacturable by JLCPCB!

Thanks to the debouncing, you can swap switches while the keyboard is on.

### Keyboard Layout

The keyboard layout is specified in `keyboard-layout.json` and can be edited in [http://www.keyboard-layout-editor.com/#/](KLE). The current layout is based on the reference ANSI-104 layout, with a macro column on the left, a collapsed arrow cluster on the right, and added thumb clusters from the venerable Ergodox keyboard.

The current layout has 46 keys on the left and 54 on the right, for a total of 100 keys.

## Development

This relies on a combination of scripts and hand-routing. Here's the general workflow:

    1. Generate the basic circuitry as a hierarchical schematic in KiCad.
    2. Specify a key layout as a [http://www.keyboard-layout-editor.com/#/](KLE) JSON file.

### Regenerate KiCad Library

The Kicad library is generated using [https://github.com/TousstNicolas/JLC2KiCad_lib](JLC2KiCad_lib). To regenerate the library, run:

```bash
JLC2KiCadLib C1525 C25779 C36871 C965555 \
    -dir temp_jlc_import/
```

Then merge the symbol and footprint libraries into the main library manually. Remember to update the Value field of the symbols.

When setting up the Footprint library, set the nickname to `footprint`.
