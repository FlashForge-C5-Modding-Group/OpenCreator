# SBC Installation and Configuration
SBC / RPI Klipper for replacing the Mips32 Host that comes with the printer

## Preamble:
### Reasons to go OpenCreator SBC over stock
1. More power to run fun projects on, add more cameras, add monitoring with AI, etc.
2. Security, airgapping the printer's old kernel version and potentially insecure packages, to only be run through the SBC.
3. Use different touchscreen apps, such as KlipperScreen instead of GrumpyScreen.
4. Run Klipper closer to main repos, without logging changes, or major changes to get performance, this also applies to AFC.
5. Use KIUAH to install utilities instead of manually.

### What is the reason for development?
On the stock system, you have amazing MCUs, but the worst SOCs, to the point of encountering issues.
That wouldn't be normally an issue, manufacturers would put the minimum spec SOC, but FlashForge,
demanded too much, for how slow, and not powerful the stock SOC is (Ingenic x2600), it frequently
encounters issues and runs into weirdness, such as on completely stock, even with the stock touchscreen,
you can't really print too fast, with toolchanges, and no prime tower, as it will Timer Too Close, which
is a issue when the host (the SOC) can't catch up and falls behind.

On OpenCreator SOC, Timer Too Close becomes a thing of the past, and you can run almost anything, as you're
running on a Raspberry Pi or similar microcomputer, that has several times the power of the stock board.

## Requirements:
- A SBC that has USB Gadget Mode support (PiZ2w, Pi4/5 on its USB-C power port, etc)
- If using an external power supply on a Pi4/5, a USB power and data splitter
- If using an external power supply on a Pi4/5, a USB-A power blocker for the USB data in/out port on the splitter
- Running a recent Debian with up-to date packages, forks of Debian like Ubuntu, or Fedora (Recommended headless / server installs)
- At least 2GB of Storage
- At least 512MB of RAM
- Internet on at least the SBC

## Installation:
### On the Printer
1. Plug in the OpenCreator Installer media as outlined on the installation page
2. Click to use "Tunneled"
3. Wait for installation to finish

### On the SBC
*tbd*

## Extra Notes:
The touchscreen might feel physically slow no matter the device, due to VNC, unless you use GrumpyScreen or similar on the device.
This could be fixed by using a touchscreen out, from the SBC, such as something [like this](https://www.aliexpress.com/item/1005007273964563.html?spm=a2g0o.productlist.main.2.c9cd551fSWesaK&algo_pvid=d8ed057c-13b2-4553-a58b-d601e81d3247&algo_exp_id=d8ed057c-13b2-4553-a58b-d601e81d3247-1&pdp_ext_f=%7B%22order%22%3A%22360%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%21100.91%2192.60%21%21%21468.26%21429.70%21%402101ca9517905632722322961e1293%2112000040028480724%21sea%21CA%210%21ABX%211%210%21n_tag%3A-29910%3Bd%3Acde07206%3Bm03_new_user%3A-29895%3BpisId%3A5000000210902380&curPageLogUid=NCNdazYF7ey6&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005007273964563%7C_p_origin_prod%3A).
