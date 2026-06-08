# Joystick arrow keys (Ferris Sweep)

Analog joystick → arrow keys, integrated into your existing `cradio.keymap`.

**Default in this folder:** joystick on the **LEFT** half (USB / central).  
If yours is on the **RIGHT** half, see [Joystick on the right half](#joystick-on-the-right-half).

## What changed

| File | Purpose |
|------|---------|
| `config/west.yml` | Adds `zmk-analog-input-driver` + `zmk-input-processor-keybind` |
| `config/cradio.conf` | Enables ADC / analog input |
| `config/cradio_left.overlay` | Reads stick + sends arrow keys (left-half setup) |

Your keymap (`cradio.keymap`) is **unchanged**.

## Wiring (nice!nano v2)

| Joystick | Pin |
|----------|-----|
| VCC | 3V3 |
| GND | GND |
| VRx | P0.02 |
| VRy | P0.29 |

Use **3.3V only**.

## Push to GitHub

Copy this folder into https://github.com/manfad/zmk-config (or commit from here), then push `main`.

## Build and flash

**Current `build.yaml`:** left half only (faster CI while testing the joystick).

1. GitHub Actions builds after push
2. Download the firmware `.zip` from Actions
3. Flash **left** half only: `*cradio_left*` uf2
4. Bootloader: double-tap **RESET** → copy `.uf2` to `NICENANO` drive
5. Test with left half on USB (right half not needed for arrow-key test)

When the joystick works, uncomment `cradio_right` in `build.yaml`, rebuild, and flash the right half.

## Test

1. Connect left half to PC (USB) or Bluetooth as usual
2. Open Notepad, move joystick → cursor should move

## Tuning (`cradio_left.overlay`)

| Setting | Effect |
|---------|--------|
| `mv-deadzone` | Bigger = less drift (try `100`) |
| `scale-divisor` | Bigger = less sensitive (try `60`) |
| `invert` on `y-ch` | Flip up/down |
| `tick` / `threshold` | Key repeat speed / how hard you must push |

## Joystick on the right half

1. Rename `cradio_left.overlay.split-central` → `cradio_left.overlay`
2. Rename `cradio_right.overlay.joystick-on-right` → `cradio_right.overlay`
3. Rebuild and flash **both** halves

## Note on MOUSE layer

Your keymap has a MOUSE layer with `&mmv`. The joystick sends arrows on all layers. If that feels wrong in MOUSE layer, we can limit arrows to specific layers.
