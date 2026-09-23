# OpenCreator Legacy and full custom firmware

OpenCreator has two related paths. **OpenCreator Legacy** improves the Creator 5
while retaining its mostly stock software. **OpenCreator full custom firmware
(CFW)** is the newer, deeper integration effort. CFW is still in development;
this page describes its goals, not a ready-to-flash release.

## OpenCreator Legacy: extend the stock experience

Legacy starts with the printer's existing FlashForge software. The
[Creator-5-Mods](https://github.com/FlashForge-C5-Modding-Group/Creator-5-Mods)
repository contains the guides and research, including root access, Loop
Script, and Mainsail. The companion
[Creator-5-Scripts](https://github.com/FlashForge-C5-Modding-Group/Creator-5-Scripts)
repository contains scripts intended to run on a rooted printer.

This is the path for modifying a mostly stock printer without replacing its
entire control stack. It remains useful even while CFW is being developed.
Follow each Legacy guide's own prerequisites and recovery notes; installing a
script is not the same thing as flashing replacement MCU firmware.

| Legacy area | Approach |
| --- | --- |
| Base software | Keep the printer's mostly stock FlashForge software and control behavior. |
| Setup | Follow individual guides and install only the modifications you want. |
| Root access and scripts | Use the documented root and Loop Script paths for printer-side tweaks. |
| Toolchanging | Continue using the factory workflow, with any compatible modifications from the guides. |
| Web access | Add tools such as Mainsail alongside the stock software. |
| Updates and recovery | Check each modification's prerequisites and recovery instructions before applying it. |

## The newer path: own more of the stack

CFW aims to make the Creator 5's printer-side software and control behavior
maintainable and extensible as one project. The work is split between
[klipper-c5](https://github.com/FlashForge-C5-Modding-Group/klipper-c5), for
Klipper and MCU integration, and
[opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs),
for filesystem changes, installation helpers, and printer configuration.

That architecture is intended to support things that are difficult to maintain
as stock-firmware tweaks alone:

| Full custom firmware area | Goal |
| --- | --- |
| Base software | Maintain Creator 5 behavior in a community-developed Klipper host and MCU stack. |
| Installation and updates | Provide a tested CFW installation and update path with recovery instructions. |
| Tool selection | Configure T0–T3 pickup, docking, and safety in host-side modules and macros. |
| Filament control | Integrate AFC standalone for toolchanger filament mapping, loading, and runout handling. |
| ACE Pro | Add support for ACE Pro as another filament system; planned, not implemented. |
| Web and touchscreen | Build custom workflows for selecting tools and filaments. |
| Raspberry Pi host | Use USB gadget mode to tunnel Klipper communication from a Raspberry Pi running the Klipper host to the printer; planned, not implemented. |

ACE Pro support and Raspberry Pi USB gadget-mode tunneling are **planned**, not
available or verified features.

These are **development goals**, not a claim that every feature is complete or
hardware-verified. For example, the local CFW configuration documents AFC
toolchanging work, but a configuration's existence does not prove that it is
safe on every machine.

## Which should I use now?

For a printer you need to operate today, start with the
[Legacy guides](https://github.com/FlashForge-C5-Modding-Group/Creator-5-Mods)
and their stated prerequisites. The organization currently labels
`klipper-c5` **work in progress** and **do not flash**. Wait for a tested CFW
release with supported hardware, installation, verification, and rollback
instructions before treating the newer path as a migration procedure.

See [Project layout](project-layout.md) for the repositories behind CFW. This
page intentionally does not provide flashing steps.
