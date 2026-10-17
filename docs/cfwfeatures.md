# OpenCreator CFW Full Feature Set

<span class="oc-status ok">Finished</span> works on a printer &nbsp;
<span class="oc-status partial">Semi-Finished</span> partly done &nbsp;
<span class="oc-status test">Testing</span> in testing or untested &nbsp;
<span class="oc-status bad">Broken</span> not working yet

| Feature | Implementation | Description | 
| --- | --- | --- |
| ZMax on Print End | <span class="oc-status ok">Finished</span> | Puts the bed to the very bottom of the build area, for easy grabbing, a suggestion from @josemyab |
| Allow for near zero purge printing | <span class="oc-status ok">Finished</span> | You can turn off purge and flow calibration to have almost zero purge on start, a half suggestion from @josemyab |
| Allow for calibration of only one toolhead | <span class="oc-status ok">Finished</span> | You don't need to calibrate all offsets to update z-offset of a nozzle, for switching nozzles. |
| Allow for configuration of nozzle calibration | <span class="oc-status ok">Finished</span> | If your nozzle doesn't fit the min mm for levelboard, you can set it higher. |
| Filament runout and switchover support | <span class="oc-status test">Untested</span> | You can either "Infinite Spool" through AFC to runout to another filament, or pause if nothing is selected. |
| AFC Toolchanger | <span class="oc-status ok">Finished</span> | Allows for the use of AFC filament system, but with toolchangers. Should be fully functional except for "loaded" filament buttons. |
| GrumpyScreen Toolchanger Support | <span class="oc-status partial">Semi-Finished</span> | GrumpyScreen now properly shows all extruders and currently in use extruder, but you cannot extrude yet. |
| Stock Feature Parity | <span class="oc-status ok">Finished</span> | Should have feature parity. |
| Full WebUI support | <span class="oc-status ok">Finished</span> | You can avoid using the touchscreen, and don't even need it in general. |
| Automatic Fan control for Heated Chamber (Pro Only) | <span class="oc-status ok">Finished</span> | Removes user fan control, and makes it automatically turn on when the chamber is set to something. |
| Make Camera Better / Faster | <span class="oc-status ok">Finished</span> | Uses pre-installed MJPEG streamer over stock camera control in the touchscreen executable, making it efficient and fast. |
| Allow to switch toolheads arbitraily | <span class="oc-status ok">Finished</span> | You can send commands T0-T3 or click the buttons to pick up the toolheads. |
| Allow to load multiple toolheads simultaneously | <span class="oc-status test">Untested</span> | You can send the load command for multiple tools, which changes them, loads, then docks. |
| Allow for reading colour information and material information on Orca | <span class="oc-status bad">Not Working</span> | You will be able to eventually syncronize filament list from "AMS" |
| Raspberry Pi Host support | <span class="oc-status test">Testing</span> | Allowing to use a Raspberry Pi over the stock MIPS32 CPU, by tunneling the MCUs through the SoC through gadget mode USBs. |
| Power Loss Recovery | <span class="oc-status ok">Finished</span> | Allows to resume a print from power loss or MCU errors. Until you resume or discard it, the bed is kept at the print's temperature so the part stays stuck down; discarding turns the heaters off. |
| Kalico Support | <span class="oc-status test">Testing</span> | On non-SoC installs, you can use Kalico's stronger MPC tuning or PID tuning, and eventually bleeding edge for better input shaper. |
| Removal of FlashForge services | <span class="oc-status ok">Finished</span> | You can completely disable FlashForge's things, and make it like it never existed. Full Orcaslicer with Moonraker Agent. |
| Filament sync | <span class="oc-status bad">Broken</span> | Probably due to an Orca bug, but you will be able to eventually syncronize your filaments (material wise) |
| Turn off cooling fan when chamber heater is on | <span class="oc-status ok">Finished</span> | It will actively say in console that it has been turned off, making it where cold air won't mess with the heated chamber |
| Bed warp stabilization (bed soak) | <span class="oc-status ok">Finished</span> | Optionally waits at bed temperature for a configurable number of minutes immediately before a fresh mesh, letting the plate finish warping/expanding so the mesh reflects its settled shape instead of its heat-up transient. |
| Chamber heat wait toggle | <span class="oc-status ok">Finished</span> | Controls whether M191 blocks until the chamber actually reaches target, or just sets the target and lets the print start heating in the background while early layers print (M141-like). Toggle with C5_MISC_CHAMBER_WAIT_ON / OFF. |
| OpenCreator Updater | <span class="oc-status ok">Finished (Pi-tunnel)</span> <span class="oc-status test">Untested (SoC)</span> | A dedicated Moonraker update_manager entry that merges the latest config template into your live printer_data/config without clobbering values you've customized (speeds, toggle defaults, etc), then pulls Klipper -- all from one "Update" click. Uses a real three-way merge, so a conflicting change is left for you to resolve by hand instead of guessed at or silently overwritten. SoC/LoopScript installs get an equivalent boot-time version, not yet verified on real hardware. |
| First-time setup dialog | <span class="oc-status test">Untested</span> | After installing, Fluidd, Mainsail and the screen walk you through turning Clog Detection and bed heat soak on or off and running PID, input shaper and VFA calibration (saved together at the end), then point you to the Discord. Shown until finished once; `C5_FIRST_SETUP` shows it again. |
| 3MF uploads | <span class="oc-status test">Untested</span> | Upload a sliced `.gcode.3mf` from OrcaSlicer or Bambu Studio straight to Fluidd or from the slicer; the first sliced plate is extracted with its thumbnail. SBC installs. |
| OpenCreator Fluidd theme | <span class="oc-status ok">Finished (Pi-tunnel)</span> | An extra-dark Fluidd theme with the OpenCreator logo and orange accents, installed and updated with the config. |
| Updater self-repair | <span class="oc-status test">Untested</span> | `repair.sh` fixes "No git repo detected" in the update manager: starts git where it is missing, syncs to the official main branch, fixes `moonraker.conf`. |
| Chamber fan follows the heater | <span class="oc-status ok">Finished</span> | The chamber heater fan turns off as soon as the chamber heater's target is cleared, instead of running until the chamber cools below 50 °C. |
| Wi-Fi beep fallback | <span class="oc-status ok">Finished (Pi-tunnel)</span> | If a board is pinned to 4 tunnel ports (no dedicated beep serial channel), C5_BUZZER can fall back to sending the beep request over the network to the tunnel bridge instead of silently going quiet. Opt-in via network_host in [creator5_remote_beeper]. |