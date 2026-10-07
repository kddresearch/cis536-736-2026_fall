# CIS 536/736 Term Project
# Asset Sourcing & Execution Guide

**Course:** Introduction to Computer Graphics
**Audience:** CIS 536 upper-division undergraduate students and CIS 736 graduate students
**Applies to:** Term Project Proposals and all subsequent CG Pipeline work

---

## Choose Assets & Toolchains First

> **Teams must secure and validate their geometry, datasets, or base rigs before committing to a complex shader, motion matching pipeline, or cloud render architecture.**

A sophisticated global illumination setup, Motion Matching controller, or AWS Deadline batch script is not a viable project if the team cannot reliably obtain, import, or generate the required 3D assets in a compatible format.

**Asset Creation (Stage 1)** and **Composition (Stage 4)** failures are the leading cause of CG term-project collapse. Common failures include:

- Downloading high-poly CAD models that crash Unity 6 upon import.
- Selecting raw drone footage for gsplatting without the VRAM required to run COLMAP.
- Attempting to manually rig and paint weights on a custom character when the project focuses on Motion Matching.
- Building assets with Z-up coordinates in Blender and failing to resolve the transform hierarchy in Unity's Y-up OpenUSD importer.
- Designing a multi-pass shader for a specific topology and discovering too late that the downloaded mesh lacks proper UV coordinates.

Your first project milestone is not shader selection. It is a **reproducible, inspectable, and renderable OpenUSD/FBX/Prefab ingestion path**.

---

## Tier List for Asset & Data Sources

Use the following hierarchy when selecting a source. Tier 1 is strongly preferred for most projects.

| Tier | Source class | Recommended use | Main benefit | Main risk |
|---|---|---|---|---|
| 🟢 **Tier 1** | Golden Path | Default choice for most projects | Stable topology, standard UVs, low import burden | Less novelty in raw geometry |
| 🟡 **Tier 2** | Raw capture or Open-Weight Generation | Teams deliberately studying 3D Gaussian Splatting, Shap-E, or real-world photogrammetry | Realistic computer-vision integration | High VRAM needs, messy topology, long compute times |
| 🔴 **Tier 3** | Tarpit | Only with a clear technical justification and instructor approval | Specialized domain (e.g., custom mocap) | High probability that asset cleanup consumes the semester |

### 🟢 Tier 1: Golden Path

Tier 1 sources are natively available, topologically sound, or highly accessible. They allow teams to focus on Shader Graph, Geometry Nodes, Animation Rigging, and AWS Deadline rendering rather than spending the term fixing broken meshes.

| Source | Best taxonomy tracks | Typical projects | Obtain method |
|---|---|---|---|
| Mixamo (Adobe) | Locomotion | Motion matching, inverse kinematics, blend trees, character controllers | Download pre-rigged `.fbx` and map to Unity Humanoid avatar |
| Unity Asset Store (Free Tier) | Interactive Environments | Procedural lighting, global illumination baking, level-of-detail tests | Import via Unity Package Manager; standardize via OpenUSD export |
| Blender Base Meshes & Math Functions | Terrain, VFX, Materials | Procedural terrain generation, particle emitters, PBR testing | Generate mathematically inside Blender; utilize Geometry Nodes |
| Public Datasets (e.g., NOAA, USGS) | InfoVis / Storytelling | Spatial density mapping, 3D bar charts, data-driven mesh deformation | Ingest JSON/CSV via C# script into Unity UI Toolkit/Charts |
| Poly Haven (HDRI / Textures) | Illumination & Shading | PBR material authoring, raytracing reflections, environmental lighting | Download standard 4K EXR/PNG maps for Base Color, Normal, Roughness |

### 🟡 Tier 2: Generative and Reconstruction Pipelines

Tier 2 is appropriate when the generation or reconstruction of the asset *is* the learning objective (Pillar 4). These require intentional VRAM budgeting and topology repair plans.

| Source | Best taxonomy tracks | Obtain requirements | Main feasibility concern |
|---|---|---|---|
| gsplat.studio / COLMAP | Interactive Environments | Local 8GB+ VRAM GPU, structured video capture plan | Poor camera pose estimation, floaters, massive PLY file sizes |
| OpenAI Shap-E / Point-E | Locomotion / VFX | Python environment, Hugging Face Hub access | Low-resolution outputs, disconnected vertices, missing textures |
| Photogrammetry (Meshroom) | Materials / Environments | High-overlap photo set, clean backgrounds | Non-manifold geometry, requiring Blender retopology |

### 🔴 Tier 3: Tarpits

