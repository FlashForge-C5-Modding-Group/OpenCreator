# Games on the Creator 5

The Creator 5 runs on an **Ingenic X2600**, a low-power embedded chip that's very
*slow* by modern standards. That's what makes these ports a cool, real games
running on hardware that was never meant to do much more than print and drive a tiny UI.

These are community-built ports you can install on only OpenCreator CFW.

OpenCreator does what FlashForge don't

!!! warning "Games are not part of printing"
    Games run on the printer's application processor, separate from the motion
    firmware. They don't affect printing, but don't run them *while* a print is
    in progress. The touchscreen will be used as the display and actively shares resources.

## DOOM

The classic 1993 shooter, running on the Creator 5's touchscreen with on-screen
controls (D-pad, action buttons, and the status bar).

![DOOM running on the Creator 5 with on-screen touch controls](../assets/images/doom.jpg){ .oc-shot }

DOOM is driven by the open **WAD** asset files, so you can also load custom
levels and total conversions.

### DOOM WADs

DOOM's game data lives in `.wad` files. The engine is free; you supply a WAD:

- **Registered `DOOM.WAD` / `DOOM2.WAD`** — from a retail copy you own.
- **Community WADs** - free fan-made levels and total conversions (many are
  standalone and need no retail files, includes FreeDOOM).

Drop the WAD onto the printer and point the port at it. Touch controls work the
same as the stock episode.

## Sonic Mania

Sonic Mania, the 2017 & 2018 platformer by SEGA, Christian Whitehead, PagodaWest Games and Headcannon, running at full speed during 2D sections, and almost full speed
during 3D sections to get emeralds. Fully completable from start to finish. Using RSDK 5U.

![Sonic Mania running on the Creator 5](../assets/images/sonic-mania.jpg){ .oc-shot }

## Installing

Games are installed as git repositories. Some may not be available, and need to be asked for.

## Notes

- These are made by OpenCreator, but not encouraged. Expect rough edges.
- Touch input is mapped to on-screen buttons, and some games may support gamepad input or keyboard.
- Performance is limited by the slow embedded processor, and may encounter slowdown.
