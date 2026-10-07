# CIS 536/736: Term Project Taxonomy (5x5x5 Matrix)

**Rules of Engagement:**
*   **Pillar (The How):** Each team (1-3 students) MUST anchor their project in exactly ONE of the 5 Methodological Pillars taught this semester (Procedural, Illumination, Rigging/Animation, Generative, Cloud Rendering).
*   **Track (The What):** Each team MUST choose a focal application track. If a team splits (e.g., a 3-student team dissolving into a 2 and a 1), the exit strategy requires branching the Git repository and selecting orthogonal CG Pipeline Stages to prevent collision.
*   **Stage (The CG Pipeline):** Tasks correspond to the 3D graphics pipeline. Students may claim 1-2 stages each. Teams must automate/mock the stages they do not explicitly claim using open-source assets or Unity 6 built-ins.

---

## Dimension 1: The 5 Methodological Pillars (The How)
1.  **Procedural Generation & Assets:** Utilizing Blender 4.2 Geometry Nodes for scatter, distribution, and parametric control.
2.  **Illumination & Shading:** Implementing PBR material authoring, Shader Graph procedural materials, or Real-Time Ray Tracing/Global Illumination in Unity 6.
3.  **Data-Driven Rigging & Animation:** Using Unity 6 Animation Rigging constraints, Animator Controllers, Blend Trees, or Motion Matching.
4.  **Generative Geometry & Reconstruction:** Utilizing open-weight models (Shap-E, Point-E) or 3D Gaussian Splatting (gsplat/COLMAP) for asset creation and radiance field rendering.
5.  **Cloud Rendering & Infrastructure:** Designing a distributed rendering pipeline utilizing OpenUSD scene graphs and AWS Deadline Cloud for scalable output.

---

## Dimension 2: The 5 Orthogonal Category Tracks (The What)
1.  **Interactive Environments & Terrain:** Procedural generation of planets, terrain, or built environments with dynamic level of detail.
2.  **VFX & Particle Systems:** Focus on dynamic simulations (fire, smoke, water, weather) using Unity VFX Graph or physically based modeling.
3.  **Character & Creature Locomotion:** Animation graphs, procedural locomotion, or Unity Reinforcement Learning (RL) applied to generalized cylinder or low-polygon meshes.
4.  **Information Visualization & Data Storytelling:** Visualizing data objects and processes, focusing on color theory, encoding quantities, and visual storytelling pipelines in Unity 6 UI Toolkit/Charts.
5.  **Physically Based Material Modeling:** Emphasizing specific aspects of realism: subsurface scattering, elasticity, fabric, liquids, heft, bounciness, and viscosity.

---

## Dimension 3: The 5 CG Pipeline Stages (The Execution)
*Teams execute a subset of these stages natively and automate/download assets for the rest.*

### Stage 1: Asset Creation (Modeling/Generation)
*   **Prototype:** Generating a procedural terrain mesh in Geometry Nodes, capturing a room via COLMAP/gsplat, or downloading a Mixamo character.
*   **Goal:** Establishing the base geometry or point cloud required for the scene.

### Stage 2: Material & Shading (Surfacing)
*   **Prototype:** Building a triplanar procedural rock texture in Unity Shader Graph, or configuring the PBR material properties for subsurface skin scattering.
*   **Goal:** Defining how light interacts with the surfaces of the generated assets.

### Stage 3: Rigging & Kinetics (Movement)
*   **Prototype:** Setting up Unity IK constraints for a mech leg, or authoring a VFX Graph to simulate fluid viscosity.
*   **Goal:** Authoring the math and constraints that govern temporal changes, deformation, or particle simulation over time.

### Stage 4: Composition & Illumination (Scene Assembly)
*   **Prototype:** Assembling assets via OpenUSD referencing, baking global illumination, or setting up volumetric fog and light probes.
*   **Goal:** Bringing isolated assets into a unified spatial coordinate system and lighting the environment.

### Stage 5: Render & Evaluation (Output)
*   **Prototype:** Submitting the USD scene to AWS Deadline Cloud for batch rendering, profiling the frame rate, or calculating VRAM consumption.
*   **Goal:** Generating the final pixels and mathematically evaluating the performance against a defined baseline metric.

---

## The 5x5 Selection Matrix

