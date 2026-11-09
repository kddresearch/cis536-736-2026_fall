# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Visualization & Special Topics | **[Module 11](../modules/module_11/):** Visualization II & Special Topics | **Lecture 31:** Information Visualization II & Particle Systems | Mon 09 Nov 2026 | [*Data Visualization* (Healy), Ch. 5–6](https://socviz.co/); Unity 6 Manual: VFX Graph |

---

## Slide 1: Lecture Overview & Agenda
### CIS 536/736 - Lecture 31
* **Core Focus:** Integrating structural information visualization design with high-performance real-time particle simulations and GPU-driven visual effects pipelines.
* **Assigned Reading:** [*Data Visualization* (Healy), Ch. 5–6](https://socviz.co/); Unity 6 Manual: VFX Graph
* **Visual Storytelling:** Transitioning from static statistical graphics to dynamic, interactive data representations.
* **Simulation Mechanics:** Fundamentals of particle emitters, forces, and GPU compute buffers.
* **VFX Graph Architecture:** Node-based GPU shader execution in Unity 6's High Definition Render Pipeline (HDRP).

**Speaker Notes:**
Welcome to Lecture 31. Today we unite two pivotal domains of modern computer graphics: how to visually encode complex multidimensional datasets using Kieran Healy's visualization principles, and how to implement massive real-time particle systems using compute-driven GPU architectures like Unity's VFX Graph.

---

## Slide 2: Information Visualization Principles (Healy Ch. 5 & 6)
### Structuring Multi-Variable Visual Encodings
* **[Data-to-Aesthetic Mapping]:** Translating quantitative variables into spatial position, scale, color hue, luminance, and geometric markers.
* **[Facet Grids & Small Multiples]:** Utilizing repeating panel grids to convey multi-category interactions without overloading single-plot axes.
* **[Perceptual Hierarchy]:** Leveraging Gestalt principles of grouping, enclosure, and proximity to guide viewer interpretation naturally.

**Speaker Notes:**
In Chapters 5 and 6 of *Data Visualization*, Kieran Healy emphasizes that effective graphics reduce cognitive load. By using facet grids and small multiples, we can unpack multi-dimensional data into digestible, visually consistent panels.

---

## Slide 3: Avoiding Graphical Distortion & Bias
### Maintaining Proportional Integrity
* **[Lie Factor Containment]:** Ensuring visual changes in graphics remain strictly proportional to numerical variations in the underlying data.
* **[Color Space Uniformity]:** Replacing non-uniform color scales (e.g., standard Rainbow/Jet) with perceptually linear palettes (Viridis, Magma, oRGB).
* **[Scale & Aspect Ratio Selection]:** Preventing artificial trend exaggeration by selecting objective, unskewed axis transformations.

**Speaker Notes:**
As graphics engineers, we must guard against visual dishonesty. Using non-linear color ramps or distorted aspect ratios misleads the user. Perceptually uniform spaces like Viridis or oRGB ensure that equal data increments yield equal visual jumps.

---

## Slide 4: Real-Time Particle System Foundations
### Core Simulation Mechanics
* **[Emitter Property Encodings]:** Controlling spatial generation shapes, emission rates, initial velocity vectors, and lifetime distributions.
* **[State Update Loops]:** Updating particle kinematics across fixed time-steps ($t \to t + \Delta t$) using numerical integration (Euler, Verlet).
* **[Lifecycle Phases]:** Managing birth initialization, time-based attribute modulation (color over lifetime, size curves), and memory allocation recycling.

**Speaker Notes:**
Shifting to dynamic graphics, particle systems simulate unstructured phenomena like dust, fire, or data streams. Every particle undergoes a lifecycle: initialized by an emitter, updated by physical forces over its lifetime, and reclaimed when its age exceeds its allocation.

---

## Slide 5: Particle Kinematics & Force Fields
### Physics-Driven Procedural Motion
* **[External Force Vector Integration]:** Applying global forces (gravity, wind drag, buoyancy) directly to per-particle acceleration vectors.
* **[Vector Field Turbulence]:** Sampling 3D Perlin or Simplex noise fields to induce organic, non-deterministic movement trajectories.
* **[Boundary & Geometry Collisions]:** Computing ray-casts or depth-buffer collisions to bounce or stick particles against scene geometry.

**Speaker Notes:**
To make particles move organically, we sample vector force fields. By layering 3D Simplex noise over directional forces like gravity, particles weave realistically through space without expensive per-element pathing algorithms.

---

## Slide 6: GPU Compute Architecture for Particles
### Parallelizing Mass Simulation
* **[CPU Bottlenecks]:** Identifying performance drop-offs caused by sequential CPU memory iterations and draw-call overhead.
* **[Compute Shader Execution]:** Offloading million-particle position, velocity, and color updates directly to massive parallel GPU threads.
* **[Structured Buffers]:** Utilizing GPU memory blocks (StructuredBuffers, RWStructuredBuffers) to preserve state across frame updates.

**Speaker Notes:**
Iterating through hundreds of thousands of particles on the CPU quickly saturates main memory bandwidth. By transferring simulation state into GPU StructuredBuffers, compute shaders update millions of particles in parallel per frame.

---

## Slide 7: Unity 6 VFX Graph Architecture
### High-Performance Node-Based VFX
* **[Context Node Pipeline]:** Structuring workflows into discrete blocks: Initialize, Update, and Output contexts running natively on the GPU.
* **[Attribute Buffer Management]:** Explicitly exposing particle attributes (position, age, size, color) for procedural graph manipulation.
* **[HDRP Integration]:** Linking the VFX Graph with volumetric lighting, shadow passes, and screen-space depth buffers.

**Speaker Notes:**
Unity 6's VFX Graph provides a visual programming interface that compiles directly into compute shader code. It breaks execution into Initialize, Update, and Output contexts, enabling low-level GPU control without manual HLSL setup.

---

## Slide 8: Custom HLSL & Shader Graph Integration
### Extending Particle Behavior
* **[Custom HLSL Operators]:** Injecting hand-authored mathematical routines into VFX Graph nodes for advanced force calculations.
* **[Texture Buffer Sampling]:** Reading external data textures or vector field maps directly inside the GPU Update context.
* **[Material Sub-Shader Binding]:** Connecting VFX Graph outputs into custom Shader Graph materials for advanced PBR lighting.

**Speaker Notes:**
When built-in nodes aren't enough, we write custom HLSL operator blocks within the VFX Graph. This allows us to sample custom 3D data textures or feed complex mathematical models directly into particle property channels.

---

## Slide 9: Data-Driven Particle Visualization
### Merging InfoViz with Particle Systems
* **[Particle Attribute Data-Mapping]:** Driving particle scale, color, and lifetime using live analytical metrics rather than static constants.
* **[Flow Field Data Projections]:** Mapping vector flow datasets (e.g., wind velocity, financial telemetry) directly to particle force fields.
* **[Interactive Point Clouds]:** Rendering massive high-dimensional datasets as responsive, real-time particle clusters in 3D space.

**Speaker Notes:**
Here is where our two topics converge: using particle systems as real-time visual encodings. By mapping live data streams to particle velocities, colors, and spatial destinations, complex statistical flows transform into immersive visual experiences.

---

## Slide 10: Summary & Next Steps
### Module 11 Progress & Capstone Alignment
* **[Key Takeaways]:** Grounding dynamic GPU particle pipelines in rigorous visual encoding principles ensures both performance and clarity.
* **[Hands-On Application]:** Complete the Unity UI Toolkit Runtime Charts Lab and experiment with the VFX Graph particle sample scenes.
* **[Next Lecture Preview]:** Lecture 32 will explore fluid dynamics simulations (GPU grid fluids) and volumetric smoke/fire rendering techniques.

**Speaker Notes:**
To summarize: high-performance particle graphics are most compelling when guided by solid visualization choices. Review Healy's chapters, complete the runtime charts lab, and experiment with GPU compute contexts in the VFX Graph. Next time, we tackle GPU fluid dynamics!
