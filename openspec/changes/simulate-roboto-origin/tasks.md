## 1. Project and source foundation

- [ ] 1.1 Add the solid-node manifest, builder profile, simulation package skeleton, and generated-artifact ignores with a root that can build its first visible subassembly.
- [ ] 1.2 Add a repeatable source-mesh probe, run it across all 24 maintained URDF visuals, and record bounds, scale, watertightness, component counts, and import limitations in `docs/measurements.md`.
- [ ] 1.3 Add source-drift contracts first and confirm they fail before transcribing the complete URDF link, joint, material, and standing-pose data into simulation constants.

## 2. Feet and legs

- [ ] 2.1 Add red contracts for ankle link scale, bilateral ankle placement, joint axes, and floor position.
- [ ] 2.2 Import the ankle visuals, declare ankle pitch/roll joints, assemble `Feet`, and make the ankle contracts green.
- [ ] 2.3 Add red contracts for knee and hip hierarchy, origins, joint limits, and left/right correspondence.
- [ ] 2.4 Import and assemble both complete leg chains from the feet upward, keep the visible root build green, and make the leg contracts green.
- [ ] 2.5 Mutate one transcribed ankle origin and one knee-axis use in node code, confirm the named contracts fail, restore them, and record the mutation evidence.

## 3. Pelvis, torso, and arms

- [ ] 3.1 Add red contracts for base height, torso joint placement, shoulder/elbow hierarchy, and link material colours.
- [ ] 3.2 Import the base and torso visuals, assemble the waist, and make the torso contracts green.
- [ ] 3.3 Import and assemble both five-joint arm chains, make the arm contracts green, and verify the complete 24-link tree.
- [ ] 3.4 Mutate one shoulder origin and one visual colour mapping, confirm the specific contracts fail, restore them, and record the mutation evidence.

## 4. Controls and demonstration

- [ ] 4.1 Add red contracts for the seven-driver control surface, all 23 driven joint coordinates, source standing pose, six instruction targets, and joint-range boundaries.
- [ ] 4.2 Add the intent relations and `Rest`, `Stand`, `Crouch`, `Turn`, `Present`, and `Wave` instructions, then make the control and endpoint contracts green.
- [ ] 4.3 Add the bounded demonstration scenario with endpoint assertions and interference sampling; prove its failing case before making the passing sequence green.
- [ ] 4.4 Mutate one bilateral sign and one instruction target in node code, confirm the symmetry and endpoint contracts fail, restore them, and record the mutation evidence.

## 5. Integration and closeout

- [ ] 5.1 Run the faceted regression for every simulation node file, verify `solid build`, inspect the complete `viewer.json` tree, operations, models, drivers, and instructions, and resolve or explicitly stop on structural contract failures.
- [ ] 5.2 Render and inspect an isometric standing snapshot plus front and side articulated snapshots, recording visual findings without committing generated images.
- [ ] 5.3 Document exact build/test commands, controls, instructions, ground-up model sequence, source findings, visualization-only safety scope, and framework findings in the README and change design.
- [ ] 5.4 Run the exact regression for every simulation node file, validate the completed change, sync accepted specs, archive it, inspect repository hygiene, and commit the delivery without pushing.
