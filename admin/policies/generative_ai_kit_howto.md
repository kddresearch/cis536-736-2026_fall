<!--
yaml_schema_version: "2.2"
document_version: "1.2"
document_last_updated_date: "2026-10-07"
-->

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
Ingest the following GenAI how-to and explain step-by-step, with citations of each platform's extant documentation, how to create a course project in each of the 5 main platforms using generative_ai_kid.md as project instructions. Emit output with the URLs inline in GFM in GitHub-flavored Markdown format in a fenced box.


The kit (goes in instructions): https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/policies/generative_ai_kit.md
The grounding documents to attach as files:
- https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/syllabus/05_term_project_taxonomy.md 
- https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/syllabus/06_term_project_sourcing_guide.md 
```

# Creating a CIS 536/736 Course Project in the 5 Major GenAI Platforms

This guide adapts `generative_ai_kit.md` into a repeatable workflow for creating a course project workspace that is grounded in the course taxonomy and sourcing constraints before any project ideation, feasibility analysis, literature review, asset selection, shader design, or implementation work.

## Common Inputs for All Platforms

### System Instructions

Use the full contents of:

- https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/policies/generative_ai_kit.md

as the primary instruction set.

### Grounding Documents

Attach or upload:

- https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/syllabus/05_term_project_taxonomy.md
- https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/syllabus/06_term_project_sourcing_guide.md

### Initial Priming Prompt

```text
Read the attached course documents completely before answering.

You must treat generative_ai_kit.md as your governing instruction set.

You must treat 05_term_project_taxonomy.md and
06_term_project_sourcing_guide.md as authoritative grounding documents.

Do not evaluate project ideas until you have incorporated:

1. The project taxonomy
2. The Five Pillars
3. The project tracks
4. The asset sourcing tiers
5. The feasibility rubric

Once you have finished ingesting all materials, ask me for:

- Project Pillar
- Project Track
- Proposed 3D Asset
- Intended asset source
- Intended implementation pipeline

Do not approve a project before evaluating it against the feasibility rubric and sourcing requirements.
```

---

# 1. ChatGPT (OpenAI)

**Best for:** project planning, C#/Python debugging, shader math, Unity implementation support.

## Relevant Documentation

- File uploads in ChatGPT: https://help.openai.com/en/articles/8555545-file-uploads-faq
- Uploading files to a conversation: https://help.openai.com/en/articles/8555545-uploading-files-and-audio-to-chatgpt

OpenAI documents that files can be attached directly to a ChatGPT conversation and analyzed within the current chat context.

## Step-by-Step

### Step 1: Create a New Chat

Open ChatGPT and start a new conversation.

Recommended models:

- GPT-5
- GPT-4o (if available)

### Step 2: Upload Grounding Files

Use the paperclip / attachment button.

Upload:

- `05_term_project_taxonomy.md`
- `06_term_project_sourcing_guide.md`

OpenAI documents that ChatGPT supports uploaded documents and can analyze their contents directly:

https://help.openai.com/en/articles/8555545-uploading-files-and-audio-to-chatgpt

### Step 3: Paste the Kit

Paste the contents of:

https://github.com/kddresearch/cis536-736-2026_fall/blob/main/admin/policies/generative_ai_kit.md

into the chat.

### Step 4: Prime the Conversation

Paste the priming prompt above.

### Step 5: Verify Grounding

Before discussing a project, ask:

```text
Summarize:

1. The Five Pillars
2. The feasibility rubric
3. Tier 1, Tier 2, and Tier 3 sourcing requirements

Quote the specific sections that support your answer.
```

### Step 6: Create the Project

Provide:

```text
Pillar:
Track:
3D Asset:
Source:
Target platform:
```

and request an evaluation.

---

# 2. Claude (Anthropic)

**Best for:** strict requirements enforcement, design review, academic critique, pipeline reasoning.

## Relevant Documentation

- Projects overview:
  https://support.claude.com/en/articles/9517075-what-are-projects

- Creating and managing projects:
  https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects

Anthropic documents that Projects provide:

- project knowledge bases
- uploaded reference materials
- project-wide instructions

which are automatically used across chats within the project.

## Step-by-Step

### Step 1: Create a New Project

Navigate to:

```text
Claude → Projects → New Project
```

Documentation:

https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects

### Step 2: Name the Project

Example:

```text
CIS536 Term Project
```

### Step 3: Upload Knowledge Files

In Project Knowledge upload:

- `05_term_project_taxonomy.md`
- `06_term_project_sourcing_guide.md`

Documentation:

https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects

### Step 4: Add Project Instructions

Paste the entire contents of:

```text
generative_ai_kit.md
```

into Project Instructions.

Anthropic explicitly supports project-level instructions that apply to all chats in the project:

https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects

### Step 5: Add Enforcement Rules

Append:

```text
Act as a strict Socratic gatekeeper.

Reject:

- Tier 3 assets
- infeasible projects
- mathematically unsupported shaders
- proposals that violate the sourcing guide

