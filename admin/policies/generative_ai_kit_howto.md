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

# Creating a CIS 536 Course Project Workspace in the Five Main GenAI Platforms

**Source:** `admin/policies/generative_ai_kit_howto.md` (provided text)

## Goal

The purpose of the Generative AI Kit is to **ground** the AI in the realities of the Computer Graphics course before discussing project ideas, Unity assets, shaders, node graphs, or C# code. The guide explicitly states that students should make the AI ingest:

- `05_term_project_taxonomy.md`
- `06_term_project_sourcing_guide.md`

before requesting project evaluation or design assistance. (Source: `generative_ai_kit_howto.md`)

---

## 1. ChatGPT or M365 Copilot

**Platform URLs**

- ChatGPT: <https://chatgpt.com/>
- M365 Copilot: <https://m365.cloud.microsoft/>

**Recommended use:** General QA, methodology brainstorming, C# debugging, Python reasoning, Shader Graph mathematics, and linear algebra validation. (Source: `generative_ai_kit_howto.md`)

### Step-by-step

1. Open a new chat in ChatGPT or M365 Copilot.
2. Select a capable reasoning model (for example, GPT-4o where available).
3. Click the paperclip / attachment icon.
4. Upload:
   - `05_term_project_taxonomy.md`
   - `06_term_project_sourcing_guide.md`
5. Paste the GenAI Kit `[SYSTEM INSTRUCTION]` block.
6. Add the following priming instruction:

   > Read the two attached markdown files. Do not respond until you have fully ingested the course taxonomy, the 5 pillars, and the feasibility rubric. Once ready, ask me for my project Pillar, Track, and proposed 3D Asset.

7. Wait for the model to request:
   - Pillar
   - Track
   - Proposed 3D Asset

### Example project-start prompt

> My Pillar is Procedural Content Generation. My Track is Terrain Synthesis. My proposed 3D asset is a stylized canyon environment. Evaluate feasibility using the course taxonomy and sourcing guide.

**Citation:** `generative_ai_kit_howto.md`

---

## 2. Claude (Anthropic) or Qwen

**Platform URLs**

- Claude: <https://claude.ai/>
- Qwen Chat: <https://chat.qwen.ai/>

**Recommended use:** Deep pipeline synthesis, enforcement of frame-rate and polygon constraints, and academic writing critique. (Source: `generative_ai_kit_howto.md`)

### Step-by-step

1. Open Claude or Qwen.
2. If using Claude Pro, create a new **Project**.
3. Upload:
   - `05_term_project_taxonomy.md`
   - `06_term_project_sourcing_guide.md`
4. In Claude Projects, place these files in the **Project Knowledge** area.
5. Paste the GenAI Kit `[SYSTEM INSTRUCTION]`.
6. Add the following constraint:

   > Act as a strict Socratic gatekeeper. Reject any proposed 3D asset that falls into Tier 3 of the sourcing guide, or any shader that does not possess a mathematical baseline.

7. Present your project concept.
8. Ask Claude/Qwen to challenge assumptions rather than simply approving the idea.

### Example project-start prompt

> Evaluate my real-time water shader pipeline against the sourcing guide. Identify violations, missing mathematics, and feasibility concerns.

**Citation:** `generative_ai_kit_howto.md`

---

## 3. Gemini

**Platform URLs**

- Gemini: <https://gemini.google.com/>
- Google AI Studio: <https://aistudio.google.com/>

**Recommended use:** Large-context analysis, coordinate-system conversion problems, and complex graphics pipelines. (Source: `generative_ai_kit_howto.md`)

### Step-by-step

1. Open Gemini Advanced or Google AI Studio.
2. Upload:
   - `05_term_project_taxonomy.md`
   - `06_term_project_sourcing_guide.md`
3. If uploads are unavailable, paste the contents directly into the conversation.
4. Paste the GenAI Kit `[SYSTEM INSTRUCTION]`.
5. Explicitly direct Gemini to evaluate graphics pipelines and constraints.
6. Describe your project.
7. Request an analysis of:
   - VRAM requirements
   - Asset complexity
   - Coordinate conversions
   - Runtime bottlenecks
   - Feasibility rubric compliance

### Example project-start prompt

> Based on the Tier 1/Tier 2 sourcing guide I provided, evaluate my plan to import a COLMAP point cloud into Unity. What VRAM limits, coordinate conversion issues, and rendering bottlenecks should I expect?

**Citation:** `generative_ai_kit_howto.md`

---

## 4. Perplexity

**Platform URL**

- Perplexity: <https://www.perplexity.ai/>

**Recommended use:** Literature reviews, SIGGRAPH baseline discovery, Unity package discovery, and sourcing external references. (Source: `generative_ai_kit_howto.md`)

### Step-by-step

1. Create a new Perplexity Pro Search thread.
2. Attach:
   - `06_term_project_sourcing_guide.md`
3. Use Perplexity primarily during:
   - Asset Creation
   - Literature Review
4. Tell Perplexity to search only for resources satisfying the course sourcing rules.
5. Specify your project topic.
6. Request direct links to:
   - SIGGRAPH papers
   - Blender tutorials
   - Open-source repositories
   - Technical documentation

### Example project-start prompt

> I am building a procedural generation pipeline for CIS 536. Read the attached sourcing guidelines. Search for SIGGRAPH papers or open-source Blender 4.2 Geometry Node tutorials matching my terrain-generation topic that satisfy Tier 1 or Tier 2 requirements. Provide direct links.

**Citation:** `generative_ai_kit_howto.md`

---

## 5. Grok (xAI)

**Platform URL**

- Grok: <https://grok.com/>

**Recommended use:** Rapid syntax generation and reviewing recent GitHub activity related to topics such as OpenUSD and gsplat. (Source: `generative_ai_kit_howto.md`)

### Step-by-step

1. Open a new Grok chat.
2. Copy and paste the raw contents of:
   - `05_term_project_taxonomy.md`
3. Paste the GenAI Kit `[SYSTEM INSTRUCTION]`.
4. Describe your Pillar and Track.
5. Ask Grok to act as the CIS 536 design coordinator.
6. Request feasibility analysis, architecture suggestions, and implementation guidance.

### Example project-start prompt

> Acting as the CIS 536 Computer Graphics Design Coordinator, evaluate my idea for a Procedural Content Generation pipeline. Here is the course taxonomy: [PASTE TAXONOMY].

**Citation:** `generative_ai_kit_howto.md`

---

# Recommended Workflow Across Platforms

1. **Start with ChatGPT, Copilot, Claude, or Qwen**
   - Define the project.
   - Validate feasibility.
   - Check mathematical foundations.

2. **Use Gemini**
   - Stress-test technical details.
   - Analyze coordinate systems, memory, and rendering constraints.

3. **Use Perplexity**
   - Find papers, tutorials, datasets, and software packages.

4. **Use Grok**
   - Generate implementation scaffolding.
   - Review emerging tooling and repository changes.

This workflow follows the platform specializations recommended by the GenAI Kit and ensures that all project evaluations are grounded in the course taxonomy, five pillars, and feasibility rubric before development begins. (Source: `generative_ai_kit_howto.md`)
