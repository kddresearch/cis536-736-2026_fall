<!--
---
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-10-26"
document_monotonicity: "REQUIRED"
routing_clearance: "INTERNAL - RESTRICTED TO COURSE MAINTAINERS"
---
-->

<br>

<div align="center">
 <strong>🏫 Active Course Repositories:</strong> &nbsp;
 🏗️ <a href="https://github.com/kddresearch/course-architecture-base">Architecture Base</a> &nbsp;|&nbsp;
 🤖 CIS 530/730 &nbsp;|&nbsp;
 📊 <a href="https://github.com/kddresearch/cis531-731-2026_fall">CIS 531/731</a> &nbsp;|&nbsp;
 📈 CIS 732 &nbsp;|&nbsp;
 🎨 <a href="https://github.com/kddresearch/cis536-736-2026_fall">CIS 536/736</a> &nbsp;|&nbsp;
 👁️ CIS 798 X &nbsp;|&nbsp;
 🧠 CIS 830
</div>

<div align="center">
 <h1>🎨 CIS 536/736 - Computer Graphics</h1>
 <p>
 <strong>Semester:</strong> Fall 2026 | <strong>Instructor:</strong> William H. Hsu<br>
 <strong>Status:</strong> ACTIVE | <strong>Canvas LMS:</strong> Closed SSO Portal
 </p>
</div>

<hr>

<div align="center">
 <h2>📚 Directory: reading_materials/</h2>
 <h3>Document: README.md (Module-Aligned Index)</h3>
 <p>
 <strong>Status:</strong> ACTIVE | <strong>Maintainer:</strong> William H. Hsu
 </p>
</div>

<hr>

# Assigned Readings & Reference Materials
**Fall 2026 | CIS 536/736: Computer Graphics**

---

### ⚠️ Licensing & Accessibility Notice
This course adheres strictly to a zero-paywall mandate. All theoretical texts ("Why") and practical system documents ("How") listed below are freely accessible online. **However, public availability does not imply open licensing.** Instructors and students must respect the verified licensing terms (e.g., MIT, Tomorrow Open Source Technology, or restricted author-hosted rights) before redistributing or remixing these artifacts.

### Module-Aligned Readings

| Module | Topic | Theoretical “Why” | Practical “How” |
| :---: | :--- | :--- | :--- |
| **0** | Orientation; reproducibility; toolchain mapping | **N/A** | **Syllabus & Toolchain Map Handout** (Instructor-authored; in repo) |
| **1** | Math foundations; transforms; ray tracing framing | **GAMES101 Lectures 2, 3, & 13** — Lingqi Yan. [Read PDFs](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html) <br> *License: Free to read; redistribution unverified.* | **Unity 6 Manual: Installation; Blender 4.2 Manual: Getting Started.** [Unity Docs](https://docs.unity3d.com/) \| [Blender Docs](https://docs.blender.org/) |
| **2** | Viewing & projections; culling; OpenUSD scene graphs | **GAMES101 Lectures 4 & 5 (Rasterization 1).** [Read PDFs](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html) <br> *License: Free to read; redistribution unverified.* | **OpenUSD Tutorials: Traversing a Stage / Referencing Layers.** [Read online](https://openusd.org/release/tut_usd_tutorials.html) |
| **3** | Illumination & shading; BRDF; real-time global illumination | **_Physically Based Rendering_ (4th ed.), Ch. 9: BSDF Representation.** [Read online](https://pbr-book.org/4ed/Reflection_Models) <br> *License: Free to read online.* | **Unity 6 Manual: Ray Tracing & Global Illumination; HDRP Material Reference.** |
| **4** | Shader Graph; procedural textures; noise mapping | **GAMES101 Lecture 8 (Texture Mapping, pp. 36-44).** [Read PDF](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES101_Lecture_08.pdf) <br> *License: Free to read; redistribution unverified.* | **Unity 6 Manual: Shader Graph & Node Reference.** |
| **5** | Geometry Nodes; procedural attributes & instances | **Blender 4.2 Manual: Geometry Nodes (Attributes & Fields).** [Read online](https://docs.blender.org/manual/en/4.2/modeling/geometry_nodes/index.html) <br> *License: GNU GPL / CC-BY-SA.* | **Blender 4.2 Manual: Geometry Nodes Modifier (Instances & Distribute Points).** |
| **6** | Rigging (Blender/Unity); OpenUSD variants & interchange | **OpenUSD Tutorials: Authoring Variants.** [Read online](https://openusd.org/release/tut_usd_tutorials.html) <br> *License: Tomorrow Open Source Technology License 1.0.* | **Blender Rigging (Rigify/Armatures); Unity Animation Rigging Constraints.** |
| **7** | Animation Graphs; Motion Matching | **"Code vs Data Driven Displacement" — The Orange Duck.** [Read online](https://theorangeduck.com/page/code-vs-data-driven-displacement) <br> *License: Free to read; code is MIT.* | **Unity Animator Controllers; JLPM22 Motion Matching Repository.** [Repo](https://github.com/orangeduck/Motion-Matching) |
| **8** | Generative geometry; Shap-E/Point-E; 3D Gaussian Splatting | **"Shap-E" (Jun & Nichol); "3D Gaussian Splatting" (Kerbl et al.).** [arXiv](https://arxiv.org/abs/2305.02463) \| [Inria](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) <br> *License: Open-access preprints.* | **`gsplat.studio` Documentation; COLMAP Documentation.** |
| **9** | Cloud rendering; distributed jobs; AWS Deadline Cloud | **AWS Deadline Cloud: Concepts and Terminology.** [Read online](https://docs.aws.amazon.com/deadline-cloud/latest/userguide/concepts-terminology.html) <br> *License: AWS documentation terms.* | **AWS Deadline Cloud: Define a Queue, Control Costs with a Budget.** |
| **10** | Color theory & perception; visual encoding | **"oRGB: A Practical Opponent Color Space" (Bratkova et al.); GAMES101 Lecture 20.** [oRGB PDF](https://graphics.stanford.edu/~boulos/papers/orgb_sig.pdf) <br> *License: Author-hosted manuscript.* | **_Data Visualization_ (Healy), Ch. 3–4; Unity UI Toolkit Runtime Charts.** [Read online](https://socviz.co/) |
| **11** | Info visualization; VFX graphs; particle simulation | **"Fast Fluid Dynamics Simulation on the GPU" (Harris, GPU Gems Ch. 38).** [Read online](https://developer.nvidia.com/gpugems/gpugems/part-vi-beyond-triangles/chapter-38-fast-fluid-dynamics-simulation-gpu) <br> *License: Publisher-hosted free reading.* | **_Data Visualization_ (Healy), Ch. 5–6; Unity 6 Manual: VFX Graph.** |
| **12** | Cross-pillar integration; procedural terrains; USD composition | **OpenUSD 24.08 Glossary: Composition Arcs.** [Read online](https://openusd.org/24.08/glossary.html) <br> *License: Tomorrow Open Source Technology License 1.0.* | **Blender 4.2 Manual: Noise Texture Node; Course GenAI Policy.** |
| **13** | Capstone presentations; final review | **N/A** | **N/A** |
