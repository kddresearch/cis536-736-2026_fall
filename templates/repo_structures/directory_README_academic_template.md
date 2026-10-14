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
 <h2>📂 Directory: {{DIRECTORY_NAME}}</h2>
 <p>
 <strong>Status:</strong> {{ACTIVE / ARCHIVED}} | <strong>Maintainer:</strong> {{MAINTAINER_NAME}}
 </p>
</div>

<hr>

## 📖 Overview

{{Replace this paragraph with a concise description of the directory. Explain what specific pedagogical artifacts, codebases, or operational files are stored here, and how they relate to the broader execution of the course.}}

> **⚠️ Sub-Directory Routing Notice**
> This directory serves a specific pedagogical function. Ensure that artifacts committed here belong in this directory, rather than a different course module or upstream folder.

<hr>

## 🗂️ Directory Structure

| Subdirectory / File | Description |
| :--- | :--- |
| `{{sub_dir_1}}/` | {{Description of contents}} |
| `{{file_1.md}}` | {{Description of file}} |
| `README.md` | This file. |

<hr>

## 🦅 Standard Operating Procedures (SOPs)

All instructional staff interacting with this directory must adhere to the following protocols:

* **Protocol 1 (SSOT Paramountcy):** Do not update content directly in Canvas. Markdown files in this directory must be updated first, and the HTML mirrored from them to preserve the GitOps ledger.
* **Protocol 2:** {{e.g., Required metadata headers for homework assignments}}
* **Protocol 3:** {{e.g., Archival procedures for past semesters}}

<br>
<hr>
<p align="center">
 <i>Maintained by the <a href="https://github.com/kddresearch">Knowledge Discovery in Databases (KDD) Lab</a> at Kansas State University.</i>
</p>
