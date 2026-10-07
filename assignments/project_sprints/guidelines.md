# CIS 536/736 Term Project: Requirements, Topics, and Deliverables (Fall 2026)

## 1. Requirements and Assignment Milestones
The term project is the capstone of this course and requires you to synthesize the mathematical foundations and toolchains learned this semester. It consists of the following graded components[cite: 20]:

1.  **Proposal Draft:** Class participation — post as a plain text comment to the Canvas discussion[cite: 20].
2.  **Formal Proposal:** Assignment (1-1.5 pages) — submit as a PDF[cite: 20].
3.  **Interim Update:** Class participation — short paragraph documenting baseline renders, pipeline setup, and any changes[cite: 20].
4.  **Presentation Slides:** Assignment — submit as PowerPoint, PDF, impress.js, or Prezi file[cite: 20].
5.  **Final Report:** Assignment (4-6 pages) — submit as a PDF formatted in APA, AAAI, IEEE, or ACM style[cite: 20].
6.  **Code, Assets, and Models:** Assignment — submit a manifest linking to a Git repository containing your Unity 6 / Blender 4.2 / OpenUSD assets (contents must remain available through grading)[cite: 20, 21].
7.  **Generative AI Audit (GenAI.pdf):** Assignment — submit a log of all AI interactions per the *CIS 536/736 GenAI Project Kit*.

---

## 2. The 5 Methodological Pillars
Your project's *methodology* must be built upon at least one of the core Fall 2026 architectural pillars taught in class[cite: 21]:

1.  **Procedural Generation & Assets:** Utilizing Blender 4.2 Geometry Nodes for scatter, distribution, and parametric control[cite: 21].
2.  **Illumination & Shading:** Implementing PBR material authoring, Shader Graph procedural materials, or Real-Time Ray Tracing/Global Illumination in Unity 6[cite: 21].
3.  **Data-Driven Rigging & Animation:** Using Unity 6 Animation Rigging constraints, Animator Controllers, Blend Trees, or Motion Matching[cite: 21].
4.  **Generative Geometry & Reconstruction:** Utilizing open-weight models (Shap-E, Point-E) or 3D Gaussian Splatting (gsplat/COLMAP) for asset creation and radiance field rendering[cite: 21].
5.  **Cloud Rendering & Infrastructure:** Designing a distributed rendering pipeline utilizing OpenUSD scene graphs and AWS Deadline Cloud for scalable output[cite: 21].

---

## 3. The 5 Orthogonal Category Tracks
Your project should select a single primary application track (the *what* you are building) to pair with your chosen pillar (the *how* you are building it). 

1.  **Interactive Environments & Terrain:** Procedural generation of planets, terrain, or built environments with dynamic level of detail[cite: 20].
2.  **VFX & Particle Systems:** Focus on dynamic simulations (fire, smoke, water, weather) using Unity VFX Graph or physically based modeling[cite: 20, 21].
3.  **Character & Creature Locomotion:** Animation graphs, procedural locomotion, or Unity Reinforcement Learning (RL) applied to generalized cylinder or low-polygon meshes[cite: 20].
4.  **Information Visualization & Data Storytelling:** Visualizing data objects and processes, focusing on color theory, encoding quantities, and visual storytelling pipelines in Unity 6 UI Toolkit/Charts[cite: 20, 21].
5.  **Physically Based Material Modeling:** Emphasizing specific aspects of realism: subsurface scattering, elasticity, fabric, liquids, heft, bounciness, and viscosity[cite: 20].

### Specific Recommendations (Do's and Don'ts)
*   **For Everyone:** Some original code/node-logic is required. Generate procedural meshes, shaders, effects, or animations rather than merely downloading pre-made assets[cite: 20].
*   **For Solo Projects:** Do not try to model, rig, and animate a character entirely from scratch. Use pre-rigged models, procedural animation, or Unity RL[cite: 20]. Keep animation playing times bounded (5-15 seconds for non-procedural; 15-60 seconds for procedural)[cite: 20].
*   **For Teams:** We recommend modeling a shared OpenUSD scene and having each member implement a specific lighting, shading, or VFX effect[cite: 20, 21].
*   **CIS 736 (Graduate Level):** 736 students must complete at least 1/3 more work than 536 students[cite: 20]. You must not simply reimplement what is built into Unity 6; you must build features *on top* of it[cite: 20]. If your task has published SOTA (State of the Art), you must define at least one baseline (e.g., standard Unity lighting) and one alternative treatment (e.g., custom multipass shader)[cite: 20].

---

## 4. Proposals & Rubrics

### Proposal Structure (1-1.5 pages)
Your proposal must contain the following sections[cite: 20]:
*   **Introduction:** A concise problem statement defining the visual effect/graphics task[cite: 20].
*   **Background and Related Work:** Survey techniques, Unity 6 / Blender 4.2 assets, algorithms, SIGGRAPH SOTA, and ML/Generative models[cite: 20].
*   **Methodology:** Detail how you will apply your chosen pillar to the task. Define your baseline vs. your alternative treatment[cite: 20].
*   **Evaluation Approach:** Define your metrics. These may be quantitative (frame rate, render time, polygon count, VRAM usage) or qualitative (photorealism, style match)[cite: 20].
*   **Milestones:** List 3-6 intermediate technical subtasks with an approximate weekly schedule[cite: 20].

### Grading Rubric Overview
*   **Proposal:** 4 points each for the 5 required sections[cite: 20].
*   **Interim Update:** Class participation plus discretionary points for progress and substance[cite: 20].
*   **Presentation:** Slide prep (20%), delivery/clarity (20%), live CG demo effectiveness (20%), answers to standard questions (20%), discussion/thought-provoking points (20%)[cite: 20].
*   **Overall Deliverable:** Originality (20%), functional demo (20%), completeness of proposed features (20%), evaluation results (20%), writeup quality (20%)[cite: 20].

---

## 5. Frequently Asked Questions (FAQ)

**Q: How many students may work on a team?**
A: 1-3 students[cite: 20].

**Q: Are teams allowed to turn in a single assignment as joint work?**
A: Yes, but for the interim and final milestones only. Separate project proposals sharing only the Introduction (problem statement) are required from each member of a team[cite: 20]. Each of the other four proposal paragraphs must be independently written to ensure a reasonable division of accountability[cite: 20]. 

**Q: Are 536 and 736 students allowed to be on the same team?**
A: Yes. 736 students are expected to be responsible for any 736-only content in the proposal (e.g., implementing advanced SOTA baselines) and must do at least 1/3 more work[cite: 20].

**Q: What is your policy on ChatGPT and generative AI?**
A: See the official **CIS 536/736 Generative AI Project Kit** in the syllabus. All AI use must be logged, and generative code must be anchored by your own baseline logic[cite: 20].

**Q: Our team wrote our proposal together. How should we split these to form individual proposal documents?**
A: Finalize your shared Introduction problem statement. Then, name specific facets of the project that belong to each team member (e.g., Student A handles Geometry Nodes procedural generation; Student B handles Shader Graph materials)[cite: 20]. Write your Background, Methodology, Evaluation, and Milestones specifically tailored to your individual subsystem[cite: 20].
