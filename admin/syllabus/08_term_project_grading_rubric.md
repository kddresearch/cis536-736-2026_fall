---
yaml_schema_version: "2.1"
document_version: "1.0"
document_last_updated_date: "2026-11-20"
document_monotonicity: "REQUIRED"
---

# Term Project Grading Rubrics
**CIS 536 / CIS 736 Point Distributions, Milestone Weights & Final Substance Criteria**

## 🎯 Capstone Architecture & Final Deliverable Breakdown
The CIS 536/736 Term Project is evaluated via a dual-grading structure worth **390 points total**: **Process Milestones (90 points: Sprint 1 Proposal & Asset Manifest [30 pts], Sprint 2 Interim Pipeline Report [30 pts], and Final Submission Milestone [30 pts])** and **Substance Evaluation (300 points on the course capstone scale; scored via the 200-point Final Submission portal rubric + 50-point live oral presentation/defense)**.

* **Teaming Governance:** Phases 1 & 2 (Sprint 1 and Sprint 2) are submitted **individually** by every team member (max 3 per team) with distinct individual Pipeline Architecture, Milestone, and Execution sections. Phases 3, 4, & 5 are submitted as **unified team deliverables** (one shared GitHub repository URL accessible to `@banazir`, one unified report, and one unified live presentation).

| Capstone Deliverable Component | Final Portal Rubric (200 pts) | 300-Pt Substance Scale | Share of Substance |
| :--- | ---: | ---: | ---: |
| **1. Final Technical Report (`REPORT.pdf`, 6–8 pp IEEE/ACM)**<br>*Double-column IEEE/ACM format (excl. refs), 5×5×5 taxonomy alignment, 5-stage pipeline exposition, controlled quantitative benchmarks, visual comparisons, and limitations.* | 100 pts | 150 pts | **50.0%** |
| **2. Unified Code, Asset Repository & GenAI Audit (`GitHub`)**<br>*Unified repo shared with `@banazir`, `README.md`, pinned Unity 6 / Blender 4.2 manifests, headless render scripts, clean OpenUSD/GLB/FBX/PLY interchange (no monolithic `.blend` bloat), and `GenAI` audit.* | 100 pts<br>*(95 Code/Assets + 5 GenAI)* | 100 pts | **33.3%** |
| **3. Live Oral Presentation, Demo & Reflection Defense**<br>*Unified team delivery (all members present), 5×5×5 overview, live/video walkthrough of baseline comparison, and individual answers to all 3 Standard Reflection Questions.* | Evaluated Live<br>*(Week 15 EC / Week 16)* | 50 pts | **16.7%** |
| **Total Substance Weight (plus 90 pts Process Milestones = 390 pts):** | **200 pts** | **300 pts** | **100.0%** |

---

## 📊 Track Expectations: CIS 536 (Undergraduate) vs. CIS 736 (Graduate)
While both sections share the same 390-point capstone structure and 5×5×5 taxonomy (1 Methodological Pillar × 1 Category Track × 5 CG Pipeline Stages), **CIS 736 (Graduate)** enforces elevated multi-stage baseline engineering, empirical rigor, and pipeline automation:

