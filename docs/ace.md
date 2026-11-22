---
description: How to set up and control an Anycubic ACE Pro or ACE 2 Pro on an OpenCreator Creator 5.
---

# ACE Pro and ACE 2 Pro

OpenCreator runs an Anycubic ACE Pro or ACE 2 Pro through AFC's own ACE unit
(`AFC_ACE` for the ACE Pro, `AFC_ACE2` for the ACE 2 Pro). On a Creator 5 each
ACE slot feeds its own head: slot 1 feeds T0, slot 2 feeds T1, and so on. The
heads' filament switches, about 450 mm before the gears, are where a feed
stops.

Everything below lives in `printer.ace.cfg` next to `printer.cfg`. Change it in
Fluidd or Mainsail and restart Klipper (`FIRMWARE_RESTART`) to apply.

!!! note
    The ACE support described here is on the `ace-upstream` Kalico branch.

## What happens with filament

| You do | The printer does |
| --- | --- |
| Put a new spool in a slot | The ACE grips it, then feeds it all the way to that head's filament switch and stops. A spool put in during a print waits until the print ends. |
| Press load (Fluidd, Mainsail or GrumpyScreen) | The filament goes from the switch into the gears, the head heats to the filament's temperature and purges. |
| Press unload | The head heats first, the filament is pulled out of the head, and the ACE winds it back to just outside the unit. |
| Press eject | The ACE winds the filament fully back into the unit so you can take the spool out. |

The load temperature comes from the spool: its Spoolman temperature if it has
one, otherwise its material (`default_material_temps` in `AFC/AFC.cfg`, then a
built-in table of about 50 filament types), otherwise the `default` there.

## The unit

```ini
[AFC_ACE2 ACE_1]          # [AFC_ACE ACE_1] for an ACE Pro
serial_port: auto
mode: direct
feed_speed: 100
retract_speed: 100
insert_feed_to_toolhead: True
```

| Option | Default | What it does |
| --- | --- | --- |
| `serial_port` | | `auto` finds the ACE on USB, or give the device path. |
| `ace_index` | 1 | Which ACE this is, when there is more than one. |
| `mode` | `combined` | `direct`: each slot feeds its own head (the Creator 5 setup). `combined`: slots share a hub. |
| `feed_speed` / `retract_speed` | 100 | Feed and wind-back speed in mm/s. |
| `insert_feed_to_toolhead` | False | Feed a new spool to its head's switch on insert (on in the Creator 5 config). Off: it is only parked `dist_hub` out of the unit. |
| `insert_feed_step` | 10 | The last part of that feed goes in steps of this many mm, checking the switch after each. |
| `insert_feed_extra` | 250 | How far past the expected switch position it keeps stepping before giving up and winding back. |
| `load_approach_length` / `load_approach_speed` | 30 / 50 | The last stretch before the switch goes this far at this slower speed. |
| `load_retry_pulse` / `load_retry_timeout` | 100 / 10 | If the switch has not triggered after the feed, pulse this many mm for up to this many seconds. |
| `sensor_to_gears` | 0 | After the switch, push this far on to the gears. 625 on a Creator 5. |
| `sensor_to_gears_speed` | 25 | Speed for that push. |
| `sensor_to_gears_catch` | 0 | The last this-many mm of it run with the extruder turning, so the gears catch the tip. 200 on a Creator 5. |
| `use_feed_assist` | True | The ACE keeps feeding while the active head prints, so the extruder does not drag filament through the tube. |
| `assist_watchdog` | True | Turns feed assist back on if it drops out during a print. |
| `stuck_spool_detection` | True (ACE 2) | Pause when the spool stops turning while it should feed (a tangle). |
| `stuck_time` | 4 | Seconds without spool movement before that counts as stuck. |
| `eject_buffer` | 475 | Extra mm to wind back on eject, past `dist_hub`, to pull the filament fully into the unit. |
| `max_dryer_temperature` | 70 (ACE 2), 55 | The highest dryer temperature `AFC_ACE_DRY` accepts. |
| `fallback_without_ace` | True | With no ACE on USB, start without it (the heads work as their own lanes) instead of failing. |
| `missing_ace_wait` | 20 | Seconds to wait for the ACE to appear after a boot before running without it. |
| `serial_log` | False | Log every ACE message to `AFC_ACE_serial.log`. Only for debugging: it grows by about 10 MB every few hours. |

## The lanes, hubs and heads

Each slot is a lane, with a hub (the tube to its head) and the head itself:

```ini
[AFC_lane lane1]
unit: ACE_1:1          # unit and slot
extruder: extruder0    # the head this slot feeds
hub: ACE_1_hub1
map: T0
dist_hub: 60           # mm from the slot to the park point just out of the unit

[AFC_hub ACE_1_hub1]
switch_pin: virtual
afc_bowden_length: 900          # park point to the head's switch
afc_unload_bowden_length: 900   # how far to wind back on unload

[AFC_extruder extruder0]
tool_stn: 70           # switch/gears to the nozzle on load
tool_stn_unload: 100   # how far the head pulls back on unload
tool_load_speed: 6
tool_unload_speed: 10
```

`afc_bowden_length` can be on the short side: the load pulses on until the
switch triggers. To measure it, run `AFC_ACE_CALIBRATE UNIT=ACE_1 LANE=lane1`
with the head empty. It feeds until the switch triggers and saves the length.

## Commands

All take `UNIT=ACE_1` (your unit's name).

| Command | What it does |
| --- | --- |
| `AFC_ACE_STATUS` | The unit's status and each slot's state. |
| `AFC_ACE_DRY TEMP=<C> DURATION=<minutes> [ROTATE=1]` | Start the dryer. `ROTATE=1` turns the empty slots now and then. |
| `AFC_ACE_DRY_STOP` | Stop the dryer. |
| `AFC_ACE_TEMP_INFO` | Box, heater and room temperature and humidity. |
| `AFC_ACE_FAN SPEED=<0-100>` | The unit's fan (ACE 2). |
| `AFC_ACE_CALIBRATE LANE=<lane>` | Measure the park point to switch distance (`afc_bowden_length`). |
| `AFC_ACE_CALIBRATE_HUB LANE=<lane>` | Measure the slot to park point distance (`dist_hub`). |
| `AFC_ACE_LANE_RESET LANE=<lane>` | Wind a lane's filament back into the unit. |
| `AFC_ACE_STUCK_SPOOL_DETECTION ENABLE=0\|1 [STUCK_TIME=<s>]` | Turn tangle detection on or off until the next restart. |
| `AFC_ACE_SET_MATERIAL SLOT=<n> NAME=<text>` | Store a material name in a slot (ACE 2). |
| `AFC_ACE_BUFFER_STATE` | Each slot's buffer and filament sensors, for troubleshooting. |

The normal AFC commands work too: `TOOL_LOAD LANE=lane1`, `TOOL_UNLOAD`,
`LANE_UNLOAD LANE=lane1` (eject), `SET_MATERIAL`, `SET_COLOR`.

## The ACE's temperature and humidity

Add the ACE's sensor to show it on the Fluidd or Mainsail dashboard:

```ini
[temperature_sensor ACE_1]
sensor_type: aht2x
afc_type: ACE2        # ACE for an ACE Pro
ace_unit: ACE_1
min_temp: 0
max_temp: 85
```

## Running without the ACE

Unplug the ACE and restart Klipper: with `fallback_without_ace: True` the
printer starts without it, and each head works as its own lane again (load by
hand at the head). Plug it back in and restart Klipper to use it.
