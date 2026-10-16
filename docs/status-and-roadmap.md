# Status and roadmap

OpenCreator is a work in progress. The table shows what's in the code and how
well it has been tested on a printer.

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
| Installer | Touchscreen installer from a USB drive: Entware, LoopScript, Kalico, Moonraker, Mainsail/Fluidd, GrumpyScreen, the root login and MCU firmware flashing, with a backup first (optional) and restore. Recovery and Update Configs when it's already installed. | Working: tested from a fully stock install to a OpenCreator install. |
| OpenCreator Updater | A Moonraker update_manager entry (Pi-tunnel installs) that 3-way-merges the latest config template into your live config without clobbering customized values, then pulls Klipper -- one "Update" click. SoC installs get an equivalent LoopScript-triggered version. | Verified live on a real Pi + printer, conflicts no longer ever touch the live file, only a sibling .conflict file. SoC version is unverified on real hardware. |
| Real Zero Purge modes | Implemented into switches | You can turn off Flow Calibration and Purging to make a basically zero waste printer. It prints a small purge line to prime the nozzle when starting the print, but thats it. It is under runout in Fluidd, or Misc in Mainsail. |
| Print Farm / Automation support | Ready | Needs testing, but it should work with anything that just uses Moonraker. You should be able to just plop in any file and it should be able to just take it. |
| RPI Support | In testing | Currently being tested, but allowing to tunnel the board MCUs to a Raspberry Pi. High quality cables are required or else you will get strange results. |
| Power Loss Recovery | Early testing | An early version is included, but seems to not work correctly. |

## Milestones

- [x] Four MCU boards ported to Klipper and Kalico
- [x] Toolchanging and AFC standalone
- [x] Print start and stop sequences
- [x] Complete removal of firmwareExe
- [x] OpenCreator Updater (Pi-tunnel)
- [x] Real Zero Purge modes
- [ ] Flow and VFA calibration without bugs on interrupted flow
- [ ] GrumpyScreen macros and extruding
- [x] Installer verified end to end on a printer
- [ ] RPi / SBC support out of testing
- [ ] Working power loss recovery

## Planned, not implemented as supported features

- [ ] ACE Pro integration as an additional filament system.
- [x] Sliced G-code 3MF intake in the print-upload path (SBC installs, not yet verified on a printer).
- [ ] Version tracking
- [ ] AI-vision overwatch for failures
- [ ] Further detection for print failures
- [ ] Reducing amount of wasted filament when runout is occured

The installer works, and the OpenCreator Updater is being finished; see their
rows in the table above.

Planned features aren't promised and may change or be dropped.
