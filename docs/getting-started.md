# Getting started

OpenCreator has two paths. [Legacy](opencreator-legacy.md) keeps the mostly
stock FlashForge control stack and adds selected modifications.
[OpenCreator CFW](opencreator-cfw.md) replaces more of that stack with
community-maintained Klipper and printer-side components. CFW is in public
alpha: download the [latest release](https://github.com/FlashForge-C5-Modding-Group/OpenCreator/releases/latest) and see
[Installation and safety](installation-and-safety.md).

If you want to improve a working stock printer today, start with the
[Creator-5-Mods Legacy guides](https://github.com/FlashForge-C5-Modding-Group/Creator-5-Mods).
Follow the prerequisites and rollback advice for each individual modification.

If you are contributing to CFW, start with [Project layout](project-layout.md)
and [Architecture](architecture.md). The printer configuration lives in
`opencreator-fs/fs/usr/data/config`, while the Creator 5 Klipper modules and
MCU code live in `kalico`. The detailed toolchanger development notes are
in `CREATOR5_TOOLCHANGER.md` inside the configuration tree.

Join our Discord for support and to report problems with the alpha.

## Before a printer trial

- [x] Read [Status and roadmap](status-and-roadmap.md)
- [x] Read [Installation and safety](installation-and-safety.md)
- [x] Know how to [revert to stock firmware](installation-and-safety.md#uninstalling-opencreator)
- [x] Have a USB drive that meets the [requirements](installation-and-safety.md#requirements-for-usb)
- [x] Join the [Discord](https://discord.gg/RsdkqDtfww) for support
