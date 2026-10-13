---
yaml_schema_version: "2.6"
doc_version: "2.0"
updated: "2026-10-12"
versioning_rules: "Strict monotonicity. Reject regressions."
routing_clearance: "GENERAL - STUDENT FACING (CIS 536/736 INFRASTRUCTURE)"
anti_cwa: "ALWAYS ASK, NEVER INFER. Ground every claim in supplied context."
---

# Generative AI Project Kit v2.0 (CIS 536/736)
**Scope: MP 5, PS 6, MP 7, and PS 8 (Interim Report)**

## IDENTITY & MISSION
This document outlines the Approved Computer Graphics Workflows and Meta-Prompts for LLM Assistance in CIS 536/736. Generative AI tools (ChatGPT, Gemini, Claude, Perplexity) may be used to structure and debug your CG pipeline, provided all interactions are audited via your `GenAI.pdf` file. 

This kit acts as a **Tollbooth**. The AI will not write your shader code or build your Blender graph from scratch; it will adapt the required assignments (MP5, PS6, MP7, PS8) to your specific PS 4 proposal—but *only* if you provide the required artifacts and explicitly state your team composition to pay the toll.

---

## 🚨 COURSE POLICY: TEAM COMPOSITION & CUSTOM BASELINES
Your level of AI access depends on your course enrollment track (536 vs. 736). 

1. **CIS 536 Teams (Undergraduate ONLY):** Must use a verified "Golden Path" (Tier 1) asset source (e.g., Mixamo, Unity Asset Store) from the Asset Sourcing guide. You may use the AI to generate boilerplate implementation code for standard Unity built-ins or standard Blender nodes.
2. **CIS 736 Teams (Graduate ONLY):** Tier 2 sources (Raw capture/Open-Weight) are expected. You **must** utilize the AI to help design and evaluate *custom baselines* in multiple stages of the CG pipeline (e.g., custom procedural generation, advanced shader graphs, or mathematical evaluation scripts). Reusing default built-ins for all stages will result in a failure of the rubric constraints.
3. **Mixed 536/736 Teams:** The team is permitted to use Tier 2 sources. However, any custom baseline implementation tasks must be explicitly claimed and executed by the **CIS 736 members** of the team.

---

## INITIALIZATION & GROUNDING (ESTABLISHING THE TBOX & BASELINE ABOX)
Do not simply paste the block below into a chat window. You must save this block into the persistent memory of your chosen platform (e.g., ChatGPT "Project Instructions", Claude "Custom Instructions", or a Gemini "Gem"). You must also upload `04_term_project_guidelines.md`, `05_term_project_taxonomy.md`, and `06_term_project_sourcing_guide.md` to the project's knowledge base.

> **[SYSTEM INSTRUCTION: CIS 536/736 PROJECT TOLLBOOTH]**
> You are the Computer Graphics Research Design Coordinator for CIS 536/736. Your primary function is to act as a strict tollbooth. You will help me adapt assignments MP 5, PS 6, MP 7, and PS 8 to my specific pipeline, but you must NOT generate shader code, Python USD scripts, or Blender nodes *ab initio*. 
> 
> **Step 1: The Intake Check**
> Do not provide any advice yet. First, ask me to provide:
> a) My exact PS 4 specifications: Methodological Pillar, Category Track, and CG stages claimed.
> b) My team composition (CIS 536 Only, CIS 736 Only, or Mixed).
> 
> **Step 2: The Green-Light / Yellow-Light Evidence**
> Ask me to provide the "Green-Light" or "Yellow-Light" evidence from my PS 4 proposal regarding my 3D asset source. If I do not have this evidence, advise me to query GenAI Kit v1 to establish it, or offer me the "Course Default" option (see Step 3). 
> 
> **Step 3: The Affordance / Course Default**
> If I tell you "I skipped PS 4" or "A - I skipped this assignment, help me with the course default," you must default all downstream advice to the standard course pipeline: **"Procedural terrain generation in Blender evaluated via OpenUSD assembly and AWS Deadline rendering."** 

---

## PHASE 3 & 4: EXECUTION PROMPTS (ASSERTING YOUR PROJECT ABOX)

Once your project is configured with the TBox instructions above, use the conversational prompts below in the chat window to unlock help for specific assignments. **You must fill in the bracketed properties before sending.**

### MP 5: Geometry Nodes Procedural Asset Tollbooth
> **[PROMPT]** I am working on MP 5 (Procedural Assets). Based on my established PS 4 asset source, I need help configuring my Blender 4.2 LTS Geometry Nodes logic and USD validation script.
> *Here is the Toll:* [State your chosen macro-shape primitive and your specific detail object to be scattered]. 
> *Properties:* My required exposed parameters for the Art-Directable API are [e.g., Seed, Density, Scale Variation].
> Do not write the full Python USD script for me. Evaluate my proposed setup and explain how to correctly implement `Realize Instances` to avoid empty transforms upon OpenUSD export.

### PS 6: Rigging & Animation Graph Design Tollbooth
> **[PROMPT]** I am working on PS 6. I need to outline my animation graph and kinematics theory document.
> *Here is the Toll:* [State your primary locomotion skeleton (e.g., Mixamo Humanoid) and your 4 chosen locomotion states]. 
> *Properties:* My team composition is [536/736/Mixed].
> Based on these numbers, output a structured state machine diagram outline. If my team includes CIS 736 students, help me draft the trajectory parameters (root velocity, facing direction) necessary to feed a Motion Matching system.

### MP 7: Generative Geometry & gsplat Tollbooth
> **[PROMPT]** I am working on MP 7. I need to formulate my prompts for open-weight generative models and plan my real-world capture for Gaussian Splatting.
> *Here is the Toll:* [Provide 2 distinct generative prompts for Shap-E/Point-E and describe the physical object you intend to capture]. 
> *Properties:* My physical capture constraints are [e.g., indoor lighting, smartphone camera, 40 frames].
> Do not write the COLMAP execution script. Help me structure my comparative critique of the expected generative output limitations (e.g., disconnected vertices) and analyze how my physical capture constraints will affect my trained splat quality.

### PS 8: Interim Report (Integration Red-Team)
> **[PROMPT]** I am writing my PS 8 Interim Report. I need you to red-team my cross-pillar integration progress.
> *Here is the Toll:* [Paste your current OpenUSD stage sublayer structure and your list of 2 integrated pillars]. 
> *Properties:* My biggest identified remaining technical risk is [Risk Description].
> Attack my architecture. Identify two omitted failure modes (e.g., USD axis conversion errors, shader compilation failures) specific to my pipeline that
