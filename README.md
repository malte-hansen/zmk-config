# Tukon ZMK firmware

This repository contains the ZMK firmware configuration for Tukon, a 36-key
wireless split keyboard with a nice!nano v2 controller in each half. The left
half is the central and connects to the host over USB or Bluetooth. The right
half sends key events to the left.

## Build and flash

1. Open [GitHub Actions](https://github.com/malte-hansen/zmk-config/actions) and
   select a successful build for the commit you want. The workflow runs on
   pushes and pull requests and can also be started manually.
2. Download and extract the firmware artifact. Use the UF2 for
   `tukon_left` on the left controller and the UF2 for `tukon_right` on the
   right controller. Both targets use `nice_nano_v2` in [`build.yaml`](build.yaml).
3. Connect one half over USB and double-tap its reset button to enter the
   bootloader. Copy that half's UF2 to the USB drive that appears. The controller
   restarts after flashing. Repeat for the other half.
4. Power both halves and reset them at about the same time so they can connect.
   Test both halves with the left controller connected over USB, or pair
   **Tukon** in your host's Bluetooth settings.

For more detail, see the [ZMK installation guide](https://zmk.dev/docs/user-setup#installing-the-firmware).

## Keymap

The default `mac` layer uses a Colemak-style layout with hold-tap modifiers.
For example, tap **A** to type A or hold it for Control. The custom hold-tap
behavior is tap-preferred with a 150 ms tapping term.

| Hold this key on `mac` | Layer                 | Main bindings                                                    |
| ---------------------- | --------------------- | ---------------------------------------------------------------- |
| Space                  | `symbols`             | Punctuation, brackets, and operators                             |
| Backspace              | `numpad`              | Digits on the right and arrows on the left                       |
| Enter                  | `navigation`          | Arrows, Page Up/Down, Home/End, and word movement                |
| Tab                    | `function and config` | F1–F15, media controls, Bluetooth profiles, and output selection |
| Escape                 | `window`              | Shortcuts for Mac window management                              |

Press the **Tab** and **Enter** thumb positions together within 50 ms for
Caps Lock. On `function and config`, the first four keys on the top left
clear the current Bluetooth profile's pairing, select the previous profile,
toggle USB/Bluetooth output, and select the next profile, respectively.

## Customize

| File                                                                                            | What to edit                                                                      |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| [`tukon.keymap`](boards/shields/tukon/tukon.keymap)                                             | Layer bindings, hold-tap behavior, and combos                                     |
| [`config/tukon.conf`](config/tukon.conf)                                                        | Idle timeout, Bluetooth options, OLED widgets, and RGB settings                   |
| [`tukon.dtsi`](boards/shields/tukon/tukon.dtsi) and the [shield overlays](boards/shields/tukon) | Matrix wiring, per-half mapping, and peripheral hardware                          |
| [`Kconfig.defconfig`](boards/shields/tukon/Kconfig.defconfig)                                   | Shield defaults, including split roles and peripheral features                    |
| [`config/west.yml`](config/west.yml)                                                            | Firmware source, currently the `power-merge-main` branch of the `fouf96/zmk` fork |

The shield enables automatic peripheral power management and the SSD1306 OLED
by default. The user configuration sets a 10-second idle timeout. The shield
defaults to RGB off. To enable the per-key WS2812 LEDs, set
`CONFIG_TUKON_RGB_UNDERGLOW=y` in `config/tukon.conf`.
