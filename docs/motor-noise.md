---
description: How OpenCreator makes the Creator 5's X and Y motors quieter, step interpolation and ripple correction, with measurements.
---

# Quieter motors

!!! note "Testing"
    This is in testing on the Kalico `phase-stepping` branch and needs its
    mainboard firmware. It isn't in a release yet.

The Creator 5's mainboard drives the X, Y and Z motors itself, through
FlashForge's MCLib motor control (see
[VFA compensation](vfa-compensation.md)). OpenCreator changes two things in
it to make the motors quieter, mostly at printing speeds up to about
100 mm/s:

- **Step interpolation** follows the real move between steps.
- **Ripple correction** cancels the motors' own wobble, calibrated per motor
  with the toolhead accelerometer.

## Step interpolation

Klipper sends the mainboard a step for each 1/16 of a full step. Between
steps the mainboard moves the motor smoothly on to the next one, about
20,000 times a second. Stock guesses when the next step comes by assuming it
takes as long as the last one. That is right at a steady speed but wrong
whenever the speed changes, so the motor runs ahead or falls behind and then
snaps back on the next step. Speed changes are constant in a print:
acceleration, input shaping and pressure advance all change it.

On OpenCreator the mainboard's step queue already knows exactly when the next
step is due, so the motor is moved on at the real rate. After the last step
of a move it stays on that step, instead of running nearly a step past it
like stock does.

Nothing changes on the Klipper side: homing, endstops and positions work as
before.

## Ripple correction

A stepper motor doesn't turn perfectly evenly. Its detent torque and the
shape of its current give it a small wobble at fixed multiples of its
electrical cycle (four full steps). On the Creator 5 the strong ones are 2,
4, 6 and 8 times per cycle; 4 is once per full step. The wobble shakes the
toolhead at those frequencies, so it moves with speed: at 10 mm/s the
once-per-step wobble is at 49 Hz, at 20 mm/s at 99 Hz. That is most of the
hum you hear at printing speeds.

Ripple correction adds a small opposite current to each motor at those
multiples, timed to the motor's position. The right current depends on the
speed: for the once-per-step wobble it shrinks as the speed nears the motor's
resonance, flips over, and grows again above it. So each multiple has a table
of corrections at five speeds, and the mainboard picks the value for the
current speed from the step rate. Just above the fastest and below the
slowest calibrated speed the correction fades out, as nothing was measured
there.

It is calibrated per motor, and per direction of travel, with the toolhead
accelerometer.

### Calibrating

Dock all the toolheads, then send:

```
RIPPLE_CALIBRATE
```

It homes, picks up the first tool and parks in the middle. Then, for each
motor, it moves along the diagonal that turns only that motor and measures the
vibration at each multiple. It takes about 10 minutes a motor and ends with
`SAVE_CONFIG`. Add `AXIS=X` or `AXIS=Y` to do one motor, or `SAVE=0` to leave
saving for later.

Each speed it tries prints the vibration before and after:

```
stepper_x harmonic 4 at 297 Hz (60 mm/s motor): forward 565 -> 33, reverse 567 -> 13 mm/s^2
```

Run it again after changing motors or belts.

### How it finds the correction

At one speed, the vibration's strength at a multiple is a simple curve
(a quadratic) in the correction's size and timing. Measuring it with no
change and with three small test corrections fixes that curve, and the best
correction is at its bottom. One more move checks it, and it is only kept if
the vibration went down. It does this twice, the second time with smaller
test corrections. Both directions are measured on the same out-and-back move.

`MCLIB_RIPPLE_MEASURE STEPPER=stepper_x SPEED=30` shows the vibration at each
multiple at one speed, and `MCLIB_SET_RIPPLE STEPPER=stepper_x` shows the
saved tables.

### With VFA compensation

[VFA compensation](vfa-compensation.md) also corrects the 1, 2 and 4 times
ripples, but tuned near the frame's resonance, so mostly at higher speeds.
The mainboard hands over by speed: where a ripple table covers the motor's
current speed, it replaces the VFA value for that ripple; outside the table
VFA takes over again, blended in between. Keep both calibrated.

### Fans during calibration

Calibration holds the mainboard and power supply fans at 50% and waits for
them to settle before measuring, like input shaper and VFA calibration do.
Otherwise they speed up as the mainboard warms during the test, and their
changing vibration throws the measurements off.

## Measured

Creator 5 Pro, toolhead accelerometer, board fans held at 50% throughout. The
new firmware (step interpolation plus calibrated ripple correction) is the
average of three sessions, against the firmware before these changes. Numbers
are how much less the toolhead shakes, in three frequency bands:
10–100 / 100–300 / 300–800 Hz. "Diagonal" moves one motor only.

| Move | Change |
|---|---|
| Y 5 mm/s | −24% / −39% / −58% |
| Y 10 mm/s | −37% / −38% / −52% |
| Y 20 mm/s | −42% / −54% / −45% |
| Y 80 mm/s | −4% / −30% / +1% |
| X 5 mm/s | −40% / −15% / −17% |
| X 10 mm/s | −34% / +18% / −4% |
| X 20 mm/s | −23% / −42% / −36% |
| X 40 mm/s | +2% / −27% / −4% |
| X 80 mm/s | −12% / −22% / −9% |
| X 150 mm/s | −11% / −6% / −4% |
| Diagonal 5 mm/s | −34% / −29% / −16% |
| Diagonal 20 mm/s | −32% / −62% / −14% |
| Diagonal 80 mm/s | +2% / −14% / −13% |
| Slow speed changes | −2 to −17% / −6 to −10% / −5 to −14% |

Repeating the same test 15 minutes apart moves single numbers by up to about
±30%, so read small changes as no change. At the speed each point was
calibrated at, the wobble it targets drops by 75 to 95%. The rest of the
vibration is the frame and belts, which this doesn't touch; input shaping
handles that.

## Why not full phase stepping

Prusa's phase stepping has the printer's computer send the motor's exact path
instead of steps. Step interpolation already follows the path to within a
fraction of a 1/16 step, and the measurements show the remaining noise is the
motors' wobble, not step size. So OpenCreator does the part that removes it,
the ripple correction, without changing how Klipper talks to the mainboard.
