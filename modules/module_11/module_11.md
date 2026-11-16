<!--
  Template Note: This file serves as the canonical template for module pages.
  When instantiated as `modules/module_YY/module_YY.md`, use relative paths 
  (`../../phases/phase_0X/` and `../../lectures/lecture_j.md`) to maintain correct routing.
  Note that the module name itself does not self-link, but lectures link directly to their markdown files.
  Module readings may be contiguous or separate per lecture; the module readings for the modules within a parent phase form a partition of that phase’s readings.
-->

# CIS 536/736: Computer Graphics

| Phase | Module | Lecture(s) | Date(s) | Assigned Reading(s) |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 5](../../phases/phase_05/):** Visualization, VFX & Capstone Synthesis | **Module 11:** Data/Info Visualization II, VFX & Interim Integration | **Lectures 34–36** | Mon 16 Nov – Fri 20 Nov 2026 | Healy, *Data Visualization*, Ch. 5–6; GPU Gems Ch. 38 (Harris); GAMES101 Lecture 21; Unity VFX Graph |

---

## [Lecture 34: Data/Info Visualization II: Objects & Processes](../../lectures/lecture_34.md) (Mon 16 Nov 2026)

1. **Multi-Layer Grammar of Graphics (`ggplot` / Declarative Pipelines):** Composing data mappings, geometric objects (`geoms`), statistical transformations (`stats`), and coordinate systems to visualize complex structural relationships without visual clutter.
2. **Visualizing Multivariate Objects & Distributions:** Encoding continuous and categorical attributes across grouped comparisons, boxplots, violin plots, and Cleveland dot plots while preserving raw data granularity.
3. **Modeling & Process Visualization:** Displaying statistical model fits, confidence intervals, residuals, and marginal effects directly alongside observed empirical distributions.
4. **Small Multiples & Faceting:** Partitioning high-dimensional datasets across aligned coordinate panels (`facet_wrap` / `facet_grid`) to support rapid visual comparison across categories and temporal states.
5. **Spatial & Network Process Encodings:** Representing relational topologies, flow dynamics, and spatial transformations within 2D and 3D scene graphs.
* **Assigned Reading:** Healy, *Data Visualization*, Ch. 5–6 (`socviz.co`)

---

## [Lecture 35: Special Topics: VFX & Particle Systems Survey](../../lectures/lecture_35.md) (Wed 18 Nov 2026)

1. **Lagrangian Particle Systems Architecture:** Managing particle lifecycles (emission, update, collision, death), state attributes (position, velocity, age, color, size), and deterministic vs. stochastic force fields.
2. **Eulerian Grid-Based Fluid Simulation (Navier–Stokes):** Discretizing velocity, pressure, and density fields on 2D/3D grids using semi-Lagrangian advection, diffusion, external force integration, and Jacobi/conjugate-gradient pressure projection.
3. **Hybrid Particle-Grid & Mass-Spring Dynamics:** Coupling discrete particle advection with continuous grid solvers (FLIP/PIC/MPM) and numerical time-integration schemes (explicit Euler, semi-implicit Euler, Verlet, Runge–Kutta).
4. **GPU Compute & Unity VFX Graph Execution:** Offloading millions of particle updates and spatial hashing to GPU compute shaders using Unity's node-based Visual Effect Graph (`Initialize`, `Update`, `Output` contexts).
5. **Volumetric & Billboard Rendering:** Rendering dense particle clouds, smoke, and fire via camera