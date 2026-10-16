---
document_title: "Feedback Rubric: PS 4 Term Project Proposal (CIS 536/736)"
document_version: "1.0"
document_last_updated_date: "2026-10-15"
status: "ACTIVE"
---

# 🧭 RUBRIC: PS 4 Term Project Proposal (CIS 536/736)

**Scope Guard:** This rubric evaluates whether the student has successfully defined a feasible, reproducible Computer Graphics system architecture explicitly aligned with the course taxonomy and asset sourcing constraints. 

## 📊 Evaluation Criteria & Scoring Weights (45-Point Scale)
*Note: The 40 points map to the formal assignment requirements. 5 points are allocated for the Discussion Board draft.*

### [1] Taxonomy Alignment & Scope Definition (10 Points)
*Does the proposal explicitly anchor to the 5x5 taxonomy matrix?*
* **0-3 pts (Fatal):** The problem statement is vague. No specific Methodological Pillar or Category Track is identified, or the chosen combination contradicts the taxonomy rules.
* **4-7 pts (Standard):** Identifies a Pillar and Track, but the proposed 3D graphics task is loosely defined or bridges multiple pillars without a clear primary focus.
* **8-10 pts (Sovereign):** Clearly and explicitly names one Methodological Pillar and one Category Track. The problem statement defines a concrete visual effect or specific computational graphics task (e.g., "Procedural generation of a forest canopy").

### [2] Data Sourcing & Feasibility (Obtain Stage) (10 Points)
*Are the base geometry and toolchains verified, realistic, and explicitly documented?*
* **0-3 pts (Fatal):** No specific asset is named (e.g., "We will find a model online"). Proposes a Tier 3 Tarpit (e.g., hand-modeling an organic character from scratch) without a detailed feasibility plan.
* **4-7 pts (Standard):** Names an asset, but fails to estimate VRAM/poly-count footprint or lacks a coordinate system conversion plan (Z-up to Y-up).
* **8-10 pts (Sovereign):** Explicitly identifies a Tier 1 or Tier 2 asset source (e.g., Mixamo, gsplat, Poly Haven). States the target pipeline format (OpenUSD, FBX). Provides a realistic VRAM/poly estimate and coordinate conversion plan. Toolchains (e.g., Unity 6 LTS) are explicitly named.

### [3] Methodology & Pipeline Distribution (10 Points)
*Is the methodology mathematically/architecturally sound, and are team responsibilities explicitly partitioned?*
* **0-3 pts (Fatal):** No baseline is established. The methodology reads like a narrative goal rather than a technical pipeline. For teams: responsibilities are vague or shared identical text is used beyond the introduction.
* **4-7 pts (Standard):** Baseline is mentioned but not clearly defined against the alternative treatment. CG pipeline stages (Asset, Shading, Kinetics, Composition, Render) are not fully assigned to specific members.
* **8-10 pts (Sovereign):** Explicitly defines the baseline (e.g., standard Unity Lit shader) versus the alternative treatment. For teams: explicit partitioning of the 5 CG pipeline stages to specific members. For solos: clear identification of which 3 stages will be automated/mocked.

### [4] Evaluation Approach & Milestones (5 Points)
*Are the metrics empirical, and is the schedule realistic?*
* **0-2 pts (Fatal):** Relies solely on subjective evaluation (e.g., "looks better"). Milestones are absent or unrealistic.
* **3-5 pts (Sovereign):** Defines concrete quantitative metrics (e.g., frame rate, render time, draw calls, VRAM usage) or specific qualitative bounds. Lists 3-6 intermediate technical subtasks mapping to the Interim and Final deliverables.

### [5] GenAI Audit (5 Points)
*Is the GenAI telemetry verifiable?*
* **0 pts (Fatal):** Neither a `GenAI.pdf` log nor an explicit "No GenAI Used" statement is included.
* **5 pts (Sovereign):** Includes the required `GenAI.pdf` log detailing specific execution prompts (e.g., Systems Engineering Audit) OR explicitly states "No GenAI Used."

### [6] Draft Participation (Discussion Board) (5 Points)
*Did the student complete the preliminary alignment gate?*
* **0 pts (Fatal):** No draft posted to the Canvas Discussion board.
* **5 pts (Sovereign):** Initial draft posted to the Discussion board prior to final PDF submission, securing an approved team number.

---

## 🤖 [SYSTEM INSTRUCTION: CLAUDE CRITICAL REVIEWER]

**Role:** You are evaluating a student's PS 4 Term Project Proposal for CIS 536/736.

**Task:** Evaluate the attached proposal PDF strictly against the `Feedback Rubric: PS 4 Term Project Proposal` provided above.

**Execution Directives:**
1. Do not hallucinate or extrapolate intent. Score the proposal based solely on the text provided.
2. For each of the six rubric criteria, provide a score (e.g., 8/10) followed by a sterile, bulleted list of evidence quoted directly from the manuscript that justifies the score.
3. Identify any "Fatal Flaws" (e.g., proposing a Tier 3 asset without a fallback, failing to assign individual pipeline stages, or lacking a quantitative baseline) that threaten the project's feasibility.
4. If the student claims a specific GenAI prompt was used, cross-reference it with the approved prompts in `generative_ai_kit_2.md`.
5. Conclude with a Total Score out of 45 and a definitive recommendation (Approved, Approved with Revisions, or Rejected/Rewrite).

Do not output performative praise. Deliver the evaluation using mechanical candor.
