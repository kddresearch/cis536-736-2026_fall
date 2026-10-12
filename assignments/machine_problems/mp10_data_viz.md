<!-- ASSIGNMENT-META
id: MP 10
phase: Phase 4: Integration & Application
module: Module 10: Color; Data & Info Visualization I
released: Sat 21 Nov 2026
due: Mon 30 Nov 2026
prev: ../machine_problems/mp9_cloud_render.md
next: ../homework/ps11_genai_audit.md
-->
# MP 10: Data & Information Visualization Build
**Encoding Quantities, Objects, and Processes in a Unity 6 Visualization Pipeline**

**🗓️ Released:** Sat 21 Nov 2026 | **📅 Due:** Mon 30 Nov 2026 | **💯 Points:** 100 | **👥 Team:** Individual

> **🚨 CRITICAL SUBMISSION POLICY**
> **ℹ️ Accessibility Requirement:** Your color encoding must be legible under at least one common color-vision deficiency simulation (e.g., deuteranopia). This is graded, not optional — color theory from Lecture 31 exists precisely so your encodings don't rely on hue alone.

## 1. Overview
Design and implement an interactive or inspectable data visualization running in Unity 6. You will begin by defining your encoding design rationale for a chosen dataset, properly mapping variables to visual channels, and ensuring your color palette is accessible. Then, you will construct a data-driven visualization featuring both object-state and time-evolving process views.

**Required Readings:**
* **Healy — Data Visualization: A Practical Introduction (socviz.co):**
  * **Ch. 3–4:** Encoding quantities — position, length, color, size channels.
  * **Ch. 5–6:** Encoding objects and processes.
* **Unity 6 Manual — UI Toolkit / Runtime Charts:**
  * **Data-Driven UI:** Binding runtime data to visual elements.

## 2. Initiating Your Draft (The AI Kit Procedure)
Use the Generative AI Kit to help structure your submission. Follow these stages to systematically break down your assignment:

1. **Dataset Exploration:** Use the AI Kit to help explore potential variables of interest within your chosen dataset (e.g., your render-farm cost/time data or a public dataset).
2. **Visual Encoding Strategy:** Consult the AI Kit to map your dataset variables to appropriate visual channels (position, length, color, size) according to best practices.
3. **Accessibility Validation:** Ask the AI Kit to review your proposed color palette hex codes for color-vision deficiency compliance.
4. **Implementation Planning:** Brainstorm with the AI Kit how to architect your data-driven UI, charting package, or custom geometry within Unity 6.
5. **Report Drafting:** Have the AI Kit help structure your write-up justifying your design choices, objects vs. processes distinction, and accessibility compliance.

## 3. Required Document Structure
Your submission must be a **GitHub Classroom repo commit URL + PDF write-up to Canvas** containing the following elements:

* **Part 1: Encoding Design Rationale:** A write-up stating your chosen dataset, which visual channels you are using to encode which variables (and why), and a justification of your color palette choice against a stated color-vision-deficiency simulation.
* **Part 2: Unity 6 Visualization Build:** Your Unity 6 project (UI Toolkit, charting package, or custom geometry) driven by real data. It must implement at least one "objects vs. processes" distinction (e.g., static object-state view vs. time-evolving process view).
* **Part 3: Visual Verification:** A screenshot set showing both the standard view and the color-vision-deficiency-simulated view of your visualization.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this artifact? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Grading Rubric (100 Points Total)

This assignment is worth 100 points total.

**Part 1: Encoding Design Rationale (35 pts)**
* **Encoding Design Justification (20 pts):** Visual channel choices are appropriate to the data type and clearly justified.
* **Accessibility Compliance (15 pts):** Palette is verified legible under the stated CVD simulation.

**Part 2: Unity 6 Visualization Build (60 pts)**
* **Implementation Correctness (35 pts):** Visualization is driven by real data and renders correctly in Unity 6.
* **Objects vs. Processes Distinction (25 pts):** Both a state view and a process view are implemented and clearly distinguished.

**GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.

---
## Assignment Navigation

| ⬅️ Previous | 📝 Index | Next ➡️ |
|:---|:---:|---:|
| [MP 9: Cloud Render Farm Task](../machine_problems/mp9_cloud_render.md) | [[All Assignments]](../README.md#schedule) | [PS 11: GenAI Audit & Process Reflection](../homework/ps11_genai_audit.md) |

[🏠 Course Home](../../README.md) · [⬆️ Module](../../modules/module_10/README.md) · [⏫ Phase](../../phases/phase_04/README.md)
