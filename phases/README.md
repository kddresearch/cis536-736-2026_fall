<!--
---
yaml_schema_version: "2.2"
document_version: "1.2"
document_last_updated_date: "2026-10-26"
document_monotonicity: "REQUIRED"
routing_clearance: "PUBLIC - STUDENT FACING"
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
 <h1>🎨 CIS 536/736 - Computer Graphics and Animation</h1>
 <p>
 <strong>Semester:</strong> Fall 2026 | <strong>Instructor:</strong> William H. Hsu<br>
 <strong>Status:</strong> ACTIVE | <strong>Canvas LMS:</strong> <a href="https://ksu.instructure.com">Closed SSO Portal</a>
 </p>
</div>

<hr>

<div align="center">
 <h2>📂 Directory: phases/ (Course Progression Index)</h2>
 <p>
 <strong>Status:</strong> ACTIVE | <strong>Maintainer:</strong> William H. Hsu
 </p>
</div>

<hr>

## 📖 Overview

This directory acts as the master index for the high-level course progression. **Phases** are conceptual, chronological wrappers that group multiple weekly modules together based on major pedagogical milestones or methodological pillars (e.g., Foundations & Scene Description, Shading & Procedural Authoring, Interchange, Animation & Generation, Cloud Rendering & Visualization, Integration & Capstone). 

Together the phases cover **Modules 0–13** and **43 lectures (Lecture 0–42)**, running through Fall 2026. The progression runs from welcome, toolchain bring-up, and OpenUSD scene graphs (Phase 1), through PBR lighting, shader graphs, and geometry nodes (Phase 2), OpenUSD interchange, modern rigging, animation graphs, motion matching, and Gaussian splatting (Phase 3), cloud render farms, color, and information visualization (Phase 4), to visualization special topics, cross-pillar integration, GenAI audits, and final project presentations (Phase 5).

> **⚠️ Parent Directory Routing Notice**
> This directory is strictly for structural routing. All actual course content, scripts, and assignment assets must be placed inside the granular `module_XX/` subdirectories located in the `modules/` repository root or within their respective phase subdirectories.

<hr>

## 🗂️ Phase Directory Structure

| Phase Directory | Pedagogical Objective |
| :--- | :--- |
| [`phase_01/`](phase_01/) | **🔵 Phase 1: Foundations & Scene Description:** Modules [00](../modules/module_00), [01](../modules/module_01), [02](../modules/module_02). Welcome & course overview, toolchain bring-up & math foundations, viewing, projections & OpenUSD scene graphs. |
| [`phase_02/`](phase_02/) | **🟢 Phase 2: Shading & Procedural Authoring:** Modules [03](../modules/module_03), [04](../modules/module_04), [05](../modules/module_05). PBR lighting & real-time RT/GI, shader graph & procedural textures, and procedural modeling with geometry nodes. |
| [`phase_03/`](phase_03/) | **🟠 Phase 3: Interchange, Animation & Generation:** Modules [06](../modules/module_06), [07](../modules/module_07), [08](../modules/module_08). OpenUSD interchange & modern rigging, animation graphs & motion matching, and generative geometry & 3D Gaussian splatting. |
| [`phase_04/`](phase_04/) | **🟣 Phase 4: Cloud Rendering & Visualization:** Modules [09](../modules/module_09), [10](../modules/module_10). Cloud render farms (AWS Deadline Cloud), color theory, and data & information visualization I. |
| [`phase_05/`](phase_05/) | **🟤 Phase 5: Integration & Capstone:** Modules [11](../modules/module_11), [12](../modules/module_12), [13](../modules/module_13). Visualization II & special topics, cross-pillar integration & final prep (GenAI audit), and final project presentations. |

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
