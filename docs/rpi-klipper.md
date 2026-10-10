# SBC Installation and Configuration
SBC / RPI Klipper for replacing the Mips32 Host that comes with the printer

## Preamble:
### Reasons to go OpenCreator SBC over stock
1. More power to run fun projects on, add more cameras, add monitoring with AI, etc.
2. Security, airgapping the printer's old kernel version and potentially insecure packages, to only be run through the SBC.
3. Use different touchscreen apps, such as KlipperScreen instead of GrumpyScreen.
4. Use KIUAH to install utilities instead of manually, and have all the support of having a good architecture.
5. Run ShakeTune natively

### What is the reason for development?
On the stock system, you have amazing MCUs, but the worst SOCs, to the point of encountering issues.
That wouldn't be normally an issue, manufacturers would put the minimum spec SOC, but FlashForge,
demanded too much, for how slow, and not powerful the stock SOC is (Ingenic x2600), it frequently
encounters issues and runs into weirdness, such as on completely stock, even with the stock touchscreen,
you can't really print too fast, with toolchanges, and no prime tower, as it will Timer Too Close, which
is a issue when the host (the SOC) can't catch up and falls behind. On 1.9.9 they even forced it to slow
down to prevent this.

On OpenCreator SOC, Timer Too Close becomes a thing of the past, and you can run almost anything, as you're
running on a Raspberry Pi or similar microcomputer, that has several times the power of the stock board.

## Requirements:
- A SBC that has USB Gadget Mode support (PiZ2w, Pi4/5 on its USB-C power port, etc)
- If using an external power supply on a Pi4/5, a USB power and data splitter
- If using an external power supply on a Pi4/5, a USB-A power blocker for the USB data in/out port on the splitter
- Running a recent Debian / Raspberry Pi OS Lite with up-to date packages (Recommended headless / server installs)
- If running a device that is slow (1ghz piz2w) to overclock to 1.2ghz or above
- Some type of cooling
- At least 4GB of Storage
- At least 512MB of RAM
- Internet on at least the SBC

## Caviats
- The camera will not work if you airgap the MCUs. You will have to make a custom connector or add your own camera. (In testing to rectify)
- The camera may slow down during fast movements due to the USB bus being overloaded, if that persists you may cause it to need to be restarted.
- A USB to spec cable is recommended, and as short as possible to keep signal integrity. Even if a cable looks good, that doesn't mean it actually is.
- Unplugging / Restarting the Pi causes the FlashForge Printer's SoC to disconnect from WiFi. It selects the virtual interface as internet and dies when it loses it.
- Obviously, no FlashForge integration at all.
- The USB gadget tunnel normally runs 5 ports (4 MCUs + a dedicated beep channel). Not every board's USB controller tolerates this under sustained traffic -- confirmed stable on a Raspberry Pi 4 (dwc2), but an Orange Pi Zero 2W's own controller destabilized the whole bus, stable at 4. The gadget script auto-falls-back to 4 ports if the 5-port bind fails outright at boot and pins that choice for future boots; a failure that only shows up at runtime isn't detectable that way and still needs a manual pin (`echo 4 | sudo tee /etc/c5-tunnel-ports`). Pinned to 4 ports, C5_BUZZER can optionally fall back to sending the beep over the network instead of just going silent -- see `network_host` in `[creator5_remote_beeper]`.

## Installation:
### On the Printer
1. Plug in the OpenCreator Installer media as outlined on the installation page
2. Click to use "Tunneled"
3. Wait for installation to finish
4. Restart your printer
5. You should see GrumpyScreen open up for a Moonraker printer, go to /usr/data/grumpyscreen/ and edit the config file to your Moonraker instance on the Pi

### On the SBC
1. Plug in the SBC through the Gadget Mode port (USB C on RPI 4/5, Power Micro USB for 3B+ and Z2W.)
2. Setup Klipper, Moonraker, and Fluidd/Mainsail (Fluidd Recommended)
3. Clone (repo)
4. Run the script in the Pi folder to install the gadget service if on RPI3B+, 4, 5 or Zero2W, if using another SBC, setup a gadget tunnel based on the service.
5. Add the configs from the Pi folder, if you are using Kalico or not too
6. Restart the firmware for Klipper, you should have it connect if you did Printer first.

## Extra Notes:
The touchscreen might feel physically slow no matter the device unless you use GrumpyScreen or similar running locally on the printer itself. Streaming KlipperScreen to the printer's own screen over VNC (instead of running GrumpyScreen) was attempted but is not currently working: a second, isolated headless KlipperScreen instance (matched to the printer panel's resolution, so the VNC feed needs no scaling) hangs indefinitely during its own window setup under cage's headless Wayland backend, for reasons not yet root-caused. GrumpyScreen remains the supported local touchscreen option.

Config files (and Klipper itself) can be kept in sync with upstream without manually re-applying your own customizations each time -- see OpenCreator Updater in [Status and roadmap](status-and-roadmap.md).
This could be fixed by using a touchscreen out, from the SBC, such as something [like this](https://www.aliexpress.com/item/1005007273964563.html?spm=a2g0o.productlist.main.2.c9cd551fSWesaK&algo_pvid=d8ed057c-13b2-4553-a58b-d601e81d3247&algo_exp_id=d8ed057c-13b2-4553-a58b-d601e81d3247-1&pdp_ext_f=%7B%22order%22%3A%22360%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%21100.91%2192.60%21%21%21468.26%21429.70%21%402101ca9517905632722322961e1293%2112000040028480724%21sea%21CA%210%21ABX%211%210%21n_tag%3A-29910%3Bd%3Acde07206%3Bm03_new_user%3A-29895%3BpisId%3A5000000210902380&curPageLogUid=NCNdazYF7ey6&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005007273964563%7C_p_origin_prod%3A).
