# Installation boundaries and safety

There is no general, verified OpenCreator full-CFW installation procedure on
this site. The source repositories and printer config are development inputs,
not a signed release package or a guarantee of bootability. The separate
OpenCreator Installer has a working framebuffer UI, input handling, preflight
checks, and constrained Legacy backup flow, but its install page currently
runs a baseline progress provider rather than installing Klipper or flashing
MCUs.

The printer has four MCU boards and a Linux host. Replacing only one component
can produce protocol or behavior mismatches. A future release guide needs to
identify supported machine and board revisions, exact host and MCU artifacts,
their checksums, the update mechanism, and a recovery route for each board.
It also needs to explain how stock-format calibration JSON and user settings
are preserved or restored.

Before declaring a build usable, testing should cover at least:

- Host startup and stable communication with all four MCUs over time.
- Cold and hot tool pickup, dock, sensor disagreement, and interrupted motion.
- Homing with and without a tool attached, safe Z offsets, and bed probing.
- Heater, fan, thermal shutdown, filament runout, pause, resume, cancellation, and print end.
- Mainsail upload and virtual-SD playback with representative single-tool and multi-tool files.
- Power-loss and rollback behavior with a documented stock recovery package.

Do not substitute a successful compile, a firmware version string, or a single
short print for those checks. Keep a known-good recovery image and follow a
board-specific tested procedure. Nothing on this page authorizes flashing an
unverified artifact.

If you need a mostly stock printer today, use the
[Legacy path](opencreator-legacy.md) and its own instructions instead.
