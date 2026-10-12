<!-- ASSIGNMENT-META
id: MP 9
phase: Phase 4: Integration & Application
module: Module 9: Cloud Render Farms — AWS Deadline Cloud
released: Thu 12 Nov 2026
due: Sat 21 Nov 2026
prev: ../homework/ps8_interim_report.md
next: ../machine_problems/mp10_data_viz.md
-->
# MP 9: Cloud Render Farm Task
**Distributed Rendering and Cost-Aware Scene Submission via AWS Deadline Cloud**

**🗓️ Released:** Thu 12 Nov 2026 | **📅 Due:** Sat 21 Nov 2026 | **💯 Points:** 100 | **👥 Team:** Individual

> **🚨 CRITICAL SUBMISSION POLICY**
> **ℹ️ Capped Credit Allocation — No Supplemental Credits:** You have a strictly capped AWS Educate/Academy credit allocation for this task. Exhausting your allocation due to an inefficient scene is a natural consequence and part of the learning objective — budget your render accordingly, and no additional credits will be issued.

## 1. Overview
Before submitting any render job, document your AWS Deadline Cloud configuration and budget. Then, submit and complete at least one render job on AWS Deadline Cloud using one of your own existing course scenes (e.g., your PBR material build, procedural asset, or rigged character). You must complete the render successfully within your capped credit allocation and analyze the cost results.

**Required Readings:**
* **AWS Deadline Cloud User Guide:**
  * **Queues, Fleets & Budgets:** Configuring a render job within a cost ceiling.
  * **Getting Started:** Submitting your first job from a local scene.
* **AWS Deadline Cloud — How It Works (docs):**
  * **Cost/Scale Tradeoffs:** How fleet size and frame count drive your compute-hour bill.

## 2. Initiating Your Draft (The AI Kit Procedure)
Use the Generative AI Kit to help structure your submission. Follow these stages to systematically break down your assignment:

1. **Budget Brainstorming:** Use the AI Kit to help outline the cost implications of different render resolutions and sample counts before calculating your final estimate.
2. **Fleet Strategy:** Ask the AI Kit for an explanation of cost tradeoffs when configuring deadline queues, fleets, and compute instance types.
3. **Troubleshooting:** Paste job submission errors or log warnings into the AI Kit to help diagnose render failures.
4. **Optimization:** Ask the AI for specific scene-complexity reduction techniques (like LODs or lower sample counts) that could keep your render under budget if your estimate is too high.
5. **Cost Analysis Drafting:** Have the AI Kit help structure your write-up comparing your estimated compute hours vs. the actual final cost.

## 3. Required Document Structure
Your submission must be a **GitHub Classroom repo commit URL (render config + output) + PDF write-up to Canvas** containing the following elements:

* **Part 1: Configuration & Estimation:** A summary of your AWS Deadline Cloud configuration, including your scene's estimated frame count, resolution, sample count, and calculated compute-hour budget against your capped allocation. Identify one scene-complexity reduction you could make if needed.
* **Part 2: Render Deliverables:** The successfully rendered output file(s) from your AWS Deadline Cloud job. Confirm that the output matches expected local-render lighting and materials.
* **Part 3: Cost Discrepancy Analysis:** A short write-up stating your actual compute-hour/credit usage and analyzing any discrepancy between your pre-submission estimate and the final cost.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this artifact? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Grading Rubric (100 Points Total)

This assignment is worth 100 points total.

**Part 1: Budget & Cost Analysis (40 pts)**
* **Budget Estimation Accuracy (20 pts):** Pre-submission estimate is methodologically sound, even if imperfect.
* **Cost Discrepancy Analysis (20 pts):** Estimated-vs-actual cost gap is explained, not just reported.

**Part 2: Render Submission & Quality (55 pts)**
* **Successful Job Completion Within Budget (30 pts):** Render job completes within the capped allocation.
* **Output Correctness (25 pts):** Rendered output matches expected local-render quality/appearance.

**GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.

---
## Assignment Navigation

| ⬅️ Previous | 📝 Index | Next ➡️ |
|:---|:---:|---:|
| [PS 8: Term Project Interim Report](../homework/ps8_interim_report.md) | [[All Assignments]](../README.md#schedule) | [MP 10: Data & Information Visualization Build](../machine_problems/mp10_data_viz.md) |

[🏠 Course Home](../../README.md) · [⬆️ Module](../../modules/module_09/README.md) · [⏫ Phase](../../phases/phase_04/README.md)
