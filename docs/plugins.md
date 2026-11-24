# Plugins

OpenCreator CFW can install plugins: add-ons that bring their own Klipper modules, macros and Moonraker
components without changing OpenCreator's own files. A plugin is a git repository with an
`opencreator-plugin.json` manifest. The [example plugin](https://github.com/FlashForge-C5-Modding-Group/OpenCreator-Plugin-Example)
shows each part and is the place to start when making one.

## Using plugins

Plugins are managed with `oc-plugin` over SSH:

| Command | What it does |
| --- | --- |
| `oc-plugin search [word]` | The [plugin catalog](#plugin-catalog), or the entries matching a word. |
| `oc-plugin install <id or git URL>` | Download and enable a plugin, by its catalog id or from any git URL. `--ref <branch or tag>` picks a version. |
| `oc-plugin list` | Installed plugins, their version, whether they're enabled, and the git branch and commit. Plugins not made for this printer are marked. |
| `oc-plugin update [id]` | Pull the newest version of one plugin, or all of them. |
| `oc-plugin disable <id>` / `enable <id>` | Turn a plugin off or back on without removing it. |
| `oc-plugin remove <id>` | Uninstall it. `--keep-data` keeps its settings for a reinstall. |
| `oc-plugin status` | Each plugin's [background programs](#startup-items-and-scripts): running or stopped, and their log file. |
| `oc-plugin logs <id> [item]` | The end of a plugin's logs. `-n 200` for more lines, `-f` to keep following. |
| `oc-plugin start` / `stop` / `restart <id> [item]` | Start, stop or restart a plugin's background programs. |
| `oc-plugin sync` | Redo the links and includes; see [How it works](#how-it-works). |
| `oc-plugin moonraker [URL]` | Tunneled printers: show or set the SBC's Moonraker. `--find` looks for it, `--scan` lists every one and asks which. See [Tunneled printers](#tunneled-printers). |

After a change, `oc-plugin` restarts Klipper and/or Moonraker itself, whichever the plugin needs. It never
does that while the printer is printing or paused: it says so, and the change takes effect at the next restart.
`--no-restart` skips the restart.

Plugins can say which printers and installs they're made for. `install` refuses one that isn't made for
yours, and `update` keeps the old version if a new one isn't, or if its manifest is broken. `--force` installs
it anyway. The [SSH login banner](installer.md#login-banner) shows how many plugins are installed.

## Plugin catalog

`oc-plugin search` lists the plugins in OpenCreator's catalog, split into official and unofficial ones, with
where each one runs (SoC, SBC or both). Install one by its id, for example `oc-plugin install hello`.

| Plugin | Id | Runs on | |
| --- | --- | --- | --- |
| [Hello OpenCreator](https://github.com/FlashForge-C5-Modding-Group/OpenCreator-Plugin-Example) | `hello` | SoC, SBC | Official. The example plugin: a Klippy module, a macro and a Moonraker endpoint. |
| [Printer Buddy Lite](https://github.com/FlashForge-C5-Modding-Group/printer-buddy-lite) | `sep-ai` | SoC, SBC (tunneled) | Official. Print failure detection on the printer's own X2600 RISC-V core, with a web page on port 7180. On a tunneled printer it's installed on the printer and finds the SBC's Moonraker by itself. |
| [Shake&Tune (numpy only)](https://github.com/FlashForge-C5-Modding-Group/numpy-shaketune) | `numpy-shaketune` | SoC | Official, experimental. Klippain Shake&Tune on the printer itself, with a numpy-only stand-in for matplotlib. Needs a lot of system resources: each graph keeps the CPU busy for about a minute and uses up to about 130 MB of memory. Needs git (`opkg install git`). Graphs go to `ShakeTune_results` next to `printer.cfg`. |
| [OctoEverywhere for OpenCreator](https://github.com/KidCe/opencreator-octoeverywhere) | `octoeverywhere` | SoC | Unofficial. OctoEverywhere remote access and cameras. After installing, run `sh /usr/data/opencreator/plugins/octoeverywhere/install.sh` and follow its README. |
| [OpenCreator Display Manager](https://github.com/pappicio/display-manager) | `display-manager` | SoC, SBC | Unofficial. Switch between HelixScreen and GrumpyScreen with Klipper macros. Install Script Runner first. May slow the printer down; no support in SoC mode, including for slowdowns or MCU shutdowns. |
| [OpenCreator Spoolman Integration](https://github.com/JayKuswahey/OpenCreator-Plugin-Spoolman) | `oc_spoolman_integration` | SoC, SBC | Unofficial. A Spoolman spool per tool through AFC, so each tool books filament on its own spool. After installing, put your Spoolman server's address in `oc-spoolman.conf` (next to `moonraker.conf`), uncomment its `[spoolman]` section, then restart Moonraker and run `FIRMWARE_RESTART`. |
| [Orca Filament Profiles](https://github.com/Xmister/opencreator-orca-filament-profiles) | `orca-filament-profiles` | SoC, SBC | Unofficial. Pick which OrcaSlicer filament preset each AFC material uses when Orca syncs filaments (OrcaSlicer 2.5 or later). AFC now supports this itself, so this plugin may be of little use. If you still want it, put your mappings in `orca-filament-profiles/mapping.cfg` next to `printer.cfg` and restart Klipper. |
| [Samba Share](https://github.com/pappicio/samba-share) | `samba-share` | SoC, SBC | Unofficial. Shares `/usr/data` on the network over SMB. Needs Entware. The share is writable with guest access, so use it only on a trusted local network. |
| [Script Runner](https://github.com/pappicio/oc-script-runner) | `script-runner` | SoC, SBC | Unofficial. Runs your own shell and Python scripts at startup, in SoC and tunneled mode, from `/usr/data/opencreator/scripts/` (`both/`, `soc/` or `tunneled/`). Needed by Touchscreen Server, so install it first. |
| [Touchscreen Server](https://github.com/pappicio/touchscreen-server-plugin) | `touchscreen-server` | SoC, SBC | Unofficial. Remote access to the GrumpyScreen or HelixScreen touchscreen from a web browser. Install Script Runner first; after installing, open the URL shown in the install output. May slow the printer down, and it's only supported on tunneled printers. |
| [API Server](https://github.com/pappicio/api-server) | `api-server` | SoC, SBC | Unofficial. HTTP API endpoints for Creator 5 mode switching and synchronization with a remote Linux host. Requires Script Runner on the Creator 5 and [Mode Monitor](https://github.com/pappicio/Raspberry-C5-klipper_mode_monitor) on the Raspberry Pi, Radxa ZERO 3W or other compatible remote Linux host. Both components are required for complete mode switching and data synchronization. |

Official plugins are made or checked by the OpenCreator team. Unofficial ones are listed so they're easy to
find, but nobody from OpenCreator has reviewed them: read what a plugin does before installing it.

The catalog is `catalog.json` in [opencreator-fs](https://github.com/FlashForge-C5-Modding-Group/opencreator-fs)
(`fs/usr/data/opencreator/plugin-manager/`). `oc-plugin` downloads it from GitHub each time, so a newly
listed plugin shows up right away, without an OpenCreator update. Offline, it uses the last download
(`plugins/.catalog.json`), or the copy that came with OpenCreator. To get a plugin listed, open a pull
request adding it; the
[example plugin's README](https://github.com/FlashForge-C5-Modding-Group/OpenCreator-Plugin-Example#getting-into-the-catalog)
shows an entry.

Plugins installed from a git URL also show up in Fluidd and Mainsail's update manager as
**oc-plugin-&lt;id&gt;**, so they can be updated from there like OpenCreator itself.

To see what's installed without SSH, run the `OC_PLUGINS` macro, or tap **Plugins** in GrumpyScreen's
macros. It opens a dialog listing every plugin with its version, greyed out if it's disabled and red if its
manifest is broken. Tap one for its description, author, what it adds to Klipper and Moonraker, and where it
came from. `oc-plugin sync` rebuilds the list (as `config/plugins/_oc-plugins.cfg`) every time Klipper
starts, so it stays current. Installing, updating and removing still happen over SSH.

To browse every plugin in the catalog, run `OC_PLUGIN_MARKET`, tap **Plugin Marketplace** in GrumpyScreen's
macros, or tap **Marketplace** at the bottom of the `OC_PLUGINS` dialog. Official plugins are blue, unofficial
ones yellow, installed ones grey and ones that don't run on this printer red. Tap one for its description,
author, notes, link and the `oc-plugin install` command for it. The list comes from the catalog the printer last
fetched: it's refreshed at every boot and whenever `oc-plugin search`, `install` or `update` runs, and Klipper
shows the new list from its next start.

### ZMOD plugins

`oc-plugin install` also takes a plugin made for [ZMOD](https://wiki.zmod.link/Plugin/), the AD5M/AD5X/C5(P) mod,
for example `oc-plugin install https://github.com/ghzserg/g28_tenz`. Support is basic. A ZMOD plugin has no
manifest, just a repo named after the plugin with `<name>.cfg` (or `en/<name>.cfg` if it's translated), so
`oc-plugin` takes its id from the repo name and its version from git, and links only that config into Klipper.
After that it's listed, updated, disabled and removed like any other plugin.

Its `install.sh`, `uninstall.sh` and `update.sh` are not run, because they expect ZMOD's folders, and Python
modules have to be linked by hand, as on ZMOD itself. A plugin whose config uses ZMOD's own macros or settings
won't work here as it is. Most plugins are expected to be rewritten.

!!! warning "Plugins run with full access"
    A plugin's Klipper and Moonraker code runs inside Klipper and Moonraker, with the same access to the
    printer as they have. Only install plugins you trust. An attacker could gain access to your network
    via a malicious plugin.

## Making an OpenCreator plugin

Copy the [example plugin](https://github.com/FlashForge-C5-Modding-Group/OpenCreator-Plugin-Example). Its
README lists every manifest key. In short:

```json
{
  "id": "hello",
  "name": "Hello OpenCreator",
  "version": "1.0.0",
  "klippy_extras": ["klippy/oc_hello.py"],
  "klipper_config": "config/hello.cfg",
  "moonraker_components": ["moonraker/oc_hello.py"],
  "moonraker_config": "moonraker/hello.conf"
}
```

- **`klippy_extras`:** Klippy modules, the same as a file in `klippy/extras`. A module is loaded by
  the config section of the same name, so `oc_hello.py` is loaded by `[oc_hello]`.
- **`klipper_config`:** one Klipper config file, with your module's section and macros. It's included after
  the rest of `printer.cfg`, so it can add to OpenCreator's sections.
- **`moonraker_components`** and **`moonraker_config`:** the same, for Moonraker.

Module names must be unique. A plugin whose module has the same name as one of Kalico's, or as
another plugin's, is refused and isn't installed. Give yours a prefix.

### Startup items and scripts

A plugin can also run its own programs:

```json
"startup": [{ "name": "server", "exec": "bin/server.sh", "type": "service" }],
"scripts": { "install": "bin/install.sh", "update": "bin/install.sh", "uninstall": "bin/uninstall.sh" }
```

- **`startup`:** programs started when the plugin is installed or enabled and at every boot, and stopped
  when it's disabled or removed. A `service` is started again if it exits (5 s later, backing off to a
  minute while it keeps failing). A `once` item runs at boot and stops. `args` and `delay` (up to 600 s)
  are optional.
- **`scripts`:** `install` runs before anything is linked, and the plugin isn't installed if it fails.
  `update` runs after `oc-plugin update` pulls a new version, and `uninstall` before `oc-plugin remove`.
  Each gets up to 15 minutes.

Both run in the plugin folder with `OC_PLUGIN_ID`, `OC_PLUGIN_DIR`, `OC_PLUGIN_DATA`, `OC_CONFIG_DIR`,
`OC_KLIPPER_DIR` and `OC_MOONRAKER_URL` set. `OC_PLUGIN_DATA` is a folder for the plugin's own files, and it's
kept across updates. Startup items log to `/tmp/oc-plugin-<id>-<name>.log`, which `oc-plugin logs` shows. On
the printer, `/tmp` is in RAM, so a log is moved aside to `.log.1` once it passes 512 KB.

### Compatibility

```json
"requires": { "models": ["Creator5Pro"], "modes": ["soc"] }
```

- **`models`:** `Creator5` and/or `Creator5Pro`, for hardware only one of them has, like the Pro's chamber
  heater. The model comes from the stock boot script, or from `printer.model.cfg` on SBC installs. If it
  can't be told, `oc-plugin` only warns.
- **`modes`:** `soc` (Klipper on the printer itself) and/or `sbc` (Klipper on a Raspberry Pi or other SBC,
  including a printer tunneled to one).

Leave `requires` out if the plugin runs everywhere.

To try a plugin while writing it, install the folder straight from disk: `oc-plugin install /usr/data/my-plugin`.
That's a copy, so install it again after each change, and it isn't offered in the update manager.

## How it works

Plugins are kept in `/usr/data/opencreator/plugins/<id>` (a git checkout). Nothing from a plugin is copied into
Kalico or Moonraker. `oc-plugin sync` links it in:

| Plugin part | Where it goes |
| --- | --- |
| `klippy_extras` | Symlinked into Kalico's `klippy/plugins/` (Klipper without that folder: `klippy/extras/`) |
| `klipper_config` | Symlinked as `plugins/<id>.cfg` next to `printer.cfg`, which includes `[include plugins/*.cfg]` |
| `moonraker_components` | Symlinked into Moonraker's `moonraker/components/` |
| `moonraker_config` | Copied, with the plugin's `[update_manager]` section, to `plugins/moonraker/<id>.conf`, included by `moonraker.conf` with `[include plugins/moonraker/*.conf]` |

Kalico's `klippy/plugins/` is git-ignored, so Kalico updates leave plugin links alone. Kalico also refuses to start
if a module there has the name of one of its own.

`oc-plugin` only removes or replaces links that point into the plugins folder. A file it didn't make is never
touched. If something is in its way, it reports that and the plugin isn't installed. The two include lines
are added to `printer.cfg` and `moonraker.conf` once, if they're missing. In `printer.cfg` the line goes
before the `SAVE_CONFIG` block. `plugins/moonraker/00-oc-plugins.conf` only holds a comment. It's there
because Moonraker, unlike Klipper, refuses an include that matches no files.

On the printer, `10-klipper.sh` runs `oc-plugin sync --only klipper` before every Klippy start, and `20-msmr.sh` runs
`oc-plugin sync --only moonraker` before every Moonraker start. This is why an update from Fluidd, which only does a
`git pull` and restarts, still links any file the new version added. `oc-plugin` itself lives in
`/usr/data/opencreator/plugin-manager/`, which the installer and the OpenCreator updater keep up to date.
`10-klipper.sh` links it into `/opt/bin`.

### Tunneled printers

A tunneled printer counts as an SBC install, so it takes plugins made for `sbc`. `oc-plugin` runs on the
printer as usual, and `tunneled.sh` starts the plugins' startup items at boot. Klipper and Moonraker are on the
SBC, so a plugin's Klipper and Moonraker parts aren't linked on the printer, and nothing is restarted. Startup
items and scripts get `OC_TUNNELED=1`.

Plugins that talk to Moonraker get the SBC's as `OC_MOONRAKER_URL`. `oc-plugin moonraker --find` finds it: it
keeps the saved address if it still answers, and otherwise tries port 7125 on every address in the printer's
own networks (neighbours it already knows first). It only accepts an OpenCreator tunnel SBC, one whose
Moonraker has the `opencreator-updater` update entry or whose Klipper's MCUs are on `/dev/ttyGS*`. With several
tunneled printers on one network it lists them all with their hostnames. Over SSH it asks which one is this
printer's; run by a plugin it stops and says so. Pick one with `oc-plugin moonraker --scan`, or set it by hand
with `oc-plugin moonraker http://<sbc>:7125`. The choice is saved in `plugins/.moonraker-url`.

### Running oc-plugin on the SBC

`oc-plugin` also works on a Raspberry Pi running Klipper. There, the default folders are
`~/opencreator-plugins`, `~/klipper`, `~/printer_data/config` and `~/moonraker`. Nothing runs `sync` at each
start there, so run `oc-plugin sync` after updating a plugin from the update manager. To use other folders,
set `OC_PLUGINS_DIR`, `OC_KLIPPER_DIR`, `OC_CONFIG_DIR` and `OC_MOONRAKER_DIR`, plus `OC_MOONRAKER_URL` for the
restarts.
