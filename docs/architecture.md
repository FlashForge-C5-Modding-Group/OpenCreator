# Architecture

The full custom firmware effort has layers *(like an onion!)*. The printer's Linux host runs
Klipper and web services. Four MCU boards perform motion, extrusion, heating,
and sensing. AFC models the removable tools and filament state. Printer-side
configuration ties those parts together.

| Layer | Current development role | Source |
| --- | --- | --- |
| Linux host | Runs the Creator 5 Klipper fork, Moonraker, and the configured web interface. The printer is MIPS32 (Ingenic X2600 / Xburst 2 with 512MB of RAM); host needs compatible Python dependencies and the Klipper C helper. | [klipper-c5](https://github.com/FlashForge-C5-Modding-Group/klipper-c5) |
| Mainboard MCU | Handles the main motion and machine I/O through the GD32 port. Chip is a `gd32h737vgt6`. Also uses a "MCLib" motion library for complex stuff like VFA compensation. | `klipper-c5/src/c5_mainboardgd.c` and related target code, `mclib.py` |
| eboard MCU | Drives the shared physical extruder motor and supplies the pressure-advance measurement interface. Chip is `n32g455ccl7`. | `klipper-c5/src/c5_eboard.c` and `klippy/extras/pa_adjust.py` |
| Heaterboard MCU | Handles tool heater and temperature I/O. Chip is `n32g455rel7`. | `klipper-c5/src/c5_heaterboard.c` |
| Levelboard MCU | Supplies probe and levelboard-specific sensing used by calibration. Chip is `n32g430f8s7`. | `klipper-c5` MCU and Klippy levelboard code |
| Toolchanger and AFC | Uses physical dock and grab inputs for T0 to T3 selection. AFC standalone provides filament and tool mapping, while `creator5_toolchanger.py` owns the machine-specific motion and safety checks. | `klipper-c5/klippy/extras` |
| Printer filesystem | Holds the Creator 5 configuration, AFC configuration, scripts, and printer data layout. | [opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs) |

## Printer-side layout

The development configuration is organized below `/usr/data/config` on the
printer. `printer.base.cfg` includes the Creator 5, probe, motor, filament,
vibration, Misc, and AFC files. G-code storage is configured at
`/usr/data/gcodes`. Stock-format calibration data remains under
`/usr/data/firmwareRes/config`, including `extruder.json`, `zoffset.json`, and
`test.json`. The host applies those saved measurements rather than treating a
successful probe command as a complete nozzle calibration.

There is one physical extrusion motor. The four Klipper extruder contexts
select the corresponding tool heaters and state; they are not four independent
extrusion drives. A toolchange must agree with the dock and grab pins before
the selected logical extruder becomes active.

## Interfaces

The printer configuration currently uses AFC standalone mode, not a BoxTurtle
mechanism. Mainsail exposes virtual Misc switches for flow calibration and
purge. Touchscreen support is a separate interface concern and must not be
assumed to make an unverified machine workflow safe. GrumpyScreen work is
separate from the core Klipper and filesystem repositories.

The OpenCreator Installer development tree is a direct-framebuffer application
and preflight framework. It does not yet
install a CFW payload or flash the MCUs. See [Installation boundaries](installation-and-safety.md).
