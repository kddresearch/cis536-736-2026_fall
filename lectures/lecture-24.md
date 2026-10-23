# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Character Animation & Motion | **Module 7:** Animation Graphs & Motion Matching | **Lecture 24:** Lab 4b: Animation Graph + Motion Matching Assignment | Friday, October 23, 2026 |

---

## Slide 1: Overview and Agenda
### CIS 536/736 - Lecture 24
* **Asset Preparation:** Importing, rigging, and retargeting character models.
* **State Machine Construction:** Building baseline locomotion with Blend Trees and Animator parameters.
* **Motion Matching Setup:** Configuring data-driven controllers and tuning trajectory weights.
* **Hybrid System Integration:** Merging deterministic Animator layers with Motion Matching solvers.
* **Debugging & Polish:** Using runtime visualizers to fix transition popping and refine cost weights.

**Speaker Notes:**
Welcome to Lab 4b. Today we transition from the theoretical design of PS 6 into the actual Unity 6 implementation. You will be building a complete character controller that uses both traditional state machines for discrete actions and modern motion matching for fluid locomotion. We will walk through the pipeline step-by-step, starting with asset preparation and ending with runtime debugging.

---

## Slide 2: Asset Preparation - Import and Rigging
### Ensuring Structural Compatibility
* **Import Settings:** Configuring FBX import settings to ensure correct scale, normals, and material mapping within Unity 6.
* **Rigging Verification:** Auditing the skeletal hierarchy to ensure bones are correctly parented and weighted.
* **Avatar Creation:** Generating a Unity Humanoid Avatar to standardize bone mapping across different meshes.

**Speaker Notes:**
Before any animation logic can be applied, your assets must be structurally sound. When you import your character and raw animation clips, the first step is generating a Humanoid Avatar. This standardizes the skeletal hierarchy, allowing Unity to map animations from our motion capture dataset onto your specific character mesh. If your rig integrity fails here, every downstream system will break.

---

## Slide 3: Asset Preparation - Retargeting
### Standardizing Motion Data
* **Motion Capture Ingestion:** Importing raw mocap clips and ensuring they align with the Unity Humanoid standard.
* **Retargeting Workflows:** Applying animations from disparate sources onto a single character rig without distortion.
* **Root Motion Alignment:** Verifying that root node transforms correctly drive the character's physical position in world space.

**Speaker Notes:**
Once your avatar is set up, you need to retarget the provided motion capture dataset. Retargeting allows you to reuse a single database of animations across multiple character models. Pay specific attention to Root Motion during this phase; if the root node's translation and rotation are not baked correctly, your character will slide across the floor instead of planting their feet.

---

## Slide 4: State Machine Construction - Baseline Locomotion
### Establishing Deterministic Control
* **Animator Controller:** Initializing the Unity Animator and defining the default entry state.
* **Discrete States:** Setting up foundational nodes like Idle, Walk, and Run.
* **Parameter Binding:** Linking float and boolean variables (e.g., Speed, isMoving) from your C# input script to the Animator.

**Speaker Notes:**
We begin the animation logic by constructing a traditional state machine. This serves as the baseline to verify that your rigged character responds correctly to player input. You will build standard discrete states and hook them up to your input scripts. This step ensures that your parameter bindings—like translating joystick magnitude into a Speed float—are functioning before we introduce the complexity of motion matching.

---

## Slide 5: State Machine Construction - Blend Trees
### Continuous Animation Blending
* **1D Blend Trees:** Blending linearly between Idle, Walk, and Run based on a single velocity parameter.
* **2D Blend Trees:** Incorporating lateral movement (strafing) using directional inputs alongside forward velocity.
* **Rig Integrity Check:** Verifying that blended animations do not cause bone collapsing or unnatural joint rotations.

**Speaker Notes:**
To smooth out our baseline locomotion, we implement Blend Trees. Instead of hard transitions, Blend Trees continuously interpolate between multiple clips based on dynamic parameters like velocity and direction. This is your final rig integrity check. If your weights are off, or if the source animations have mismatched poses, you will see joints collapse during the blend.

