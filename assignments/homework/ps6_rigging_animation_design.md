<!-- ASSIGNMENT-META
id: PS 6
phase: Phase 3: Procedural Generation, Interchange & Character Systems
module: Module 7: Animation Graphs & Motion Matching
released: Sat 17 Oct 2026
due: Mon 26 Oct 2026
prev: ../machine_problems/mp5_structured_streaming.md
next: ../machine_problems/mp7_gsplat.md
-->
# PS 6: Rigging & Animation Graph Design
**Forward/Inverse Kinematics Theory and a Blend-Tree / Motion-Matching Design Document**

**🗓️ Released:** Sat 17 Oct 2026 | **📅 Due:** Mon 26 Oct 2026 | **💯 Points:** 100 | **👥 Team:** Individual

> **🚨 CRITICAL SUBMISSION POLICY**
> **Design Document, Not a Full Build:** This Problem Set is theoretical/design-focused. You are not required to fully implement your animation graph in Unity 6 for this submission — Lab 4a/4b cover hands-on implementation. You must, however, produce a rig and graph design precise enough that it could be implemented without further clarification. Submit as a PDF upload to Canvas (no code repo required for this PS).

## 1. Overview
Answer kinematics and constraint theory questions using notation from Lectures 19–20, and design a Unity 6 Animator/Animation Graph for a chosen locomotion set that could plausibly feed a Motion Matching system.

**Required Readings:**
* **Blender 4.2 Manual — Rigging:** Armatures & Bone Constraints, Control Rig Workflow.
* **Unity 6 Manual — Animation Rigging & Animator Controllers:** Two-Bone IK Solver.

## 2. Initiating Your Draft (The AI Kit Procedure)
Use the Generative AI Kit to help structure your submission. Follow these stages to systematically break down your assignment:

1. **Theory Concept Mapping:** Have the AI Kit outline the standard matrix composition for a 3-bone forward kinematics chain.
2. **IK Edge Cases:** Ask the AI Kit to detail pole-vector ambiguity scenarios for two-bone IK.
3. **Constraint Comparison:** Draft the operational differences between Copy Transform and Limit Rotation.
4. **State Machine Blueprinting:** Use the AI to generate a structural map for a 4-state locomotion blend tree with requisite transition parameters.
5. **Failure Mode Analysis:** Brainstorm plausible blending failure modes (e.g., foot sliding) and mitigation strategies with the AI Kit.

## 3. Required Document Structure
Your submission must be a **Single PDF** containing the following sections exactly as named below.

* **Part 1: Kinematics Derivation:** Derive the forward-kinematics transform for a 3-bone chain and explain how a two-bone IK solver inverts this relationship.
* **Part 2: Constraint Theory:** Explain the difference between Copy Transform and Limit Rotation constraints, and define pole vector implications.
* **Part 3: Animation Graph Diagram:** A state-machine or blend-tree diagram covering at least 4 locomotion states.
* **Part 4: Motion Matching Parameters:** Trajectory parameters (root velocity, facing direction, etc.) for at least 2 states.
* **Part 5: Failure Mode Mitigation:** Identify one failure mode your design is vulnerable to and state how to mitigate it.
* **GenAI Audit:** Did you use the Generative AI Kit to synthesize this artifact? You must include your `GenAI.pdf` log or explicitly state "No GenAI Used."

## 4. Grading Rubric (100 Points Total)

This assignment is worth 100 points total.

**Part 1: Kinematics & Constraint Theory (40 pts)**
* **Kinematics Derivation Correctness (25 pts):** FK/IK derivations are mathematically correct and composition order is explicit.
* **Constraint Theory Accuracy (15 pts):** Copy Transform vs. Limit Rotation distinction and pole-vector explanation are both correct.

**Part 2: Animation Graph & Motion Matching Design Document (55 pts)**
* **Animation Graph Design Completeness (30 pts):** State machine/blend tree covers ≥4 states with correct transition parameters.
* **Motion Matching & Failure-Mode Analysis (25 pts):** Trajectory parameters are well-specified and the identified failure mode/mitigation is plausible and specific.

**GenAI Verification (5 pts):** Includes the required GenAI log (`GenAI.pdf`) or explicitly states AI was not used.

---
## Assignment Navigation

| ⬅️ Previous | 📝 Index | Next ➡️ |
|:---|:---:|---:|
| [MP 5: Data Analytics Pipelines (Structured Streaming)](../machine_problems/mp5_structured_streaming.md) | [[All Assignments]](../README.md#schedule) | [MP 7: 3D Gaussian Splatting (GSplat)](../machine_problems/mp7_gsplat.md) |

[🏠 Course Home](../../README.md) · [⬆️ Module](../../modules/module_07/README.md) · [⏫ Phase](../../phases/phase_03/README.md)
