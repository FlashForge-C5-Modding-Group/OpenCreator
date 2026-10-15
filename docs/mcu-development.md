# MCU development and verification

OpenCreator has separate host software, printer configuration, and firmware for
four controller boards. A successful firmware build is not the same as a
successful printer test. This page records the development state as of
October 8, 2026, this is still pre-release.

| Board | MCU | Primary role |
| --- | --- | --- |
| MainboardGD | GD32H737VGT6 | Main motion and machine I/O |
| eBoard | N32G455CCL7 | Shared extrusion drive and pressure-advance sensing |
| Heaterboard | N32G455REL7 | Tool heaters and temperature I/O |
| Levelboard | N32G430F8S7 | Probing and levelboard sensing |

The source for the MCU ports is in
[klipper-c5](https://github.com/FlashForge-C5-Modding-Group/klipper-c5).
The [Kalico port](https://github.com/FlashForge-C5-Modding-Group/kalico)
contains the corresponding Creator 5 support for that host. The configuration
is maintained separately in
[opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs)
and [opencreator-tunnel](https://github.com/FlashForge-C5-Modding-Group/opencreator-tunnel).

## One motor, four logical tools

T0 through T3 represent four removable toolheads, but they share one physical
extruder motor and its eBoard TMC2209 driver. The logical extruders share the
eBoard step, direction, and enable signals. Tool selection associates that
drive with the selected heater, sensors, and filament state; it does not switch
among four independent motor drivers.

The printer and tunneled configurations have a `[tmc2209 extruder]` through
`[tmc2209 extruder3]` section, all with identical settings and all addressing
the same driver. Each logical extruder needs one so that it requests the same
step-on-both-edges (DEDGE) mode as the others. A tool without one sends
single-edge pulses, and the driver, already in DEDGE mode, counts each pulse
twice, so that tool extrudes about double. Their configured pins are eBoard PB11 for UART receive, PB10 for UART transmit, address 0,
PB14 for step, PB15 for direction, and PB12 for enable. The eBoard uses USART3
for this driver. The host's Creator 5 UART transport and the eBoard's MCU
command must both be present; ordinary GPIO UART bitbanging does not implement
this hardware path. The configured 0.70 A current approximates stock behavior.
The assumed 0.110-ohm sense resistor has been verified working.

Each logical extruder is a separate MCU stepper on the same PB15 direction
pin. Stock Klipper toggles a stepper's direction pin relative to that
stepper's own last direction, so with four steppers on one pin the actual
level ends up as the XOR of all four. A tool then runs backwards whenever an
odd number of the other tools last moved in reverse, for example a retract
before docking. eBoard builds (`CONFIG_C5_EBOARD`) therefore write the
absolute direction at the start of every move in `src/stepper.c`. This needs
the eBoard reflashed; until then a `FIRMWARE_RESTART` resets all four to
forward.

## Test packages

The four-MCU Creator 5 Pro test package was built from Kalico commit
`e48df776`, with the eBoard transport change from Klipper-c5 commit `336ba5fa`.
These are test builds, not finished releases.
