# AngloDox Keyboard

This is a modified version of the ErgoDox keyboard with the following changes:

- Per-key RGB LEDs
- Separate function row
- Debouncing for each key
- Hot-swap sockets
- RJ11 connectors for the halves
- Key polling via shift register.

And entirely manufacturable by JLCPCB!

Thanks to the debouncing, you can swap the switches while the keyboard is on.

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
