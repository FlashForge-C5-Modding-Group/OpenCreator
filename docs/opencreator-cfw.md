# OpenCreator CFW

OpenCreator full custom firmware, or CFW, is the deeper Creator 5 integration
effort. It aims to make the printer's host software, MCU behavior, toolchanging,
and configuration maintainable as one community-developed stack. To gain is more features
and more stable software overall compared to stock. 
CFW is in public alpha; get the [latest release](https://github.com/FlashForge-C5-Modding-Group/OpenCreator/releases/latest). The mostly stock alternative is
[OpenCreator Legacy](opencreator-legacy.md).

The work is split between
[kalico](https://github.com/FlashForge-C5-Modding-Group/kalico) for
Klipper and MCU integration (it replaces klipper-c5, which is archived), and
[opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs)
for printer files, configuration, and installation helpers.

## What the newer path aims to support

| Full custom firmware area | Goal | Status |
| --- | --- | --- |
| Base software | Maintain Creator 5 stock behavior in a community-developed Klipper host and MCU stack. | <span class="oc-status ok">Finished</span> |
| Installation and updates | Provide a tested CFW installation. | <span class="oc-status ok">Finished</span> |
| Tool selection | Configure T0 to T3 pickup, docking, and safety in host-side modules and macros. | <span class="oc-status ok">Finished</span> |
| Filament control | Integrate AFC standalone for toolchanger filament mapping, loading, and runout handling. | <span class="oc-status ok">Finished</span> |
| ACE Pro | Use an ACE Pro as another filament system, each slot feeding its own head, with auto feed and auto load. | <span class="oc-status ok">Finished</span> |
| ACE Pro multiplexing | Feed several ACE slots into one head and swap between them. | <span class="oc-status">Planned</span> |
| Web and touchscreen | Allow for switching filaments on the web. | <span class="oc-status partial">Partial</span> |
| Raspberry Pi host | Use USB gadget mode to tunnel Klipper communication from a Raspberry Pi running the Klipper host to the printer. | <span class="oc-status test">Testing</span> |
| Extend stock features | Better VFA testing, stallguard and other similar improvements to software | <span class="oc-status partial">Partial</span> |

To see more features that are directly implemented in detail look at [CFW Features](cfwfeatures.md).

These are goals, and not all of them are finished. The ACE Pro works with auto
feed and auto load, multiplexing is in the planning stages, and the Raspberry Pi tunnel is in testing (see
[OpenCreator for SBC](rpi-klipper.md)).

Gadget mode Klipper / RPI Klipper is very close to what AnyCubic's modding scene calls [Tunneled Vanilla Klipper](https://github.com/Kobra-S1/vanilla-klipper-swu/blob/main/tunneled-klipper.md)
and a lot of inital insperation was taken to go this route over direct MCU soldering,
as it may not be possible on this board, as there has been no direct way (yet) to
solder and allow USB passthrough for the MCUs.
