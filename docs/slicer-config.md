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

!!! tip "3MF on SBC installs"
    [SBC installs](rpi-klipper.md#3mf-uploads) also accept sliced 3MF uploads, but plain G-code
    works on every install, so keep 3MF off unless you need it.

### Machine G-code

*tbd*

## Optional / niceties

!!! tip "Faster retractions"
    Set retraction and deretraction speed to 50 on all extruders. It should make it much faster,
    and doesn't seem to cause much stringing.

!!! tip "Lower Z-hop"
    Set Z-hop height to 0.24 on all extruders. It reduces stress on the Z axis and makes your
    prints slightly faster.
