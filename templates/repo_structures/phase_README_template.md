<!--
---
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-10-12"
document_monotonicity: "REQUIRED"
routing_clearance: "INTERNAL - RESTRICTED TO COURSE MAINTAINERS"
---
-->

<br>

<div align="center">
 <strong>🏫 Active Course Repositories:</strong> &nbsp;
 🏗️ <a href="https://github.com/kddresearch/course-architecture-base">Architecture Base</a> &nbsp;|&nbsp;
 🤖 CIS 530/730 &nbsp;|&nbsp;
 📊 CIS 531/731 &nbsp;|&nbsp;
 📈 CIS 732 &nbsp;|&nbsp;
 🎨 CIS 536/736 &nbsp;|&nbsp;
 👁️ CIS 798 X &nbsp;|&nbsp;
 🧠 CIS 830
</div>

<div align="center">
 <h1>{{EMOJI}} {{COURSE_ID}} - {{COURSE_TITLE}}</h1>
 <p>
 <strong>Semester:</strong> {{SEMESTER}} | <strong>Instructor:</strong> William H. Hsu<br>
 <strong>Status:</strong> {{ACTIVE / ARCHIVED}} | <strong>Canvas LMS:</strong> <a href="{{CANVAS_URL}}">Closed SSO Portal</a>
 </p>
</div>

<hr>

<div align="center">
 <h2>📂 Phase {{PHASE_NUMBER}}: {{PHASE_TITLE}}</h2>
 <p>
 <strong>Status:</strong> {{ACTIVE / ARCHIVED}} | <strong>Maintainer:</strong> {{MAINTAINER_NAME}}
 </p>
</div>

<hr>

## 📖 Overview

This directory defines **Phase {{PHASE_NUMBER}}** of the course progression. Phases act as high-level chronological and thematic wrappers that group multiple weekly modules together based on major pedagogical milestones (e.g., the OSEMN framework transitions). 

This phase specifically focuses on `{{PHASE_OBJECTIVE_SUMMARY}}`.

> **⚠️ Phase Routing Notice**
> This directory acts primarily as an organizational wrapper. Ensure that granular lecture assets, code snippets, and weekly materials are placed within their respective `module_XX/` subdirectories, rather than loosely at the root of the phase directory.

<hr>

## 🗂️ Phase Structure (Included Modules)

| Module Directory | Topic / Description |
| :--- | :--- |
| `module_{{XX}}/` | **{{MODULE_TITLE_1}}:** {{Brief description of module focus}} |
| `module_{{YY}}/` | **{{MODULE_TITLE_2}}:** {{Brief description of module focus}} |
| `README.md` | This phase overview and index. |

<hr>

## 🦅 Standard Operating Procedures (SOPs)

All instructional staff interacting with this phase directory must adhere to the following protocols:

* **Protocol 1 (Phase Cohesion):** Ensure all modules contained within this phase cleanly map to the overarching phase objective defined in the syllabus taxonomy.
* **Protocol 2 (Navigation):** Maintain cross-links in the module READMEs pointing back to this Phase root to preserve GitOps navigability.
* **Protocol 3 (Milestone Anchoring):** Ensure the terminal module of this phase explicitly prepares the student for the associated major assignment/milestone.

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
