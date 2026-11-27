<!--
---
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-11-26"
document_last_updated_time: "23:48:00 CST"
document_monotonicity: "REQUIRED"
routing_clearance: "PUBLIC - STUDENT FACING"
template_metadata:
  template_id: "modules_index_README_template"
  description: "Generic weekly modules index README template organized across 5 phases by default"
  target_filename: "README.md"
  hydration_instructions:
    step_1: "Replace all {{PLACEHOLDER}} tokens with course-specific metadata, dates, module topics, and lecture ranges."
    step_2: "Add or remove module rows or phase sections to match the target course's module and phase count (defaulting to 5 phases)."
    step_3: "Ensure relative links to module_XX/ and ../lectures/lecture_XX.md match the target course repository."
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
 <h1>{{COURSE_BADGE_EMOJI}} {{COURSE_NUMBER}} - {{COURSE_TITLE}}</h1>
 <p>
 <strong>Semester:</strong> {{SEMESTER}} | <strong>Instructor:</strong> {{INSTRUCTOR_NAME}}<br>
 <strong>Status:</strong> {{COURSE_STATUS}} | <strong>Canvas LMS:</strong> <a href="{{CANVAS_URL}}">Closed SSO Portal</a>
 </p>
</div>

<hr>

<div align="center">
 <h2>📦 Directory: modules/ (Weekly Content Index by Phase)</h2>
 <p>
 <strong>Status:</strong> {{DIRECTORY_STATUS}} | <strong>Maintainer:</strong> {{MAINTAINER_NAME}}
 </p>
</div>

<hr>

## 📖 Overview

This directory acts as the master index for the weekly, topic-based execution of the course, organized by {{PHASE_ORGANIZATION_FRAMEWORK}}. Each **Module** spans {{MODULE_SPAN_DESCRIPTION}} and encompasses a specific technical concept, homework assignment, or laboratory exercise.

> **📚 Quick Links:**
> [📚 View Master Lectures Schedule](../lectures/README.md) | [📂 Phases Index](../phases/README.md) | [📝 Assignments Index](../README.md#schedule)

<hr>

## 🗂️ Modules Index by Phase

### 🔵 PHASE 1: {{PHASE_01_TITLE}}
| Module | Date Range | Topic & Description | Lectures |
| :--- | :--- | :--- | :--- |
| [`module_00/`](module_00/) | {{MOD_00_DATES}} | **{{MOD_00_TITLE}}:** {{MOD_00_DESC}} | {{MOD_00_LECTURES}} |
| [`module_01/`](module_01/) | {{MOD_01_DATES}} | **{{MOD_01_TITLE}}:** {{MOD_01_DESC}} | {{MOD_01_LECTURES}} |
| [`module_02/`](module_02/) | {{MOD_02_DATES}} | **{{MOD_02_TITLE}}:** {{MOD_02_DESC}} | {{MOD_02_LECTURES}} |

### 🟢 PHASE 2: {{PHASE_02_TITLE}}
| Module | Date Range | Topic & Description | Lectures |
| :--- | :--- | :--- | :--- |
| [`module_03/`](module_03/) | {{MOD_03_DATES}} | **{{MOD_03_TITLE}}:** {{MOD_03_DESC}} | {{MOD_03_LECTURES}} |
| [`module_04/`](module_04/) | {{MOD_04_DATES}} | **{{MOD_04_TITLE}}:** {{MOD_04_DESC}} | {{MOD_04_LECTURES}} |

### 🟠 PHASE 3: {{PHASE_03_TITLE}}
| Module | Date Range | Topic & Description | Lectures |
| :--- | :--- | :--- | :--- |
| [`module_05/`](module_05/) | {{MOD_05_DATES}} | **{{MOD_05_TITLE}}:** {{MOD_05_DESC}} | {{MOD_05_LECTURES}} |
| [`module_06/`](module_06/) | {{MOD_06_DATES}} | **{{MOD_06_TITLE}}:** {{MOD_06_DESC}} | {{MOD_06_LECTURES}} |

### 🟣 PHASE 4: {{PHASE_04_TITLE}}
| Module | Date Range | Topic & Description | Lectures |
| :--- | :--- | :--- | :--- |
| [`module_07/`](module_07/) | {{MOD_07_DATES}} | **{{MOD_07_TITLE}}:** {{MOD_07_DESC}} | {{MOD_07_LECTURES}} |
| [`module_08/`](module_08/) | {{MOD_08_DATES}} | **{{MOD_08_TITLE}}:** {{MOD_08_DESC}} | {{MOD_08_LECTURES}} |

### 🟤 PHASE 5: {{PHASE_05_TITLE}}
| Module | Date Range | Topic & Description | Lectures |
| :--- | :--- | :--- | :--- |
| [`module_09/`](module_09/) | {{MOD_09_DATES}} | **{{MOD_09_TITLE}}:** {{MOD_09_DESC}} | {{MOD_09_LECTURES}} |
| [`module_10/`](module_10/) | {{MOD_10_DATES}} | **{{MOD_10_TITLE}}:** {{MOD_10_DESC}} | {{MOD_10_LECTURES}} |
| [`module_11/`](module_11/) | {{MOD_11_DATES}} | **{{MOD_11_TITLE}}:** {{MOD_11_DESC}} | {{MOD_11_LECTURES}} |
| [`module_12/`](module_12/) | {{MOD_12_DATES}} | **{{MOD_12_TITLE}}:** {{MOD_12_DESC}} | {{MOD_12_LECTURES}} |
| [`module_13/`](module_13/) | {{MOD_13_DATES}} | **{{MOD_13_TITLE}}:** {{MOD_13_DESC}} | {{MOD_13_LECTURES}} |

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
````[cite: 6]
