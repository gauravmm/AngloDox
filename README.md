# AngloDox Keyboard

This is yet another split keyboard with the following features:

- Per-key RGB LEDs
- Separate function row
- RC-circuit debouncing for each key (so you can swap switches while the keyboard is on!)
- Kailh hot-swap sockets
- RJ11 connectors for the halves
- Key polling via shift register instead of matrix
- Entirely manufacturable by JLCPCB, using basic parts as much as possible.

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

### Design Notes

Component selection is driven by what is available on JLCPCB as a basic part, making construction as cheap as possible. The only extended parts are the USB ESD protection circuit and the RJ11 connectors.

#### Crystal Design

We choose a [generic 12MHz oscillator](https://jlcpcb.com/partdetail/YangxingTech-X322512MSB4SI/C9002) as it is the cheapest basic part available on LCSC. As the load capacitance is 20pF, and the stray capacitance is assumed to be 5pF, we need a pair of 30pF capacitors to get the right frequency. (See [section 3.3 of the application note](https://www.st.com/resource/en/application_note/cd00221665-oscillator-design-guide-for-stm8af-al-s-stm32-mcus-and-mpus-stmicroelectronics.pdf) for the derivation.)

#### ESD Protection

For peace of mind, we add ESD protection to the USB port.

1. Use a [generic bi-directional 6V TVS diode](https://jlcpcb.com/partdetail/Liown-SMF60CA/C2990473) between `VBUS` and `GND`.
2. Use a [USB-specific ESD protection circuit](https://jlcpcb.com/partdetail/Stmicroelectronics-USBLC62SC6/C7519) on the data lines. This is one of the few extended parts we use.
3. Leave the shield pin disconnected, allowing the host to absorb the ESD.

This is based on the suggestions in [Phil's Lab tutorial](https://youtu.be/C7-8nUU6e3E?t=3842).
