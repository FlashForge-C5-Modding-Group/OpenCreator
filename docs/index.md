# OpenCreator

OpenCreator is the Creator 5 and Creator 5 Pro community full custom firmware
project. It brings the printer's four controller boards, Klipper host,
toolchanging, filament handling, and printer-side configuration into a
maintainable stack. OpenCreator Legacy deals with older versions that keep
a mostly stock experience, whilst giving important creature comforts.

Join our [Discord](https://discord.gg/RsdkqDtfww) for updates on development, support and a community of
Creator 5 / 5 Pro owners. We can even help with alternate custom firmwares, even though support may be
diminished.

| Read this | For |
| --- | --- |
| [Getting started](getting-started.md) | Decide between the stock-based Legacy path and development CFW. |
| [Architecture](architecture.md) | Understand the host, four MCUs, AFC, touchscreen, and filesystem roles. |
| [Print workflow](print-workflow.md) | See tool selection, calibration, priming, meshing, and print-end behavior. |
| [Status and roadmap](status-and-roadmap.md) | Separate implemented work from printer-verified behavior and plans. |
| [Installation boundaries](installation-and-safety.md) | Understand why there is no general installation recipe yet. |
| [Project layout](project-layout.md) | Find the source repositories. |
| [OpenCreator Legacy](opencreator-legacy.md) | See the "Legacy" version of OpenCreator, where it works around FlashForge instead of replacing. |
| [OpenCreator CFW](opencreator-cfw.md) | See the newer replacement stack and its development goals. |
| [Slicer Configuration for CFW](slicer-config.md) | Check out the needed slicer changes for CFW |
| [OpenCreator CFW for SBC](rpi-klipper.md) | OpenCreator running on a RPi or SBC instead of the host mips32 SOC. |

!!! warning "Development software"
    Do not treat any full custom firmware from us as complete.
    Anything you may of seen may be out of date, different
    or just completely not working, and you should wait
    unless you know exactly what you are doing and how to revert.

!!! warning "Warranty & Responsibility Disclosure"
    OpenCreator's team and contributors do not take any responsibility over 
    any damage, unintended wear, or anything else done to your printer
    and the responsibility is on you if you "brick" or break your printer,
    and installing mods may void your warranty, so of course, be careful!