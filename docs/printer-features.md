# Printer Features

## Flow Calibration in the Creator 5 factory firmware

The touchscreen calls this **Flow Calibration**, but the routine tests
**pressure advance**. It does not measure a printed line's width, weigh
extruded plastic, or calculate an extrusion multiplier. The host tries several
pressure-advance values while the eboard observes the extruder motor-driver
signal. The eboard reports whether each test waveform passes its check.

The sequence below is reconstructed from the factory `firmwareExe.i64`
database, principally `BuildPage::clearNozzlePrint` and
`CommMgr::paTestMgr`, and from the factory `eBoard.hex`. 

### The stock test, step by step

1. The host resets the extruder position with `G92 E0`, switches to relative
   extrusion with `M83`, sets acceleration to 5000 mm/s² and square-corner
   velocity to 9 mm/s, enables the X/Y motor-driver control pins, and waits
   for queued motion to finish.
2. It tries these pressure-advance candidates in this order: `0.0100`,
   `0.0200`, `0.0150`, `0.0350`, `0.0250`, `0.0300`, and `0.0400`. For each
   candidate it sends `PA_ACTION ACTION=11 PC=666` to arm the eboard
   measurement, then `SET_PRESSURE_ADVANCE ADVANCE=<candidate>`.
3. The carriage starts each candidate at X40 and a Y position from 50 to 80 
   in 5 mm steps. It extrudes across six X strokes ending at X60, X100, X120,
   X140, X180, and X200. The X and Y movements don't actually happen and get discarded, this is mostly to trick 
   Klippy into thinking PA can be used. The short strokes extrude 1.13573 mm of filament at
   `F1080` (18 mm/s); the long strokes extrude 2.27146 mm at `F10980`
   (183 mm/s). Alternating slow and fast motion creates the motor-load
   transient that the eboard examines. 
4. The host sends `PA_ACTION ACTION=0 PC=666`, waits with `M400`, and reads
   `PA_GET`. It treats a returned value of `9` as a pass for that candidate.

The eboard firmware's `pa_action` handler resets its result and sample area
when action `11` starts a measurement, switches its acquisition mode, then
finalizes the capture when action `0` arrives. `get_emcu_pa_value` reports the
result back to the host as `pa_value value=<number>`. The stock eboard's
internal waveform criteria are not completely established from the available
decompilation; `9` is the pass code used by the host, not the measured
pressure-advance number. The `PC=666` argument is sent by the host, but this
does not establish that it is a material profile or a calibration value.

### How it chooses and applies a result

One pass tests up to seven candidates and keeps the **smallest** candidate
for which the eboard returned `9`. The host allows up to five passes, stopping
once three passes produce a valid candidate. It averages those three selected
values to produce that tool's result. If fewer than three passes succeed, the
test reports failure rather than producing a new value. The routine also
cleans up its motor-driver test mode after completion or interruption.

After preparing the tools, the print-start code sends `SET_PA_ADVANCE` with
the successful T0–T3 results and `ENABLE=1`. Entries without a result are
filled with `99.0` as placeholders. If flow testing is off, or no valid tool
result is available, it sends `SET_PA_ADVANCE T0=99.0 T1=99.0 T2=99.0
T3=99.0 ENABLE=0`. These final per-tool settings are distinct from the
temporary `SET_PRESSURE_ADVANCE` candidate applied during each test.
