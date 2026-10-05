# CIS 536/736: Computer Graphics (Fall 2026)

| Phase | Module | Lecture(s) | Date(s) |
| :--- | :--- | :--- | :--- |
| **Phase [X]:** [Phase Name] | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lectures 17-21** | Mon 05 Oct - Fri 16 Oct 2026 |

---

## Lecture 17: OpenUSD Interchange for Procedural Assets: Variant Sets, Layer Referencing (Mon 05 Oct)

1. **OpenUSD Architecture Overview:** Understanding the core philosophy of Universal Scene Description for non-destructive pipeline interchange.
2. **Layer Referencing:** Utilizing USD Layers to separate geometry, materials, and overrides without destructive file merging.
3. **Variant Sets:** Defining and packaging parameterized variations of procedural assets within a single USD stage.
4. **Procedural Export Readiness:** Preparing attribute fields generated in Blender's Geometry Nodes for USD serialization.
5. **Asset Composition:** Assembling sub-components into a unified `.usd` or `.usda` stage for downstream consumption.

---

## Lecture 18: Term Project Proposal Workshop + Lab 3b Checkpoint: Export Pipeline (Wed 07 Oct)

1. **Term Project Scoping:** Defining the technical requirements, milestones, and grading rubrics for the CIS 536/736 capstone project.
2. **Proposal Ideation Workshop:** Formulating project pitches that integrate procedural generation, USD interchange, and real-time rendering.
3. **Export Pipeline Execution (Geometry Nodes → OpenUSD):** Hands-on validation of exporting parameterized Blender assets using the USD exporter.
4. **Unity 6 USD Import Mechanics:** Importing the assembled USD stage into the Unity 6 runtime environment.
5. **Lab 3b Pipeline Troubleshooting:** Resolving common data-loss vectors (missing materials, corrupted normals, dropped instances) during DCC-to-Engine transfer.

*(Note: Friday 09 Oct 2026 – NO CLASS / Wildcat Pause Day)*

---

## Lecture 19: Rigging Fundamentals: Skeletons, Skinning, Control Rig (Mon 12 Oct)

1. **Armature and Skeleton Hierarchy:** Defining bone chains, root nodes, and parent-child transformation inheritance in 3D space.
2. **Forward vs. Inverse Kinematics (FK/IK):** Mathematical and practical distinctions between driving bones via local rotations (FK) versus end-effector targets (IK).
3. **Vertex Weight Painting & Skinning:** Binding structural geometry to armature bones using normalized weight influences to dictate mesh deformation.
4. **Control Rig Construction (Blender):** Building non-deforming control curves and shapes to act as the animator's interface for the skeletal rig.
5. **Bone Constraints:** Implementing basic structural limits (e.g., pole vectors, limit rotation, copy transforms) to prevent mesh tearing and unnatural deformation.

---

## Lecture 20: Unity Animation Rigging: Constraints & IK Solvers (Wed 14 Oct)

1. **Animation Rigging Package Overview:** Integrating Unity's procedural Animation Rigging package to augment static, baked animations at runtime.
2. **Two-Bone IK Solvers:** Setting up procedural inverse kinematics for limbs (arms/legs) to dynamically reach targets in the Unity engine.
3. **Multi-Aim & Look-At Solvers:** Configuring dynamic aim constraints for heads, eyes, and weapons to track targets independently of the animation clip.
4. **Rig Transform Constraints:** Utilizing parent, copy, and damp constraints within Unity to dynamically attach objects (e.g., equipment, IK targets) to the skeletal hierarchy.
5. **Procedural Rig Weighting & Blending:** Controlling the influence of procedural IK solvers versus baked animation data via script and animator parameters.

---

## Lecture 21: Lab 4a: Control Rig + Unity Animation Rigging Hands-On (Fri 16 Oct)

1. **Lab 4a Objectives & Deliverables:** Outlining the requirements for building and exporting a functional rig to Unity.
2. **Blender to Unity Skeleton Validation:** Verifying that bone axes, scales, and hierarchies remain intact across the `.fbx` or `.usd` export bridge.
3. **Applying Unity Constraints to Imported Rigs:** Hands-on setup of the Rig Builder component and associating procedural constraints with the imported skeleton.
4. **Debugging IK Targets and Pole Vectors:** Troubleshooting common runtime issues, such as joint flipping, hyperextension, and incorrect pole vector alignment.
5. **Interactive Rig Finalization:** Tuning weights and solver parameters to ensure smooth transitions and robust interactive behavior within the Unity 6 scene.
