<!--
---
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-11-02"
document_monotonicity: "REQUIRED"
routing_clearance: "PUBLIC - STUDENT FACING"
---
-->

<br>

<div align="center">
 <strong>🏫 Active Course Repositories:</strong> &nbsp;
 🏗️ <a href="https://github.com/kddresearch/course-architecture-base">Architecture Base</a> &nbsp;|&nbsp;
 🤖 CIS 530/730 &nbsp;|&nbsp;
 📊 <a href="https://github.com/kddresearch/cis531-731-2026_fall">CIS 531/731</a> &nbsp;|&nbsp;
 📈 CIS 732 &nbsp;|&nbsp;
 🎨 <a href="https://github.com/kddresearch/cis536-736-2026_fall">CIS 536/736</a> &nbsp;|&nbsp;
 👁️ CIS 798 X &nbsp;|&nbsp;
 🧠 CIS 830
</div>

<div align="center">
 <h1>🎨 CIS 536/736 - Computer Graphics</h1>
 <p>
 <strong>Semester:</strong> Fall 2026 | <strong>Instructor:</strong> William H. Hsu<br>
 <strong>Status:</strong> ACTIVE | <strong>Canvas LMS:</strong> Closed SSO Portal
 </p>
</div>

<hr>

<div align="center">
 <h2>📦 Module 09: Cloud Render Farms — AWS Deadline Cloud</h2>
 <p>
 <strong>Phase:</strong> <a href="../../phases/phase_04">Phase 4: Cloud Rendering & Visualization</a> | <strong>Maintainer:</strong> William H. Hsu
 </p>
</div>

<hr>

## 📖 Overview

This directory contains the localized pedagogical artifacts, demonstration code, and operational files strictly related to **Module 09**. This module spans `3` lectures and focuses on the transition from local workstation computation to distributed cloud rendering, evaluating cost-to-scale tradeoffs, and configuring budget-capped job submissions using AWS Deadline Cloud and the Open Job Description (OpenJD) standard.

> **⚠️ Module Routing Notice**
> Core lecture markdown files and major assignments live in the top-level `lectures/` and `assignments/` directories to maintain structural parity. Use this module directory exclusively for localized data snippets, lab code examples, module-specific images, or environment configs required for this week's content.

<hr>

## 📅 Lecture Schedule & Learning Objectives

| Phase | Module | Lecture(s) | Date(s) |
| :--- | :--- | :--- | :--- |
| **Phase 4:** Cloud Rendering & Visualization | **Module 9:** Cloud Render Farms — AWS Deadline Cloud | **Lectures 29-31** | Mon 02 Nov - Fri 06 Nov 2026 |

---

### Lecture 29: Cloud Rendering Concepts — Distributed Rendering, Cost/Scale Tradeoffs (Mon 02 Nov)

1. **Distributed Rendering Architectures:** Transitioning from monolithic local workstation rendering to distributed, elastic cloud environments (AWS Deadline Cloud).
2. **Fleets and Queues:** Understanding the decoupling of job queues from compute fleets, and configuring worker node allocation strategies.
3. **Cost/Scale Tradeoffs:** Evaluating how instance types (e.g., Spot vs. On-Demand), core counts, memory footprints, and frame sampling affect compute-hour billing.
4. **Scene Complexity Reduction:** Identifying scene optimization strategies—such as Level of Detail (LOD) swaps and reduced sampling—to stay under strict credit allocations.
5. **Open Job Description (OpenJD):** Deconstructing the YAML-based standard for defining compute environments, parameters, and render tasks across agnostic workers.

---

### Lecture 30: AWS Deadline Cloud Walkthrough (Wed 04 Nov)

1. **Environment & Identity Configuration:** Establishing IAM roles, permissions, and AWS Educate/Academy credentials for Deadline Cloud access.
2. **Infrastructure Initialization (Instructor Demo):** Walking through the AWS Console to provision a functional queue and attach a cost-controlled fleet (instructor high-compute demo).
3. **Job Bundle Architecture:** Authoring the `template.yaml` (step definitions) and `parameter_values.yaml` (variables) required for a complete OpenJD job bundle.
4. **CLI Submission Workflows:** Using the AWS Deadline Cloud CLI (`deadline bundle submit`) to dispatch cross-pillar OpenUSD or Blender scenes to the farm.
5. **Worker Telemetry and Logging:** Diagnosing render failures by inspecting AWS CloudWatch logs, container startup overhead, and task chunking latency.

---

### Lecture 31: Lab 5b — Capped AWS Educate/Academy Credit Render Task (Fri 06 Nov)

1. **Budget Estimation (Pre-Flight):** Calculating a strict mathematical compute-hour and USD cost estimate before deploying a workload to prevent quota exhaustion.
2. **OpenUSD Cross-Pillar Audit:** Auditing local OpenUSD stages to ensure all texture and payload references use portable, relative paths before cloud upload.
3. **Cost-Aware Job Execution:** Disbatching a constrained render job (e.g., 1080p, 128 SPP) that remains safely within the 80% threshold of the capped credit allocation.
4. **Local vs. Cloud Render Parity:** Validating that the cloud-rendered output frames match expected local lighting and materials (MSE/PSNR checks).
5. **Cost Discrepancy Analysis:** Executing a formal post-mortem to compare the estimated compute budget against the actual telemetry and overhead billed by AWS.[cite: 65.3]

<hr>

## 🗂️ Module Artifacts

| Subdirectory / File | Description |
| :--- | :--- |
| `lab05b.ipynb` | Lab 5b: Capped AWS Educate/Academy Credit Render Task & OpenUSD Cross-Pillar Audit notebook. |
| `README.md` | This module overview and index. |

<hr>

## 🔗 Associated Course Materials

* **Lectures:** [Lecture 29](../../lectures/lecture-29.md) | [Lecture 30](../../lectures/lecture-30.md) | [Lecture 31](../../lectures/lecture-31.md)
* **Active Assignments:** [Lab 5b Notebook](../../assignments/labs/lab05b.ipynb) | [PS 8: Interim Report](../../assignments/homework/ps8_interim_report.md) *(Due Thu 12 Nov)* | [MP 9: Cloud Render Farm Task](../../assignments/machine_problems/mp9_cloud_render.md) *(Released Wed 11 Nov)*
* **Required Reading:** AWS Deadline Cloud User Guide ("How It Works", "Getting Started", "Queues, Fleets & Budgets").

<hr>

## 🦅 Standard Operating Procedures (SOPs)

All instructional staff interacting with this module directory must adhere to the following protocols:

* **Protocol 1 (SSOT Paramountcy):** Do not update content directly in Canvas. Markdown files and localized scripts in this directory must be updated first, and the HTML mirrored from them to preserve the GitOps ledger.
* **Protocol 2 (Artifact Segregation):** Do not duplicate master assignment files or lecture notes here. Use the "Associated Course Materials" cross-links provided above.
* **Protocol 3 (Deprecation):** Ensure any outdated code snippets in this module are formally tagged as `[ARCHIVED]` if updating for a new semester to prevent student confusion.

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
