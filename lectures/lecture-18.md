# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Toolchains & Interchange | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 18:** Term Project Proposal Workshop + Lab 3b Checkpoint: Export Pipeline | Wed, Oct 07, 2026 |

---

## Slide 1: Term Project Scoping & Requirements
### CIS 536/736 - Lecture 18
* **The Capstone Execution:** The term project requires synthesizing the mathematical foundations and toolchains (Unity 6, Blender 4.2, OpenUSD) learned this semester.
* **The 5 Methodological Pillars (The How):** Procedural Assets, Illumination/Shading, Data-Driven Rigging, Generative Geometry, or Cloud Infrastructure.
* **The 5 Category Tracks (The What):** Terrain, VFX/Particles, Locomotion, Information Visualization, or Physically Based Materials.
* **The Deliverables:** Formal Proposal (PDF), Interim Update, Presentation Slides, Final Report, Code/Asset Manifest, and the Generative AI Audit Log.

**Speaker Notes:**
Welcome to Lecture 18. Today is a critical pivot point in the semester. We are bridging the gap between the isolated techniques you've learned—like building a procedural asset in Blender—and the architectural requirements of a full computer graphics pipeline. The term project is your capstone. It requires you to select one of the five core methodological pillars we teach, and apply it to one of five application tracks. You are not building an entire AAA game; you are engineering a specific, measurable visual effect or rendering pipeline. Your success depends on defining a tight scope today.

---

## Slide 2: The 5x5x5 Selection Matrix
### Mapping Your CG Pipeline
* **The Rules of Engagement:** 1 Track, 1 Pillar, 5 Stages (Asset, Shading, Kinetics, Composition, Render).
* **Team Division:** Teams (1-3 students) must claim 1-2 stages each. You must automate or use built-ins (e.g., Mixamo rigs, Unity primitives) for the stages you do not explicitly claim.
* **The CIS 736 Requirement:** Graduate students must complete at least 1/3 more work, build features *on top* of Unity built-ins, and define a baseline vs. an alternative treatment.
* **The Artifact Gate:** Your proposal must provide exact evidence for your Asset Creation (Stage 1) and Composition (Stage 4) feasibility.

**Speaker Notes:**
Let's look at the 5x5x5 selection matrix. The most common failure mode in this course is a team biting off more than they can chew. If your project is about Motion Matching (Pillar 3), do not spend six weeks hand-modeling and weight-painting a character from scratch. That's a tarpit. Download a pre-rigged Mixamo character and focus your engineering effort on the animation state machine and IK constraints. If you are a graduate student, you must establish a baseline. If you are building a custom multi-pass shader, you must first render the scene with Unity's standard lit shader so we can measure the exact performance and visual delta of your custom work.

---

## Slide 3: Proposal Ideation & The Generative AI Kit
### Grounding Your Architecture
* **The GenAI Tollbooths:** No free shaders, no free pipelines. You must provide the mathematical baseline or performance constraint before asking AI for code or architecture.
* **Phase 1 (Scoping):** Use AI for editing, critique, and mathematical debugging. Do not use it to hallucinate a project concept.
* **The Socratic Gatekeeper Prompt:** Force the AI to evaluate your proposal against Mathematical Soundness, Evaluation Rigor (FPS/VRAM), and Toolchain Reproducibility.
* **The Interoperability Audit:** Run your proposed asset pipeline through the Systems Engineering Auditor prompt to catch Z-up/Y-up coordinate mismatches or format deprecations early.

**Speaker Notes:**
We are operating in the era of Generative AI, and you are allowed to use it—but under strict auditing rules defined in your GenAI Project Kit. You cannot ask Claude to "write a realistic water shader." You *can* ask Claude to evaluate your linear algebra formulation for a view-dependent specular highlight. Before you submit your proposal, I highly recommend using the Systems Engineering Auditor prompt provided in the syllabus. Have the AI cross-check your intended asset pipeline. If you plan to generate raw point clouds and import them into Unity, the AI will warn you about VRAM limits and coordinate system mismatches before you waste a week debugging it.

---

## Slide 4: Lab 3b Checkpoint - Export Pipeline Execution
### Geometry Nodes → OpenUSD → Unity 6
* **The Export Validation:** Moving parameterized procedural geometry from Blender 4.2 to Unity 6 LTS.
* **Preserving Attributes:** Ensuring that vertex colors, custom UV maps, and geometry node outputs survive the USD serialization process.
* **The Unity USD Importer:** Configuring the import settings to correctly interpret the USD stage hierarchy and coordinate spaces.
* **Non-Destructive Workflows:** Using USD referencing to ensure that upstream changes in Blender automatically update the Unity scene without breaking applied materials.

**Speaker Notes:**
For the second half of today's class, we are moving into Lab 3b. We are going to physically execute the pipeline we discussed on Monday. You will take the procedural asset you built in Geometry Nodes, serialize it out via the Blender USD exporter, and ingest it into Unity 6. The goal here is to establish a non-destructive pipeline. If you realize your generated terrain needs more noise detail, you shouldn't have to rebuild your entire Unity lighting setup. By referencing the USD stage correctly, you can re-export from Blender and watch the geometry update live in your Unity scene while preserving your materials and composition.

---

## Slide 5: Pipeline Troubleshooting & Data Loss Vectors
### Bridging the DCC-to-Engine Gap
* **The Y-Up / Z-Up Mismatch:** Correcting axis orientation between Blender (Z-up) and Unity (Y-up) during the export/import handshake.
* **Dropped Instances:** Troubleshooting why Geometry Node instances (e.g., scattered trees) might export as empty transforms instead of realized geometry.
* **Material Binding Failures:** Resolving issues where USD material bindings drop, requiring material reassignment in the Unity HDRP environment.
* **Normal Corruption:** Fixing flipped faces or broken smoothing groups caused by non-manifold geometry or export triangulation settings.

**Speaker Notes:**
Finally, we will cover the common tarpits of the DCC-to-Engine bridge. The most notorious is the coordinate system mismatch. Blender is Z-up; Unity is Y-up. If you don't configure your USD export and import settings correctly, your entire scene will load in sideways, and your physics will break. We will also look at 'Realize Instances' in Geometry Nodes. If you scatter a thousand rocks using instances, but forget to realize them before export, Unity will just see a thousand empty points in space. We'll walk through these specific data loss vectors so your baseline pipeline is bulletproof before you begin your term projects.
