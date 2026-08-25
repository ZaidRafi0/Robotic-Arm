# Robotic Arm — 4-Axis Articulated Arm (Custom Design)

> A 4-DOF robotic arm designed and built from scratch — base rotation, shoulder, elbow, and wrist, plus a rack-and-pinion gripper. ~205 mm reach, servo-driven, fully 3D-printed in PLA, controlled with an inverse kinematics solver. Every joint, the base bearing, the gear train, and the actuator selection were designed around a worked torque budget rather than copied from a reference build.

**Work in progress.** See [Status](#status) for what's done and what's left.

![Demo](media/demo.gif)
<!-- Record a short clip of the arm moving, convert to GIF, and drop it in /media as demo.gif. -->

## Overview

A custom 4-axis arm built around the actuators on hand: four MG996R servos and one SG90 micro servo. Rather than starting from a fixed payload/reach target, I worked backward from what those servos could actually hold — the shoulder is the binding constraint, so link lengths were sized to keep it under half of the MG996R's stall torque.

Two joints drove most of the design work. The **base rotation joint** carries the full weight and tipping moment of the arm on a dedicated bearing surface rather than on the servo's output shaft, which is rated for torque only. The **gripper** uses a double rack-and-pinion so both jaws move symmetrically and stay parallel, centering the object rather than pushing it to one side.

## Status

v1 of every part is modeled and in `cad/`.

| Part | Status |
|---|---|
| Base | Complete |
| Shoulder bracket | Complete |
| Upper arm | Complete |
| Forearm | Complete |
| Wrist bracket | Needs revision |
| Gripper housing | Needs revision |
| Pinion | Needs revision |
| Rack | Needs revision |

**Remaining work on v1:**

1. Fix the gripper assembly — wrist bracket, housing, pinion, and racks (see [What broke](#what-broke)).
2. Wire the arm: five servos to a PCA9685, separate 5-6 V supply, common ground.
3. Write and tune the inverse kinematics solver.

## Specs

| Property | Value |
|---|---|
| Degrees of freedom | 4 (base, shoulder, elbow, wrist) + gripper |
| Upper arm (shoulder to elbow) | 110 mm |
| Forearm (elbow to wrist) | 95 mm |
| Working reach | ~205 mm to wrist, ~245 mm to grasp point |
| Design payload | 75 g at full extension |
| Shoulder holding torque (worst case) | ~5.2 kg·cm (47% of MG996R stall) |
| Jaw opening | ~44 mm |
| Gear module / pressure angle | 1.5 / 20 degrees |
| Pinion | 14 teeth, 21 mm pitch dia, 24 mm OD |
| Repeatability | ___ mm *(to measure)* |
| Material | PLA throughout |

## Design decisions

The reasoning behind each major choice. The decisions matter more than the parts.

| Decision | Why | Result |
|---|---|---|
| 4-DOF all-servo layout (SG90 gripper + 4x MG996R) | Designed around available actuators; shoulder torque fits an MG996R only at reduced reach/payload | Working arm without sourcing steppers |
| Link lengths sized from the shoulder torque budget | Shoulder holding the arm horizontal is the binding load; sized links to stay under 50% of stall | ~5.2 kg·cm worst case, leaving thermal margin |
| SG90 at the gripper instead of an MG996R | Saves ~46 g at the far end of the arm, the worst place for weight | Lower shoulder torque for free |
| Rotation load on a bearing surface, not the servo shaft | Servo spline is rated for torque, not the arm's weight + tipping moment | Eliminates base wobble; servo drives rotation only |
| Rim-supported platform + center hold-down pin | Arm's center of mass sits outside the support rim, so an unpinned platform would lift and rock when extended | Rim takes weight, pin resists tipping — stable at full reach |
| Slotted horn coupling holes | The bearing should locate the platform; rigidly bolting the horn fights it and side-loads the servo | Horn transmits torque while floating radially — no binding |
| Border-frame construction on both links | Perimeter material carries the bending; hollow center saves mass out on the lever arm | Links stay within the 40 g / 32 g budget |
| Double rack-and-pinion gripper | Two racks on opposite sides of one pinion move in mirror, so both jaws close at equal rate | Parallel jaws that center the object |
| Module 1.5 gears rather than a finer module | Fine teeth print mushy on FDM and strip under load | Teeth survive printing and grip loads |
| Kept the SG90 gripper actuator instead of switching to a stepper | A NEMA17 adds ~270 g at the arm's longest lever — roughly 8 kg·cm at the shoulder, past the MG996R's limit | Grip force still adequate; shoulder margin preserved |
| Internal fillets at boss roots (2 mm) and floor-wall corner (3 mm) | Load-bearing junctions are stress concentrators; both face up when printed floor-down | Stronger parts with no support-material penalty |

## Actuators & BOM

| Joint | Actuator | Notes |
|---|---|---|
| Base rotation | MG996R | Drives rotation only; load carried by the base bearing |
| Shoulder | MG996R | Binding torque constraint — sets the reach/payload limit |
| Elbow | MG996R | |
| Wrist | MG996R | Single axis (pitch) |
| Gripper | SG90 | Drives the pinion through its horn |
| Controller | Arduino Uno + PCA9685 | PCA9685 drives all 5 servos over I2C |
| Power | Separate 5-6 V, 3 A+ supply | Common ground with the Arduino; never powered off the board |
| Fasteners | M3 bolts, nuts and washers; M2 at the pinion | Nyloc nuts at vibration-loaded joints |

## Gripper

Double rack-and-pinion driven by the SG90.

- Pinion: module 1.5, 14 teeth, 20 degree pressure angle, 6 mm face width, bolted to the SG90 star horn through its outer holes (torque path) with the center screw retaining the horn on the spline.
- Racks: two identical, 9 teeth, ~41 mm toothed length, 6 mm backing bar, 6 mm face width. Mounted on opposite sides of the pinion so they travel in opposite directions.
- Mesh geometry: pinion axis sits 9.0 mm from each rack's tooth-tip plane (pitch radius 10.5 mm minus one module).
- Travel: ~22 mm per rack over 120 degrees of servo rotation, giving ~44 mm total jaw opening.
- Housing captures each rack's backing bar in a channel; a cover plate keeps the racks from lifting out of mesh.

## Control

Five servos driven from a PCA9685 over I2C, commanded by an Arduino Uno. Target end-effector positions are resolved to joint angles by an inverse kinematics solver rather than commanding each joint by hand — the base handles yaw, and the shoulder, elbow, and wrist solve as a planar chain within the plane the base points at.

## Print settings

All PLA.

| Part | Walls | Infill |
|---|---|---|
| Base | 4 | 30% gyroid |
| Rotating platform | 4 | ribbed (solid rim and hub) |
| Shoulder bracket | 4 | 30% gyroid |
| Upper arm | 4 | 15% (target ≤40 g) |
| Forearm | 3 | 15% (target ≤32 g) |
| Wrist bracket | 4 | 25-30% |
| Gripper housing | 4 | 20% |
| Racks, pinion | 4 | 40%+ |
| Fingers | 4 | 40-50% |

Two rules used throughout: **walls carry load, infill fills space** — perimeters go up before infill on anything that feels weak. And **print orientation beats every other setting** — links flat along their length, gear teeth in-plane rather than stacked as layer edges.

## What broke

**Press-fit and sliding clearances came out too tight.** Three symptoms, one root cause — the clearances modeled for these fits didn't leave enough room once printed. The SG90 push-fit pocket in the gripper housing is too tight, the rack channels are too tight for the racks to slide, and the pinion pocket is too tight for the gear to spin freely. Fix is to open up every clearance on the next revision and print a test coupon of any sliding or press fit before committing to the full part.

**The wrist bracket is too short.** Needs more length for the gripper to clear the forearm through the full pitch range.

**Gripper jaws initially couldn't close.** The two racks sit 36.5 mm apart with the pinion between them, so straight fingers extending from each rack travel in separate planes and slide past each other rather than meeting. The fingers need to be cranked inward so both jaw faces reach a common centerline.

## v2 ideas

Brainstorms collected while researching, not committed work:

- **NEMA17 stepper motors** in place of hobby servos, for true repeatability and higher payload.
- **Cycloidal gearboxes** at the major joints — a cycloidal reducer trades motor speed for torque in a single high-ratio stage with very low backlash, which should give smoother motion than servo-grade positioning allows.

## License

MIT — see [LICENSE](LICENSE).
