# Installation and safety

OpenCreator CFW isn't ready to install yet. The code is still in development
and hasn't been released.

If you want to use the printer today, use the
[Legacy path](opencreator-legacy.md) and its own instructions instead.

CFW will be installed with the [OpenCreator Installer](installer.md), a
touchscreen app that backs up the printer before changing anything. It is in
testing and not released yet.

## Requirements for USB

- [x] FAT32 Partition
- [x] At least 4GB in size (For nowifi install)
- [x] USB is recommended to have at least 16GB for the backup step

## Steps to install OpenCreator

1. Load the USB with the install environment by loading the files on a USB drive
2. Reboot the printer with the USB plugged in
3. Go through the steps, SoC installs are recommended for people who don't know what tunneled is
4. Wait about 25-30 minutes for things to install
5. After it is finished, it will prompt you to remove the USB drive and press the reboot button
6. Once you reboot, you should see a new boot logo, and Grumpyscreen, and on the web under the printer IP, Fluidd.

## Steps to update OpenCreator
There are two ways.

### Way 1: Fluidd/Mainsail
Fluidd and Mainsail have a way for smaller updates that don't effect most packages on the printer.

1. Navigate to the software update section
2. Update "OpenCreator" then "Klipper"
3. Reboot via switch or host restart

### Way 2: OpenCreator Installer Update
Sometimes an update can't be distributed like that, this is the other way for more major updates.

1. Load the update by copying files overtop the existing files
2. Plug in the USB drive and reboot
3. Update with backups or without backups (Backups will make it last significantly longer and are not required for updates)
4. Restart & when it prompts you


