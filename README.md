<!--
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-09-30"
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
 <strong>Semester:</strong> Fall 2026 | <strong>Principal Investigator / Instructor:</strong> William H. Hsu, Ph.D.<br>
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
| `assignments/` | Homework, machine problems, and project sprint specifications. |
| `lectures/` | Core lecture materials and related assets. |
| `modules/` | Weekly/topic-based module directories (`module_00` to `module_13`). |
| `phases/` | High-level course progression phase wrappers (`phase_01` to `phase_05`). |
| `platforms/` | Platform-specific configurations and assets (Canvas, Gemini, Piazza). |
| `reading_materials/` | Assigned papers, texts, and supplementary reading. |
| `slides/` | Slide decks and archived presentations. |
| `term_project/` | Final project guidelines, rubrics, and `docker` compute configurations. |
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
