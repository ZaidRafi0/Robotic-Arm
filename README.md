Robotic Arm — 4-Axis Articulated Arm (Custom Design)


A 4-DOF robotic arm designed and built from scratch — base rotation, shoulder, elbow, and wrist, plus a gripper. ~205 mm reach, servo-driven, fully 3D-printed structure. Every joint, the base bearing, and the actuator selection were designed around a worked torque budget rather than copied from a reference build.



Show Image

<!-- Record a short clip of the arm moving, convert to GIF, and drop it in /media as demo.gif.
     This is the single most important asset in the README — motion sells a robotics project. -->
Overview

This is a custom 4-axis arm built around the actuators I had on hand: four MG996R servos and one SG90 micro servo. Rather than start from a fixed payload/reach target, I worked backward from what those servos could actually hold — the shoulder joint is the binding constraint, so the link lengths were sized to keep it under half of the MG996R's stall torque. The result is a working arm with honest, measured specs instead of optimistic ones.

The base rotation joint was the most involved part of the design: the full weight and tipping moment of the arm is carried by a dedicated bearing surface, not by the servo's output shaft, which is rated for torque only.

Specs

PropertyValueDegrees of freedom4 (base, shoulder, elbow, wrist) + gripperUpper arm (shoulder -> elbow)110 mmForearm (elbow -> wrist)95 mmWorking reach~205 mm to wrist, ~245 mm to grasp pointDesign payload75 g at full extensionShoulder holding torque (worst case)~5.2 kg·cm (47% of MG996R stall)Repeatability___ mm (measure and fill in)MaterialPETG

Design decisions

The reasoning behind each major choice. This is the core of the project — the decisions matter more than the parts.

DecisionWhyResult4-DOF all-servo layout (SG90 gripper + 4x MG996R)Designed around available actuators; shoulder torque fits an MG996R only at reduced reach/payloadWorking arm without sourcing steppersLink lengths sized from the shoulder torque budgetShoulder holding the arm horizontal is the binding load; sized links to stay <50% of stall~5.2 kg·cm worst case, leaving thermal marginSG90 at the gripper instead of an MG996RSaves ~46 g at the far end of the arm, the worst place for weightLower shoulder torque for freeRotation load on a bearing surface, not the servo shaftServo spline is rated for torque, not the arm's weight + tipping momentEliminates base wobble; servo drives rotation onlyRim-supported platform + center hold-down pinArm's center of mass sits outside the support rim, so an unpinned platform would lift and rock when extendedRim takes weight, pin resists tipping — stable at full reach, simple to fabricateSlotted horn coupling holesBearing should locate the platform; rigidly bolting the horn fights it and side-loads the servoHorn transmits torque while floating radially — no bindingInternal fillets at boss roots (2 mm) and floor-wall corner (3 mm)Load-bearing junctions are stress concentrators; both face up when printed floor-downStronger base with no support-material penalty

Actuators & BOM

JointActuatorNotesBase rotationMG996RDrives rotation only; load carried by the base bearingShoulderMG996RBinding torque constraint — sets the reach/payload limitElbowMG996RWristMG996RSingle axis (pitch)GripperSG90Light servo at the far end to minimize shoulder torqueControllerArduino Uno + PCA9685PCA9685 drives all 5 servos over I2CPowerSeparate 5–6 V, 3 A+ supplyCommon ground with the Arduino; never powered off the boardFastenersM3 bolts + nuts / heat-set insertsNyloc nuts at vibration-loaded joints

See docs/bom.csv for the full parts list.

Base & bearing design


Base: 98 mm outer diameter, 8 mm walls (~82 mm bore), with an integrated servo cradle and a wire pass-through in the wall.
The rotating platform sits on the base rim as a plain (sliding) thrust surface, captured by a center hold-down pin that resists the tipping moment.
A PTFE/nylon washer ring (or greased, finely-printed rim face) reduces stick-slip at the sliding contact.
The servo sits in the base with its spline up; the horn couples to the platform center through slotted holes.


Print settings

PartInfillWallsNotesBase30% gyroid45 top/bottom layers; solid material under all boss rootsRotating platformwagon-wheel ribbed4Solid rim (race) + solid center hub; ribs to stay flat; print rim-side upArm linkslow (~15–20%)3Hollow/ribbed cross-section — keep upper arm <=40 g, forearm <=32 g to hold the torque budget

General rule used throughout: walls carry load, infill fills space — perimeters are raised before infill on any part that feels weak.

Build


Print all parts in PETG (settings above).
Press heat-set inserts into the base and platform mounting points.
Mount the base servo in its cradle, spline up.
Assemble the rotating platform onto the base rim with the center hold-down pin; couple the servo horn to the platform via the slotted holes.
Build the arm links shoulder -> gripper, fitting each servo before the next.
Wire all servos to the PCA9685; power from the separate supply with common ground.
Home/zero each servo, then calibrate.


What broke / what I'd do differently

(Fill this in as you go — a documented failure and fix is one of the strongest things on the page.)






Roadmap


v2: replace base and shoulder servos with NEMA17 + planetary gearboxes and TMC2209 drivers for true repeatability and higher payload; add a 5th DOF (wrist roll); relocate the wrist servo inward via a linkage to cut shoulder torque.


License

MIT — see LICENSE.
