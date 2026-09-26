# Getting started

OpenCreator has two paths. [Legacy](opencreator-legacy.md) keeps the mostly
stock FlashForge control stack and adds selected modifications.
[OpenCreator CFW](opencreator-cfw.md) replaces more of that stack with
community-maintained Klipper and printer-side components. CFW is still under
development.

If you want to improve a working stock printer today, start with the
[Creator-5-Mods Legacy guides](https://github.com/FlashForge-C5-Modding-Group/Creator-5-Mods).
Follow the prerequisites and rollback advice for each individual modification.
Do not use the CFW source tree as an installation package.

If you are contributing to CFW, start with [Project layout](project-layout.md)
and [Architecture](architecture.md). The printer configuration lives in
`opencreator-fs/fs/usr/data/config`, while the Creator 5 Klipper modules and
MCU code live in `klipper-c5`. The detailed toolchanger development notes are
in `CREATOR5_TOOLCHANGER.md` inside the configuration tree.

If you want to run the CFW, there is no place for you yet, as development is
not yet finished and is under-way for making an easy installer, adding features,
and unlocking the system from FlashForge's chains.

Before attempting a printer trial, read [Status and roadmap](status-and-roadmap.md)
and [Installation boundaries](installation-and-safety.md). There is no general,
verified full-CFW flashing or recovery procedure on this site yet.
