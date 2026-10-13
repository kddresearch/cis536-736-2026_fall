# How to Prime the Generative AI Kit (v2.0)
**Path:** `admin/policies/generative_ai_kit_howto_v2.md`

To use the Generative AI Kit effectively, you must strictly separate the global rules of the course from the specific homework task or project milestone you are working on. If you paste everything into a single chat, the AI will suffer from context amnesia. 

You must establish a persistent workspace by defining your **TBox** (Terminological Box: the rules and constraints of the course) at the **Project Level**, and submitting your specific **ABox** assertions (your Machine Problem payloads or project sprint deliverables) at the **Chat Level**.

*(Note: If you are looking for previous course iteration initialization guides, check the repository history or syllabus archive.)*

---

## 0. Prepare the Course Files (The TBox)
Before configuring any platform, download the raw `.md` files of the following documents to your local machine:
1. [generative_ai_kit_v2.md](./generative_ai_kit_v2.md) (The System Instructions & Tollbooth Gates)
2. [term_project_guidelines.md](../../term_project/term_project_guidelines.md) (The Milestone Constraints & Sprint Deliverables)
3. [term_project_taxonomy.md](../../term_project/term_project_taxonomy.md) (The 5x5 Matrix: 5 Pillars × 5 Tracks)
4. [asset_discovery_matrix.md](../../term_project/asset_discovery_matrix.md) (The Verifiable Golden Path & Tier 1–3 Sourcing Rules)

---

## 1. Establish the Project Environment
Follow these steps to permanently anchor the TBox in your chosen platform. Do this **once** for the semester.

### ChatGPT (Projects) or M365 Copilot (Notebooks)
* **Create the Space:** In ChatGPT, select **New Project** (requires Plus/Team/Pro). In M365, select **New notebook**. Name it `CIS 536-736 Computer Graphics`.
* **Establish the TBox (Instructions):** Copy the *entire contents* of `generative_ai_kit_v2.md` and paste it into the **Project instructions** field. 
* **Establish the Baseline ABox (Knowledge):** Upload `term_project_guidelines.md`, `term_project_taxonomy.md`, and `asset_discovery_matrix.md` to the project's persistent knowledge base/reference section.

### Claude (Anthropic) or Qwen
* **Create the Space:** In Claude, click **+ New Project**. In Qwen (Alibaba Model Studio), select **Create Agent Application**. 
* **Establish the TBox (Instructions):** Paste the complete contents of `generative_ai_kit_v2.md` into the Custom Instructions / System prompt box. 
* **Establish the Baseline ABox (Knowledge):** Upload the `term_project` markdown files to the Project Knowledge area.

### Gemini (Google)
* **Create the Space:** Open Gemini, select **Gems** (or **Skills**), and click **New Gem**. Name it `CIS 536-736 Project Tollbooth`.
* **Establish the TBox (Instructions):** Paste the complete contents of `generative_ai_kit_v2.md` into the Gem's Instructions field.
* **Establish the Baseline ABox (Knowledge):** Under **Knowledge**, upload the course markdown files directly from your drive (do not use Google Docs links to ensure pure Markdown parsing).

### Perplexity or Grok (xAI)
* **Create the Space:** In Perplexity, select **Create a Project** (formerly Spaces). In Grok, navigate to **Projects**.
* **Establish the TBox (Instructions):** Paste the complete contents of `generative_ai_kit_v2.md` into the Space/Project custom instructions field.
* **Establish the Baseline ABox (Knowledge):** Upload the course markdown files to the Space/Project files area.

---

## 2. Execute the Chat (The Assignment/Sprint ABox)
Once your Project is configured, you will use it to complete individual machine problems and term project milestones. **Do not upload assignment specs or code submissions to the persistent Project Knowledge Base.** Attach them directly to the individual **Chat window** at the exact moment you are working on them.

When you are ready to execute a Tollbooth prompt from the GenAI Kit v2.0, download the corresponding raw assignment `.md` or submission asset and attach it to your chat message:

* **For MP 3 (PBR & ShaderGraph):** Attach [student_submission_README_template.md](../../assignments/machine_problems/mp3_pbr_shadergraph/student_submission_README_template.md) and your ShaderGraph/HLSL code snippet to the chat.
* **For Term Project Sprint 1 (Asset Pipeline & Rigging):** Attach [project_sprints/sprint1_guidelines.md](../../assignments/project_sprints/sprint1_guidelines.md) and your asset manifest to the chat.
* **For Term Project Sprint 2 (Shading & Kinetics):** Attach [project_sprints/sprint2_guidelines.md](../../assignments/project_sprints/sprint2_guidelines.md) and your ShaderGraph/Avatar mapping log to the chat.
* **For Term Project Final (Render & Composition):** Attach [project_sprints/final_report_guidelines.md](../../assignments/project_sprints/final_report_guidelines.md) and your AWS Deadline Cloud or headless Blender/Unity render script to the chat.

### Execution Flow:
1. Open a new chat *inside* your configured `CIS 536-736` Project/Gem/Space.
2. Run the Initialization **"Intake Check"** prompt defined in `generative_ai_kit_v2.md`.
3. **Wait** for the AI to acknowledge your project state and confirm the 5-stage pipeline routing spine (Asset, Shading, Kinetics, Composition, Render).
4. Attach the specific assignment or milestone `.md` / code artifact to the chat box.
5. Execute the specific Tollbooth prompt for that deliverable, providing your verification payload (the "Toll").