# Robotic Arm — 4-Axis Articulated Arm (Custom Design)

> A 4-DOF robotic arm designed and built from scratch; base rotation, shoulder, elbow, and wrist, plus a gripper. ~205 mm reach, servo-driven, fully 3D-printed structure. Every joint, the base bearing, and the actuator selection were designed around a worked torque budget rather than copied from a reference build.

![Demo](media/demo.gif)
<!-- Record a short clip of the arm moving, convert to GIF, and drop it in /media as demo.gif.
     This is the single most important asset in the README — motion sells a robotics project. -->

## Overview

This is a custom 4-axis arm built around the actuators I had on hand: four MG996R servos and one SG90 micro servo. Rather than start from a fixed payload/reach target, I worked backward from what those servos could actually hold. The shoulder joint is the binding constraint, so the link lengths were sized to keep it under half of the MG996R's stall torque. The result is a working arm with honest, measured specs instead of optimistic ones.

The base rotation joint was the most involved part of the design: the full weight and tipping moment of the arm is carried by a dedicated bearing surface, not by the servo's output shaft, which is rated for torque only.

## Specs

| Property | Value |
|---|---|
| Degrees of freedom | 4 (base, shoulder, elbow, wrist) + gripper |
| Upper arm (shoulder -> elbow) | 110 mm |
| Forearm (elbow -> wrist) | 95 mm |
| Working reach | ~205 mm to wrist, ~245 mm to grasp point |
| Design payload | 75 g at full extension |
| Shoulder holding torque (worst case) | ~5.2 kg·cm (47% of MG996R stall) |
| Repeatability | ___ mm *(measure and fill in)* |
| Material | PETG |

## Design decisions

The reasoning behind each major choice. This is the core of the project — the decisions matter more than the parts.

| Decision | Why | Result |
|---|---|---|
| 4-DOF all-servo layout (SG90 gripper + 4x MG996R) | Designed around available actuators; shoulder torque fits an MG996R only at reduced reach/payload | Working arm without sourcing steppers |
| Link lengths sized from the shoulder torque budget | Shoulder holding the arm horizontal is the binding load; sized links to stay <50% of stall | ~5.2 kg·cm worst case, leaving thermal margin |
| SG90 at the gripper instead of an MG996R | Saves ~46 g at the far end of the arm, the worst place for weight | Lower shoulder torque for free |
| Rotation load on a bearing surface, not the servo shaft | Servo spline is rated for torque, not the arm's weight + tipping moment | Eliminates base wobble; servo drives rotation only |
| Rim-supported platform + center hold-down pin | Arm's center of mass sits outside the support rim, so an unpinned platform would lift and rock when extended | Rim takes weight, pin resists tipping — stable at full reach, simple to fabricate |
| Slotted horn coupling holes | Bearing should locate the platform; rigidly bolting the horn fights it and side-loads the servo | Horn transmits torque while floating radially — no binding |
| Internal fillets at boss roots (2 mm) and floor-wall corner (3 mm) | Load-bearing junctions are stress concentrators; both face up when printed floor-down | Stronger base with no support-material penalty |

## Actuators & BOM

| Joint | Actuator | Notes |
|---|---|---|
| Base rotation | MG996R | Drives rotation only; load carried by the base bearing |
| Shoulder | MG996R | Binding torque constraint — sets the reach/payload limit |
| Elbow | MG996R | |
| Wrist | MG996R | Single axis (pitch) |
| Gripper | SG90 | Light servo at the far end to minimize shoulder torque |
| Controller | Arduino Uno + PCA9685 | PCA9685 drives all 5 servos over I2C |
| Power | Separate 5–6 V, 3 A+ supply | Common ground with the Arduino; never powered off the board |
| Fasteners | M3 bolts + nuts / heat-set inserts | Nyloc nuts at vibration-loaded joints |

See [`docs/bom.csv`](docs/bom.csv) for the full parts list.

## Base & bearing design

- Base: 98 mm outer diameter, 4 mm walls (~90 mm bore), with an integrated servo cradle and a wire pass-through in the wall.
- The rotating platform sits on the base rim as a plain (sliding) thrust surface, captured by a center hold-down pin that resists the tipping moment.
- *A PTFE/nylon washer ring (or greased, finely-printed rim face) reduces stick-slip at the sliding contact.*
- The servo sits in the base with its spline up; the horn couples to the platform center through slotted holes.

## Print settings

| Part | Infill | Walls | Notes |
|---|---|---|---|
| Base | 30% gyroid | 4 | 5 top/bottom layers; solid material under all boss roots |
| *Rotating platform | wagon-wheel ribbed | 4 | Solid rim (race) + solid center hub; ribs to stay flat; print rim-side up* |
| *Arm links | low (~15–20%) | 3 | Hollow/ribbed cross-section — keep upper arm <=40 g, forearm <=32 g to hold the torque budget* |

General rule used throughout: **walls carry load, infill fills space** — perimeters are raised before infill on any part that feels weak.

## Build

1. Print all parts in PETG (settings above).
2. Press heat-set inserts into the base and platform mounting points.
3. Mount the base servo in its cradle, spline up.
4. Assemble the rotating platform onto the base rim with the center hold-down pin; couple the servo horn to the platform via the slotted holes.
5. Build the arm links shoulder -> gripper, fitting each servo before the next.
6. Wire all servos to the PCA9685; power from the separate supply with common ground.
7. Home/zero each servo, then calibrate.

## What broke / what I'd do differently

*(Fill this in as you go — a documented failure and fix is one of the strongest things on the page.)*

- In V1 of the base, the base was way to big, leading to an expensive print, and an unproportionally large base for the rest of the robot. V1 originally designed with the idea of keeping the Arduino Uno inside the base, however for easy of fabrication, V2 was designed to only house the servo, and the Arduino Uno would sit outside the robotic arm.
- 


## License

MIT — see [LICENSE](LICENSE).
