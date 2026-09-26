# OpenCreator CFW

OpenCreator full custom firmware, or CFW, is the deeper Creator 5 integration
effort. It aims to make the printer's host software, MCU behavior, toolchanging,
and configuration maintainable as one community-developed stack. CFW is still
in development, not a ready-to-flash release. The mostly stock alternative is
[OpenCreator Legacy](opencreator-legacy.md).

The work is split between
[klipper-c5](https://github.com/FlashForge-C5-Modding-Group/klipper-c5) for
Klipper and MCU integration, and
[opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs)
for printer files, configuration, and installation helpers.

## What the newer path aims to support

| Full custom firmware area | Goal |
| --- | --- |
| Base software | Maintain Creator 5 behavior in a community-developed Klipper host and MCU stack. |
| Installation and updates | Provide a tested CFW installation and update path with recovery instructions. |
| Tool selection | Configure T0 to T3 pickup, docking, and safety in host-side modules and macros. |
| Filament control | Integrate AFC standalone for toolchanger filament mapping, loading, and runout handling. |
| ACE Pro | Add support for ACE Pro as another filament system. This is planned, not implemented. |
| Web and touchscreen | Build custom workflows for selecting tools and filaments. |
| Raspberry Pi host | Use USB gadget mode to tunnel Klipper communication from a Raspberry Pi running the Klipper host to the printer. This is planned, not implemented. |

These are development goals, not claims that every feature is complete or
hardware-verified. In particular, ACE Pro support and Raspberry Pi USB
gadget-mode tunneling are planned, not available features. A configuration's
existence does not prove that its toolchanging or calibration sequence is safe
on every printer. A pre-alpha version for gadget mode testing was made by ano
and seems to work fine, but it will most likely not be used in the final versions
implemented.

Gadget mode Klipper / RPI Klipper is very close to what AnyCubic's modding scene calls [Tunneled Vanilla Klipper](https://github.com/Kobra-S1/vanilla-klipper-swu/blob/main/tunneled-klipper.md)
and a lot of insperation was taken to go this route over direct MCU soldering,
as it may not be possible on this board, as there has been no direct way (yet) to
solder and allow USB passthrough for the MCUs.

## Where the work stands

The development stack has Creator 5-specific Klipper modules, MCU ports,
AFC standalone integration, printer configuration, and a print workflow under
active test. Read [Architecture](architecture.md) for the component roles,
[Print workflow](print-workflow.md) for the current sequence, and
[Status and roadmap](status-and-roadmap.md) for the difference between source
implementation and printer verification.

Currently, nothing is end-user ready, so you should not be trying to clone,
and build firmware / run custom firmware. Everything will be done automatically 
for you when it is ready.
