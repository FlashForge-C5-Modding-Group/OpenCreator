# OpenCreator CFW Full Feature Set

| Feature | Implementation | Description | 
| --- | --- | --- |
| ZMax on Print End | Finished | Puts the bed to the very bottom of the build area, for easy grabbing, a suggestion from @josemyab |
| Allow for near zero purge printing | Finished | You can turn off purge and flow calibration to have almost zero purge on start, a half suggestion from @josemyab |
| Allow for calibration of only one toolhead | Finished | You don't need to calibrate all offsets to update zoffset of a nozzle, for switching nozzles. |
| Filament runout and switchover support | Untested | You can either "Infinite Spool" through AFC to runout to another filament, or pause if nothing is selected. |
| AFC Toolchanger | Finished | Allows for the use of AFC filament system, but with toolchangers. |
| GrumpyScreen Toolchanger Support | Semi-Finished | GrumpyScreen now properly shows all extruders and currently in use extruder, but you cannot extrude yet. |
| Stock Feature Parity | Semi-Finished | Currently doesn't have clog detection, but runout, auto PA, and others are done. |
| Full WebUI support | Finished | You can avoid using the touchscreen, and don't even need it in general. |
| Automatic Fan control for Heated Chamber (Pro Only) | Finished | Removes user fan control, and makes it automatically turn on when the chamber is set to something. |
| Make Camera Better / Faster | Finished | Uses pre-installed MJPEG streamer over stock camera control in the touchscreen executable, making it efficient and fast. |
| Allow to switch toolheads arbitraily | Finished | You can send commands T0-T3 or click the buttons to pick up the toolheads. |
| Allow to load multiple toolheads simultaneously | Untested | You can send the load command for multiple tools, which changes them, loads, then docks. |
| Allow for reading colour information and material information on Orca | Not Working | You will be able to eventually syncronize filament list from "AMS" |