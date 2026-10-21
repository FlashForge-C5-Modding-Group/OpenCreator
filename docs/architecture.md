# Architecture

The full custom firmware effort has layers <small>*(like an onion!)*</small> The printer's Linux host runs
Klipper and web services. Four MCU boards perform motion, extrusion, heating,
and sensing. AFC models the removable tools and filament state. Printer-side
configuration ties those parts together.

| Layer | Current development role | Source |
| --- | --- | --- |
| Linux host | Runs Kalico (the Creator 5 Klipper fork), Moonraker, and the configured web interface. The printer is MIPS32 (Ingenic X2600 / Xburst 2 with 512MB of RAM) | [kalico](https://github.com/FlashForge-C5-Modding-Group/kalico) |
| Mainboard MCU | Handles the main motion and machine I/O through the GD32 port. Chip is a `gd32h737vgt6`. Also uses a "MCLib" motion library for complex stuff like VFA compensation. | `kalico/src/c5_mainboardgd.c`, `c5_mclib.c` and related target code, `klippy/extras/mclib.py` |
| eboard MCU | Drives the shared physical extruder motor and supplies the pressure-advance measurement interface. Chip is `n32g455ccl7`. | `kalico/src/c5_eboard.c`, `c5_eboard_pa.c` and `klippy/extras/pa_adjust.py` |
| Heaterboard MCU | Handles tool heater and temperature I/O. Chip is `n32g455rel7`. | `kalico/src/stm32` N32G45x support (`C5_HEATERBOARD` build option) |
| Levelboard MCU | Supplies probe and levelboard-specific sensing used by calibration. Chip is `n32g430f8s7`. | `kalico/src/c5_levelboard.c` and Klippy levelboard code |
| Toolchanger and AFC | Uses physical dock and grab inputs for T0 to T3 selection. AFC standalone provides filament and tool mapping, while `creator5_toolchanger.py` owns the machine-specific motion and safety checks. | `kalico/klippy/extras` |

klipper-c5, the original host and MCU port, is archived: everything above now
lives in Kalico.
| Printer filesystem | Holds the Creator 5 configuration, AFC configuration, scripts, and printer data layout. | [opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs) |

## Printer-side layout

The development configuration is organized below `/usr/data/config` on the
printer. `printer.base.cfg` includes the Creator 5, probe, motor, filament,
vibration, Misc, and AFC files. G-code storage is configured at
`/usr/data/gcodes`. Stock-format calibration data remains under
`/usr/data/firmwareRes/config`, including `extruder.json`, `zoffset.json`, and
`test.json`. 

The Creator 5 has no chamber heater, chamber fans or aux fan, so those are in `printer.pro.cfg`. They're only
included on a Creator 5 Pro, through `printer.model.cfg`. On the printer, that file is written at every boot,
from the model in the stock boot script (`MACHINE=` in `/usr/prog/app_startup.sh`). In tunnel mode, the Pi
installer asks which printer it's connected to. On a Creator 5, `M106`/`M107 P2` and `P3` do nothing, and
`M191` says there's no chamber heater instead of failing.

The config layout is originally based on FlashForge's default config layout, just expanded with new macros, settings, and removals of useless settings.
This does not mean our config is interchangable with stock firmware, and if you try, it will not work as a lot of settings are intertwined.

There is one physical extrusion motor. The four Klipper extruder contexts
select the corresponding tool heaters and state; they are not four independent
extrusion drives. A toolchange must agree with the dock and grab pins before
the selected logical extruder becomes active.

The single motor driver is configured on the eBoard as identical
`[tmc2209 extruder]` through `[tmc2209 extruder3]` sections, one for each
logical extruder for CFW.
See [MCU development and verification](mcu-development.md) for pin assignments.

## Interfaces

The printer configuration currently uses AFC standalone mode, not a BoxTurtle or AMS-like 
mechanism. Mainsail/Fluidd exposes virtual Misc switches for flow calibration and
purge. GrumpyScreen work is
separate from the core Klipper and filesystem repositories, and is mostly kept up to date and commited to mainline
SimpleAF GrumpyScreen.

## Hardware
Other than the previously explained hardware for the MCU chips, the printer
has some unexplained "maximum" limits for temperature

| Hardware | Default Max | Actual Max | Safe Max |
| --- | --- | --- | --- |
| Nozzles | 320c | 350c | 350c? |
| Chamber Heater (Pro) | 65c | 80c | 70c? |
| Bed | 120 | 130 | 125? |

Safe Max temps are the maximium safe temperatures before you run into actual
hardware limitations. Anything with a `?` may mean untested.

!!! danger "Known hardware limitations"
    - Above 70 degrees chamber temperature, the grill will deform
    - Above 320 degrees, the nozzle may start recording wildly off temperatures
