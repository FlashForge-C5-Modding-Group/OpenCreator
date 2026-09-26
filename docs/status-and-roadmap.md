# Status and roadmap

OpenCreator is a work in progress. The labels below distinguish code that is
present from behavior verified on a specific printer. A unit test, successful
build, or one flashed board is not end-to-end verification.

| Area | Present in development source | Current evidence and remaining work |
| --- | --- | --- |
| Four MCU ports | Mainboard, eboard, heaterboard, and levelboard support is under development in `klipper-c5`. | The levelboard was reported flashed and working in an earlier trial. That does not validate every later build or the combined four-board stack. Record exact image versions and test each board again for a release. |
| Klipper host on printer | Creator 5 modules, MCU protocol support, and printer configs exist. | The MIPS32 host needs a compatible Python environment and Klipper C helper. Complete boot, motion, temperature, timeout, and long-print checks are still required for a supported release. |
| Toolchanging and AFC standalone | T0 to T3 motion, physical dock and grab checks, filament mapping, runout integration, and safety interlocks are implemented in development. | Verify measured holder coordinates, latch clearance, rapid moves, door and heater rules, and recovery on the actual printer before unattended use. |
| Print start and stop | Automatic virtual-SD start, four-tool preparation, optional flow and purge, bed mesh, nozzle Z checks, and normal EOF stop are implemented. | Verify the full sequence with fitted nozzles and appropriate materials. Canceled and failed prints must be tested separately from normal completion. |
| Flow and VFA calibration | The host has an eboard pressure-advance test and MCLib vibration calibration integration. | The short bucket flow path and VFA results need printer validation. Do not present them as calibrated merely because commands run. |
| Installation | A direct-framebuffer installer UI and preflight framework exist in `OCInstaller`. | The installer does not yet install a CFW payload or flash MCUs. A reviewed package, exact board targets, rollback, and recovery procedure are still needed. |
| Touchscreen and web UI | Mainsail controls and separate touchscreen work exist. | A complete custom touchscreen workflow and its compatibility matrix are not yet a release claim. |

## Planned, not implemented as supported features

- ACE Pro integration as an additional filament system.
- Raspberry Pi USB gadget-mode tunneling so a Pi can host Klipper while communicating with the printer.
- A tested full-CFW installer, recovery path, release matrix, and upgrade policy.
- Sliced G-code 3MF intake in the print-upload path.

The planned list is not a promise that the current board, kernel, or host image
already supports those features. Record source commit, build artifact,
printer model and board revisions, test procedure, and outcome when promoting
any item from development to verified.
