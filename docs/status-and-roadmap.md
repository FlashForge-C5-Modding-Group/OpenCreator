# Status and roadmap

OpenCreator is a work in progress. The labels below distinguish code that is
present from behavior verified on a specific printer. 

| Area | Present in development source | Current evidence and remaining work |
| --- | --- | --- |
| Four MCU ports | Mainboard, eBoard, heaterboard, and levelboard support is under development in `klipper-c5`. | The new eBoard TMC UART path has been built and looks to be working See [MCU development and verification](mcu-development.md). |
| Kalico MCU port | Creator 5 MCU support has been ported to Kalico, and a four-board development archive was structurally validated from commit `e48df776`. | They were tested to be working, and actually toolchange faster. |
| Klipper host on printer | Creator 5 modules, MCU protocol support, and printer configs exist. | It is mostly working. |
| Toolchanging and AFC standalone | T0 to T3 motion, physical dock and grab checks, filament mapping, runout integration, and safety interlocks are implemented. | Mostly finished testing, and should almost be 100%. |
| Print start and stop | Four-tool preparation, optional flow and purge, bed mesh, nozzle Z checks, and normal EOF stop are implemented. | All sequences are working as intended |
| Flow and VFA calibration | The host has an eboard pressure-advance test and MCLib vibration calibration integration. | It mostly works, but may still have some bugs if the flow is interupted. |
| Touchscreen and web UI | Mainsail/Fluidd controls and separate touchscreen. | GrumpyScreen has had multi-extruder pulled in, macros are in dev. |
| Complete removal of firmwareExe | The main pain point for FlashForge printers | Their bad touchscreen + camera + print uploading sequence has been completely removed and do not function at all. |
| Installer | SoC install now really installs: shallow git clones of Klipper and Moonraker, latest GitHub release downloads for Mainsail/Fluidd/GrumpyScreen, and the config/loopscript overlay from opencreator-fs, all with automatic backup-before-replace. | The LVGL GUI has a working Demo install button; the actual on-device install flow (git clone, GitHub release fetch) has been tested, but not yet on a printer running the real touchscreen GUI end to end. MCU firmware flashing and the squashfs OTA rootfs swap are separate, lower-level steps this doesn't replace. |
| OpenCreator Updater | A Moonraker update_manager entry (Pi-tunnel installs) that 3-way-merges the latest config template into your live config without clobbering customized values, then pulls Klipper -- one "Update" click. SoC installs get an equivalent LoopScript-triggered version. | Verified live on a real Pi + printer, conflicts no longer ever touch the live file, only a sibling .conflict file. SoC version is unverified on real hardware. |
| Real Zero Purge modes | Implemented into switches | You can turn off Flow Calibration and Purging to make a basically zero waste printer. It prints a small purge line to prime the nozzle when starting the print, but thats it. It is under runout in Fluidd, or Misc in Mainsail. |
| Print Farm / Automation support | Ready | Needs testing, but it should work with anything that just uses Moonraker. You should be able to just plop in any file and it should be able to just take it. |
| RPI Support | In testing | Currently being tested, but allowing to tunnel the board MCUs to a Raspberry Pi. High quality cables are required or else you will get strange results. |
| Power Loss Recovery | Early testing | An early version is included, but seems to not work correctly. |

## Planned, not implemented as supported features

- ACE Pro integration as an additional filament system.
- Sliced G-code 3MF intake in the print-upload path.
- Version tracking
- AI-vision overwatch for failures
- Further detection for print failures
- Reducing amount of wasted filament when runout is occured

The full-CFW installer (with real backups and a demo mode to inspect
changes first) and a Moonraker-update_manager-based plugin updater are no
longer purely planned -- see the Installer and OpenCreator Updater rows
above for what's actually implemented and what's still unverified.

The planned list is not a promise that the current board, kernel, or host image
already supports those features. Any planned feature may be scrapped at any point.