| Dimension / Milestone | CIS 536: Undergraduate Track | CIS 736: Graduate Track |
| :--- | :--- | :--- |
| **Asset Source Tier (Phase 1 / 2)** | 🟢 **Tier 1 (Golden Path)** strongly preferred (Mixamo, Unity Asset Store, Poly Haven, Blender math meshes, NOAA/USGS) | 🟢 **Tier 1** or 🟡 **Tier 2 (Generative / Reconstruction)** (COLMAP/`gsplat` on 8 GB+ VRAM floor, Shap-E/Point-E, Meshroom) with explicit VRAM & topology-repair plan |
| **Pipeline Stage Depth (5×5×5 Matrix)** | Execute **1–2 claimed stages** natively per student; automate/mock unclaimed stages with Tier 1 assets or Unity 6 built-ins | Must execute **custom baselines and implementation across multiple CG Pipeline Stages** (per `term_project_taxonomy.md`) |
| **Empirical Profiling & Baselines (Phase 3)** | Controlled baseline comparison on primary quantitative metric (FPS/frame time, draw calls, VRAM, or render time) across controlled test scenes | **Multi-stage / multi-parameter ablation or trade-off sweep** (e.g., resolution/particle/fleet scaling) with distribution statistics (mean/median + 95% CIs or percentiles) and explicit failure-boundary analysis |
| **Interchange & Headless Automation (Phase 2 / 5)** | Reproducible `Blender 4.2 (Z-up) -> Unity 6 (Y-up)` round-trip via OpenUSD / glTFast / FBX + documented `README.md` & manifests | Full **headless export/render automation** (parametric OpenUSD VariantSets, scripted Unity Profiler capture, or AWS Deadline Cloud vs. local Cycles cost/time scaling) |
| **Process Milestones (Sprint 1, Sprint 2, Final)** | **90 pts** ($3 \times 30$ pts) | **90 pts** ($3 \times 30$ pts) |
| **Final Deliverable Substance** | **300 pts** | **300 pts** |
| **Total Capstone Points** | **390 pts** | **390 pts** |

---

## 🔬 Detailed Final Deliverable Substance Rubric (300-Point Scale / 200-Point Portal)

### Part 1: Final Technical Report (`REPORT.pdf` — 150 Pts Substance / 100 Pts Portal)

| Criterion | Substance Weight | Portal Weight | Assessment Standard |
| :--- | :---: | :---: | :--- |
| **Format, Completeness & 5×5×5 Alignment** | **35 pts** | **25 pts** | Strictly formatted in the **double-column IEEE or ACM conference proceedings template**, **6–8 pages** (excluding references), containing all six mandatory sections (*Abstract, Introduction, Background/Related Work, System Architecture & Pipeline, Experiments & Results, Conclusion*). Explicitly defines the team's Category Track × Methodological Pillar × Stage mapping and documents exact Asset Evidence (identifiers, poly/texture/VRAM budget, Z-up $\rightarrow$ Y-up conversion plan). |
| **System Architecture & 5-Stage Pipeline Rigor** | **40 pts** | **25 pts** | Clear mathematical and engineering exposition of the end-to-end 5-stage spine (`Asset -> Shading -> Kinetics -> Composition -> Render`) validated across Blender 4.2 LTS, Unity 6 LTS, and/or Houdini/AWS Deadline. Accurately details claimed-stage algorithms (Geometry Nodes, PBR/Shader Graph/RT-GI, Animation Rigging/Motion Matching, `gsplat`/COLMAP, VFX Graph, or OpenUSD composition) and cleanly scopes automated/mocked stages. (`CIS 736`: custom baselines across multiple stages). |
| **Empirical Profiling, Baselines & Visual Proof** | **55 pts** | **35 pts** | Evaluates the proposed system against a **concrete naive baseline on controlled test scenes** (identical camera paths, lighting, and resolution) using quantitative metrics (frame time/FPS, draw calls, GPU VRAM, shader overhead, pose mismatch error, or AWS Deadline vs. local Cycles render wall-clock time/cost). Includes side-by-side visual comparisons and perceptually honest charts (Healy Ch. 5–6). **Heavy automatic penalty** for unquantified qualitative assertions (*"Scene B looks better than Scene A"*). |
| **Limitations, Trade-Offs & Future Work** | **20 pts** | **15 pts** | Candid analysis of visual and computational failure modes (overdraw cliffs, VRAM saturation, floaters/non-manifold geometry, UV/axis seams, solver stiffness, or cloud-cost bottlenecks), honest assessment of Tier 1/2/3 sourcing trade-offs, and grounded technical future work. |

### Part 2: Unified Code, Asset Repository & GenAI Audit (`GitHub` — 100 Pts Substance / 100 Pts Portal)

