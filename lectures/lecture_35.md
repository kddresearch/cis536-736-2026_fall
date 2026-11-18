# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
**[Phase 5](../phases/phase_05/):** Visualization, VFX & Capstone Synthesis | **[Module 11](../modules/module_11/):** Data/Info Visualization II, VFX & Interim Integration | **Lecture 35:** Special Topics: VFX & Particle Systems Survey | Wed 18 Nov 2026 | GPU Gems Ch. 38 (Harris); Unity VFX Graph |

---

## Slide 1: Special Topics: VFX & Particle Systems Survey - Overview
### CIS 536/736 - Lecture 35
* **Overview:** Survey of visual effects architectures, bridging Lagrangian particle dynamics, Eulerian fluid grids, and GPU compute execution.
* **Assigned Reading:** GPU Gems Ch. 38 (Harris); Unity VFX Graph documentation.
* **Core Topics:** Lagrangian lifecycle management, Navier–Stokes grid solvers, hybrid FLIP/PIC methods, and GPU compute parallelism.
* **Architecture:** Decoupling CPU game logic from GPU-driven parallel simulations.
* **Context:** Preparing for real-time VFX integration and capstone simulation pipelines.

**Speaker Notes:**
Welcome back, everyone. Today we move into Module 11's second lecture, focusing on Special Topics in Visual Effects and Particle Systems Survey. We will examine how modern real-time engines and offline renderers simulate complex natural phenomena—ranging from sparks and smoke to incompressible fluid flows—by combining particle methods and grid-based solvers.

---

## Slide 2: Lagrangian Particle Systems Architecture
### Managing Lifecycles, Attributes, and Force Fields
* **Lifecycle Management:** Dynamic emission, state updates, collision detection, and deterministic death phases.
* **State Attributes:** Tracking position, velocity, mass, age, color, size, and custom user-defined channels per particle.
* **Force Fields:** Applying deterministic gravity, drag, turbulence, and stochastic Brownian noise vectors.
* **Memory Layout:** Structuring particle data as Arrays of Structures (AoS) vs. Structures of Arrays (SoA) for cache efficiency.

**Speaker Notes:**
Lagrangian particle systems treat phenomena as discrete moving samples. Each particle maintains its own private state data through a strict lifecycle. Managing millions of these efficiently requires careful memory layout design, typically favoring Structures of Arrays or flat GPU buffers over traditional object-oriented AoS approaches to maximize cache locality during parallel updates.

---

## Slide 3: Eulerian Grid-Based Fluid Simulation (Navier–Stokes)
### Discretizing Velocity, Pressure, and Density Fields
* **Grid Discretization:** Mapping continuous fluid properties onto staggered MAC (Marker-And-Cell) grids.
* **Semi-Lagrangian Advection:** Tracing velocity fields backward in time to compute stable scalar transport without excessive numerical dissipation.
* **Diffusion & External Forces:** Integrating viscosity coefficients, buoyancy forces, and external source terms.
* **Pressure Projection:** Solving the Poisson equation via Jacobi or conjugate-gradient iterations to enforce incompressibility (divergence-free velocity).

**Speaker Notes:**
In contrast to particle models, Eulerian methods discretize space into a fixed grid. Simulating incompressible fluids requires solving the Navier-Stokes equations, where pressure projection is computationally the most demanding step. We use iterative solvers like conjugate gradients to ensure mass conservation across every grid cell at every time step.

---

## Slide 4: Hybrid Particle-Grid & Mass-Spring Dynamics
### Coupling Discrete Advection with Continuous Solvers
* **FLIP/PIC Methods:** Combining Particle-in-Cell (PIC) for smooth momentum transfer and Fluid-Implicit-Particle (FLIP) for preserving energetic detail.
* **Material Point Method (MPM):** Transferring state between Lagrangian particles and Eulerian grids to simulate snow, foam, and hyperelastic materials.
* **Mass-Spring Meshes:** Simulating elastic deformations, cloth, and soft bodies through networked point masses and Hookean springs.
* **Numerical Integration:** Selecting explicit Euler, semi-implicit Euler, Verlet, or higher-order Runge–Kutta schemes based on stiffness.

