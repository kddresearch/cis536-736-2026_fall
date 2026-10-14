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
 <h2>📦 Module {{MODULE_NUMBER}}: {{MODULE_TITLE}}</h2>
 <p>
 <strong>Phase:</strong> <a href="../../phases/phase_{{PHASE_NUMBER}}">Phase {{PHASE_NUMBER}}</a> | <strong>Maintainer:</strong> {{MAINTAINER_NAME}}
 </p>
</div>

<hr>

## 📖 Overview

This directory contains the localized pedagogical artifacts, demonstration code, and operational files strictly related to **Module {{MODULE_NUMBER}}**. This module spans `{{NUMBER_OF_LECTURES}}` lectures and focuses on `{{DETAILED_TOPIC_SUMMARY}}`.

> **⚠️ Module Routing Notice**
> Core lecture markdown files and major assignments live in the top-level `lectures/` and `assignments/` directories to maintain structural parity. Use this module directory exclusively for localized data snippets, lab code examples, module-specific images, or environment configs required for this week's content.

<hr>

## 🗂️ Module Artifacts

| Subdirectory / File | Description |
| :--- | :--- |
| `{{demo_script.py}}` | {{Optional: Localized demonstration code utilized during lectures}} |
| `{{dataset_sample.csv}}` | {{Optional: Bounded data sample for in-class workshops}} |
| `README.md` | This module overview and index. |

<hr>

## 🔗 Associated Course Materials

* **Lectures:** [Lec {{XX}}](../../lectures/lecture-{{XX}}.md) | [Lec {{YY}}](../../lectures/lecture-{{YY}}.md)
* **Active Assignments:** [{{ASSIGNMENT_ID}}](../../assignments/homework/{{assignment_file.md}})
* **Required Reading:** {{READING_REFERENCE_AND_LINK}}

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