| Criterion | Weight | Assessment Standard |
| :--- | :---: | :--- |
| **Reproducibility, Environment & Headless Scripts** | **35 pts** | Hosted in a **single unified team GitHub repository shared with `@banazir`** (same URL submitted by all team members). Includes a comprehensive `README.md`, pinned environment/dependencies manifest (Unity 6 `Packages/manifest.json`, Blender 4.2 add-on versions, Python/COLMAP/Houdini versions), and reproducible headless export/render/profiling scripts that rebuild the baseline and proposed outputs from a clean clone. |
| **Interchange Hygiene & Coordinate Integrity** | **30 pts** | Clean, inspectable interchange assets (`OpenUSD` `.usda`/`.usdc`, `GLB`/`glTFast`, `FBX`, `PLY`, `.pcache`/`.vf`, linear `EXR`) with verified Z-up $\rightarrow$ Y-up transforms, outward normals, and intact UVs. **Penalty applied** for committing raw monolithic `.blend` binary bloat without evaluated export equivalents or broken external asset paths. |
| **Technical Implementation of Claimed Stages** | **30 pts** | Clean, modular, working implementation of the claimed CG pipeline stages (Blender Geometry Nodes, Unity Shader Graph / HDRP-URP lighting, Animation Rigging / Motion Matching, `gsplat` reconstruction, VFX Graph 17, or AWS Deadline Cloud job bundles) matching the 5×5×5 matrix. (`CIS 736`: verifiable custom baselines across multiple stages). |
| **GenAI Audit & Original-Input Provenance** | **5 pts** | Includes a complete **GenAI Audit** (per `generative_ai_kit_v2.md` and the Course GenAI Policy) explicitly documenting the non-generative original student inputs provided to any LLM used as a synthesizer (written specs, node graphs, asset permalinks, error logs, profiling CSVs), or an explicit attestation that no Generative AI was used. |

### Part 3: Live Oral Presentation, Demo & Reflection Defense (50 Pts Substance — Evaluated Live)

| Criterion | Weight | Assessment Standard |
| :--- | :---: | :--- |
| **Technical Clarity, 5×5×5 Framing & Team Balance** | **15 pts** | Unified team slide deck clearly articulating the problem, 5×5×5 Pillar × Track × Stage intersection, and individual contributions, with **all team members taking balanced turns presenting**. |
| **Live Demo / Rendered Walkthrough & Baseline Proof** | **20 pts** | Working live demonstration or high-resolution rendered video walkthrough clearly exhibiting the 5-stage pipeline and controlled baseline-vs.-proposed performance/visual comparison. |
| **Individual Reflection Questions & Q&A Defense** | **15 pts** | **Every team member individually answers** all three Standard Reflection Questions in the slides: (1) most technically challenging aspect of CG/pipeline engineering, (2) neural or traditional CG concept learned most about, and (3) top priority for future research/independent study—plus accurate technical defense during Q&A. |

---

## 📅 Scheduling Protocols & Policy Invariants

* **Early Presentations (Extra Credit):** Thursday of Week 15 (`Thu 03 Dec 2026`) through the Sunday prior to Dead Week (`Sun 06 Dec 2026`).
* **Standard Presentations:** Dead Week / Week 16 (`Mon 07 Dec – Fri 11 Dec 2026`).
* **Finals Week (Week 17):** **NO presentations** will be scheduled during Finals Week unless pre-approved in writing before the end of Week 16 for OSSA-confirmed accommodations or documented special circumstances.
* **Team-Split Exit Protocol:** If a multi-student team dissolves, the departing branch must fork/branch the Git repository and claim orthogonal CG Pipeline Stages to prevent scope collision.
* **Critical Path Rule:** Phase 3 empirical profiling, lighting bakes, simulations, and AWS Deadline Cloud runs must begin immediately after Sprint 2; unquantified visual claims and missing baseline telemetry cannot be waived during Dead Week.
