---
description: How VFA (vertical fine artifact) calibration works on the stock Creator 5 firmware and on OpenCreator.
---

# VFA compensation

VFA (vertical fine artifacts) are fine, evenly spaced vertical lines on
printed walls. They come from small ripples in how the X and Y motors turn.
At certain speeds a ripple matches the frame's resonance and shakes the
toolhead enough to show on the print.

The Creator 5's mainboard drives its motors through FlashForge's MCLib motor
library, which can cancel these ripples. It adds a small opposite ripple to
each motor's current. Three ripples are corrected per motor: once, twice and
four times per motor cycle (`td1`, `td2` and `td4`). Each has an amplitude
and two phases, one per direction of travel. Calibration measures those
numbers with the toolhead's accelerometer.

## On the stock firmware

The **VFA** button on the stock touchscreen sends:

1. `G1 Z100`, then `G1 X130 Y130`: the bed down and the head to the middle.
2. `STEPPER_RESONANCE_FACTORY_CALIBRATE`, a macro in the stock
   `printer.vibration.cfg`.

That macro turns all compensation off, then calibrates X and Y with
`STEPPER_RESONANCE_SEARCH_CALIBRATE`. For each ripple and each direction, the
search:

- moves the head back and forth along one motor's axis at the speed where that
  ripple should hit the resonance, worked out from a **fixed 200 Hz**
  (`stepper_x_freq` / `stepper_y_freq`). The macro's step that would measure
  the real frequency is commented out.
- measures the vibration with no compensation, then probes four phases, then
  finer phases, then amplitudes, and settles on the best phase and amplitude.
  Each measurement is about 0.6 s from the middle of one move.
- uses the larger of the two directions' amplitudes, because the motor has one
  amplitude for both.

The results are sent to the motors straight away. The macro's `SAVE_CONFIG` is
commented out, and the stock touchscreen doesn't send one with it, so as far as
we can tell the results last until the next restart. After that the
factory-calibrated values in `printer.cfg` come back.

The stock config also has a `STEPPER_RESONANCE_DAMP_ENABLE` macro, but it sets
fixed numbers rather than calibrated ones: X off, and one hard-coded set for Y.
Anything that ran it would replace a calibration's results with those.

The calibration also doesn't check that its result is better than no
compensation. It leaves the printer's acceleration at the test's 5000 mm/s²,
and the analysis code crashes if the accelerometer gives no usable data five
times in a row.

## On OpenCreator

Run **VFA** from the touchscreen or Fluidd's *Calibrations* group, or send
`VFA_COMPENSATION` in the console. **VFA + Graphs** (`VFA_COMPENSATION_GRAPH`)
does the same and draws graphs too (see below). Both are loud, and take a
while.

1. **Safety checks.** It refuses during a print, and unless all four heads are
   docked.
2. **Homes, and picks up T0.** The head carries the accelerometer, and
   calibrating with a tool on matches the weight it prints with. Then
   `G1 Z100` and `G1 X130 Y130`, as on stock.
3. **Finds each motor's real resonance.** With compensation off, the head
   makes test moves at 20 to 120 mm/s in 5 mm/s steps, then finer steps around
   the strongest. The speed where the main ripple shakes hardest gives that
   motor's frequency, and everything after is aimed at it. A result is used
   only when there's a clear peak, inside the swept speeds and at least 1.5
   times the typical level. Otherwise it keeps the configured frequency (200 Hz
   unless saved) and says why.
4. **Searches each ripple** the same way stock does, with three differences:
    - Each measurement uses the whole steady-speed part of the move, found from
      the move's own timing, so there's less noise in every number it finds.
    - The two directions' amplitudes are combined into the one that gives the
      least vibration over both, not the larger of the two.
    - The result is measured again as it will run and compared with no
      compensation. A ripple that isn't at least 5% better is left off
      (amplitude 0) rather than saved.
5. **Puts things back.** The printer's own acceleration settings are restored.
   If calibration stops with an error, the motors go back to their saved
   values instead of being left half-calibrated.
6. **Saves.** `SAVE_CONFIG` writes the values and the measured frequencies to
   `printer.cfg` and restarts Klipper. With `SAVE=0` nothing is written, but the
   new values are still used until Klipper restarts.

Every print start runs `STEPPER_RESONANCE_DAMP_ENABLE`. On OpenCreator that
restores the calibrated values (`MCLIB_APPLY_CALIBRATION`), not fixed numbers.

A CSV of every measurement is written to
`/tmp/stepper_x_search_calibrate-<time>.csv` (and `stepper_y`), for anyone who
wants the raw numbers.

### Graphs

**VFA + Graphs** runs the same calibration, then repeats the speed sweep with
compensation on. It draws one picture per motor to `config/vfa/`
(`vfa_stepper_x.png`, `vfa_stepper_y.png`), which you can open from Fluidd's or
Mainsail's config files.

- **Top chart:** how hard the toolhead shakes at each print speed, for each of
  the three ripples. Dotted lines are before compensation and solid lines
  after, so lower is better. Peaks are speeds where a ripple matches the
  frame's resonance, and a marker shows the speed each ripple was tuned at.
- **Bottom chart:** each ripple at its worst speed, before and after, with how
  much less it shakes. A ripple that was left off because it didn't help is
  labelled as such.

It takes longer than **VFA**, by one more sweep per motor.

### When to run it

Run it if you see VFA lines on your prints, and again after changing or
re-tensioning belts, since that moves the resonance.

## Side by side

| | Stock | OpenCreator |
|---|---|---|
| Resonance frequency | Fixed 200 Hz | Measured for each motor, 200 Hz if no clear peak |
| Data per measurement | About 0.6 s from the middle of each move | The whole steady-speed part of each move |
| One amplitude for both directions | The larger of the two | The one with the least vibration over both |
| Checked against no compensation | No | Yes: a ripple that isn't 5% better is left off |
| Saved | No (lasts until restart) | Yes, unless `SAVE=0` |
| What prints use | The factory values from `printer.cfg`, once restarted | The calibrated values, restored at every print start |
| Acceleration afterwards | Left at 5000 mm/s² | Restored |
| Accelerometer failure | Crashes | Stops with a clear error, motors on saved values |
| Graphs | No | **VFA + Graphs** |