---

## Slide 5: GPU Compute & Unity VFX Graph Execution
### Massive Parallelism via Compute Shaders
* **Compute Dispatch:** Launching thread groups to process millions of independent particle simulation elements concurrently.
* **Unity VFX Graph Contexts:** Structuring data flow through distinct `Initialize`, `Update`, and `Output` GPU graph contexts.
* **Spatial Hashing:** Utilizing uniform grid or bounding volume hierarchies on the GPU for rapid neighbor searching and collision.
* **Zero-Copy Interop:** Sharing data directly between compute buffers and graphics pipelines without CPU round-trips.

---

## Slide 6: Volumetric & Billboard Rendering
### Visualizing Dense Particle Clouds and Participating Media
* **Billboard Primitives:** Facing camera-aligned quad polygons toward the viewpoint to represent complex smoke or fire puffs.
* **Ray Marching:** Sampling 3D density textures and voxel grids along camera rays to render clouds and atmospheric fog.
* **Transparency Sorting:** Managing depth sorting or order-independent transparency (OIT) for overlapping semi-transparent particle sprites.
* **Lighting Integration:** Accumulating shadow maps and ambient occlusion across participating media volumes.

---

## Slide 7: Case Study: Spark and Fire Simulation Pipeline
### End-to-End Particle System Implementation
* **Emitter Configuration:** Spawning particles with randomized initial velocities along normal vectors of a dynamic mesh surface.
* **Force Integration:** Applying upward buoyancy acceleration alongside curl-noise turbulence for organic flame flicker.
* **Lifetime Color Mapping:** Interpolating particle color and opacity across normalized lifespan ($t \in [0, 1]$) from white-hot core to dissipating soot.
* **Collision Response:** Interacting with static scene geometry via implicit signed distance fields (SDFs).

---

## Slide 8: Performance Optimization & Bottlenecks
### Balancing Visual Fidelity and Frame-Rate Budgets
* **Fill-Rate Management:** Controlling overdraw caused by large, overlapping particle billboards near the camera lens.
* **LOD Scaling:** Dynamically reducing particle emission rates and simulation frequency based on screen-space bounding box size.
* **Async Compute:** Overlapping heavy VFX simulation dispatches with main rendering passes on modern graphics hardware.
* **Memory Budgeting:** Pre-allocating fixed-size particle pools to prevent garbage collection spikes and memory fragmentation.

---

## Slide 9: Advanced Topics & Research Frontiers
### Neural Fluid Surrogates and Real-Time Path Tracing
* **Neural Fluid Simulations:** Accelerating Eulerian grid updates and pressure solvers using deep learning surrogate models.
* **Real-Time Path Traced Particles:** Incorporating participating media ray tracing into particle rendering pipelines for accurate global illumination.
* **Multiphysics Coupling:** Intertwining rigid body dynamics, cloth simulation, and fluid drag within a unified solver framework.
* **Generative VFX:** Utilizing diffusion models and procedural text prompts to author complex particle behaviors and texture atlases.

---

## Slide 10: Summary & Next Steps
### Transitioning to Capstone VFX Integration
* **Key Takeaway 1:** Lagrangian models excel at sparse, bounded phenomena; Eulerian grids are mandatory for continuous volumetric fluids.
* **Key Takeaway 2:** Modern real-time VFX relies entirely on GPU compute shaders and graph-based execution nodes (e.g., Unity VFX Graph).
* **Next Session:** Lecture 36 will synthesize our visualization and VFX tools into interim capstone project architectures.
* **Action Item:** Review GPU Gems Chapter 38 in preparation for Friday's capstone integration lab.

**Speaker Notes:**
To wrap up, Lecture 35 has surveyed the entire spectrum of visual effects simulation—from foundational particle mechanics to heavy fluid solvers and GPU compute graphs. Next time, in Lecture 34/36 integration, we will bring these simulation concepts directly into our capstone project frameworks. Thank you, and see you on Friday.
