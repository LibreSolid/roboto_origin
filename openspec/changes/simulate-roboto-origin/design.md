## Context

ROBOTO_ORIGIN is a snapshot aggregation repository for a DIY humanoid. The current `modules/rpo_description/urdf/rpo.urdf` is the repository's explicit kinematic description: it names 24 links, 23 revolute joints, the parent/child tree, joint origins and axes, limits in radians, visual colours, and one visual STL per link. The training module supplies the author's nominal standing pose at a 0.75 m base height.

The visual STLs are expressed in metres and their link-local origins agree with the URDF. A direct mesh probe found that only the two ankle-pitch files are watertight single bodies; most files are display exports containing open or disconnected shells. They are suitable evidence for the robot's appearance but are not print-ready solids. The simulation must not rewrite anything under `modules/`, and it must not imply that a pose is dynamically balanced or safe to command on physical hardware.

### Coordinates

- The simulation uses the URDF frame: +X forward, +Y left, and +Z up.
- The root `base_link` origin is 750 mm above the virtual floor, matching the training configuration's initial state.
- Every URDF joint origin is multiplied by 1000 and applied as the child's rest translation from its parent link.
- Every joint axis is stated unchanged in the child link's own rest frame because all URDF joint-origin rotations are zero.
- Joint limits and pose values are converted from radians to degrees. Viewer drivers are expressed in human-readable degrees except the normalized `crouch` intent.

## Goals / Non-Goals

**Goals:**

- Publish the complete visible robot as a solid-node assembly without modifying or copying upstream design geometry.
- Preserve the URDF tree and make all 23 joint coordinates reachable from no more than seven intent controls.
- Demonstrate the robot with six named poses and test source fidelity, pose endpoints, joint limits, bilateral placement, floor contact, and articulated motion.
- Keep a repeatable mesh probe and measured findings beside the simulation.

**Non-Goals:**

- Dynamics, torque, contact-force, balance, gait synthesis, inverse kinematics, self-collision planning, or safety certification.
- Physical motor control, ROS 2 integration, firmware output, or actuator calibration.
- Repairing, simplifying, replacing, or claiming printability for upstream mesh files.
- Adding the separate exterior appearance-shell set or reconstructing the SolidWorks assembly.

## Decisions

### The current RPO URDF is the kinematic authority

`modules/rpo_description/urdf/rpo.urdf` is used rather than the older hardware and teleoperation copies because the root README identifies `rpo_description` as the maintained description module. A project test parses the source and compares every transcribed link, parent, child, origin, axis, limit, visual filename, and colour. This is necessary because runtime data reads are outside solid-node's static Python source tracking.

Alternative considered: read the URDF dynamically during every build. That would hide the declarative node tree from static source tracking and move model identity outside the supported public contract.

### Link visuals remain link-local display geometry

Each link is a rigid visual leaf imported from its existing STL and scaled from metres to millimetres. Link assemblies add only hierarchy, rest placement, and a joint declared in the moving body's own frame. The implementation begins by exercising `StlNode`, the supported STL adapter; if its mandatory body selection cannot represent a source link's open multi-shell display export, it exercises an `OpenScadNode` import wrapper that preserves the complete visual rather than selecting one fragment.

Alternative considered: redraw links as boxes or convex hulls. That would conceal the actual design and violate the thin-layer requirement. Committing repaired or converted STL files is also rejected because generated CAD artifacts do not belong in the repository.

### Mesh limitations are evidence, not silently repaired geometry

`simulation/tools/probe.py` records source bounds, scale, watertightness, and connected-component counts in `docs/measurements.md`. The root retains solid-node's connectivity and non-interference contracts. If the framework cannot distinguish a render-only visual surface from a printable solid, implementation stops at that red contract, records a framework finding, and returns the representation choice to the pilot; it does not skip the contract or discard parts to turn it green.

Alternative considered: accept the largest connected component from every STL. That would produce a green but visibly incomplete robot.

### Seven intent controls drive all 23 joints

The control surface is `crouch`, `leg_yaw`, `torso_turn`, `arm_pitch`, `arm_spread`, `elbow_bend`, and `right_wave`. Relations couple bilateral joints, preserve the source standing pose offsets, and hold unexposed ankle-roll and arm-yaw freedoms at zero from an existing intent source. Joint coordinates remain explicit on their owning links, so a later project change can expose independent control without restructuring the assembly.

Alternative considered: one slider per actuator. Twenty-three sliders describe wiring rather than an understandable humanoid pose.

### Demonstrations use the author's standing pose as their reference

`Stand` uses the training module's published values: hip pitch -0.1 rad, knee 0.3 rad, ankle pitch -0.2 rad, arm pitch 0.18 rad, arm roll ±0.06 rad, and elbow pitch 0.78 rad. `Rest`, `Crouch`, `Turn`, `Present`, and `Wave` are small variations expressed through the intent controls. Durations are bounded by the URDF velocity limits and each instruction lands exactly on its targets.

Alternative considered: import learned-policy motion data. That would widen this request into dynamics and gait playback and would not provide a small, legible demo set.

## Findings

- The maintained URDF contains 24 links and 23 revolute joints; every joint-origin rotation is zero.
- Link visuals are metre-scale despite STL being unitless; the largest dimensions become plausible only after a 1000× conversion to millimetres.
- The current source display meshes are not a print catalogue: only the ankle-pitch meshes probe as watertight single bodies, while major links contain many disconnected/open surface components.
- The training configuration's nominal joint pose bends the knees and ankles and places the base at 0.75 m; that is a stronger standing reference than inventing a pose from the zeroed URDF alone.

## Risks / Trade-offs

- [The visual meshes may not satisfy solid-node's printed-solid adapter or connectivity contract] → Exercise the public import paths red-first, preserve a measured inventory, and stop rather than weaken the contract if a render-only visual cannot be represented.
- [The URDF and transcribed declarative layout can drift] → Parse and compare the complete source description in tests.
- [Intent coupling can obscure an individual joint] → Keep every joint declared and relation-driven, and document the mapping.
- [A geometrically plausible pose could be read as physically safe] → Label the model as visualization-only in the README and exclude hardware output and dynamic safety claims.
- [Full-resolution mesh builds and interference sweeps may be expensive] → Use the faceted kernel during development, a small scenario cadence, and one exact regression at completion; never simplify source geometry merely to improve runtime.
