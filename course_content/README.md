# CIS 536/736: Computer Graphics / Game Engines
**Term:** Fall 2026
**Principal Investigator:** Prof. William H. Hsu
**Repository SSOT:** `kddresearch/education/cis536`

---

## Lecture Schedule (Final, 43 Lectures)
*Note: All assigned readings are open-access/non-paywalled per KDD Lab invariants.*

| Week | Date | Lecture # | Topic | Assigned Reading |
|---|---|---|---|---|
| 1 | Mon 24 Aug 2026 | 0 | Module 0 – Welcome & Course Overview | Syllabus; toolchain map handout |
| 1 | Wed 26 Aug 2026 | 1 | Module 1 – Lab 0: Toolchain Bring-Up (Unity 6 LTS / Blender 4.2 LTS / OpenUSD) & Transform Pipeline Hands-On | Unity 6 Manual: Installation & Project Setup; Blender 4.2 Manual: Getting Started |
| 1 | Fri 28 Aug 2026 | 2 | Module 1 – Math Foundations I: Linear Algebra & Transformation Matrices | GAMES101 Lecture 2: Linear Algebra Review |
| 2 | Mon 31 Aug 2026 | 3 | Module 1 – Math Foundations II: Coordinate Systems & Homogeneous Coordinates | GAMES101 Lecture 3: Transformation |
| 2 | Wed 02 Sep 2026 | 4 | Module 1 – Math Foundations III: Rasterization vs. Ray Tracing — Framing the Semester | GAMES101 Lecture 1 (Overview) & Lecture 14 (intro framing) |
| 2 | Fri 04 Sep 2026 | 5 | Module 2 – Viewing & Projections I: Camera Models, Perspective/Orthographic | GAMES101 Lecture 4: Transformation Cont. |
| 3 | Mon 07 Sep 2026 | — | **NO CLASS — Labor Day** | — |
| 3 | Wed 09 Sep 2026 | 6 | Module 2 – Viewing & Projections II: Clipping & Culling | GAMES101 Lecture 5: Rasterization 1 |
| 3 | Fri 11 Sep 2026 | 7 | Module 2 – OpenUSD Scene Graphs: Stage/Prim/Layer Composition; Lab 1b: Scene Assembly Intro | OpenUSD.org: "USD Tutorials — Traversing a Stage / Layering & Referencing" |
| 4 | Mon 14 Sep 2026 | 8 | Module 3 – Illumination & Shading Fundamentals: BRDF, PBR Material Model | pbr-book.org: Reflection Models chapter |
| 4 | Wed 16 Sep 2026 | 9 | Module 3 – Real-Time Ray Tracing & Global Illumination (Unity 6) | Unity 6 Manual: Ray Tracing & Global Illumination |
| 4 | Fri 18 Sep 2026 | 10 | Module 3 – Lab 2a: PBR Material Authoring & RT/GI Setup | Unity 6 Manual: HDRP Material Reference (PBR) |
| 5 | Mon 21 Sep 2026 | 11 | Module 4 – Shader Graph Fundamentals (Unity 6) | Unity 6 Manual: Shader Graph — Getting Started |
| 5 | Wed 23 Sep 2026 | 12 | Module 4 – Procedural Textures & Mapping: UV, Triplanar, Noise-Driven | GAMES101 Lecture 9: Texture Mapping |
| 5 | Fri 25 Sep 2026 | 13 | Module 4 – Lab 2b: Shader Graph Procedural Material Build | Unity 6 Manual: Shader Graph Node Reference |
| 6 | Mon 28 Sep 2026 | 14 | Module 5 – Geometry Nodes Fundamentals: Attributes, Fields, Instancing | Blender 4.2 Manual: Geometry Nodes — Attributes & Fields |
| 6 | Wed 30 Sep 2026 | 15 | Module 5 – Geometry Nodes Patterns: Scatter, Distribution, Parametric Control | Blender 4.2 Manual: Geometry Nodes — Instances & Distribute Points |
| 6 | Fri 02 Oct 2026 | 16 | Module 5 – Lab 3a: Geometry Nodes Procedural Asset (seed/density-parameterized) | Blender 4.2 Manual: Geometry Nodes Modifier |
| 7 | Mon 05 Oct 2026 | 17 | Module 6 – OpenUSD Interchange for Procedural Assets: Variant Sets, Layer Referencing | OpenUSD.org: "USD Tutorials — Variants" |
| 7 | Wed 07 Oct 2026 | 18 | Module 6 – Term Project Proposal Workshop + Lab 3b Checkpoint: Export Pipeline (Geometry Nodes → OpenUSD → Unity 6) | Blender 4.2 Manual: USD Export; Unity 6 Manual: USD Import |
| 7 | Fri 09 Oct 2026 | — | **NO CLASS — Wildcat Pause Day** | — |
| 8 | Mon 12 Oct 2026 | 19 | Module 6 – Rigging Fundamentals: Skeletons, Skinning, Control Rig (Blender) | Blender 4.2 Manual: Rigging — Armatures & Control Rig |
| 8 | Wed 14 Oct 2026 | 20 | Module 6 – Unity Animation Rigging: Constraints & IK Solvers | Unity 6 Manual: Animation Rigging Package |
| 8 | Fri 16 Oct 2026 | 21 | Module 6 – Lab 4a: Control Rig + Unity Animation Rigging Hands-On | Unity 6 Manual: Animation Rigging — Rig Constraints Reference |
| 9 | Mon 19 Oct 2026 | 22 | Module 7 – Animation Graphs: State Machines, Blend Trees (Unity 6) | Unity 6 Manual: Animator Controllers & Blend Trees |
| 9 | Wed 21 Oct 2026 | 23 | Module 7 – Motion Matching: Data-Driven Animation Selection | Unity 6 Manual: Motion Matching Package Documentation |
| 9 | Fri 23 Oct 2026 | 24 | Module 7 – Lab 4b: Animation Graph + Motion Matching Assignment | Unity 6 Manual: Motion Matching — Trajectory & Pose Search |
| 10 | Mon 26 Oct 2026 | 25 | Module 8 – Generative Geometry Survey: Shap-E, Point-E (open-weight, local) | Shap-E (arXiv:2305.02463); Point-E (arXiv:2212.08751) |
| 10 | Wed 28 Oct 2026 | 26 | Module 8 – 3D Gaussian Splatting Fundamentals: gsplat, COLMAP Pose Estimation | Kerbl et al., "3D Gaussian Splatting for Real-Time Radiance Field Rendering" (open project page); gsplat.studio docs |
| 10 | Fri 30 Oct 2026 | 27 | Module 8 – Lab 5a: Capture & Local gsplat Reconstruction (8GB VRAM floor) | gsplat GitHub README & Quickstart; COLMAP documentation |
| 11 | Mon 02 Nov 2026 | 28 | Module 9 – Cloud Rendering Concepts: Distributed Rendering, Cost/Scale Tradeoffs | AWS Deadline Cloud: "How It Works" (docs) |
| 11 | Wed 04 Nov 2026 | 29 | Module 9 – AWS Deadline Cloud Walkthrough (instructor demo) | AWS Deadline Cloud User Guide: Getting Started |
| 11 | Fri 06 Nov 2026 | 30 | Module 9 – Lab 5b: Capped AWS Educate/Academy Credit Render Task | AWS Deadline Cloud User Guide: Queues, Fleets & Budgets |
| 12 | Mon 09 Nov 2026 | 31 | Module 10 – Color Theory & Perception | Healy, *Data Visualization: A Practical Introduction* (socviz.co), Ch. 1 |
| 12 | Wed 11 Nov 2026 | 32 | Module 10 – Data & Information Visualization I: Encoding Quantities | Healy, *Data Visualization*, Ch. 3–4 |
| 12 | Fri 13 Nov 2026 | 33 | Module 10 – Lab 6a: Visualization Pipeline in Unity 6 | Unity 6 Manual: UI Toolkit & Runtime Charts (or equivalent viz package docs) |
| 13 | Mon 16 Nov 2026 | 34 | Module 11 – Data/Info Visualization II: Objects & Processes | Healy, *Data Visualization*, Ch. 5–6 |
| 13 | Wed 18 Nov 2026 | 35 | Module 11 – Special Topics: VFX & Particle Systems Survey | GAMES101 Lecture 20: Cameras, Lenses and Light Fields (particle/VFX-adjacent survey unit); Unity 6 Manual: VFX Graph |
| 13 | Fri 20 Nov 2026 | 36 | Module 11 – Lab 6b: Term Project Interim Integration Check-In | — (project consult; no new reading) |
| 14 | Mon 23 Nov 2026 | — | **NO CLASS — Fall Break** | — |
| 14 | Wed 25 Nov 2026 | — | **NO CLASS — Fall Break** | — |
| 14 | Fri 27 Nov 2026 | — | **NO CLASS — Fall Break** | — |
| 15 | Mon 30 Nov 2026 | 37 | Module 12 – Cross-Pillar Integration Review (Procedural + Rigging/Animation + Generative + Cloud, via OpenUSD) | Review: OpenUSD.org "Composition" overview (synthesis reading) |
| 15 | Wed 02 Dec 2026 | 38 | Module 12 – GenAI Audit & Term Project Final-Prep Workshop | Course GenAI Policy (Syllabus) |
| 15 | Fri 04 Dec 2026 | 39 | Module 12 – Lab 7: Final Polish Lab (procedural terrain/fractals as capstone technique) | Blender 4.2 Manual: Geometry Nodes — Noise Textures & Terrain |
| 16 | Mon 07 Dec 2026 | 40 | Module 13 – Project Presentations, Day 1 | — |
| 16 | Wed 09 Dec 2026 | 41 | Module 13 – Project Presentations, Day 2 | — |
| 16 | Fri 11 Dec 2026 | 42 | Module 13 – Project Presentations, Day 3 (Merged/Makeup) / Final Review | — |
