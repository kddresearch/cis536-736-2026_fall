<!--
  Template Note: This file serves as the canonical template for module pages.
  When instantiated as `modules/module_YY/module_YY.md`, use relative paths 
  (`../../phases/phase_0X/` and `../../lectures/lecture_j.md`) to maintain correct routing.
  Note that the module name itself does not self-link, but lectures link directly to their markdown files.
  Module readings may be contiguous or separate per lecture; the module readings for the modules within a parent phase form a partition of that phase’s readings.
-->

# CIS 536/736: Computer Graphics

| Phase | Module | Lecture(s) | Date(s) | Assigned Reading(s) |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 5](../../phases/phase_05/):** Visualization, VFX & Capstone Synthesis | **Module 12:** Cross-Pillar Integration, GenAI Audit & Final Polish | **Lectures 37–39** | Mon 30 Nov – Fri 04 Dec 2026 | [OpenUSD.org: *"Composition" Overview* (Synthesis Reading)](https://openusd.org/docs/USD-Glossary.html#USDGlossary-Composition); [Course GenAI Policy (Syllabus)](../../syllabus/) & `generative_ai_kit_v2.md`; [Blender 4.2 Manual: *Geometry Nodes — Noise Textures & Terrain*](https://docs.blender.org/manual/en/4.2/modeling/geometry_nodes/texture/noise.html) |

---

## [Lecture 37: Cross-Pillar & Multi-DCC Integration Review (Houdini, Blender 4 & Unity 6 via OpenUSD)](../../lectures/lecture_37.md) (Mon 30 Nov 2026)

1. **5×5×5 Taxonomy Synthesis Across All Five Pillars & Stages:** Auditing how each team's single **Methodological Pillar** (*Procedural Generation, Illumination & Shading, Data-Driven Rigging & Animation, Generative Geometry & Reconstruction, Cloud Rendering & Infrastructure*) and **Category Track** (*Terrain, VFX, Locomotion, InfoVis, Materials*) execute cleanly across the 5-stage spine (`Stage 1: Asset -> Stage 2: Shading -> Stage 3: Kinetics -> Stage 4: Composition -> Stage 5: Render & Evaluation`).
2. **OpenUSD Composition Arcs (LIVRPS) & Golden Path Assembly:** Using **L**ocal opinions, **I**nherits, **V**ariantSets, **R**eferences, **P**ayloads, and **S**pecializes to compose non-destructive scene graphs across both Golden Path architectures (**Procedural Asset Pipeline** and **Motion Matching Pipeline**) while resolving right-handed Z-up (Blender/OpenUSD) and right-handed Y-up (Houdini) transforms into Unity 6 left-handed Y-up space.
3. **Multi-DCC VFX, Rigged & Radiance-Field Unification:** Integrating Houdini caches (`.pcache`, `.vf` vector fields, linear `EXR` VATs), Blender 4.2 Geometry Nodes and Mixamo/Control Rig assets (`UsdSkel`), and `gsplat`/COLMAP radiance fields inside Unity 6 LTS without sRGB gamma or texture-compression corruption.
4. **Asset Tier Verification & Cloud Scaling (Stage 5):** Confirming that unclaimed stages rely on verified **Tier 1 (Golden Path)** assets while **Tier 2** reconstructions satisfy the 8 GB+ VRAM and topology-repair gates, and validating relative-path OpenUSD stages for **AWS Deadline Cloud** batch rendering against local Cycles/Unity baselines.
5. **Unified Team Governance & Exit-Strategy Audit:** Verifying Phase 3–5 unified team integration (max 3 members) and enforcing Git branch isolation plus orthogonal CG stage ownership for any team invoking the split protocol.
* **Assigned Reading:** [OpenUSD.org: *"Composition" Overview* (Synthesis Reading)](https://openusd.org/docs/USD-Glossary.html#USDGlossary-Composition)

---

## [Lecture 38: GenAI Provenance Audit, Empirical Benchmarking & Term Project Final-Prep Workshop](../../lectures/lecture_38.md) (Wed 02 Dec 2026)

1. **Synthesizer-Only GenAI Audit (`generative_ai_kit_v2.md` & Course Syllabus):** Auditing each team's `GenAI` provenance log to verify that LLMs acted strictly as synthesizers of student-authored, non-generative inputs (written specifications, Shader Graph / Geometry Nodes / VFX Graph 17 networks, asset permalinks, error logs, and profiler CSVs), disallowing ungrounded prompt-only generation.
2. **Phase 3 Quantitative Profiling & Controlled Baseline Parity:** Auditing controlled baseline comparisons (mean/median frame time with 95% confidence intervals, draw calls, GPU VRAM, shader compilation overhead, pose mismatch error, or AWS Deadline vs. local Cycles speedup) and enforcing Healy Ch. 5–6 visualization standards to eliminate unquantified qualitative claims.
3. **Phase 5 Unified Repository & Interchange Hygiene Gate:** Inspecting each team's single shared GitHub archive (accessible to `@banazir`) for a complete `README.md`, pinned Unity 6 / Blender 4.2 / Houdini environment manifests, reproducible headless render/profiling scripts, and clean interchange exports (`OpenUSD`/`GLB`/`FBX`/`PLY`/`.pcache`/`.vf`) free of monolithic `.blend` binary bloat.
4. **Phase 5 IEEE / ACM Technical Report Workshop (6–8 Pages):** Structuring the unified double-column IEEE or ACM conference proceedings manuscript (6–8 pages excluding references) across *Abstract, Introduction, Background/Related Work, System Architecture & Pipeline, Experiments & Results,* and *Conclusion* against the 300-point Substance Rubric.
5. **Phase 4 Oral Defense & Individual Reflection Readiness:** Confirming presentation scheduling (**Early Extra Credit:** Thu 03 Dec – Sun 06 Dec 2026; **Standard:** Dead Week, Mon 07 Dec – Fri 11 Dec 2026; **Finals Week:** no presentations without written OSSA or special-circumstance approval prior to the end of Week 16) and verifying that every team member prepares individual answers to the three Standard Reflection Questions.
* **Assigned Reading:** [Course GenAI Policy (Syllabus)](../../syllabus/) & `generative_ai_kit_v2.md`

---

## [Lecture 39: Lab 7: Final Polish Lab — Procedural Terrain, Fractals & End-to-End Capstone Freeze](../../lectures/lecture_39.md) (Fri 04 Dec 2026)

1. **Multi-Octave Procedural Noise & Fractal Terrain Synthesis (`Pillar 1 × Track 1`):** Combining Perlin/Simplex Noise Textures, Voronoi cellular patterns, and multifractal ridge displacement in **Blender 4.2 LTS Geometry Nodes** to author seed- and density-parameterized terrain geometry (Stage 1: Asset Creation).
2. **Surface Attribute Capture & Triplanar PBR Surfacing (Stage 2):** Capturing elevation, slope normal, and curvature masks as named attributes in Geometry Nodes to drive **Unity 6 Shader Graph** triplanar and height-blended PBR materials paired with Poly Haven 4K EXR/PNG textures.
3. **Rule-Driven Environmental Instancing & LOD Budgets (Stages 3–4):** Distributing rocks, foliage, and props via `Distribute Points on Faces` and `Instance on Points` weighted by slope/altitude masks while enforcing explicit polygon, draw-call, and overdraw budgets against a static-mesh loading baseline.
4. **OpenUSD Scene Composition & Atmospheric Look-Dev (Stage 4):** Exporting parameterized terrain and variant sets via `.usda`/`.usdc` with verified Z-up $\rightarrow$ Y-up transforms, assembling the unified scene in Unity 6 HDRP/URP with Light Probes, Global Illumination, and Volumetric Fog.
5. **Pre-Dead Week Scene Freeze & Reproducible Telemetry Archive (Stage 5):** Locking controlled camera trajectories, running headless render and `ProfilerRecorder` scripts to capture final baseline-vs.-capstone CSV benchmarks and side-by-side renders, and freezing the unified GitHub repository ahead of Week 16 presentations.
* **Assigned Reading:** [Blender 4.2 Manual: *Geometry Nodes — Noise Textures & Terrain*](https://docs.blender.org/manual/en/4.2/modeling/geometry_nodes/texture/noise.html)