---

## Slide 6: Motion Matching Setup - Data-Driven Control
### Fluid Pose Selection
* **Motion Matching Component:** Adding the native Unity 6 Motion Matching solver to your character.
* **Pose Database Generation:** Pre-processing your mocap clips into an optimized, searchable database of poses and trajectories.
* **Querying the Database:** Replacing rigid state transitions with continuous database lookups based on current pose and future intent.

**Speaker Notes:**
Now we move to the core of Lab 4b: Motion Matching. We abandon the rigid transitions of the state machine for normal locomotion. Instead, you will feed your raw mocap clips into a pose database. At runtime, the Motion Matching solver queries this database every few frames, searching for the animation clip that best matches both the character's current physical pose and the player's inputted trajectory.

---

## Slide 7: Motion Matching Setup - Trajectory and Cost Functions
### Tuning the Search Space
* **Trajectory Prediction:** Generating a future path based on user input (stick direction, velocity, and camera facing).
* **Cost Weights:** Tuning the mathematical cost function that penalizes mismatched bone velocities and misaligned trajectories.
* **Data Tagging:** Marking specific clip ranges (e.g., planting a foot, turning sharply) to constrain the search space.

**Speaker Notes:**
The success of Motion Matching relies entirely on how you tune the cost function. The solver calculates a penalty for every potential next frame. If you weight the trajectory too heavily, the character will skate to reach the goal. If you weight pose matching too heavily, the character will feel sluggish and unresponsive. You must also tag your data to prevent the solver from picking physically invalid transitions, like suddenly swapping which foot is planted.

---

## Slide 8: Hybrid System Integration
### Best of Both Worlds
* **Deterministic Layers:** Using standard Animator layers for discrete, non-interruptible actions (e.g., jumping, attacking, interacting).
* **Solver Layers:** Delegating all continuous movement and idle behaviors to the Motion Matching solver.
* **Avatar Masks:** Isolating upper-body deterministic animations (like aiming) while the lower body remains data-driven.

**Speaker Notes:**
Motion Matching is excellent for locomotion but terrible for exact, discrete actions like swinging a sword or interacting with a lever. Therefore, we use a Hybrid System. You will place your Motion Matching solver on the base layer of your Animator, and overlay standard state machines on higher layers. By using Avatar Masks, you can have a character aim a weapon using deterministic logic while their legs fluidly navigate the terrain using data-driven motion.

---

## Slide 9: Debugging & Polish - Runtime Visualizers
### Troubleshooting the Solver
* **Debug Visualizers:** Enabling Unity's runtime gizmos to see the predicted trajectory and the solver's top candidate poses.
* **Identifying Transition Popping:** Spotting frames where the cost function failed, resulting in visual snapping or broken momentum.
* **Analyzing the Cost Graph:** Reading the debug output to determine if pose mismatch or trajectory mismatch caused the pop.

**Speaker Notes:**
Debugging a data-driven system is fundamentally different from debugging a state machine. You cannot just look at a transition arrow. You must use Unity's runtime debug visualizers. These tools render the predicted trajectory and the skeletal poses the solver is considering. If your character "pops" or snaps to a new pose, the visualizer will show you exactly which part of the cost function failed, allowing you to trace the error back to the database.

---

## Slide 10: Wrap-Up and Final Polish
### Refining Character Responsiveness
* **Tuning Responsiveness:** Balancing input latency against animation fidelity to ensure the character feels good to control.
* **Final Database Bake:** Optimizing the pose database for performance after all data tagging and weighting are complete.
* **Submission Requirements:** Packaging the Unity project, including the Animator Controller, Pose Database, and integration scripts.

**Speaker Notes:**
For the final polish, focus on the "feel" of the controller. A mathematically perfect animation blend might feel sluggish to the player. Adjust your trajectory weights to prioritize responsiveness over absolute fidelity if necessary. Once your hybrid system is stable and transition popping is minimized, perform a final bake of your pose database to optimize runtime performance. Package your complete Unity project for submission to GitHub Classroom.
