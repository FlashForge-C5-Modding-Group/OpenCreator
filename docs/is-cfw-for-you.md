# Is OpenCreator CFW for you?

OpenCreator CFW replaces FlashForge's software on the Creator 5 and Creator 5 Pro with Kalico, Moonraker
and a web UI. It's in public alpha, so it suits some people more than others.

## You'll like CFW if you

<div class="grid cards" markdown>

-   :material-timer-sand-complete: **Want faster toolchanges**

    ---

    Up to 2x faster: about 6 seconds on stock, as little as 3 seconds on CFW.

-   :material-web: **Want a full web UI**

    ---

    Fluidd or Mainsail with Moonraker, straight from the mainstream projects. You don't need the
    touchscreen at all.

-   :material-tune-variant: **Like to tune and tinker**

    ---

    Kalico with MPC, corrected PID, input shaper graphs on the printer, per-tool Z offsets, toggles for purge, flow
    calibration, bed soak and more. See [CFW Features](cfwfeatures.md).

-   :material-printer-3d: **Use OrcaSlicer**

    ---

    Print over the Moonraker agent, with material and colour synced from every tool like an AMS.

-   :material-shield-off: **Want FlashForge's services gone**

    ---

    You can disable them completely, cloud included.

-   :material-raspberry-pi: **Want to run a Raspberry Pi as host**

    ---

    Move Klipper onto an SBC for more power and cameras. See [OpenCreator for SBC](rpi-klipper.md).

-   :material-quality-high: **Want better prints and camera quality**

    ---

    With features like better [VFA compensation](vfa-compensation.md) you can get cleaner prints, and with camera tweaks, you can have the best looking camera.

-   :material-fan-off: **Want a quieter printer**

    ---

    We adaptively scale fan noise to what your temperature actually is, making it quiet when it's cold, and loud when its hot.

-   :material-alert-circle-check: **Want AI detection**

    ---

    With our special hand-crafted model in collaboration with [Printer Buddy](https://github.com/printer-buddy/printer-buddy), we give you [Printer Buddy Lite](https://github.com/FlashForge-C5-Modding-Group/printer-buddy-lite)
    The best available local AI vision model for your Creator 5, running as a plugin to addon, with zero performance overhead.

-   :material-shopping: **Want an ever growing plugin list**

    ---

    [With a small command or two, you can download addons instantly compatible and available for your printer.](plugins.md)

-   :material-rocket-launch-outline: **Want what's coming next**

    ---

    More is on the way, including more accurate pressure advance tuned to your filament, and stall
    and skipped step detection (StallGuard-like) on X, Y and Z, to even better VFA testing and compensation.

</div>

## CFW may not be for you yet if you

- **Rely on the stock touchscreen.** GrumpyScreen replaces it fully, new UI.
- **Use FlashForge's app, cloud or FlashStudio.** These go away once FlashForge's services are gone.
- **Care about your warranty.** Installing mods may void it. You can clean up any traces but they still may figure it out.
- **Want official support.** You can't ask FlashForge for software support if you're on custom firmware.
- **Have zero prior experience with 3D printing.** You're expected to have at least a basic understanding of Klipper. We can help, but we aren't perfect.
- **Your main language is anything other than English.** Currently, multi-lingual support is basic and not automatic. In development!

## Not sure?

[OpenCreator Legacy](opencreator-legacy.md) keeps the mostly stock FlashForge experience, with creature comforts
like better camera views. If CFW sounds right, head to [Getting started](getting-started.md), and ask in the
[Discord](https://discord.gg/RsdkqDtfww) if you have questions.
