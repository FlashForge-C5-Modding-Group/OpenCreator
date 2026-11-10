# The difference with OpenCreator
There's a plethora of mods now for the Creator 5, what makes us different?

It's not special that there's a couple mods apart from us now.
Even though we were first on the scene with our stock mods, doesn't mean we stopped innovating when others came out with their solutions.

## Features
We're the first to fully reverse all MCU code and thus, can improve
certain features with it such as allowing for double stepping on the 
extruder motor, having completely addressable TMC2209s instead of hardcoding.
Due to it no longer being hardcoded, you can now adjust motor current on the fly
for the extruder. 

This also makes our firmware completely incompatible with stock and other mods
that piggy back off of the stock firmware which is all at the time of writing.

We're also currently the only custom firmware that has this.

We also actively take in suggestions.

We also have Power Loss Recovery, which **isn't a given** as most CFWs made do not have fully implemented PLR.
On power loss, it will prompt you to restart your print, and will keep the bed warmed, so even if you aren't there, your print can be saved.

## Up to 2x faster toolchanges
A stock toolchange usually takes about 6 seconds. On OpenCreator CFW one can take as little as 3 seconds,
so multi-color and multi-material prints spend far less time swapping tools.

<video class="oc-video" controls preload="metadata" playsinline>
  <source src="https://opencreatorcfw.github.io/assets/videos/toolchange-speed.mp4" type="video/mp4">
  <a href="https://opencreatorcfw.github.io/assets/videos/toolchange-speed.mp4">Watch the toolchange video</a>
</video>

## Better timelapses
OpenCreator CFW takes a camera frame at every layer change and turns them into
a smooth MP4 when the print finishes, using
[Moonraker Timelapse](printer-features.md#timelapse). The videos are under
**Timelapse** in Fluidd and Mainsail, where you can also change the frame rate
or park the toolhead for each frame.

<video class="oc-video" controls preload="metadata" playsinline muted>
  <source src="https://opencreatorcfw.github.io/assets/videos/timelapse.mp4" type="video/mp4">
  <a href="https://opencreatorcfw.github.io/assets/videos/timelapse.mp4">Watch the timelapse</a>
</video>

## Fixes stock firmware bugs
FlashForge's firmware has a bug where switching extruders whilst one is unloading
can make the extruder run backwards. OpenCreator CFW fixes this.

## Klipper 
We're fully Kalico, a fork of Klipper with more features. No FlashForge, no old Klipper.

## Different hosts
You're not forced to cope with a bad SoC with the printer, just
plug in a new host like a Raspberry Pi, and get to printing!

## Support over Discord
Unlike others, we have a centralized Discord where we take feedback and support people. Check it out on the landing page.

## Support for stock installs
If you really don't want to break too much of your warranty, you can stick with OpenCreator Legacy, where we only modify a little bit, and keep things mostly stock, FlashForge services and all.

## Maintainable Source
Unlike some other mods, our source code is easily readable and is reasonably split up on the repository, making it very easy to understand what we do.

## Mainstream UIs
Most of our UIs and backends such as Mainsail and Fluidd, along with Moonraker are mainstream, not forked, and only come with a CSS theme, update as soon as updates come out.

## Up to 3x* better performance
With our custom compiled special ingredients that we sprinkle into our custom firmware, you are less likely to ever run into a Timer Too Close, or Motor Too Far In The Past, and can run a print without a purge tower without the printer crashing.

(* - 3x best case scenario, most of the time from 1.5 to 2x from completely stock 1.9.9)

## No slowed-down short moves
Since 1.9.9, stock firmware avoids Timer Too Close by slowing down short moves,
whether your print needed it or not:

- Every move of 0.75 mm or less is slowed so it takes at least 2.5 ms. A 0.2 mm
  move can go no faster than 80 mm/s, however fast the slicer asked for.
- Any short move that extrudes (0.75 mm or less across) is capped at 80 mm/s.
- On top of that, the motion planner limits acceleration on them. In 1.9.9,
  always: short moves to 5000 mm/s², and short extruding moves to 80 mm/s at
  1500 mm/s². In 2.0.0, moves of 0.3 mm or less are limited to 10000 mm/s², and
  extruding moves of 0.5 mm or less to 120 mm/s at 5000 mm/s². That applies when
  moves come in faster than 80 a second (fully from 200), and for the first 100
  moves of a print.

Curves, arcs, small details and fine infill are made of exactly these short
moves, so stock prints them slower than the slicer planned for.

OpenCreator CFW has none of these limits: every move runs at the speed in your
G-code. We keep up with the moves instead of slowing them down (see
[above](#up-to-3x-better-performance)).

## Full installer
Instead of picking afterwards, or not being able to pick at all, our installer environment will allow you to pick what you like, before it even installs,
and if your printer breaks but can still access USB drives, you can use our installer to fix any problems, over SSH.

## Better VFA compensation
FlashForge keeps a lot of their special sauce for their VFA calibration disabled. We fix up their disabled options and make it better.

Read the wiki page on [VFA Compensation](vfa-compensation)