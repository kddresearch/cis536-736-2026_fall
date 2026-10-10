# MP 5: Geometry Nodes Procedural Asset
**Building a Parameter-Driven, Non-Destructive Asset for OpenUSD Export (Blender 4.2 LTS)**

**📅 Assigned:** Fri 09 Oct 2026 | **⏰ Due:** Sun 18 Oct 2026 (11:59 PM) | **💯 Points:** 100 | **👥 Team:** Individual (Solo)

> **🚨 CRITICAL SUBMISSION POLICY**
> **Mandatory Exception Notice:** Due to the extension of PS 4, the gap between the PS 4 due date and the MP 5 due date is 4 days. This is an explicit, one-time exception to the standard 6-day minimum gap policy. The standard 9-day lead time from release to due date remains in effect.
> **No Late Submissions:** This assignment closes at the deadline. Late work receives zero points; your lowest 2 regular-assignment scores are dropped automatically over the semester, which is the designed accommodation for this policy — plan accordingly.

## 1. Overview
Following Lectures 14–16, you will construct a non-destructive, parameter-driven procedural asset (e.g., a ruined pillar, a sci-fi crate, or a scattered-debris cluster) using the Blender 4.2 LTS Geometry Nodes modifier. Your graph must use the Field system rather than hardcoded values, scatter and instance detail geometry via Poisson Disk distribution, and expose at least three parameters (Seed, Density, Scale Variation) to the Modifier panel so an artist could use your tool without opening the node editor. The asset must then be realized (`Realize Instances`) and exported to OpenUSD, with its topology verified by the Python `pxr` validation script before submission. **This assignment covers the Lab 3a scope only** — single-asset export and validation. Multi-variant composition and Unity import (Lab 3b) belong to a later assignment and are out of scope here.

## 2. Initiating Your Draft (The AI Kit Procedure)
Use the Generative AI Kit to help structure your submission. Follow these stages to systematically break down your assignment:

1. **Asset Concept Decomposition:** Decide what your procedural asset actually is (pillar, crate, debris field, etc.) and list the structural nodes (`Transform`, `Extrude Mesh`, `Subdivision Surface`) you'll need for the base shape before touching the scatter system — ask the Kit to flag scope creep if your concept is too ambitious for the time budget.
2. **Scatter/Instance Planning:** Map out your `Distribute Points on Faces` → `Instance on Points` chain and decide where `Random Value` nodes feed Scale and Rotation to break visual uniformity, per Lecture 14's entropy discussion.
3. **Parameter API Design:** Before wiring anything, use the Kit to help you decide exactly which three-plus values belong on the `Group Input` node (Seed, Density, Scale Variation at minimum) — this is your "art-directable API," not an afterthought.
4. **Export Readiness Check:** Confirm `Realize Instances` sits at the end of your instancing chain and `Set Material` sits just before `Group Output`, per Lecture 16 — have the Kit help you reason through what happens to an un-realized instance stream at USD export time if you're not sure.
5. **Validation Dry-Run:** Before your final export, use the Kit to help you interpret the Python `pxr` validation script's output so you know what a passing vs. failing stage check actually looks like, rather than running it blind.

## 3. Required Document Structure
Your submission must be a **ZIP archive containing your `.blend` source, your exported `.usd`/`.usda` file, and a completed validation notebook**, with a short Markdown report containing the following sections exactly as named below.

* **Geometry Nodes Graph Overview:** A screenshot or export of your full node graph, annotated enough to identify the structural, scatter/instance, and parameter-exposure sections.
* **Exposed Parameter Panel:** A screenshot of your Modifier Properties panel showing Seed, Density, and Scale Variation as working, user-facing controls (not raw node sockets).
* **Realize Instances & Export Settings:** Confirmation that `Realize Instances` and `Set Material` are placed correctly, plus your USD export settings (UVs, Normals, Evaluation Mode).
* **Python Validation Output:** The output of running the provided starter notebook against your exported file, showing mesh prim count and vertex counts.
* **Stress-Test Notes:** Per Lecture 16's validation checklist — what happens when you scrub Seed, Density, and Scale to their extremes? Any clipping, floating/orphan instances, or viewport performance drop, and how you addressed it.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this artifact? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Submission Artifacts & Allowed File Types (Canvas)

Submit a single ZIP (or a GitHub commit URL, per the course's standard GitOps protocol — see below) containing:

| Artifact | Required? | Allowed File Type(s) |
|---|:---:|---|
| Blender source file | Required | `.blend` |
| Exported procedural asset | Required | `.usd` or `.usda` (ASCII preferred for debuggability) |
| Completed validation notebook, with output cells showing a successful run | Required | `.ipynb` |
| Modifier panel / node graph screenshots | Required | `.png` or `.jpg` |
| Markdown report (sections above) | Required | `.md` or `.pdf` (rendered from Markdown) |
| GenAI log | Required (or explicit "No GenAI Used" statement) | `.pdf` |

**GitOps submission protocol (standard for this course):** Commit your `.blend` source, your `.usd`/`.usda` export, and your completed `.ipynb` notebook to your `cis536_736_hw` GitHub repository. Post the absolute commit URL (not a branch or repo root URL — the specific commit SSOT) to the Canvas assignment drop box as your submission. Do not upload binary assets directly to Canvas; Canvas is for the commit URL and your Markdown/PDF report only.

## 5. Grading Rubric (100 Points Total)

This assignment is worth 100 points total.

**Part 1: Procedural Graph Construction (35 pts)**
* **Non-Destructive Field Usage (20 pts):** The graph uses the Field system (attributes, scatter, instancing) correctly and avoids hardcoded, non-parametric values.
* **Parameter Exposure Functional (15 pts):** Seed, Density, and Scale Variation (minimum 3 parameters) are correctly routed to the Modifier panel and functionally alter the output when scrubbed.

**Part 2: Export & Validation Correctness (65 pts)**
* **Realize Instances & Export Correctness (30 pts):** `Realize Instances` and `Set Material` are correctly placed, and the exported `.usd`/`.usda` file contains real, serialized geometry (not empty transforms).
* **Python Validation Executed Successfully (30 pts):** The provided notebook runs against the student's own export and correctly reports mesh prim count and vertex counts matching the Blender source.
* **GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.
