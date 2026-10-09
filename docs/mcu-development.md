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

The current printer and tunneled configurations therefore have **one**
`[tmc2209 extruder]` section, not a TMC section for each tool. Its configured
pins are eBoard PB11 for UART receive, PB10 for UART transmit, address 0,
PB14 for step, PB15 for direction, and PB12 for enable. The eBoard uses USART3
for this driver. The host's Creator 5 UART transport and the eBoard's MCU
command must both be present; ordinary GPIO UART bitbanging does not implement
this hardware path. The configured 0.70 A current approximates stock behavior.
The assumed 0.110-ohm sense resistor has been verified working.

## What package validation proves

The Kalico source at commit `e48df776` produced a four-MCU Creator 5 Pro
development archive. The package generator checked the board images, template
hashes, control metadata, and archive structure. The eBoard transport change in the
Klipper-c5 source was at commit `336ba5fa`. These commit IDs identify tests and are not fully finished.

Use a matching host fork, its MCU firmware, and its configuration as one set.
In particular, an MCU archive built from Kalico source does not establish that
an installed Klipper-c5 host has been updated to Kalico. Firmware packaging
cannot establish whether homing, heating, probing, extrusion, or a toolchange
is safe on a physical printer. See [installation boundaries](installation-and-safety.md).
