# Current print workflow

This page describes the development configuration in `opencreator-fs` and
`klipper-c5`. It is a source-level description, not a claim that every step
has passed an unattended printer test.

## Tool preparation and printing

`C5_PRINT_START` currently performs this sequence:

1. Home XY. If a tool is attached, dock it using physical sensor checks, then home Z.
2. Start the configured bed temperature and apply saved MCLib vibration compensation.
3. If flow calibration or purge is enabled, pick up T0, T1, T2, and T3 in turn. Heat each selected hotend, optionally measure its pressure advance, optionally purge into the rear-right bucket, cool it at the wiper, and dock it.
4. Clear the prior mesh. Run adaptive bed meshing when bed leveling is enabled, or load the saved `default` profile when available. Probe the bed center.
5. Pick up the requested print tool, apply its saved nozzle Z relationship, verify a positive offset, and heat it for the file.
6. Let the print file run. At normal virtual-SD end-of-file, run `C5_PRINT_STOP` once to turn off heat and the part fan, dock the current tool when homed, and disable motors.

The preparation purge and pressure-advance test are separate. Flow testing
extrudes measurement strokes even when purge is switched off. With both
switches off, the T0 to T3 preparation loop is skipped. An explicit
`C5_TOOL_PURGE`, `C5_PURGE_LINE`, or purge move already present in a slicer's
G-code is not removed by the Misc switch. The current `C5_PRINT_START` macro
does not automatically call `C5_PURGE_LINE`.

All four tools need fitted nozzles and filament when four-tool preparation is
enabled. `HOTEND` is the default preparation temperature; `HOTEND0` through
`HOTEND3` can supply different temperatures for different materials. Verify
those values before heating another material.

## Mainsail controls

The virtual Misc switches are named `flow_calibration` and `purge`. They
default to on at Klippy startup and affect the next `C5_PRINT_START`. The
print-start command can override them per job with `FLOW_CALIBRATION=0` or
`PURGE=0`. Bed leveling is controlled separately and can be overridden with
`BED_LEVELING=0`. The switch states are session settings, not proof that a
particular file contains no extrusion commands.

## Calibration and safety

There are two different offset workflows. Extruder Position Calibrate measures
holder locations for pickup and docking. Toolhead offset calibration measures
the levelboard reference and then the tools, and writes stock-format data to
`extruder.json`. Removing the build plate belongs to the offset calibration
procedure, not ordinary print startup or holder position calibration.

Automatic nozzle Z uses the saved tool and station measurements, saved
touchscreen Z adjustment, and print-specific temperature and first-layer
terms. It rejects missing or implausible results. The center probe check does
not, by itself, measure nozzle contact with the bed. 

The bucket flow test is a short-path approximation of the stock touchscreen's
longer motor-current test. It retains the touchscreen's extrusion pulse sizes
and scales travel timing to fit the bucket. Software tests pass, but valid
eboard readings from this exact path still need hardware confirmation. 

The automatic virtual-SD start hook reads a G-code header to select the first
tool and temperatures. It recognizes `.gcode`, `.g`, and `.gco` print files.

For exact parameters and known caveats, read `CREATOR5_TOOLCHANGER.md` and
`CREATOR5_VFA.md` in the
[opencreator-fs repository](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs).
The exact files described here may still be on a development branch rather
than the repository's default branch.
