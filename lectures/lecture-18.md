# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Toolchains & Interchange | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 18:** Term Project Proposal Workshop + Lab 3b Checkpoint: Export Pipeline | Wed, Oct 07, 2026 |

---

## Slide 1: Term Project Scoping & Requirements
### CIS 536/736 - Lecture 18
* **The Capstone Objective:** Synthesizing core mathematical foundations with modern production toolchains (Unity 6 LTS, Blender 4.2 LTS, OpenUSD) into a demonstrable graphics system.
* **The 5 Methodological Pillars (The How):** Procedural Assets, Illumination/Shading, Data-Driven Rigging, Generative Geometry, or Cloud Infrastructure.
* **The 5 Category Tracks (The What):** Interactive Environments, VFX/Particles, Locomotion, Information Visualization, or Physically Based Materials.
* **Deliverable Sequence:** Project Proposal (PDF), Interim Update, Presentation Slides, Final Technical Report, Code/Asset Manifest, and the audited `GenAI.pdf` log.

**Speaker Notes:**
Welcome to Lecture 18. Today is our formal Term Project Proposal Workshop. In this capstone, you will move beyond isolated weekly exercises into architecting a cohesive, defensible computer graphics pipeline. The term project requires selecting exactly one Methodological Pillar—which defines *how* you compute your effect—and applying it to one Category Track—which defines *what* visual problem you solve. Notice the final requirement: your Generative AI audit log. You are encouraged to leverage LLMs as cognitive prostheses for debugging Shader Graph math or USD schema validation, but every prompt and output must be logged under our course's strict anti-spooning rules.

---

## Slide 2: The 5x5x5 Selection Matrix
### Structuring Scope & Accountability
* **Rules of Engagement:** Select 1 Category Track, anchor in 1 Methodological Pillar, and map your work across the 5 CG Pipeline Stages (Asset, Shading, Kinetics, Composition, Render).
* **Division of Effort:** Teams (1–3 members) claim 1–2 specific pipeline stages per person; unclaimed stages must be automated or fulfilled using standard primitives/built-ins.
* **CIS 736 Graduate Standard:** Graduate students must complete at least 1/3 more work than undergraduate teams, build capabilities *on top* of engine defaults rather than reimplementing them, and formally evaluate a baseline versus an alternative treatment.
* **Asset Gate:** Proposals must present concrete feasibility evidence for Asset Creation (Stage 1) and Scene Composition (Stage 4) before shaders or code are approved.