*(Teams map their project by selecting one Track and one Pillar, then executing the 5 stages. CIS 736 students must execute custom baselines in multiple stages).*

### Table A: Procedural Generation & Assets (Pillar 1)
| Category Track | Asset (S1) | Shading (S2) | Kinetics (S3) | Composition (S4) | Render (S5) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Terrain** | GeoNode Mesh | Altitude Shader | Erosive Sim | USD Assembly | Local Render |
| **2. VFX** | Particle Mesh | Emissive Mat. | VFX Graph Sim | Scene Integration | Frame Rate Eval |
| **3. Locomotion** | Low-Poly Base | Basic PBR | Unity RL Agent | Col. Bounds | Runtime Perf. |
| **4. InfoVis** | Data-driven Mesh | Color Encode | Temporal Morph | UI Toolkit | Readability Eval |
| **5. Materials** | Fluid Volume | Viscosity Shader | Liquid Flow | Lighting Setup | Subsurface Render |

### Table B: Illumination & Shading (Pillar 2)
| Category Track | Asset (S1) | Shading (S2) | Kinetics (S3) | Composition (S4) | Render (S5) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Terrain** | Pre-made Map | Multi-pass Shader | Day/Night Cycle | GI Baking | Raytrace Eval |
| **2. VFX** | Billboard Quad | Refraction Shader | Distortion | Post-Processing | Overdraw Eval |
| **3. Locomotion** | Mixamo Rig | Fur/Hair Shader | Pre-baked Anim | Spotlighting | Shadow Map Eval |
| **4. InfoVis** | 3D Scatter | Semantic PBR | Hover States | Chart Assembly | A/B Clarity Test |
| **5. Materials** | Cloth Mesh | Elastic Shader | Fabric Sim | Studio Lighting | Texture Res Eval |

### Table C: Data-Driven Rigging & Animation (Pillar 3)
| Category Track | Asset (S1) | Shading (S2) | Kinetics (S3) | Composition (S4) | Render (S5) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Terrain** | Walkable Mesh | Standard Mat. | Dynamic Foot IK | NavMesh Setup | Collision Eval |
| **2. VFX** | Trigger Vol. | Alpha Blend | Anim-driven VFX | Scene Graph | Trigger Latency |
| **3. Locomotion** | Custom Rig | Standard Mat. | Motion Match | Blend Trees | Pose Search Eval |
| **4. InfoVis** | Node Graph | Color Scale | Anim Timeline | Camera Rig | Transition Smooth |
| **5. Materials** | Softbody Mesh | Deformation Mat | Bone Weighting | Scene Assembly | Rig Compute Cost |

### Table D: Generative Geometry & Reconstruction (Pillar 4)
| Category Track | Asset (S1) | Shading (S2) | Kinetics (S3) | Composition (S4) | Render (S5) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Terrain** | gsplat Drone | SH Colors | N/A (Static) | COLMAP Align | VRAM Usage Eval |
| **2. VFX** | Shap-E Prompt | Gen-UV Map | Particle Spawn | Volumetric Merge | Mesh Density |
| **3. Locomotion** | Point-E Cloud | Point Color | Mesh Skinning | Rig Assembly | Artifact Eval |
| **4. InfoVis** | Generative Graph| Semantic Tint | Interpolation | Dashboard | Visual Coherence |
| **5. Materials** | Gen-Texture | AI Normal Map | Tiling Logic | Scene Lighting | Resolution Eval |

### Table E: Cloud Rendering & Infrastructure (Pillar 5)
| Category Track | Asset (S1) | Shading (S2) | Kinetics (S3) | Composition (S4) | Render (S5) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Terrain** | Heavy GeoNodes | 4K Textures | Camera Path | USD Layering | AWS Deadline |
| **2. VFX** | High-density Sim | Raytraced Mat. | Cached Physics | USD Referencing | AWS Cost/Time Eval |
| **3. Locomotion** | Crowd Crowd | Variant Sets | Instanced Anim | USD Assembly | Fleet Scaling Eval |
| **4. InfoVis** | Massive Dataset | Batch Colors | Render Queue | USD Output | Distributed Render |
| **5. Materials** | High-Res Mesh | Complex PBR | Subsurface Sim | Scene Setup | Render Time Eval |
