# Slicer Configuration for Orca / Flash Studio
A collection of configurations and settings for Custom Firmware OpenCreator.

## Print Profile changes
Under "Quality" & "Precision" switch "`Arc fitting`" to `off`.
This will improve performance with no quality loss as Arcs
are not supported under Klipper.

## Printer Profile changes
These will change stuff like printer agents.

### General connection changes
Go to the Wifi connect button (next to printer) and change your host type to "`Moonraker`"
and remove any serial numbers or API keys.

### General profile changes
Go to the printer settings, go to "Printer Agent" and switch it to `Moonraker`.
Set "Use 3MF instead of G-code to `off` from `on`.
Set `Disable set remaining print time` to `off`

### Machine G-code
*tbd*

## Optional / Niceties
Set retraction and deretraction to 75 on all extruders, should make it much faster, and doesn't seem to have much stringing.
Set Z-hop height to 0.24 on all extruders, as it will reduce stress on the Z axis and will make your prints slightly faster.