**Speaker Notes:**
Let us examine the architecture of the 5x5x5 selection matrix. The most common point of failure in student graphics projects is unbounded scope—such as trying to hand-sculpt a photorealistic human, rig it from scratch, paint weight maps, write a fur shader, and build an open world all at once. That is a tarpit. If your focus is Motion Matching (Pillar 3), download a clean Mixamo rig and invest your engineering cycles entirely in kinematic pose databases and trajectory search. If you are enrolled in CIS 736, you cannot simply drop in an existing Unity asset and call it a project; you must define an explicit baseline (for example, Unity's built-in Lit shader) and evaluate your alternative custom implementation against it using measurable metrics like frame time or peak VRAM.

---

## Slide 3: Track 1: Interactive Environments & Terrain
### Pillar Implementations across Pipeline Stages
* **Pillar 1 (Procedural Generation):** Multi-octave noise displacement in Blender 4.2 Geometry Nodes; altitude-driven slope erosion; OpenUSD variant export into Unity 6.
* **Pillar 2 (Illumination & Shading):** Multi-pass or triplanar terrain shader in Shader Graph; dynamic time-of-day lighting; real-time raytraced directional shadows.
* **Pillar 3 (Data-Driven Rigging):** Terrain-adaptive character foot placement using Unity Two-Bone IK constraints dynamically raycasting ground normal offsets.
* **Pillar 4 (Generative & Reconstruction):** Real-world terrain capture reconstructed via COLMAP pose estimation and 3D Gaussian Splatting (`gsplat`).
* **Pillar 5 (Cloud Infrastructure):** Tiled OpenUSD terrain stages composed via payload layering; batch frame generation orchestrated via AWS Deadline Cloud.

**Speaker Notes:**
Slide 3 demonstrates how the 5 Methodological Pillars map specifically to Track 1: Interactive Environments and Terrain. If you choose Pillar 1, your project lives in Blender Geometry Nodes, using procedural fields to calculate hydraulic erosion and scattering foliage, then serializing the stage to OpenUSD. If you choose Pillar 2, the geometry can be a simple digital elevation model, but your technical focus shifts to Shader Graph: solving texture stretching on steep cliffs using triplanar projection and evaluating real-time raytraced shadows in Unity 6. For Pillar 4, you might take drone footage, run camera alignment through COLMAP, train a 3D Gaussian Splat model, and import the radiance field into Unity. Notice how the core task remains 'terrain,' but the engineering discipline shifts completely depending on your chosen pillar.

---

## Slide 4: Track 2: VFX & Particle Systems
### Pillar Implementations across Pipeline Stages
* **Pillar 1 (Procedural Generation):** Parameterized mathematical emitters (e.g., logarithmic spirals, Lorenz attractors) feeding particle spawn positions via Geometry Nodes.
* **Pillar 2 (Illumination & Shading):** Custom volumetric scattering shaders; screen-space refraction for shockwaves; emissive HDR bloom optimization in Unity 6.
* **Pillar 3 (Data-Driven Rigging):** Animation-coupled VFX; attaching particle sources to skeletal sockets with kinematic velocity inheritance and collision callbacks.
* **Pillar 4 (Generative & Reconstruction):** Text-to-3D point clouds (Point-E) converted into structured particle guide meshes or volumetric density grids.
* **Pillar 5 (Cloud Infrastructure):** High-density cached fluid/smoke simulations serialized to OpenUSD and rendered across multi-node fleets using AWS Deadline Cloud.

**Speaker Notes:**
Track 2 centers on VFX and dynamic simulations. Under Pillar 1, you can mathematically generate particle coordinate vectors using procedural node patterns. Under Pillar 2, the focus is light interaction: writing Shader Graph nodes that compute chromatic aberration, heat distortion via scene color sampling, or volumetric shadow casting inside dense smoke. Under Pillar 3, you link visual effects directly to character motion—ensuring footsteps emit dust particles whose initial velocity matches the foot bone's instantaneous kinematic vector. CIS 736 students taking this track should watch overdraw: evaluate how your custom shader handles fill-rate limits when hundreds of transparent particle quads overlap on screen.

---

## Slide 5: Track 3: Character & Creature Locomotion
### Pillar Implementations across Pipeline Stages
* **Pillar 1 (Procedural Generation):** Procedural creature generation (e.g., L-System joint generation or parameterized arthropod anatomy) with automated bone placement.
* **Pillar 2 (Illumination & Shading):** Anisotropic hair/fur lighting models; multi-layer subsurface scattering (SSS) for skin tone and ear translucency.
* **Pillar 3 (Data-Driven Rigging):** Motion Matching pose database traversal; Unity 6 Animation Rigging with dynamic IK solvers and blend trees.
* **Pillar 4 (Generative & Reconstruction):** Open-weight mesh generation (Shap-E) retopologized and bound to a humanoid control rig.
* **Pillar 5 (Cloud Infrastructure):** High-volume crowd simulation assembling character variant sets via OpenUSD, rendered across cloud compute nodes.

**Speaker Notes:**
Track 3 focuses on characters and locomotion. If you select Pillar 3, this is the canonical home of Motion Matching and Animation Rigging. Instead of manually blending walking and running clips in an animator graph, you construct an animation pose database and write trajectory prediction queries that pick the next best pose at runtime. Under Pillar 2, your challenge is biological material shading: implementing Kajiya-Kay or Marschner hair models to achieve realistic anisotropic specular highlights, or dual-lobe subsurface scattering to ensure ears and fingers exhibit realistic back-lit translucency. For solo projects here, follow our Golden Path: do not hand-model a character; use standard Mixamo or CMU mocap data to jump straight into the technical implementation.

---

## Slide 6: Track 4: Information Visualization & Data Storytelling
### Pillar Implementations across Pipeline Stages
* **Pillar 1 (Procedural Generation):** Ingesting multidimensional CSV/GeoJSON tables to procedurally construct 3D charts, glyphs, and flow lines via node instancing.
* **Pillar 2 (Illumination & Shading):** Perceptually uniform color mapping (Viridis/Magma); view-aligned data callouts; custom depth-glow shaders for point density.
* **Pillar 3 (Data-Driven Rigging):** Timeline-driven data animation; smooth interpolation across temporal slices; interactive camera rigs tracking data focal points.
* **Pillar 4 (Generative & Reconstruction):** Generative 3D categorical icons anchoring semantic data clusters; neural radiance visualization environments.
* **Pillar 5 (Cloud Infrastructure):** Massive-scale spatio-temporal data playback (100k+ data points) rendered to video sequences via AWS Deadline Cloud.

**Speaker Notes:**
Track 4 addresses 3D Information Visualization. Computer graphics is not just entertainment; it is an analytical instrument. When applying Pillar 1 here, your geometry nodes or C# scripts read external datasets—such as global seismic logs or network graphs—and convert data dimensions into spatial coordinates and parametric primitives. Pillar 2 requires perceptual rigor: you will study color theory and optical perception, ensuring that color palettes like Viridis map luminance monotonically so data values are accurately interpreted without visual distortion. Pillar 3 allows you to rig cameras and animation graphs to scrub through multi-year time-series datasets cleanly.

---

## Slide 7: Track 5: Physically Based Material Modeling
### Pillar Implementations across Pipeline Stages
* **Pillar 1 (Procedural Generation):** High-frequency micro-displacement surfaces; procedural weave and stitch generation using Blender Geometry Nodes.
* **Pillar 2 (Illumination & Shading):** Custom BRDF authoring in Shader Graph; microfacet specular distribution (GGX); energy-conserving Fresnel equations.
* **Pillar 3 (Data-Driven Rigging):** Secondary dynamic bone rigs simulating elastic recoil, cloth draping, and soft-body inertia under kinematic movement.
* **Pillar 4 (Generative & Reconstruction):** Radiance field extraction of real-world materials; photographic normal and roughness map synthesis via diffusion priors.
* **Pillar 5 (Cloud Infrastructure):** High-sample path tracing of refractive glass, participating media, and caustics dispatched to AWS Deadline Cloud queues.

**Speaker Notes:**
Track 5 is dedicated to the physics of materials. If your interest lies in pure rendering equations, this is your track. Under Pillar 2, you move beyond default PBR toggles into the core BRDF mathematics: authoring microfacet specular distribution using GGX models, ensuring strict energy conservation between diffuse and specular lobes, and modeling thin-film iridescence. Under Pillar 1, you can model the exact thread geometry of carbon fiber or fabric weaves in Blender, bake the resulting normal and cavity maps, and test them under HDR environmental probes. For Pillar 5, materials with complex transmission—such as murky liquids or diamond dispersion—require heavy ray budgets that you will dispatch to AWS Deadline Cloud render queues.

---

## Slide 8: Proposal Ideation & The Generative AI Kit
### Grounding Your Architecture & Avoiding Tarpits
* **Strict Procedural Tollbooths:** The Anti-Spooning rule is active. You must present your mathematical formulation or architectural hypothesis *before* querying an LLM for code.
* **Pre-Flight Proposal Audits:** Use the `[SYSTEM INSTRUCTION]` block from our GenAI Kit to prime your AI as an adversarial reviewer before writing your proposal.
* **Systems Engineering Auditor:** Paste the prompt to generate an interoperability matrix evaluating toolchain dependencies (e.g., Blender 4.2 LTS → OpenUSD → Unity 6 LTS).
* **Sourcing Tarpits:** Avoid Tier 3 data/asset sources (e.g., uncleaned raw CAD STEP files, hand-sculpting from zero) that consume technical bandwidth without adding graphics rigor.

**Speaker Notes:**
Slide 8 reviews your proposal workflow and our Generative AI Kit policies. As you write your formal proposal, remember that AI is authorized exclusively for critique, debugging, and synthesis—not for concept generation. We have provided a Systems Engineering Auditor prompt in the syllabus. Use it. If you feed your proposed pipeline into Claude or ChatGPT, it will evaluate your planned data flow and flag issues—like attempting to export Blender attributes that OpenUSD cannot serialize, or importing high-poly meshes that exceed Unity's real-time buffer limits. Pay close attention to our Sourcing Guide: stay on the Tier 1 Golden Path for assets so your time is spent solving graphics mathematics rather than repairing broken non-manifold meshes.

---

## Slide 9: Lab 3b Checkpoint - Export Pipeline Execution
### Geometry Nodes → OpenUSD → Unity 6
* **Lab 3b Core Objective:** Validate the digital content creation (DCC) to real-time engine bridge using Universal Scene Description.
* **Procedural Serialization:** Exporting Blender 4.2 Geometry Nodes geometry to `.usdc` (binary) or `.usda` (ASCII) stages.
* **Attribute Retention:** Preserving vertex colors, custom UV channels, and per-instance transform matrices across the export bridge.
* **Unity 6 USD Ingestion:** Utilizing the official USD Package in Unity to reference the stage while maintaining non-destructive upstream synchronization.

**Speaker Notes:**
Now let us shift into the hands-on component: our Lab 3b checkpoint. Today you must prove that your procedural geometry can cross the DCC-to-engine bridge. You will open your Blender 4.2 project from Lab 3a, take the parameterized Geometry Nodes asset you built, and export it through Blender's Universal Scene Description exporter. In Unity 6, you will import this `.usd` stage. The key operational advantage here is non-destructive referencing: if your terrain density or tree scatter parameters change in Blender, re-exporting the USD file immediately propagates those changes to your Unity scene without requiring you to manually re-link lights or re-apply shaders.

---

## Slide 10: Pipeline Troubleshooting & Data Loss Vectors
### Bridging the DCC-to-Engine Gap
* **Coordinate Basis Transformation:** Resolving the right-handed Z-up convention of Blender/OpenUSD against Unity’s left-handed Y-up runtime world.
* **Realize Instances Requirement:** Un-realized Geometry Node instances export as empty transform locators; apply the `Realize Instances` node before USD export.
* **Material Binding Mapping:** Establishing USD Preview Surface materials in DCC to ensure shader slots bind cleanly to Unity HDRP material graphs.
* **Winding Order & Normals:** Checking face orientations before export to prevent backface culling artifacts and inverted lighting vectors in Unity.

**Speaker Notes:**
To conclude Lecture 18, let us review the four primary failure vectors when bridging Blender and Unity. First is coordinate space: Blender is right-handed Z-up; Unity is left-handed Y-up. Ensure your OpenUSD export settings specify the up-axis correctly so your models do not load rotated 90 degrees on the X-axis. Second: Geometry Nodes instances. If you scatter a forest using 'Instance on Points', Blender stores point transforms, not geometry. Unless you place a 'Realize Instances' node before your output, Unity will import thousands of empty GameObjects with zero polygons. Third: ensure your normals are pointing outward; reversed vertex winding will cause Unity's backface culling to make your surfaces invisible. Take the remainder of the period to complete your Lab 3b export check.