Tier 3 sources are not prohibited. However, they are **not recommended** unless the difficult sourcing work is explicitly part of the project’s educational objective and the team has a detailed feasibility plan.

| Source type | Why it is high risk | Preferred alternative |
|---|---|---|
| Hand-modeled organic characters from scratch | Requires modeling, UV unwrapping, weight painting, and rigging before animation can even begin. Will consume the semester. | Use a Mixamo pre-rigged character or a generalized cylinder primitive |
| Uncleaned CAD data (STEP/IGES files) | Massive polygon counts, inverted normals, detached surfaces; crashes real-time renderers | Use optimized game-ready `.fbx` or `.usd` assets |
| Custom Motion Capture (Mocap) | Requires specialized hardware, massive clean-up of optical jitter, and complex retargeting matrices | Use pre-cleaned BVH data from CMU Graphics Lab or Unity Motion Matching examples |

---

## Required Proposal Evidence

Your project proposal must demonstrate that the **Asset Creation** and **Composition** stages are feasible. Naming an asset is not sufficient. A proposal should enable another student to understand how raw geometry becomes a lit pixel on the screen without relying on undocumented manual tweaking.

### Asset Evidence (Stage 1 & 4)

Include all of the following:

*   [ ] **Exact Asset Identifier:** State the Mixamo rig name, Unity Asset Store package, generative model (e.g., Shap-E), or data source (e.g., USGS).
*   [ ] **Minimal Import Test:** Demonstrate or describe a successful test that brings the asset into Unity 6 or Blender 4.2 without crashing.
*   [ ] **Target Pipeline Format:** Identify the core interchange format (e.g., OpenUSD, FBX, or PLY).
*   [ ] **Poly Count & VRAM Estimate:** Estimate the polygon count, texture resolution (e.g., 4K vs 1K), and required VRAM to ensure the scene will run on local lab hardware or AWS Deadline.
*   [ ] **Coordinate System Plan:** Explicitly state how you will handle Z-up (Blender/OpenUSD) to Y-up (Unity) conversions.

### Minimum Pipeline Plans by Track

| Category Track | Minimum viable pipeline work |
|---|---|
| **Terrain / Environments** | Generate mesh via GeoNodes or heightmap, import to Unity, apply basic PBR material, light with directional light and HDRI. |
| **Locomotion** | Import pre-rigged FBX, map to Unity Humanoid, set up basic Animator Controller or Rig Constraint. |
| **VFX / Materials** | Build basic node graph (Shader or VFX), apply to primitive mesh, test under different lighting conditions. |
| **InfoVis** | Parse raw dataset via C#, instantiate 3D objects based on array length, map data values to scale/color. |
| **Generative Reconstruction** | Capture video, extract frames, run COLMAP for camera poses, train basic Gaussian splat, view in web or Unity renderer. |

---

## Golden Path Architectures

The following architectures are recommended because they clearly separate geometry generation, shading, rigging, assembly, and rendering. Each stage should persist a reproducible OpenUSD or Unity Prefab artifact.

### The Procedural Asset Pipeline (Pillars 1 & 5)
**Best for:** Terrain generation, dense forests, cityscapes, and cloud batch rendering.

```text
Blender 4.2 LTS
Geometry Nodes (Scatter, Deform, Generate)
        |
        v
OpenUSD Export (.usda / .usdc)
Retain parametric variants if applicable
        |
        v
Unity 6 LTS (Scene Assembly)
USD Importer / Reference Node
        |
        v
Shader Graph / HDRP Lighting
Apply PBR Materials, Global Illumination
        |
        v
AWS Deadline Cloud (or Local Render)
Batch output to EXR/PNG sequence
```

### The Motion Matching Pipeline (Pillars 3 & 2)
**Best for:** Realistic character movement, crowd simulation, AI navigation.

```text
Mixamo / CMU Mocap DB
Download pre-rigged character (.fbx) & raw animations
        |
        v
Unity 6 LTS
Map to Humanoid Avatar
        |
        v
Animation Rigging Package
Set up IK constraints / Pose Search DB
        |
        v
Motion Matching System
Data-driven animation state machine
        |
        v
Runtime Evaluation
Profile Frame Rate, Pose mismatch errors
```

---

## Final Decision Rule

Choose the simplest asset pipeline that allows your team to demonstrate meaningful mathematical or architectural work in the CG stages you intend to claim. A successful CIS 536/736 project is not the one with the most complex hand-modeled character. It is the project that reliably imports geometry, authors a mathematically sound shader or node graph, assembles the scene via OpenUSD, evaluates render performance against a baseline, and communicates limitations honestly.
