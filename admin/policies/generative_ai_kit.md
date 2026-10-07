---
yaml_schema_version: "2.3"
doc_version: "1.0"
updated: "2026-10-07"
versioning_rules: "Strict monotonicity. Reject regressions."
routing_clearance: "GENERAL - STUDENT FACING (CIS 536/736 INFRASTRUCTURE)"
anti_cwa: "ALWAYS ASK, NEVER INFER. Ground every claim in supplied context."
---

# Generative AI Project Kit (CIS 536/736)

## IDENTITY & MISSION
This document outlines the Approved Computer Graphics Workflows and Meta-Prompts for LLM Assistance in CIS 536/736. Generative AI tools (ChatGPT, Gemini, Claude, Perplexity) may be used throughout your term project to assist with Shader Graph math, Geometry Nodes logic, C# Unity scripting, and 3D reconstruction pipelines, provided all interactions are strictly audited via your `GenAI.pdf` file. To foster engineering maturity and prevent "knowledge piñata" exploitation, your use of AI is bound by absolute structural constraints.

## STRICT PROCEDURAL DEPENDENCIES (THE TOLLBOOTHS)

**1. The Anti-Spooning Stick (No Free Shaders/Scripts)**
You are strictly forbidden from asking an AI to write a Unity C# script, Shader, or OpenUSD pipeline from scratch UNLESS you provide your own mathematical or logical baseline first.
*   **DO NOT ASK:** "Write a Unity 6 shader for realistic water" or "Give me a Blender Python script to scatter trees."
*   **DO ASK:** "Here is my linear algebra formulation for calculating the view-dependent specular highlight. Audit my math against the standard PBR BRDF."

**2. The Systems Thinking Carrot (No Free Pipelines)**
Do not chase incremental code fixes without defining the performance impact. You must provide a rendering or computational constraint justification before asking for architectural design.
*   **DO NOT ASK:** "How do I make my scene render faster?"
*   **DO ASK:** "The target metric of my scene is 60fps, but my draw calls are exceeding 2000 due to un-instanced geometry. Based on this, help me design an OpenUSD layer composition or Geometry Nodes instancing architecture to reduce overhead."

---

## PHASE 1: PROJECT PROPOSALS & SCOPING
GenAI may be used for **editing, critique, and mathematical debugging** only. You may not use it to generate your core visual concept or project task from scratch. Before asking an LLM for help evaluating your proposal, copy and paste this block to ground it in the CIS 536/736 taxonomy.

> **[SYSTEM INSTRUCTION]**
> You are the Computer Graphics Design Coordinator for CIS 536/736. Provide CRITIQUES ONLY. Do NOT write the proposal, literature review, or pipeline code for me. [COURSE CONTEXT] Apply adversarial self-critique: identify what technical bottleneck most efficiently refutes my feasibility claim, and what hidden mathematical or pipeline assumptions carry it. Evaluate my proposal against these constraints: (1) Mathematical & Technical Soundness, (2) Evaluation Rigor (Frame rate/Polygons/Visual artifacts), and (3) Toolchain Reproducibility (Unity 6 / Blender 4.2 / OpenUSD / AWS Deadline).

---

## PHASE 2: THE "GRAMMARLY ON STEROIDS" SYNTHESIS
For your Interim Report, generative writing is allowed under the **"Grammarly on Steroids" policy**. The LLM must act strictly as a synthesizer of *your* original thoughts. You must provide the model with non-generative input (your raw bullet points, scene hierarchy, node graphs, or math matrices). **You must include your raw notes in this prompt to receive credit.**

> **[SYSTEM INSTRUCTION]**
> You are an expert technical academic editor. I am providing my original bullet points, mathematical formulas, and architectural choices below. Please synthesize this into a formal, concise academic response for my Interim Report. Do NOT add new claims, hallucinate render techniques, or alter my original technical intent. 
> 
> **[MY ORIGINAL INPUT]** 
> *Section:* [e.g., Methodology / Baselines / Node Graph Architecture] 
> *My Raw Notes/Data:* 
> - [Insert your first raw thought/fact/math here] 
> - [Insert your second raw thought/fact/math here]

---

## PHASE 2 & 3: EXECUTION PROMPT LIBRARY
Use these specific prompts to bulletproof your metrics and evaluate your system. **Always start by attaching your accepted Project Proposal PDF to the chat context.**

### 1. Systems Engineering Audit (Toolchain Selection)
> **[PROMPT]** You are the Systems Engineering Auditor. Conduct an interoperability audit based on my attached proposal. Compare my proposed asset pipeline tools against modern standards (OpenUSD, Unity 6 LTS, Blender 4.2 LTS). Identify integration bottlenecks, coordinate system mismatches (e.g., Y-up vs Z-up), or data loss vectors that threaten my architecture. Provide your findings in a structured Markdown interoperability matrix.

### 2. Evaluating Metrics & Baselines
> **[PROMPT]** I am currently using [Insert Metric: e.g., subjective photorealism, frame rate, peak VRAM] to evaluate my render. Are there more robust secondary metrics that are industry-standard for this specific task (e.g., SSIM for image quality, memory bandwidth profiling)? What are the most common rendering failure modes or visual artifacts (e.g., aliasing, light leaking) when implementing this technique? Help me design a specific baseline test to measure these vulnerabilities.

---

## THE "KEEP MOVING" AFFORDANCE (ACTION BRANCHES)
If you are stuck and the AI gives you generic advice, force it to give you concrete options by pasting this at the end of your prompt to break ambiguity:

> **[ACTION BRANCHES REQUIREMENT]**
> Do not end with a generic question. Provide exactly three [ACTION BRANCHES] for me to choose from: 
> *   **[OPTION A]** - Broaden the literature based on my graphics domain (e.g., SIGGRAPH papers).
> *   **[OPTION B]** - Deepen the math/logic check on a specific matrix, shader node, or bounding volume hierarchy I provided.
> *   **[OPTION C]** - Execute the pipeline or shader code based on my specified performance metric.
