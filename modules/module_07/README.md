CIS 536/736: Computer Graphics
| Phase | Module | Lecture(s) | Date(s) |
|---|---|---|---|
| Phase 3: Character Animation & Motion | Module 7: Animation Graphs & Motion Matching | Lectures 22-24 | Mon 19 Oct - Fri 23 Oct 2026 |
Lecture 22: Animation Graphs: State Machines, Blend Trees (Unity 6) (Mon 19 Oct)
 * Animator Controllers: Understanding the node-based architecture and execution flow for managing character animation states within Unity.
 * State Machines: Defining discrete animation states and configuring the conditional logic and transition durations governing movement between them.
 * Blend Trees: Blending multiple animation clips (e.g., idle, walk, run) continuously based on dynamic parameters such as velocity, direction, or angular momentum.
 * Transition Parameters: Utilizing animator variables (floats, integers, booleans, and triggers) driven by user input scripts to execute conditional state changes.
 * Animation Layers & Avatar Masks: Isolating and applying specific animation sequences to targeted skeletal hierarchies (e.g., upper-body aiming) without disrupting base locomotion.
Lecture 23: Motion Matching: Data-Driven Animation Selection (Wed 21 Oct)
 * Motion Matching Fundamentals: Transitioning from rigid, manually authored state machines to fluid, data-driven pose selection querying large animation databases.
 * Pose Search & Cost Functions: Evaluating the mathematical cost of transitioning between animation frames based on trajectory, bone velocity, and current pose alignment.
 * Trajectory Generation: Predicting the character's future path and desired momentum via player input to query the optimal next sequence from the animation dataset.
 * Data Tagging & Curation: Organizing and marking up raw motion capture data to restrict search spaces and prevent physically invalid transitions.
 * Unity 6 Integration: Configuring the native Unity Motion Matching package, generating pose databases, and binding the solver to character controllers.
Lecture 24: Lab 4b: Animation Graph + Motion Matching Assignment (Fri 23 Oct)
 * Asset Preparation: Importing, rigging, and retargeting character models and raw animation clips to ensure structural compatibility within the Unity environment.
 * State Machine Construction: Building a functional, baseline locomotion state machine using standard Blend Trees and Animator parameters to verify rig integrity.
 * Motion Matching Setup: Configuring a basic data-driven controller using a provided dataset of motion capture clips and adjusting trajectory weights.
 * Hybrid System Integration: Combining traditional deterministic Animator layers (for discrete actions like jumping or interacting) with the Motion Matching solver (for fluid locomotion).
 * Debugging & Polish: Utilizing runtime debug visualizers to troubleshoot transition popping, adjust pose-matching cost weights, and refine overall character responsiveness.