Require explicit feasibility justification before approval.
```

### Step 6: Begin Project Chat

Open a project chat and use the common priming prompt.

---

# 3. Gemini (Google)

**Best for:** large-context synthesis, literature review support, coordinate-system and graphics pipeline analysis.

## Relevant Documentation

- Upload files in Gemini:
  https://support.google.com/gemini/answer/14903178

- Gemini file support:
  https://ai.google.dev/gemini-api/docs/files

Google documents that Gemini can accept uploaded files and analyze them directly within a conversation.

## Step-by-Step

### Step 1: Open Gemini

Use either:

- Gemini Advanced
- Gemini at https://gemini.google.com

### Step 2: Upload Grounding Documents

Click:

```text
+ Add Files
```

Upload:

- `05_term_project_taxonomy.md`
- `06_term_project_sourcing_guide.md`

Documentation:

https://support.google.com/gemini/answer/14903178

### Step 3: Paste the Kit

Paste:

```text
generative_ai_kit.md
```

into the conversation.

### Step 4: Force Grounding

Use:

```text
Read all attached files.

Before answering any future questions, build an internal model containing:

- project taxonomy
- project tracks
- sourcing tiers
- feasibility criteria

Use those materials as higher-priority guidance than defaults.
```

### Step 5: Validate

Ask:

```text
What project ideas would automatically fail under the sourcing guide?
```

and compare the answer to the uploaded documents.

### Step 6: Submit Project Proposal

Provide:

```text
Pillar
Track
Asset
Pipeline
Expected hardware
Expected VRAM requirements
```

and request feasibility analysis.

---

# 4. Perplexity

**Best for:** literature review, SIGGRAPH searches, benchmark discovery, open-source asset discovery.

## Relevant Documentation

- File Uploads:
  https://www.perplexity.ai/help-center/en/articles/10354807-file-uploads

Perplexity documents that users can attach files using the Attach button and ask follow-up questions grounded in those files.

## Step-by-Step

### Step 1: Create a New Thread

Open a new Perplexity search session.

### Step 2: Attach Files

Click:

```text
+ Attach
```

Upload:

- `06_term_project_sourcing_guide.md`

Recommended:

- also upload `05_term_project_taxonomy.md`

Documentation:

https://www.perplexity.ai/help-center/en/articles/10354807-file-uploads

### Step 3: Paste the Kit

Paste:

```text
generative_ai_kit.md
```

into the thread.

### Step 4: Set Research Scope

Use:

```text
Read the attached CIS 536 project requirements.

Restrict recommendations to assets and papers that satisfy Tier 1 or Tier 2 requirements.
```

### Step 5: Perform Literature Search

Example:

```text
Find SIGGRAPH, TOG, Eurographics, and open-source implementations relevant to:

[topic]

Only return resources that satisfy the attached sourcing guide.
```

### Step 6: Convert Findings into a Proposal

Ask:

```text
Generate a project proposal using the course taxonomy and feasibility rubric.
```

---

# 5. Grok (xAI)

**Best for:** rapid implementation help, GitHub ecosystem tracking, OpenUSD and recent open-source developments.

## Relevant Documentation

- Grok overview:
  https://docs.x.ai/grok/overview

- File support:
  https://docs.x.ai/developers/files

xAI documents that Grok supports uploaded files and can reason over attached documents during conversation.

## Step-by-Step

### Step 1: Create a New Grok Chat

Open:

https://grok.com

or Grok in X.

### Step 2: Upload Documents

Upload:

- `05_term_project_taxonomy.md`
- `06_term_project_sourcing_guide.md`

According to xAI documentation, Grok can search through and reason over attached documents:

https://docs.x.ai/developers/files

### Step 3: Paste the Kit

Paste:

```text
generative_ai_kit.md
```

as the first message.

### Step 4: Create Role Definition

Add:

```text
You are the CIS 536 Computer Graphics Design Coordinator.

All recommendations must satisfy:

- the project taxonomy
- the sourcing guide
- the feasibility rubric

Reject proposals that violate any of those constraints.
```

### Step 5: Verify Grounding

Ask:

```text
Summarize the Five Pillars and the sourcing tiers from the uploaded documents.
```

### Step 6: Evaluate a Project

Example:

```text
Pillar: Procedural Content Generation

Track: Terrain Generation

Asset: Open-source photogrammetry terrain

Source: [URL]

Evaluate feasibility, VRAM requirements, sourcing compliance,
expected bottlenecks, and likely grading rubric outcome.
```

---

# Recommended Final Configuration (All Platforms)

For maximum consistency:

1. Upload `05_term_project_taxonomy.md`.
2. Upload `06_term_project_sourcing_guide.md`.
3. Paste the entire `generative_ai_kit.md`.
4. Require the system to summarize all three artifacts before proceeding.
5. Refuse project approval until:
   - Pillar identified
   - Track identified
   - Asset identified
   - Source identified
   - Feasibility evaluated
   - Sourcing tier verified
6. Maintain all subsequent discussion inside the same grounded project/workspace rather than opening fresh chats.

This produces the closest approximation to a course-specific design coordinator rather than a generic LLM assistant.
