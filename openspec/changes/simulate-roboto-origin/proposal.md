## Why

The request is to "simulate Robots/roboto_origin". The repository ships a complete 24-link, 23-joint URDF description and visual meshes, but those files do not provide a lightweight project-local way to assemble, pose, and inspect the robot as one machine in the solid-node viewer or to detect drift between the simulation and the source description.

## What Changes

- Add a thin `simulation/` package that imports the existing RPO link visuals, preserves their URDF hierarchy, metres-to-millimetres scale, joint origins, axes, limits, and material colours, and leaves every upstream file untouched.
- Add a compact intent-level control surface for the humanoid instead of exposing its 23 actuators as unrelated sliders. Every modeled joint remains driven through an explicit relationship.
- Add a small demonstration set—`Rest`, `Stand`, `Crouch`, `Turn`, `Present`, and `Wave`—with durations appropriate to the URDF actuator velocity limits.
- Add mechanical contracts for source drift, the link/joint tree, joint range behavior, bilateral placement, grounded standing poses, motion scenarios, rigid-link integrity, and the source mesh limitations discovered during import.
- Add build, test, control, and findings documentation, including the distinction between visualization and safe physical robot control.

## Capabilities

### New Capabilities

- `roboto-origin-assembly`: Assemble the existing RPO visual links in the URDF coordinate system and prove that the simulation remains faithful to the source link/joint description.
- `roboto-origin-poses`: Drive the assembled humanoid through understandable intent controls and repeatable poses while checking its articulated motion.

### Modified Capabilities

None.

## Impact

- Adds a root `pyproject.toml`, a project-local `simulation/` Python package and tests, baseline specifications under `openspec/specs/`, and simulation usage/findings in `README.md`.
- Adds `solid-node` as the simulation runtime dependency; it does not replace or become a dependency of the ROS 2, Isaac Lab, MuJoCo, firmware, hardware, or appearance modules.
- Uses `modules/rpo_description/urdf/rpo.urdf` and its 24 visual meshes as upstream inputs. Their current visual meshes are metre-scale and many are open, disconnected display surfaces, so the simulation will preserve and document that evidence rather than claim they are printable solids.
- Excludes dynamics, balance control, collision-safe trajectory planning, firmware commands, electronics, exterior appearance shells, fabrication exports, and changes to any file under `modules/`.
