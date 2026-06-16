# Inverse Kinematics Robotic Arm

> A 4-DOF desktop robotic arm with a redesigned base and gripper, built around MG996R and MG90 servos and an Arduino Uno

![Demo](media/demo.gif)
<!-- Put your demo GIF here. This is the most important thing in the whole README.
     Record a short clip of the arm moving, convert to GIF (ezgif.com or `ffmpeg`),
     drop it in /media. If you only do one thing, do this. -->

---

## What this is

This project is a fully custom 3D printed 4-DOF robotic arm with a gripper. The arm is powered by 4 MG996R servos, 1 MG90 Micro Servo, and an Arduino Uno. All parts are original and individually modeled. 

## What I changed (and why)

This is the most important section for anyone evaluating the project. List each modification,
the reason, and the result. Use before/after images where you can.

| Original part | My change | Why | Result |
|---|---|---|---|
| e.g. stock base bracket | Redesigned for MG996R clearance | Original fouled the servo horn at full rotation | +30° range of motion |
| | | | |
| | | | |

<!-- Before/after image example:
| ![before](media/base-before.png) | ![after](media/base-after.png) |
-->

## Results / specs

Fill in real numbers — even rough measurements beat none.

- **Degrees of freedom:** 
- **Reach:** ___ mm
- **Payload at full extension:** ___ g
- **Repeatability:** ±___ mm
- **Cycle time:** ___ s

## Bill of materials

See [`docs/bom.csv`](docs/bom.csv) for the full list. Key components:

- Servos: 
- Controller: 
- Power supply: 
- Printed parts: PETG/ASA (list)

## Build

1. Print the parts in `cad/` (settings: ___ infill, ___ walls, material ___).
2. Wiring — see [`docs/wiring.png`](docs/wiring.png).
3. Flash the firmware in `firmware/` (board: ___, libraries: ___).
4. Calibrate: 

## What broke / what I'd do differently

Short, honest section. List a failure and how you solved it, plus one or two things you'd
change with more time. Interviewers love this — it shows engineering judgment, not just assembly.

- 
- 

## Credits

Based on [original tutorial/design](link) by [author]. Modifications and documentation by [your name].

## License

MIT — see [LICENSE](LICENSE).
