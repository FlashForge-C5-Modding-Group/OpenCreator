# Making the configs yours

OpenCreator's configs are plain Klipper config files, and you're free to change
them. Updates merge into them instead of overwriting them, so your changes stay.
This page shows where they are, how updates treat your edits, and the easiest
way to keep your own settings separate.

## Where the configs are

=== "Printer (SoC)"

    `/usr/data/opencreator/klipper/config`

=== "Tunneled (SBC)"

    `~/printer_data/config` on the SBC.

Open them in the web UI: **Configuration** (the `{…}` icon) in Fluidd, or
**Machine** in Mainsail. Edit a file there, then press **Save & Restart** so
Klipper loads it. A Klipper restart is all it takes; you don't need to reboot.

## What's in each file

`printer.cfg` is the one Klipper reads. It includes the rest:

| File | What it holds |
| --- | --- |
| `printer.cfg` | The includes, the TMC settings and, at the bottom, the `SAVE_CONFIG` block with your calibration |
| `printer.base.cfg` | The printer itself: MCUs, steppers, heaters and fans. It includes most of the files below |
| `printer.creator5.cfg` | The toolchanger (`[creator5_toolchanger]`): dock positions, toolchange speeds, purge and calibration settings |
| `printer.misc.cfg` | The [switches](toggle-features.md) and their macros |
| `printer.macro.cfg` | Print start, print end and the other OpenCreator macros |
| `printer.filament.cfg` | The filament wheel (clog) sensors |
| `printer.probe.cfg`, `printer.mesh.cfg` | The probe and the bed mesh |
| `printer.vibration.cfg` | The accelerometer and input shaper |
| `printer.motor.cfg` | The tool lock motor |
| `printer.pro.cfg` | Creator 5 Pro only: chamber heater and fans |
| `printer.mpc.cfg` | Kalico MPC helpers |
| `printer.override.cfg` | Shared pins and sections other files need first |
| `AFC/AFC.cfg`, `printer.afc-standalone.cfg` | AFC, lanes and tools |

The installer and the printer write a few files themselves:
`printer.model.cfg`, `printer.accessory.*.cfg` and `printer.ace*.cfg`.
`c5_misc_variables.cfg` holds the switch states and `AFC/AFC.var.*` AFC's state.
Leave these alone.

## How updates treat your changes

Every file OpenCreator ships is a template. When an update brings a new
version, the updater merges it into your copy:

- **Your edits stay**, unless the update changed the very same lines.
- **Your calibration stays.** The `SAVE_CONFIG` block (input shaper, PID,
  meshes, skew) is never merged, only carried along.
- **Same lines changed on both sides:** your file is left exactly as it is, and
  the merged version, with `<<<<<<<` markers, is put next to it as
  `<file>.conflict`. See [Resolving a conflict](#resolving-a-conflict).

Files that aren't templates, like a file you create, are never touched by
updates.

## The easy way: your own config file

Instead of editing the shipped files, put your changes in a file of your own.
Klipper reads a section that appears twice as one, and the value it reads last
wins, so a file included at the end overrides the shipped values without
changing them. Updates then never conflict with your settings.

1. Create `my-printer.cfg` in the config folder. Any name works that isn't one of
   OpenCreator's files.
2. In `printer.cfg`, add one line under `[include plugins/*.cfg]`, above the
   `SAVE_CONFIG` block:

    ```ini
    [include plugins/*.cfg]
    [include my-printer.cfg]
    ```

3. Put the options you want to change in `my-printer.cfg`, under their section
   name, and restart Klipper.

Only the options you list change; everything else keeps the shipped value. For
example:

```ini
# my-printer.cfg

# Slower final approach into the docks
[creator5_toolchanger]
dock_approach_speed: 80

# A finer bed mesh
[bed_mesh]
probe_count: 9, 9

# Your own macro
[gcode_macro PARK_FRONT]
description: Park the toolhead at the front
gcode:
    G90
    G1 X130 Y10 F12000
```

A macro you define again here replaces the shipped one, since its `gcode` is just
another option. To see which value Klipper actually used, open
`http://<printer-ip>:7125/printer/objects/query?configfile=settings` in a
browser, or check the config dump near the top of `printer.log` after a restart.

!!! warning "Leave calibration to SAVE_CONFIG"
    Don't put values that calibration saves, like input shaper, PID, the bed
    mesh or the probe `z_offset`, in your own file. `SAVE_CONFIG` refuses to save
    an option whose value comes from an included file. Run the calibration and
    let `SAVE_CONFIG` store the result at the bottom of `printer.cfg`.

!!! note
    You can't remove a section from another file this way, only change its
    options. To turn a section off, comment it out where it is. Updates keep
    that edit like any other.

## Editing the shipped files

Editing a template directly is fine too. Updates keep your change unless they
change the same lines. It's the better choice for small fixes in place, like
commenting out a line, where an override file can't help.

## Resolving a conflict

When an update and your edit changed the same lines, the updater leaves your
file alone and writes `<file>.conflict` next to it. Your printer keeps running on
your version; only that file's update is waiting. The updater's log is
`/tmp/opencreator-updater.log` on the printer.

1. Open `<file>.conflict`. Each conflict shows your version (`current`) and the
   update's (`latest`):

    ```text
    <<<<<<< current
    dock_approach_speed: 80
    =======
    dock_approach_speed: 100
    >>>>>>> latest
    ```

2. Keep the lines you want, delete the other side and the three marker lines.
3. Copy the result over the original file, delete the `.conflict` file, and
   restart Klipper.

The next update merges from there. Moving your changes to your own file
(above) stops them from conflicting again.

## Going back

- **One option:** delete it from your own file, or put the shipped value back.
- **A whole file:** the last template the updater applied is kept in
  `.opencreator-updater/base/<file>`. Copy it over your file to get the shipped
  version back.
- **Everything:** *Restore backup* in the [OpenCreator Installer](installer.md),
  if you made a backup when you installed.

Keep a copy of `my-printer.cfg` somewhere safe. It's all you need to set a fresh
install up the way you like it.
