# OpenCreator CFW Full Feature Set

| Feature | Implementation | Description | 
| --- | --- | --- |
| ZMax on Print End | Finished | Puts the bed to the very bottom of the build area, for easy grabbing, a suggestion from @josemyab |
| Allow for near zero purge printing | Finished | You can turn off purge and flow calibration to have almost zero purge on start, a half suggestion from @josemyab |
| Allow for calibration of only one toolhead | Finished | You don't need to calibrate all offsets to update z-offset of a nozzle, for switching nozzles. |
| Allow for configuration of nozzle calibration | Finished | If your nozzle doesn't fit the min mm for levelboard, you can set it higher. |
| Filament runout and switchover support | Untested | You can either "Infinite Spool" through AFC to runout to another filament, or pause if nothing is selected. |
| AFC Toolchanger | Finished | Allows for the use of AFC filament system, but with toolchangers. Should be fully functional except for "loaded" filament buttons. |
| GrumpyScreen Toolchanger Support | Semi-Finished | GrumpyScreen now properly shows all extruders and currently in use extruder, but you cannot extrude yet. |
| Stock Feature Parity | Finished | Should have feature parity. |
| Full WebUI support | Finished | You can avoid using the touchscreen, and don't even need it in general. |
| Automatic Fan control for Heated Chamber (Pro Only) | Finished | Removes user fan control, and makes it automatically turn on when the chamber is set to something. |
| Make Camera Better / Faster | Finished | Uses pre-installed MJPEG streamer over stock camera control in the touchscreen executable, making it efficient and fast. |
| Allow to switch toolheads arbitraily | Finished | You can send commands T0-T3 or click the buttons to pick up the toolheads. |
| Allow to load multiple toolheads simultaneously | Untested | You can send the load command for multiple tools, which changes them, loads, then docks. |
| Allow for reading colour information and material information on Orca | Not Working | You will be able to eventually syncronize filament list from "AMS" |
| Raspberry Pi Host support | Testing | Allowing to use a Raspberry Pi over the stock MIPS32 CPU, by tunneling the MCUs through the SoC through gadget mode USBs. |
| Power Loss Recovery | Testing | Allows to resume a print from power loss or MCU errors. |
| Kalico Support | Testing | On non-SoC installs, you can use Kalico's stronger MPC tuning or PID tuning, and eventually bleeding edge for better input shaper. |
| Removal of FlashForge services | Finished | You can completely disable FlashForge's things, and make it like it never existed. Full Orcaslicer with Moonraker Agent. |
| Filament sync | Broken | Probably due to an Orca bug, but you will be able to eventually syncronize your filaments (material wise) |
| Turn off cooling fan when chamber heater is on | Finished | It will actively say in console that it has been turned off, making it where cold air won't mess with the heated chamber |
| Bed warp stabilization (bed soak) | Finished | Optionally waits at bed temperature for a configurable number of minutes immediately before a fresh mesh, letting the plate finish warping/expanding so the mesh reflects its settled shape instead of its heat-up transient. |
| Chamber heat wait toggle | Finished | Controls whether M191 blocks until the chamber actually reaches target, or just sets the target and lets the print start heating in the background while early layers print (M141-like). Toggle with C5_MISC_CHAMBER_WAIT_ON / OFF. |
| OpenCreator Updater | Finished (Pi-tunnel) / Untested (SoC) | A dedicated Moonraker update_manager entry that merges the latest config template into your live printer_data/config without clobbering values you've customized (speeds, toggle defaults, etc), then pulls Klipper -- all from one "Update" click. Uses a real three-way merge, so a conflicting change is left for you to resolve by hand instead of guessed at or silently overwritten. SoC/LoopScript installs get an equivalent boot-time version, not yet verified on real hardware. |
| Wi-Fi beep fallback | Finished (Pi-tunnel) | If a board is pinned to 4 tunnel ports (no dedicated beep serial channel), C5_BUZZER can fall back to sending the beep request over the network to the tunnel bridge instead of silently going quiet. Opt-in via network_host in [creator5_remote_beeper]. |