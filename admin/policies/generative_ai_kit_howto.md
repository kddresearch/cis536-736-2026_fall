# How to Prime the Generative AI Kit (v1.0)
**Path:** `admin/policies/generative_ai_kit_howto.md`

To use the Generative AI Kit effectively, you must "ground" the AI in the reality of this Computer Graphics course. If you ask a generic AI "Is my Unity project good?", it will hallucinate a positive response that ignores polygon limits, VRAM, and mathematical soundness. You must force the AI to read the **05_term_project_taxonomy.md** and **06_term_project_sourcing_guide.md** files before evaluating your node graphs or C# scripts.

Below are the exact procedures for priming the major platforms. 

**Instructor Recommendation:** 
*   **ChatGPT / M365 Copilot** or **Claude / Qwen** are best for Shader Graph math, C# Unity script debugging, and checking linear algebra matrices. 
*   **Gemini** or **Perplexity** are best for finding SIGGRAPH papers, specific OpenUSD documentation, or evaluating open-weight generative models (like Shap-E).

---

### 1. ChatGPT or M365 Copilot
*Best for: General QA, methodology brainstorming, and robust C# / Python code reasoning.*
*   **The Setup:** Open a new chat (GPT-4o or M365 Copilot). 
*   **Grounding:** Click the attachment (paperclip) icon. Upload both `05_term_project_taxonomy.md` and `06_term_project_sourcing_guide.md` as files.
*   **Priming:** Paste the `[SYSTEM INSTRUCTION]` block from the GenAI Kit into the chat box. Add: *"Read the two attached markdown files. Do not respond until you have fully ingested the course taxonomy, the 5 pillars, and the feasibility rubric. Once ready, ask me for my project Pillar, Track, and proposed 3D Asset."*

### 2. Claude (Anthropic) or Qwen
*Best for: Deep pipeline synthesis, strict adherence to frame rate/poly constraints, and academic writing critique.*
*   **The Setup:** If using Claude, create a new "Project" (if on Pro) or a standard chat. 
*   **Grounding:** In a Claude Project, upload the `05` and `06` `.md` files to the "Project Knowledge" base. If using standard chat, attach the files directly.
*   **Priming:** Paste the `[SYSTEM INSTRUCTION]` into the "Custom Instructions" box of the Project, or as your first prompt. Claude is highly obedient to constraints; explicitly tell it: *"Act as a strict Socratic gatekeeper. Reject any proposed 3D asset that falls into Tier 3 of the sourcing guide, or any shader that does not possess a mathematical baseline."*

### 3. Gemini (Google)
*Best for: Massive context windows (Gemini 1.5 Pro) and evaluating complex coordinate system math (Z-up to Y-up).*
*   **The Setup:** Open Gemini Advanced (or Gemini 1.5 Pro via Google AI Studio).
*   **Grounding:** Use the `+` icon to upload the `05` and `06` `.md` files, or paste the raw text of both documents directly into the prompt if file uploads are restricted.
*   **Priming:** Paste the `[SYSTEM INSTRUCTION]` block. **Crucial:** Ask Gemini to actively evaluate asset pipelines. *Example: "Based on the Tier 1/Tier 2 sourcing guide I provided, evaluate my plan to import a COLMAP point cloud into Unity. What are the specific VRAM and coordinate conversion bottlenecks I will face?"*

### 4. Perplexity
*Best for: Live literature reviews, finding SIGGRAPH SOTA baselines, and discovering Unity packages.*
*   **The Setup:** Open a new Perplexity Pro Search thread.
*   **Grounding:** Attach the `06_term_project_sourcing_guide.md` file using the `Attach` button.
*   **Priming:** Use Perplexity specifically for the **Asset Creation** and **Literature** stages. *Prompt Example: "I am building a procedural generation pipeline for CIS 536. Read the attached sourcing guidelines. Search for SIGGRAPH papers or open-source Blender 4.2 Geometry Node tutorials matching [My Terrain Topic] that strictly meet the 'Tier 1 or Tier 2' requirements outlined in my document. Provide direct links."*

### 5. Grok (xAI)
*Best for: Rapid syntax generation and checking recent GitHub PRs for OpenUSD/gsplat updates.*
*   **The Setup:** Open a new Grok chat.
*   **Grounding:** Paste the raw text of the `05_term_project_taxonomy.md` directly into the prompt.
*   **Priming:** Combine the taxonomy with the system instruction. *Prompt Example: "Acting as the CIS 536 Computer Graphics Design Coordinator, evaluate my idea for a [Insert Pillar/Track] pipeline. Here is the course taxonomy: [Paste Taxonomy]."*

---

# M365 Copilot Version

Generated using this prompt:
```text
Ingest the following genai how-to and explain step-by-step, with citations, how to create a course project in each of the 5 main platforms. Emit output with the URLs inline in GFM in GitHub-flavored Markdown format in a fenced box.
```
