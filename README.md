<!--
yaml_schema_version: "2.2"
document_version: "1.3"
document_last_updated_date: "2026-10-06"
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
 <strong>Status:</strong> ACTIVE | <strong>Canvas LMS:</strong> <a href="https://k-state.instructure.com/courses/">Closed SSO Portal</a>
 </p>
</div>

<hr>

## 📖 Overview

An advanced exploration of modern computer graphics workflows, bridging fundamental 3D representation mathematics with state-of-the-art neural graphic pipelines. This course emphasizes programmatic asset generation, integrating local generative models (Shap-E/Point-E), comprehensive OpenUSD-to-Unity rigging frameworks, and high-performance AWS Deadline Cloud rendering to navigate local compute constraints.

> **⚠️ Single Source of Truth (SSOT) Notice**
> This repository is the designated SSOT for all public-facing course materials, assignments, and infrastructure. While grades and closed discussions occur in the Canvas SSO environment, all operational state, Machine Problems (MPs), codebases, and structural rubrics MUST be committed here before being mirrored. A non-paywalled HTML mirror of Canvas content is derived from this repository.

<hr>

## 🗂️ Repository Structure

| Directory / File | Description |
| :--- | :--- |
| `.github/` | CI/CD workflows and sync configurations (e.g., `sync_architecture_template.yml`). |
| `admin/` | Course policies and syllabus resources. |
| `assignments/` | Homework, machine problems, project milestones, and labs. |
| `exams_quizzes/` | Quizzes, exams, and other assessments  **that are *taken*, not submitted***. |
| `lectures/` | Core lecture materials and related assets. |
| `modules/` | Weekly/topic-based module directories (`module_00` to `module_13`). |
| `phases/` | High-level course progression phase wrappers (`phase_01` to `phase_05`). |
| `platforms/` | Platform-specific configurations and assets (Canvas, Gemini, Piazza). |
| `reading_materials/` | Assigned papers, texts, and supplementary reading. |
| `slides/` | Slide decks and archived presentations. |
| `term_project/` | Rubrics, sprint specs, and `docker` compute configs for the term project. |
| `README.md` | This file. |

<hr>

## 🚀 Quick Start & Execution

**1. Prerequisites**
* Git (for repository cloning and SSOT synchronization)
* Docker (for DevContainer execution and generative modeling compute fallbacks)

**2. Initialization**
```bash
git clone [https://github.com/kddresearch/cis536-736-2026_fall.git](https://github.com/kddresearch/cis536-736-2026_fall.git)
cd cis536-736-2026_fall
# Proceed to /admin/syllabus for setup and phase alignment
```

## 🦅 Lab Execution Protocols

All Teaching Assistants and GRAs operating in this repository fall under the KDD Lab **Keep Flying Directive v2.1**.

* **Observable State:** Progress is measured by commits, drafts, logs, and reproducible outputs—not intentions.
* **The Triad:** When opening an issue or Pull Request, provide explicit goals, current blockers, and proposed next actions.
* **Artifact-Gated Routing:** Do not request synchronous meetings for routine status updates. Push your grading state or syllabus updates to this repository first.

## 👥 Instructional Staff

| Role | Name | GitHub Handle |
| --- | --- | --- |
| **Instructor** | William H. Hsu | [@banazir](https://github.com/banazir) |
| **Teaching Asssistant** | Joshua Garcia | |

## 📂 Repository Structure (Full Tree)

<details>
<summary><b>Click to expand: Course Repository Template Directory Tree</b></summary>

```
Folder PATH listing for volume Windows
Volume serial number is 60E2-4FEF
C:.
|   README.md
|   structure_dump.txt
|   
+---.github
|   \---ISSUE_TEMPLATE
|           .gitkeep
|           
+---admin
|   +---policies
|   |       .gitkeep
|   |       
|   \---syllabus
|           .gitkeep
|           lecture_schedule.md
|           
+---assignments
|   +---homework
|   |       .gitkeep
|   |       week5.ipynb
|   |       week6.ipynb
|   |       week7.ipynb
|   |       
|   +---machine_problems
|   |   |   .gitkeep
|   |   |   
|   |   \---mp3_pbr_shadergraph
|   |           student_submission_README_template.md
|   |           
|   \---project_sprints
|           .gitkeep
|           
+---lectures
|       lecture-12.md
|       lecture-13.md
|       lecture-14.md
|       lecture-15.md
|       lecture-16.md
|       lecture-17.md
|       
+---modules
|   +---module_00
|   |       .gitkeep
|   |       
|   +---module_01
|   |       .gitkeep
|   |       
|   +---module_02
|   |       .gitkeep
|   |       
|   +---module_03
|   |       .gitkeep
|   |       
|   +---module_04
|   |       .gitkeep
|   |       
|   +---module_05
|   |       .gitkeep
|   |       README.md
|   |       
|   +---module_06
|   |       .gitkeep
|   |       README.md
|   |       
|   +---module_07
|   |       .gitkeep
|   |       
|   +---module_08
|   |       .gitkeep
|   |       
|   +---module_09
|   |       .gitkeep
|   |       
|   +---module_10
|   |       .gitkeep
|   |       
|   +---module_11
|   |       .gitkeep
|   |       
|   +---module_12
|   |       .gitkeep
|   |       
|   \---module_13
|           .gitkeep
|           
+---platforms
|   +---canvas
|   |       .gitkeep
|   |       
|   +---gemini
|   |       .gitkeep
|   |       
|   +---github
|   |       github_intro.md
|   |       
|   +---moodle
|   |       .gitkeep
|   |       
|   \---piazza
|           .gitkeep
|           
\---slides
    |   README.md
    |   
    \---archive
            README.md
```
</details>
