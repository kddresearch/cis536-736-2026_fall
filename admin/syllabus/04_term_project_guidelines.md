---
yaml_schema_version: "2.1"
document_version: "1.0"
document_last_updated_date: "2026-10-12"
document_monotonicity: "REQUIRED"
---

# Term Project Guidelines
**Design, Implementation, and Evaluation of a Computer Graphics System**

## 🎯 Architecture & Grading Structure
The Term Project is the capstone of this course. It is evaluated via a dual-grading structure worth a total of **390 points**:

*   **Process Milestones (90 pts):** You earn assignment points for successfully completing the mandatory milestones on time: Sprint 1 Proposal & Asset Manifest (30 pts), Sprint 2 Interim Pipeline Report (30 pts), and Final Submission (30 pts).
*   **Substance Evaluation (300 pts):** Your final deliverables (the report, the code/scene repository, and the presentation) are evaluated holistically against a 300-point technical rubric at the end of the semester.

> **The Canonical Taxonomy:** To ensure academic rigor, every project must be explicitly defined across two dimensions: a Category Track and a Methodological Pillar. Review the approved combinations in the `term_project_taxonomy.md` file and verified assets in `asset_discovery_matrix.md`.

## 👥 Teaming Protocols

**Phase 1 & Phase 2: Individual Process Milestones**
*   Even if forming a team (max 3), **EACH** member must submit their own document for the early milestones (Sprint 1 and Sprint 2) to earn process points.
*   Teammates may share a jointly developed problem statement, introduction, and scene conceptualization. However, your Pipeline Architecture, Milestones, and Execution Results sections **must** distinctly detail your individual role and technical contributions.

**Phases 3, 4, & 5: Unified Team Deliverables**
*   **Presentations:** You will prepare a unified team presentation, with all members taking turns presenting.
*   **Final Submission:** Your team will produce a unified final report and codebase/repository. All members will submit the same URL to a unified GitHub archive shared with the instructor for the final submission.

---

## Phase 1: Project Proposal & Asset Sourcing
*The Sprint 1 proposal milestone establishes your system architecture and asset pipeline scope.*

**💡 Bring Your Own Asset / Domain (BYOA)**
If you are proposing a custom graphics domain or bringing your own dataset/models for a project pivot, your concept must still map strictly to the course framework. Consult the `asset_discovery_matrix.md` and use the **Phase 1 prompts** in the `generative_ai_kit_v2.md` to evaluate your idea. You can feed the AI your BYOA summary and explicitly ask it to verify whether your assets and task fit crisply into the intersection of an existing Category Track (Terrain, VFX, Locomotion, InfoVis, Materials) and Methodological Pillar (Procedural, Illumination, Rigging/Animation, Generative, Cloud Rendering).

---

## Phase 2: Interim Pipeline Report (Sprint 2)
The Interim Report evaluates your process, methodology, and initial asset ingestion pipeline. It is conducted via the Sprint 2 milestone. You are required to demonstrate a working end-to-end 5-stage routing spine (Asset $\rightarrow$ Shading $\rightarrow$ Kinetics $\rightarrow$ Composition $\rightarrow$ Render) validated across Blender 4.2 LTS and Unity 6 (using glTFast or OpenUSD pipelines).

**🤖 Generative AI Usage & Meta-Prompting**
For the Interim Report, generative writing is allowed under one strict condition: the LLM must be acting as a synthesizer of your original thoughts. You must provide the model with non-generative input—such as your own written specifications, shader node graphs, raw asset permalinks, or specific pipeline error logs. You may not simply ask the model to "write an interim report based on my proposal." Your AI Audit must explicitly state what original inputs you provided to guide the generation.

See `generative_ai_kit_v2.md` for specific execution prompts tailored to the Interim Report.

---

## Phase 3: Experimentation, Profiling & Rigorous Evaluation
Once your interim pipeline baseline is established, the core computational and visual work of the project begins. You must empirically demonstrate that your proposed graphics system or pipeline optimization represents a quantifiable improvement (or meaningful performance/visual trade-off) compared to a naive baseline.

> **⚠️ Critical Path Warning**
> **Do NOT wait for the Final Report assignment to open before beginning Phase 3.** Baking lighting, running cloth/destruction simulations, configuring AWS Deadline Cloud rendering nodes, and optimizing ShaderGraphs takes significant compute time. Both your Final Presentation and Final Report rely entirely on the data and renders generated during this phase. Begin your comparative execution runs immediately after submitting Sprint 2.

**Visual Fidelity, Performance Profiling & Rig Verification**
A qualitative assertion that "Scene B looks better than Scene A" will result in a heavy penalty on your final rubric. You must formally define and test your empirical results. 

*   **Performance Metrics:** Formulate quantitative benchmarks for your system (e.g., frame rate / render time per frame at target resolution, draw call count, GPU memory footprint, or shader compilation overhead).
*   **Comparative Analysis:** Evaluate your system against a baseline (e.g., local Cycles render vs. AWS Deadline Cloud distributed render, or procedural geometry generation vs. static mesh loading) using controlled test scenes.

---

## Phase 4: Final Project Presentations
All teams must deliver a unified, live oral presentation of their graphics system, taking turns to present. Your presentation should clearly articulate your problem, your 5x5 taxonomy intersection, the performance/visual evaluation of your pipeline comparison, and a live demonstration or video walkthrough of your rendered results.

**Standard Reflection Questions**
In your presentation slides, **EACH** team member must individually answer the following three standard questions:
1. What aspect of computer graphics or pipeline engineering did you find most technically challenging in completing this term project?
2. What aspect of neural or traditional computer graphics do you think you learned the most about over the course of the project?
3. What is your top priority for future work, if you were to continue this project as a research initiative or independent study?

**📅 Scheduling Protocols**
*   **Early Presentations (Extra Credit):** Thursday of Week 15 through the Sunday prior to Dead Week.
*   **Standard Presentations:** Dead Week (Week 16).
*   **Finals Week (Week 17):** NO presentations will be scheduled during Finals Week unless previously permitted due to an OSSA-confirmed accommodation or special circumstances. All exceptions must be requested and approved in writing before the end of Week 16.

---

## Phase 5: Final Report & Source/Asset Repository
Your unified final deliverables will be submitted via the final assignment portal. These artifacts form the bulk of your 300-point substance grade. 

*   **The Report:** Must be formatted strictly using the standard double-column IEEE or ACM conference proceedings template. The text must be 6 to 8 pages in length (excluding references) and contain standard sections: Abstract, Introduction, Background/Related Work, System Architecture & Pipeline, Experiments & Results (including performance benchmarks and visual comparisons), and Conclusion.
*   **The Code & Assets:** Must be hosted in a unified team repository (e.g., GitHub) accessible to the instructor. All members will submit the same URL to this archive, and the repository **must be shared with the instructor (@banazir on GitHub)**. It must contain a comprehensive `README.md`, an environment/dependencies file (e.g., Unity project manifest or Blender add-on requirements), reproducible headless render scripts, and clean interchange assets (GLB/FBX; do not commit raw monolithic `.blend` binary bloat without evaluated export equivalents).
