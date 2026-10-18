# Slicer Configuration for Orca / Flash Studio

A collection of configurations and settings for Custom Firmware OpenCreator.

## Print profile

- [ ] Under **Quality → Precision**, switch `Arc fitting` **off**.
  This will improve performance with no quality loss, as arcs are not
  supported under Klipper.

## Printer profile

These will change stuff like printer agents.

### Connection

- [ ] Click the Wi-Fi connect button (next to the printer) and change the host type to `Moonraker`.
- [ ] Remove any serial numbers or API keys.

### Printer settings

- [ ] **Printer Agent:** switch to `Moonraker`.
- [ ] **Use 3MF instead of G-code:** switch from `on` to `off`.
- [ ] **Disable set remaining print time:** `off`.

[![OrcaSlicer printer settings, Basic information: Printer Agent set to Moonraker, Use 3MF instead of G-code and Disable set remaining print time both unticked](assets/images/orca-printer-settings.png){ .oc-shot loading=lazy }](assets/images/orca-printer-settings.png)

!!! tip "3MF on SBC installs"
    [SBC installs](rpi-klipper.md#3mf-uploads) also accept sliced 3MF uploads, but plain G-code
    works on every install, so keep 3MF off unless you need it.

### Machine G-code

- [ ] **File start G-code:** clear it (leave it empty).
  The FlashForge profile puts a FlashForge-style time estimate there, and Orca
  also copies that setting into the end of the file, where Moonraker reads it
  instead of the real estimate. With it in place, Fluidd and Mainsail show no
  slicer time estimate.

!!! note "Files sliced before the change"
    OpenCreator's Moonraker component works around this for files uploaded from
    now on. Files already on the printer show no estimate until you upload them
    again.

- [ ] **Layer change G-code (optional, for timelapses):** add
  `TIMELAPSE_TAKE_FRAME` on its own line, so [Timelapse](printer-features.md#timelapse)
  takes a frame at every layer.

## Optional / niceties

!!! tip "Faster retractions"
    Set retraction and deretraction speed to 50 on all extruders. It should make it much faster,
    and doesn't seem to cause much stringing.

!!! tip "Lower Z-hop"
    Set Z-hop height to 0.24 on all extruders. It reduces stress on the Z axis and makes your
    prints slightly faster.
