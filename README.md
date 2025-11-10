<p align="center">
<img src="buildroot/share/pixmaps/logo/flyingMarlin.png" alt="FlyingBear's logo" />
</p>

# Marlin for FlyingBear Ghost 6

This repository is a focused Marlin build for the FlyingBear Ghost 6. It tracks upstream Marlin and layers printer-specific tuning plus a handful of build-time options for common hardware upgrades.

## What’s Included

- Tuned configuration for printer
- PlatformIO environments in `ini/stm32f4.ini` for board-specific builds and feature variants
- Optional build flags for BLTouch and fixed-time motion out of the box

Upstream Marlin sources live under `Marlin/`; only the configuration directory and PlatformIO metadata are tailored for the Ghost 6.

## Variants

| Variant | PlatformIO env | Build flags | Notes |
| ------- | -------------- | ----------- | ----- |
| Stock Ghost 6 | `env:mks_nano4_v3_1` | _(none)_ | Baseline profile matching the factory motion system |
| FT Motion | `env:mks_nano4_v3_1_ft_motion` | `-DFT_MOTION` | Enables faster acceleration / jerk profile |
| BLTouch | `env:mks_nano4_v3_1_bltouch` | `-DBLTOUCH` | Adds BLTouch probe support |
| BLTouch + FT Motion | `env:mks_nano4_v3_1_bltouch_ft_motion` | `-DBLTOUCH -DFT_MOTION` | Combines both feature sets |

Prefer the prebuilt environments when possible. To stay on a single environment, export your own flags:

```
export PLATFORMIO_BUILD_FLAGS="-DBLTOUCH -DFT_MOTION"
platformio run -e mks_nano4_v3_1
```

Omit any macros you don’t need. You can also append standard Marlin overrides, e.g. `-DMOTHERBOARD=BOARD_MKS_ROBIN_NANO_V3`.

## Build Workflow

1. Install [Visual Studio Code](https://code.visualstudio.com/) with the [PlatformIO extension](https://platformio.org/).
2. Clone this repository and open it in VS Code.
3. Select the desired PlatformIO environment:
   - Command Palette → `PlatformIO: Set Project Task` → `<env>: Build`
   - or run `platformio run -e <env>` from a shell
4. The compiled binary appears at `.pio/build/<env>/firmware.bin`.

## Flashing the Board

1. Copy `firmware.bin` to a FAT32-formatted SD card.
2. Power off the printer and insert the card into the control board.
3. Power on the printer. The bootloader flashes the firmware and renames the file to indicate success.

Refer to the FlyingBear documentation for board-specific recovery steps if the flash fails.

## Repository Layout

```
Marlin/                # Ghost 6 configuration bundle
ini/stm32f4.ini        # PlatformIO environments for this printer
buildroot/             # Helper scripts inherited from Marlin
```

## Contributing & Support

Bug fixes and tuning tweaks are welcome. Focus issues and pull requests on the Ghost 6; upstream Marlin topics belong in the [main Marlin repository](https://github.com/MarlinFirmware/Marlin).

For general Marlin help, visit:

- [marlinfw.org](https://marlinfw.org/) – official docs
- [Marlin Discord](https://discord.gg/marlinfirmware) – community support

## License

This project remains under the [GPLv3 license](LICENSE) inherited from Marlin. Distribute any binaries with the matching source, including your local modifications.
