<!--
---
yaml_schema_version: "2.2"
document_version: "1.1"
document_last_updated_date: "2026-11-26"
document_last_updated_time: "23:43:00 CST"
document_monotonicity: "REQUIRED"
routing_clearance: "PUBLIC - STUDENT FACING"
template_metadata:
  template_id: "phases_index_README_template"
  description: "Generic course progression index README template for phases/ with 5 phases by default"
  target_filename: "README.md"
  hydration_instructions:
    step_1: "Replace all {{PLACEHOLDER}} tokens with course-specific metadata, dates, module ranges, lecture ranges, and phase objectives."
    step_2: "Add or remove phase_XX rows in the Phase Directory Structure table if the course uses more or fewer than 5 phases."
    step_3: "Ensure relative links to ../modules/module_XX and phase_XX/ match the target course repository."
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
 <h2>📂 Directory: phases/ (Course Progression Index)</h2>
 <p>
 <strong>Status:</strong> {{DIRECTORY_STATUS}} | <strong>Maintainer:</strong> {{MAINTAINER_NAME}}
 </p>
</div>

<hr>

## 📖 Overview

This directory acts as the master index for the high-level course progression. **Phases** are conceptual, chronological wrappers that group multiple weekly modules together based on major pedagogical milestones or methodological pillars (e.g., {{PHASE_GROUPING_EXAMPLES}}).

Together the phases cover **{{MODULE_RANGE_SUMMARY}}** and **{{LECTURE_COUNT_SUMMARY}}**, meeting {{MEETING_DAYS}} from {{TERM_START_DATE}} through {{TERM_END_DATE}}. The progression runs from {{PHASE_01_SHORT_GOAL}} (Phase I), through {{PHASE_02_SHORT_GOAL}} (Phase II), {{PHASE_03_SHORT_GOAL}} (Phase III), and {{PHASE_04_SHORT_GOAL}} (Phase IV), to {{PHASE_05_SHORT_GOAL}} (Phase V). Class-free days and breaks fall across the term as scheduled in {{SCHEDULE_BREAKS_SUMMARY}}.

> **⚠️ Parent Directory Routing Notice**
> This directory is strictly for structural routing. All actual course content, scripts, and assignment assets must be placed inside the granular `module_XX/` subdirectories located in the `modules/` repository root or within their respective phase subdirectories.

<hr>

## 🗂️ Phase Directory Structure

| Phase Directory | Pedagogical Objective |
| :--- | :--- |
| [`phase_01/`](phase_01/) | **🔵 Phase I: {{PHASE_01_TITLE}}:** Modules {{PHASE_01_MODULES}} ({{PHASE_01_DATES}}; {{PHASE_01_LECTURES}}). {{PHASE_01_OBJECTIVE}} |
| [`phase_02/`](phase_02/) | **🟢 Phase II: {{PHASE_02_TITLE}}:** Modules {{PHASE_02_MODULES}} ({{PHASE_02_DATES}}; {{PHASE_02_LECTURES}}). {{PHASE_02_OBJECTIVE}} |
| [`phase_03/`](phase_03/) | **🟠 Phase III: {{PHASE_03_TITLE}}:** Modules {{PHASE_03_MODULES}} ({{PHASE_03_DATES}}; {{PHASE_03_LECTURES}}). {{PHASE_03_OBJECTIVE}} |
| [`phase_04/`](phase_04/) | **🟣 Phase IV: {{PHASE_04_TITLE}}:** Modules {{PHASE_04_MODULES}} ({{PHASE_04_DATES}}; {{PHASE_04_LECTURES}}). {{PHASE_04_OBJECTIVE}} |
| [`phase_05/`](phase_05/) | **🟤 Phase V: {{PHASE_05_TITLE}}:** Modules {{PHASE_05_MODULES}} ({{PHASE_05_DATES}}; {{PHASE_05_LECTURES}}). {{PHASE_05_OBJECTIVE}} |

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
