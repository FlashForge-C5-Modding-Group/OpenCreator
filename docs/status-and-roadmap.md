# Status and roadmap

OpenCreator is a work in progress. The labels below distinguish code that is
present from behavior verified on a specific printer. A unit test, successful
build, or one flashed board is not end-to-end verification.

| Area | Present in development source | Current evidence and remaining work |
| --- | --- | --- |
| Four MCU ports | Mainboard, eboard, heaterboard, and levelboard support is under development in `klipper-c5`. | All boards are known to be working as well as on stock.  |
| Klipper host on printer | Creator 5 modules, MCU protocol support, and printer configs exist. | It is mostly working. |
| Toolchanging and AFC standalone | T0 to T3 motion, physical dock and grab checks, filament mapping, runout integration, and safety interlocks are implemented. | Mostly finished testing, and should almost be 100%. |
| Print start and stop | Four-tool preparation, optional flow and purge, bed mesh, nozzle Z checks, and normal EOF stop are implemented. | All sequences need further testing |
| Flow and VFA calibration | The host has an eboard pressure-advance test and MCLib vibration calibration integration. | It mostly works, but may still have some bugs |
| Touchscreen and web UI | Mainsail/Fluidd controls and separate touchscreen. | Grumpyscreen is currently being changed to fit our needs. In active development. |
| Complete removal of firmwareExe | The main pain point for FlashForge printers | Their bad touchscreen + camera + print uploading sequence has been completely removed and do not function at all |
| Installer | No real work of substance has been done | Major work still needs to go into making a way to install and update the printer to get onto custom firmware, such as an installer helper. |
| Real Zero Purge modes | Implemented into switches | You can turn off Flow Calibration and Purging to make a basically zero waste printer. It prints a small purge line to prime the nozzle when starting the print, but thats it. |
| Print Farm / Automation support | Ready | Needs testing, but it should work with anything that just uses Moonraker. You should be able to just plop in any file and it should be able to just take it. |
| RPI Support | In testing | Currently being tested, but allowing to tunnel the board MCUs to a Raspberry Pi. |
| Power Loss Recovery | Early testing | An early version is included, but not tested. |

## Planned, not implemented as supported features

- ACE Pro integration as an additional filament system.
- A full-CFW installer with an actual installer, backups, security, and choices.
- Sliced G-code 3MF intake in the print-upload path.
- A proper plugin system with Moonraker / Fluidd / Mainsail integration
- AI-vision overwatch for failures
- Further detection for print failures
- Reducing amount of wasted filament when runout is occured

The planned list is not a promise that the current board, kernel, or host image
already supports those features. Any planned feature may be scrapped at any point.
Record source commit, build artifact,
printer model and board revisions, test procedure, and outcome when promoting
any item from development to verified.
