## ADDED Requirements

### Requirement: Intent-sized control surface
The robot SHALL expose no more than seven controls whose names let a maker predict the visible motion, while every one of its 23 joint coordinates remains explicitly driven.

#### Scenario: Inspect published controls
- **WHEN** a maker opens the complete robot model
- **THEN** the viewer lists `crouch`, `leg_yaw`, `torso_turn`, `arm_pitch`, `arm_spread`, `elbow_bend`, and `right_wave` and no per-motor control bank

### Requirement: Source standing reference
The `Stand` instruction SHALL reproduce the joint values published by the training module's initial state at a base height of 750 mm.

#### Scenario: Stand endpoint
- **WHEN** the `Stand` instruction completes
- **THEN** both legs and arms reach the source standing values and both feet remain at the virtual floor within the measured tolerance

### Requirement: Demonstration poses
The robot SHALL provide `Rest`, `Stand`, `Crouch`, `Turn`, `Present`, and `Wave` instructions that land exactly on their documented intent targets.

#### Scenario: Trigger each pose
- **WHEN** the six instructions are triggered in sequence
- **THEN** each move completes at its target state within its declared duration

### Requirement: Coupled bilateral motion
The leg and arm intent controls SHALL move paired joints according to documented bilateral relationships, including opposite signs where the robot's mirrored anatomy requires them.

#### Scenario: Present pose symmetry
- **WHEN** the `Present` instruction completes
- **THEN** corresponding left and right arm links occupy mirrored placements about the robot's sagittal plane within the measured tolerance

### Requirement: Joint-limit enforcement
Every instruction target and every value inside a control's published range SHALL keep all driven joints inside the limits declared by the maintained URDF.

#### Scenario: Control boundary poses
- **WHEN** each intent control is exercised at both published endpoints
- **THEN** no driven joint raises a range violation

### Requirement: Articulated scenario integrity
The demonstration sequence SHALL sample rigid-part interference at a cadence justified by the instruction durations and SHALL assert its final target after every move.

#### Scenario: Complete demonstration sequence
- **WHEN** the scenario runs through all six instructions
- **THEN** target assertions and whole-assembly interference checks execute throughout the bounded sequence

### Requirement: Visualization-only operation
The simulation SHALL not emit physical robot commands and SHALL state that its poses are not evidence of dynamic balance, collision-safe planning, or operational safety.

#### Scenario: Follow simulation instructions
- **WHEN** a maker reads the run and control documentation
- **THEN** it clearly separates viewer posing from ROS 2, firmware, and physical actuation
