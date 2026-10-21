# Plugins

OpenCreator CFW can install plugins: add-ons that bring their own Klipper modules, macros and Moonraker
components without changing OpenCreator's own files. A plugin is a git repository with an
`opencreator-plugin.json` manifest. The [example plugin](https://github.com/FlashForge-C5-Modding-Group/OpenCreator-Plugin-Example)
shows each part and is the place to start when making one.

## Using plugins

Plugins are managed with `oc-plugin` over SSH:

| Command | What it does |
| --- | --- |
| `oc-plugin install <git URL>` | Download and enable a plugin. `--ref <branch or tag>` picks a version. |
| `oc-plugin list` | Installed plugins, their version, whether they're enabled, and the git branch and commit. |
| `oc-plugin update [id]` | Pull the newest version of one plugin, or all of them. |
| `oc-plugin disable <id>` / `enable <id>` | Turn a plugin off or back on without removing it. |
| `oc-plugin remove <id>` | Uninstall it. |
| `oc-plugin sync` | Redo the links and includes; see [How it works](#how-it-works). |

After a change, `oc-plugin` restarts Klipper and/or Moonraker itself, whichever the plugin needs. It never
does that while the printer is printing or paused: it says so, and the change takes effect at the next restart.
`--no-restart` skips the restart.

Plugins installed from a git URL also show up in Fluidd and Mainsail's update manager as
**oc-plugin-&lt;id&gt;**, so they can be updated from there like OpenCreator itself.

!!! warning "Plugins run with full access"
    A plugin's Klipper and Moonraker code runs inside Klipper and Moonraker, with the same access to the
    printer as they have. Only install plugins you trust.

## Making a plugin

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

### Tunneled and SBC installs

`oc-plugin` also works on a Raspberry Pi running Klipper. There, the default folders are
`~/opencreator-plugins`, `~/klipper`, `~/printer_data/config` and `~/moonraker`. Nothing runs `sync` at each
start there, so run `oc-plugin sync` after updating a plugin from the update manager. To use other folders,
set `OC_PLUGINS_DIR`, `OC_KLIPPER_DIR`, `OC_CONFIG_DIR` and `OC_MOONRAKER_DIR`, plus `OC_MOONRAKER_URL` for the
restarts.
