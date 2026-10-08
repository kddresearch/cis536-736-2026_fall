# PS 4: Term Project Proposal
**Milestone 1 of 3: Defining the Scope**

**📅 Due:** Sun 11 Oct 2026 | **💯 Points:** 40 (Undroppable) | **👥 Team:** 1–3 Students

> **🚨 CRITICAL SUBMISSION POLICY**
> **Proposals are INDIVIDUAL:** Even if you are forming a team, **EACH** member must submit their own proposal document. Do not rely on a teammate to submit on your behalf. Failure to submit your own PDF will result in a zero for this milestone.

## 1. Overview
For this first milestone, you will define your computer graphics task, identify your base geometry or dataset, and propose a concrete technical methodology aligned with the course's 5x5x5 taxonomy matrix.

## 2. Initiating Your Draft (The 5-Stage AI Kit Procedure)
Use the Generative AI Kit to help structure your proposal. Follow these five stages to systematically break down your project:

1. **Pick a Pillar & Track:** Choose one of the 5 Methodological Pillars (e.g., Procedural Generation, Illumination/Shading) and one of the 5 Category Tracks (e.g., Terrain, Locomotion). The GenAI kit is designed to answer concrete questions about *one* prospective project at a time. If you choose an asset from the Unity Asset Store or Mixamo, be clear whether you are proposing a project track that matches its original intent, or one of your own choosing.
2. **Identify the Asset Basis:** Identify the primary source geometry by polygon count, format (FBX/USD/PLY), and VRAM footprint. If unsure, present an asset concept and your pillar choice to the AI kit. Ask it to analyze the feasibility and identify the principal data loss vectors (e.g., coordinate axis flips or broken normals). 
3. **Specify Toolchain & Sourcing:** Specify the desired pipeline (Unity 6, Blender 4.2, AWS Deadline). Ask the AI to apply the *Asset Sourcing & Execution Guide* (Rubric 06) to identify potential ingestion bottlenecks and categorize your source as Tier 1 (Golden Path) or Tier 2/3.
4. **Vet the Statement & Distribute the Pipeline:** Ask the AI kit to vet your shared Problem Statement. Then, use it to help distribute the CG Pipeline across your team. Outline what each student should write for their Background/Related Work, Methodology, Evaluation Criteria, and Milestones mapping to interim and final deliverables.
5. **Assign Crucial Stages:** The CG pipeline has 5 stages (Asset, Shading, Kinetics, Composition, Render). Some pipeline stages may be trivial compared to others. Identify the proposed overall visual effect and assign each *crucial* CG stage to a distinct team member. For solo projects, focus on automating or using built-ins for at least three of the stages so you can focus deeply on your chosen Pillar.

## 3. Required Document Structure
Your proposal must be a **1 to 1.5-page PDF** containing the following sections exactly as named below. The structure maps directly to the Term Project Taxonomy and Sourcing Guide.

* **Introduction:** A concise problem statement defining the visual effect or graphics task you are trying to solve. *(Note: For teams, this is the ONLY section you may share identically).*
* **Background and Related Work:** Survey techniques, Unity 6 / Blender 4.2 assets, algorithms, SIGGRAPH SOTA, and Generative/ML models relevant to your track.
* **Methodology:** Detail how you will apply your chosen Pillar to the task. Define your baseline (e.g., standard Unity Lit shader) versus your alternative treatment (e.g., custom Shader Graph setup). *Teams must explicitly detail their individual pipeline stage responsibilities here.*
* **Data Sourcing & Assets (The Obtain Stage):** Naming a source is not sufficient. You must explicitly name your Tier 1 or Tier 2 asset source (e.g., Mixamo, gsplat/COLMAP, Poly Haven), identify the target pipeline format (OpenUSD, FBX), provide a VRAM/poly-count estimate, and state your coordinate system plan (Z-up to Y-up).
* **Evaluation Approach:** Define your metrics. How will you measure success against your baseline? State whether these are quantitative (frame rate, render time, polygon count, VRAM usage) or qualitative (photorealism, subjective style match).
* **Milestones:** List 3-6 intermediate technical subtasks with an approximate weekly schedule mapping to the Interim Update and Final Report.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this proposal? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Grading Rubric (40 Points Total)

This milestone is worth 40 points total, split between your Discussion Draft (10 pts) and your Formal PDF Submission (30 pts).

**Part 1: Draft Participation (10 pts)**
* **Draft Submission (10 pts):** Initial draft posted to the Canvas Discussion board, including Pillar, Track, and Asset definition. Required to receive an approved team number.

**Part 2: Formal Proposal PDF (30 pts)**
* **Scope & Alignment (10 pts):** Clearly identifies a Methodological Pillar and Category Track. Problem statement and 3D graphics task are well-defined.
* **Feasibility Check (10 pts):** Target assets and toolchains exist and are explicitly named (e.g., Unity 6 LTS, OpenUSD). VRAM/poly scope is realistic. Baseline is established.
* **Clarity & Format (5 pts):** Follows the exact required section structure. Submitted as a clean PDF.
* **GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.
