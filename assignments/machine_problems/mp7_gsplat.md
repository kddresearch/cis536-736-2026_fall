<!-- ASSIGNMENT-META
id: MP 7
phase: Phase 3: Procedural Generation, Interchange & Character Systems
module: Module 8: Generative Geometry & 3D Gaussian Splatting
released: Sun 25 Oct 2026
due: Tue 03 Nov 2026
prev: ../problem_sets/ps6_rigging_animation_design.md
next: ../homework/ps8_interim_report.md
-->
# MP 7: Generative Geometry & gsplat Reconstruction
**Open-Weight Text/Image-to-3D Generation and Local 3D Gaussian Splatting Reconstruction**

**🗓️ Released:** Sun 25 Oct 2026 | **📅 Due:** Tue 03 Nov 2026 | **💯 Points:** 100 | **👥 Team:** Individual

> **🚨 CRITICAL SUBMISSION POLICY**
> **ℹ️ Open-Weight, Local-Execution Requirement:** Per the course's generative-tooling invariant, both halves of this assignment must run on open-weight models executed locally (Shap-E/Point-E for generation, gsplat for splatting). Proprietary generative APIs are forbidden for this assignment's required deliverable — see the GenAI Policy if you want to explore a proprietary alternative for extra credit instead.

## 1. Overview
Using Shap-E or Point-E (your choice, run locally on your own or lab hardware), generate a 3D asset from either a text prompt or a reference image, then prepare it for the engine. For the second half, capture a real-world object or small scene and reconstruct it as a trained Gaussian Splat using gsplat, with camera poses from COLMAP.

**Required Readings:**
* **Shap-E / Point-E Papers (arXiv):**
  * **arXiv:2305.02463:** Shap-E — generating conditional 3D implicit functions from text/images.
  * **arXiv:2212.08751:** Point-E — a system for generating 3D point clouds from complex prompts.
* **gsplat & COLMAP Documentation:**
  * **gsplat.studio Quickstart:** Local training setup, hardware requirements, and export formats.

## 2. Initiating Your Draft (The AI Kit Procedure)
Use the Generative AI Kit to help structure your submission. Follow these stages to systematically break down your assignment:

1. **Generative Prompting Strategy:** Brainstorm prompts or image inputs for Shap-E/Point-E to ensure high-quality initial candidate outputs.
2. **Critique Formulation:** Use the AI Kit to help structure a comparative evaluation of the quality and limitations of your 2 candidate generative outputs.
3. **OpenUSD Conversion Workflow:** Outline the steps required to retopologize or convert your chosen point cloud/mesh to an OpenUSD-compatible mesh for Unity 6.
4. **Capture Strategy:** Plan your 30–50 multi-view capture angles and lighting conditions for the COLMAP camera pose recovery and gsplat reconstruction.
5. **Reconstruction Analysis:** Draft your write-up on how your capture conditions and hardware constraints affected your trained splat quality.

## 3. Required Document Structure
Your submission must be a **GitHub Classroom repo commit (`cis536_736_hw`) + PDF write-up to Canvas** containing the following elements:

* **Part 1: Generation & Cleanup:** Your chosen generative-geometry asset (OpenUSD-ready) imported into a Unity 6 scene, alongside a brief evaluation critiquing 2 candidate outputs from different prompts/seeds.
* **Part 2: 3D Gaussian Splatting Reconstruction:** Your exported trained scene (`.ply` or `.splat`) generated via gsplat and COLMAP using 30–50 multi-view photos or video frames.
* **Capture/Compute Documentation:** A short write-up noting your capture conditions (hardware, lighting) and how they affected reconstruction quality.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this artifact? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Grading Rubric (100 Points Total)

This assignment is worth 100 points total.

**Part 1: Generative Geometry Pass (45 pts)**
* **Generative Output Quality & Critique (20 pts):** Candidate outputs are genuinely compared and limitations are honestly assessed.
* **OpenUSD/Unity Integration (25 pts):** Generated asset is cleaned up and imports correctly into a Unity 6 / OpenUSD scene.

**Part 2: 3D Gaussian Splatting Reconstruction (50 pts)**
* **Splat Reconstruction Quality (30 pts):** Trained splat is visually coherent and loads correctly in a viewer.
* **Capture/Compute Documentation (20 pts):** Capture conditions and hardware constraints are clearly documented.

**GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.

---
## Assignment Navigation

| ⬅️ Previous | 📝 Index | Next ➡️ |
|:---|:---:|---:|
| [PS 6: Generative AI Exploration](../homework/ps6_rigging_animation_design.md) | [[All Assignments]](../README.md#schedule) | [PS 8: Term Project Interim Report](../homework/ps8_interim_report.md) |

[🏠 Course Home](../../README.md) · [⬆️ Module](../../modules/module_08/README.md) · [⏫ Phase](../../phases/phase_03/README.md)
