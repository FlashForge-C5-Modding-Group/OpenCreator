# Attributions to OpenSource Projects:
We've used some open source projects, here's the ones we've used, this isn't fully finished.

| Project | Use | License |
|---|---|---|
| Klipper | MCU & Motor Control / Control | [GPLv3](https://github.com/klipper3d/klipper?tab=GPL-3.0-1-ov-file) |
| Kalico | Same as Klipper | [GPLv3](https://github.com/KalicoCrew/kalico) |
| AFC | Toolchanger and Filament Setting | [GPLv3](https://github.com/AFCProject/AFC-Klipper-Add-On?tab=GPL-3.0-1-ov-file) |
| GrumpyScreen | Touchscreen | [GPLv3](https://github.com/pellcorp/grumpyscreen) |
| Moonraker | API server for the web interfaces and slicers | [GPLv3](https://github.com/Arksine/moonraker) |
| Fluidd | Web interface | [GPLv3](https://github.com/fluidd-core/fluidd) |
| Mainsail | Web interface | [GPLv3](https://github.com/mainsail-crew/mainsail) |
| Moonraker Timelapse | Print timelapses | [GPLv3](https://github.com/mainsail-crew/moonraker-timelapse) |
| Zensical | Builds this documentation site | [MIT](https://github.com/zensical/zensical) |
| Material for MkDocs | The site's theme and template overrides | [MIT](https://github.com/squidfunk/mkdocs-material) |
| CleanFFC5P | A cleanup script for removing old G-Code, mods, and similar things | [MIT](https://github.com/d3dorsett/CleanFlashforgeCreator5Pro?tab=MIT-1-ov-file) |
| HelixScreen | Optional screen which some people may like better than Grumpy | [GPLv3](https://github.com/prestonbrown/helixscreen?tab=GPL-3.0-1-ov-file) |

We've also used a non open source project, which should be.<br>
FlashForge's MCU and Touchscreen research was done.

## Research and inspiration

Work we learned from without using its code:

- **Prusa Research, phase stepping** ([blog post](https://blog.prusa3d.com/phase-stepping-how-we-busted-vibrations-and-improved-print-quality-on-the-xl-printer-with-just-a-firmware-update_94793),
  [help article](https://help.prusa3d.com/article/phase-stepping_673616),
  [Prusa-Firmware-Buddy](https://github.com/prusa3d/Prusa-Firmware-Buddy), GPLv3).
  Their finding that each stepper motor has its own ripple, and that a per-motor correction calibrated with
  the toolhead accelerometer on diagonal moves can cancel most of it, is the idea behind
  [Quieter motors](motor-noise.md). Our version runs on the Creator 5's own motor control (MCLib) rather than
  Trinamic drivers, keeps Klipper's steps, and calibrates a correction per speed; see that page for how it
  differs.

## Current Contributions

- @ano.space --- MCU Firmware, some Klippy bugfixes
- @cartri --- Klippy Modules and some MCU modifications

## Special Thanks

- @pappicio0000 --- first outside tester, and the first to test OpenCreator on the Creator 5 (non-Pro).
- @ChrisBBaken4536 --- another tester, and first to mess around on older firmwares, and sacrificed a nozzle (RIP).
- @Null__f --- another tester which found some small bugs.
- @spmomeiette --- another tester which found small critiques.
- @Qba$ --- another tester, added support for rk3318.
- @hanabiratachi --- fixing display flickering.

### There were both human and AI use.
