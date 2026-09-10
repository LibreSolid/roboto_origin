## ADDED Requirements

### Requirement: Complete URDF link assembly
The simulation SHALL present all 24 ROBOTO_ORIGIN visual links in the parent-child hierarchy declared by the maintained RPO URDF, with the upstream geometry left unchanged.

#### Scenario: Default robot build
- **WHEN** a maker builds the project model
- **THEN** the published tree contains one visual for every URDF link beneath the corresponding articulated link assembly

### Requirement: URDF coordinate fidelity
The simulation SHALL preserve each joint's parent, child, origin, axis, and limits, converting source metres to millimetres and source radians to degrees without changing their physical meaning.

#### Scenario: Source description check
- **WHEN** the source-drift contract parses the maintained URDF
- **THEN** every transcribed joint relationship and value matches the source within the declared numeric tolerance

### Requirement: Source material identity
The simulation SHALL preserve the material colour assigned to each URDF visual and SHALL identify its geometry as visualization data rather than as a print-ready artifact.

#### Scenario: Published visual materials
- **WHEN** the complete assembly is built
- **THEN** each visual link carries the colour converted from its URDF material and the documentation disclaims fabrication use

### Requirement: Measured source-mesh evidence
The project SHALL keep a repeatable probe and recorded measurements for the visual meshes' bounds, scale, watertightness, and connected components without repairing or replacing the upstream files.

#### Scenario: Repeat mesh survey
- **WHEN** the probe is run against the maintained visual mesh directory
- **THEN** it reports one record per URDF visual and its measurements can be compared with the committed findings

### Requirement: Assembly structural contracts
The simulation SHALL carry whole-model connectivity and non-interference contracts and SHALL not suppress a failure caused by incomplete, disconnected, or overlapping geometry.

#### Scenario: Structural regression
- **WHEN** the simulation regression is run
- **THEN** every rigid visual is checked for connectedness and every articulated pose is checked for positive-volume interference

### Requirement: Upstream source preservation
The simulation SHALL add project-owned files outside `modules/` and SHALL not edit, regenerate, or reformat the aggregated design sources.

#### Scenario: Review simulation delivery
- **WHEN** the simulation changes are reviewed
- **THEN** all implementation, evidence, and specifications are confined to project-owned simulation, documentation, manifest, ignore, and OpenSpec paths
