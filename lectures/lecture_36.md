# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 5](../phases/phase_05/):** Visualization, VFX & Capstone Synthesis | **[Module 11](../modules/module_11/):** Data/Info Visualization II, VFX & Interim Integration | **Lecture 36:** Interim Integration: Houdini, Blender 4 & Unity 6 Pipeline Synthesis; Wrap-Up | Fri 20 Nov 2026 | [Healy, *Data Visualization*, Ch. 5–6](https://socviz.co/); [Harris, *GPU Gems* Ch. 38: *Fast Fluid Dynamics Simulation on the GPU*](https://developer.nvidia.com/gpugems/gpugems/part-vi-beyond-triangles/chapter-38-fast-fluid-dynamics-simulation-gpu); [GAMES101 Lecture 21: *Animation (Cont.)*](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html); [Unity 6 *Visual Effect Graph* Manual](https://docs.unity3d.com/Packages/com.unity.visualeffectgraph@latest); *(Pipeline Integration)*: [SideFX Houdini *USD, VAT & Point Cache* Docs](https://www.sidefx.com/docs/houdini/), [Blender 4.2 *OpenUSD & OpenVDB* Manual](https://docs.blender.org/manual/en/4.2/files/import_export/usd.html) |

---

## Slide 1: Interim Integration: Houdini, Blender 4 & Unity 6 Pipeline Synthesis; Wrap-Up
### CIS 536/736 - Lecture 36
* **Module 11 Synthesis:** Bridging declarative data visualization (Healy, Ch. 5–6) and GPU particle/fluid simulation (Harris; GAMES101 Lecture 21; Unity VFX Graph) across a multi-DCC (`Houdini -> Blender 4.2 LTS -> Unity 6 LTS`) production spine.
* **Assigned Reading:** [Healy, *Data Visualization*, Ch. 5–6](https://socviz.co/); [Harris, *GPU Gems* Ch. 38](https://developer.nvidia.com/gpugems/gpugems/part-vi-beyond-triangles/chapter-38-fast-fluid-dynamics-simulation-gpu); [GAMES101 Lecture 21](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html); [Unity 6 *Visual Effect Graph* Manual](https://docs.unity3d.com/Packages/com.unity.visualeffectgraph@latest)
* **Multi-DCC Role Partitioning:** Separating procedural simulation and field generation (Houdini), mesh/rig/look-dev assembly (Blender 4.2), and real-time runtime execution (Unity 6).
* **Deterministic Interchange Bridges:** Choosing among OpenUSD, OpenVDB, Vertex Animation Textures (VAT), Point Caches (`.pcache`), and 3D Vector Fields (`.vf`) without silent data loss.
* **Capstone Roadmap:** Empirical baseline benchmarking (Phase 3), Fall Break execution, Week 15 extra-credit presentations, Dead Week presentations, and IEEE/ACM final reports.

**Speaker Notes:**
Welcome to Lecture 36 and the wrap-up of Module 11 ahead of Fall Break. On Monday we covered multi-layer visualization, statistical overlays, and faceting in Healy Chapters 5 and 6. On Wednesday we surveyed Lagrangian particle systems, Eulerian Navier–Stokes fluid solvers on the GPU, time-integration stability, and Unity's Visual Effect Graph. Today we synthesize those foundations into an end-to-end multi-DCC pipeline integrating SideFX Houdini, Blender 4.2 LTS, and Unity 6 LTS—showing how procedural simulations and scientific data flows move cleanly into real-time runtime scenes and get benchmarked against controlled baselines before Fall Break.

---

## Slide 2: Multi-DCC Role Partitioning & Data-Driven VFX Architecture
### Assigning Each Tool to Its Mathematical & Computational Strength
* **SideFX Houdini (Procedural Geometry, Simulation & Field Authoring):**
  * Authors procedural SOP networks (`Attribute Wrangle` / VEX), DOP solvers (Pyro smoke/fire, FLIP fluids, Vellum cloth/soft-body), and TOPs/PDG batch sweeps.
  * Ingests structured tabular/scientific datasets (`CSV`/`JSON`/tensors) into per-point or voxel attributes (`@P`, `@v`, `@Cd`, `@density`, `@pscale`) for InfoVis and physical VFX.
* **Blender 4.2 LTS (Asset Assembly, Rigging, UVs & Look-Dev):**
  * Builds clean polygonal topology, UV layouts, armature rigs/Shape Keys, and Geometry Nodes scatter/instancing; inspects imported Houdini OpenUSD/OpenVDB/Alembic caches.
* **Unity 6 LTS (Real-Time Runtime, Shader Graph & VFX Graph):**
  * Executes real-time scene graphs, Shader Graph vertex/surface shaders, and GPU compute particle systems (`GraphicsBuffer`, `.pcache`, `.vf` vector fields) with Healy-compliant visual diagnostics.

**Speaker Notes:**
A common capstone trap is trying to force one Digital Content Creation (DCC) tool to do everything—like hand-painting skin weights in Houdini or simulating high-resolution FLIP fluids inside Unity's main thread. Instead, partition your pipeline by mathematical strength. Houdini excels at procedural SOP generation, VEX attribute wrangling, and heavy Eulerian/Lagrangian physics. Blender 4.2 LTS excels at interactive modeling, UV unwrapping, character rigging, and Geometry Nodes instancing. Unity 6 LTS is your real-time execution and profiling engine. When each tool emits clean, typed interchange artifacts, your team can iterate in parallel without blocking one another.

---

## Slide 3: Cross-DCC Coordinate Invariants & Interchange Spine
### Navigating the Houdini (Y-Up) $\leftrightarrow$ Blender 4.2 (Z-Up) $\rightarrow$ Unity 6 (Y-Up) Double Flip
* **Coordinate & Handedness Alignment:**
  * **Houdini:** Right-handed, **Y-up** ($1\text{ unit} = 1\text{ m}$).
  * **Blender 4.2 LTS:** Right-handed, **Z-up** ($1\text{ unit} = 1\text{ m}$).
  * **Unity 6 LTS:** Left-handed, **Y-up** ($1\text{ unit} = 1\text{ m}$).
  * **Pipeline Rule:** If an asset does not require Blender modeling/rigging, route Houdini caches directly to Unity 6 (Y-up $\rightarrow$ Y-up) to avoid a redundant Z-up round-trip; when routing through Blender 4.2, apply transforms (`Ctrl+A`) and enforce explicit USD/FBX axis conversion on every hop.
* **Canonical Format Selection:**
  * **OpenUSD (`.usda`/`.usdc`):** Non-destructive scene hierarchies, instancing, `UsdVol` OpenVDB bindings, and `UsdSkel` rigs across Houdini Solaris (LOPs), Blender 4.2 USD I/O, and Unity 6 (`com.unity.usd-toolkit`).
  * **FBX / glTF 2.0 (`glTFast`):** Armature rigs, BlendShapes, and static PBR meshes from Blender 4.2 into Unity 6.

**Speaker Notes:**
Look closely at the coordinate axes across these three tools: Houdini and Unity 6 are both Y-up, while Blender 4.2 sits in the middle as Z-up, and Unity flips handedness from right-handed to left-handed. If you export a Houdini asset to Blender for retopology or rigging and then export to Unity without locking your axis settings, you will get 90-degree pitch rotations or mirrored winding orders. Always verify a single calibration cube with asymmetric L/R/U/F face markers across your full chain before importing hero assets.

---

## Slide 4: Bridging Houdini Simulations to Unity 6: VAT, Point Caches & VFX Graph
### Deterministic Baking & Runtime GPU Replay Without Live Engine Overhead
* **Why Native DOP/SOP Networks Do Not Run Live in Unity Builds:** Live Houdini Engine (`HDA`) evaluation inside Unity Editor is useful for level layout, but standalone Unity 6 builds require lightweight, GPU-native runtime representations:
  1. **Vertex Animation Textures (VAT 3.0):** SideFX Labs bakes Vellum cloth, soft-body impacts, RBD destruction, or fluid surface meshes into floating-point `EXR` position/normal lookup textures paired with a low-poly proxy mesh (validated in Blender 4.2 UV2/UV3 channels) and decoded in Unity 6 Shader Graph.
  2. **Point Caches (`.pcache`) & 3D Vector Fields (`.vf`):** Export Houdini POP/FLIP particle attributes (`P`, `v`, `Cd`, `age`, `size`) to `.pcache` or export divergence-free velocity volumes (Harris, *GPU Gems* Ch. 38) to `.vf` files, sampling them directly inside Unity 6 VFX Graph (`Sample Vector Field` / `Point Cache`).
  3. **Alembic (`.abc`):** Constant- or varying-topology geometry caches via `com.unity.formats.alembic` for hero shots (strictly size-bounded to avoid Git bloat).

**Speaker Notes:**
This slide is the technical core of Houdini-to-Unity integration. Suppose you simulate a tearing Vellum banner or splashing FLIP fluid in Houdini. If you export a raw 200-frame Alembic cache, you may end up with a 1.5 GB file that stalls Git and streams heavily off disk at runtime. By contrast, Vertex Animation Textures (VAT) encode per-vertex displacement and normals into compact 2D `EXR` textures that Unity 6 Shader Graph samples in a single vertex shader pass—giving you complex Houdini physics at real-time GPU frame rates. Similarly, exporting Houdini velocity grids as `.vf` vector fields lets Unity 6 VFX Graph advect hundreds of thousands of live particles through pre-computed Navier–Stokes flow fields at 60+ FPS.

---

## Slide 5: Numerical Stability, Attribute Hygiene & PBR Shader Parity
### Preventing Silent Drift Across Multi-DCC Conversions
* **Attribute & Color-Space Invariants:**
  * Preserve linear color space (`ACEScg` / Linear sRGB) on `Cd` vertex attributes and `EXR` maps—never apply sRGB gamma curves to VAT position/normal textures or `.vf` velocity grids (`Filter Mode = Point` / `Bilinear` with `sRGB = Off` and uncompressed `RGBAHalf`/`RGBAFloat` in Unity).
  * Match Houdini DOP sub-steps and CFL time-step limits ($\Delta t$, GAMES101 Lecture 21) with Unity `Time.fixedDeltaTime` when coupling exported vector fields to runtime colliders.
* **Cross-DCC PBR & Volumetric Shading Parity:**
  * Pack Blender 4.2 Principled BSDF maps (`BaseColor`, `Roughness`, `Metallic`, `Normal`) into Unity 6 HDRP `MaskMap` or URP `MetallicGlossMap` channels so materials match Cycles/EEVEE look-dev.
  * Control transparent billboard overdraw in Unity VFX Graph via **Soft Particles** (depth-buffer fade) and Six-Way Lightmaps rather than unlit alpha stacks.

**Speaker Notes:**
Two subtle bugs ruin Houdini-to-Unity pipelines every semester: color-space gamma corruption and texture compression on data textures. If Unity imports a Houdini Vertex Animation Texture or vector field with sRGB enabled or BC7 block compression active, your mesh vertices will explode into spiky garbage because the GPU is decoding compressed gamma-curved numbers as world-space coordinates. Always disable sRGB and compression on VAT `EXR` maps and `.vf` volumes.

---

## Slide 6: Quantitative Profiling Across the Multi-DCC Pipeline (Phase 3 Rubric)
### Controlled Baselines for Houdini, Blender 4 & Unity 6 Trade-Offs
* **Zero-Tolerance Rule on Unquantified Claims:** Every architectural choice in your pipeline must be evaluated empirically against a controlled baseline under identical camera paths, resolutions, and lighting.
* **Canonical Multi-DCC Benchmark Comparisons:**
  * **VFX / Fluid Replay:** Houdini Alembic cache (`.abc`) vs. Houdini VAT (`EXR` + Shader Graph) vs. Native Unity 6 VFX Graph driven by Houdini `.vf` vector field.
  * **Procedural Scattering / Geometry:** Houdini SOP/OpenUSD instancing vs. Blender 4.2 Geometry Nodes scatter vs. uninstanced static mesh import.
  * **Offline vs. Cloud Batch Render (Pillar 5):** Local Houdini Mantra/Karma or Blender Cycles render vs. AWS Deadline Cloud OpenUSD fleet execution.
* **Required Telemetry Metrics:** Mean and 95th-percentile GPU frame time ($\text{ms/frame}$ via Unity Profiler), draw calls / `SetPass` calls, peak VRAM footprint ($\text{MB}$), and interchange file size ($\text{MB}$).

**Speaker Notes:**
To score full marks on Phase 3 of the rubric, turn your multi-DCC integration into a controlled scientific experiment. For example, export the exact same Houdini simulation both as an Alembic cache and as a Vertex Animation Texture (or `.vf` VFX Graph driver), place both in identical Unity 6 test scenes with a locked camera spline, and measure GPU frame time, VRAM allocation, and disk footprint. That gives you hard empirical proof of why your architecture works.

---

## Slide 7: Visualizing Pipeline Benchmarks & Uncertainty (Healy Ch. 5–6)
### Reporting Multi-DCC Performance with Statistical Rigor
* **Summarize Frame Distributions, Not Single Frames (Healy, Ch. 5):** GPU frame time fluctuates across camera angles and particle lifetimes; plot **mean/median performance with 95% confidence intervals or interquartile ranges** across captured frame sequences rather than cherry-picking peak FPS.
* **Layered & Faceted Comparison Plots (Healy, Ch. 6):**
  * Use **Cleveland dot plots** or **aligned horizontal bar charts** (with zero baselines) to compare `.abc`, VAT, and VFX Graph implementations.
  * Use **small multiples (faceting)** across particle/vertex complexity tiers ($10^4$, $10^5$, $10^6$) and hardware targets using colorblind-safe Viridis/Okabe–Ito palettes.
* **Failure-Boundary Documentation:** Explicitly report precision limits (e.g., 16-bit vs. 32-bit VAT quantization error), UV seam artifacts, or overdraw bottlenecks alongside visual crops.

**Speaker Notes:**
Apply Healy Chapters 5 and 6 directly to your profiling charts in the final report. Frame time across a 300-frame camera flythrough is a distribution, not a single number. Plotting median and 95% interval bounds across particle-count or mesh-resolution facets lets a reader immediately see where Alembic bandwidth saturates or where VFX Graph stays flat at 16.6 milliseconds.

---

## Slide 8: Interim Integration Workshop — Multi-DCC Diagnostic Audit
### 15-Minute Pre-Break Pipeline Verification Checklist
* **1. Double-Axis & Scale Verification:** Confirm assets traversing `Houdini -> Blender 4.2 -> Unity 6` arrive upright (Y-up), at $1.0\times$ scale, with outward normals and intact UVs.
* **2. Data-Texture & Cache Import Verification:** Verify that VAT `EXR` textures have `sRGB = Off` and `Compression = None`, and that `.pcache`, `.vf`, or OpenUSD layers resolve via project-relative paths on a clean Git clone.
* **3. Controlled Baseline Parity:** Confirm your naive baseline and proposed pipeline run on identical scenes, fixed time steps ($\Delta t$), and deterministic seeds.
* **4. Repository & Binary Hygiene:** Ensure monolithic `.hip`/`.blend` working files are accompanied by clean interchange exports (`USD`/`GLB`/`FBX`/`EXR`/`.vf`), large caches obey repo limits, and `@banazir` has repository access.

**Speaker Notes:**
Use the last 15 minutes of today's class—and your team sync prior to Fall Break—to run through these four diagnostic checks on a clean clone of your repository. Catching a broken path, sRGB flag on a VAT texture, or Z-up/Y-up flip today saves days of debugging after break.

---

## Slide 9: Capstone Final Deliverables Blueprint (Phases 4 & 5)
### Aligning Your Technical Report, Repository & Oral Defense
* **1. Final Technical Report (`6–8 pages`, excluding references):**
  * Formatted strictly in the **standard double-column IEEE or ACM conference template**.
  * Required sections: *Abstract, Introduction, Background/Related Work, System Architecture & Multi-DCC Pipeline, Experiments & Results (quantitative benchmarks + visual comparisons), Conclusion*.
* **2. Unified Source & Asset Repository (`GitHub`, shared with `@banazir`):**
  * Comprehensive `README.md` pinning exact Houdini, Blender 4.2 LTS, and Unity 6 LTS versions/packages, reproducible headless export/render scripts, and clean interchange assets.
* **3. Live Oral Presentation & Individual Reflection:**
  * Every team member presents turns covering problem, 5x5x5 taxonomy intersection, controlled baseline comparison, and live/video demo—plus **each member's individual answers** to the 3 Standard Reflection Questions.

**Speaker Notes:**
Keep the final deliverables firmly in view as you plan your post-break sprint. Your report must follow the double-column IEEE or ACM conference template strictly within 6 to 8 pages excluding references, and every team member must answer the three standard reflection questions individually in the presentation slides: (1) the most technically challenging pipeline issue, (2) the neural or traditional graphics concept you learned most about, and (3) your top priority for future work.

---

## Slide 10: Module 11 Wrap-Up & Post-Break Presentation Schedule
### Key Takeaways & Critical Academic Calendar
* **Module 11 Takeaways:**
  * Partition responsibilities cleanly across **Houdini** (procedural simulation & field generation), **Blender 4.2 LTS** (modeling, rigging & look-dev), and **Unity 6 LTS** (real-time runtime & profiling).
  * Bridge simulations deterministically via OpenUSD, VAT, `.pcache`, and `.vf` vector fields while reporting benchmarks with Healy Ch. 5–6 statistical rigor.
* **Academic Calendar & Presentation Windows:**
  * **Fall Break:** Mon 23 Nov – Fri 27 Nov 2026 (**No Class**).
  * **Early Presentations (Extra Credit):** Thu 03 Dec – Sun 06 Dec 2026 (Week 15 through Sunday prior to Dead Week).
  * **Standard Presentations:** Mon 07 Dec – Fri 11 Dec 2026 (**Dead Week / Week 16**).
  * **Finals Week (Week 17):** **NO presentations** scheduled unless pre-approved in writing before the end of Week 16 for OSSA-confirmed accommodations or documented special circumstances.

**Speaker Notes:**
Have a safe and restorative Fall Break next week. Remember not to wait until Dead Week to bake VAT caches, run fluid simulations, configure AWS Deadline Cloud jobs, or collect Unity Profiler benchmarks. Execute your controlled baseline comparisons right after Fall Break, and sign up for an early extra-credit presentation slot in Week 15 if your pipeline is verified. See you after Fall Break.
