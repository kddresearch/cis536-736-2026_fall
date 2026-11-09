<!--
---
yaml_schema_version: "2.2"
document_version: "2.0"
document_last_updated_date: "2026-11-08"
document_monotonicity: "REQUIRED"
routing_clearance: "INTERNAL - RESTRICTED TO COURSE MAINTAINERS"
module_metadata:
  module_number: 10
  module_identifier: "color_theory_information_visualization_i"
  parent_phase:
    phase_number: 4
    phase_identifier: "visualization_and_special_topics"
    color_scheme: "Purple"
    hex_code: "#800080"
    badge_pill: "🟣"
  module_color_tokens:
    tint_class: "[!IMPORTANT]"
    shade_hex: "#660066"
    light_tint_hex: "#f3e6f3"
---
-->

**🏫 Active Course Repositories:** 🏗️ [Architecture Base](https://github.com/kddresearch/course-architecture-base) &nbsp;|&nbsp; 🤖 CIS 530/730 &nbsp;|&nbsp; 📊 [CIS 531/731](https://github.com/kddresearch/cis531-731-2026_fall) &nbsp;|&nbsp; 📈 CIS 732 &nbsp;|&nbsp; 🎨 [CIS 536/736](https://github.com/kddresearch/cis536-736-2026_fall) &nbsp;|&nbsp; 👁️ CIS 798 X &nbsp;|&nbsp; 🧠 CIS 830

# 🎨 CIS 536/736 - Computer Graphics and Animation

**Semester:** Fall 2026 | **Instructor:** William H. Hsu  
**Status:** ACTIVE | **Canvas LMS:** [Closed SSO Portal](https://ksu.instructure.com)

---

## 📦 Module 10: Color Theory & Information Visualization I

**Parent Phase:** 🟣 [Phase 4: Visualization & Special Topics](../phases/) (`#800080`) | **Maintainer:** Course Maintainers / KDD Lab

---

> [!IMPORTANT]
> **Module Color Palette: Purple Token (`#800080`)**  
> **Module Scope:** Color theory and human visual perception, opponent color spaces (`oRGB`), GAMES101 color science principles, visual encoding, Kieran Healy's *Data Visualization* (Ch. 3–4), and Unity UI Toolkit Runtime Charts.

---

## 📖 Overview

This directory defines **Module 10** for CIS 536/736 (Fall 2026). For the complete canonical specification, detailed lecture objectives, and localized reading sections, see [Module 10 Specification (`module_10.md`)](./module_10.md).

This module establishes the foundational principles of color theory, perceptual color spaces, and information visualization techniques, bridging computer graphics rendering with rigorous data representation.

> **⚠️ Module Routing Notice**  
> Core lecture markdown files and major assignments live in the top-level `lectures/` and `assignments/` directories to maintain structural parity. Use this module directory exclusively for localized data snippets, lab code examples, module-specific images, or environment configs required for this week's content.

---

## 🗂️ Module Structure & Artifacts

| Subdirectory / File | Topic / Description | Lectures | Date Range | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| `module_10.md` | **[Module 10 Canonical Specification](./module_10.md):** Color theory, opponent color spaces (`oRGB`), visual encoding, and runtime charts. | Lectures 28–30 | Nov 2026 | [Module 10 Readings](#-associated-course-materials) |
| `README.md` | This module overview and index. | — | — | — |

---

## 🔗 Associated Course Materials

* **Lectures:** [Lecture 28: Color Theory & Human Perception](../../lectures/lecture_28.md) | [Lecture 29: Opponent Color Spaces & oRGB](../../lectures/lecture_29.md) | [Lecture 30: Visual Encoding & Unity UI Charts](../../lectures/lecture_30.md)
* **Active Assignments:** [Unity UI Toolkit Runtime Charts Lab](../../assignments/homework/lab_viz_charts.ipynb)
* **Assigned Readings (Module-Aligned):**
  * **Theoretical “Why”:** ["oRGB: A Practical Opponent Color Space" (Bratkova et al.)](https://graphics.stanford.edu/~boulos/papers/orgb_sig.pdf); [GAMES101 Lecture 20](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html)
  * **Practical “How”:** [*Data Visualization* (Healy), Ch. 3–4](https://socviz.co/); [Unity UI Toolkit Runtime Charts Documentation](https://docs.unity3d.com/)

---

## 🦅 Standard Operating Procedures (SOPs)

All instructional staff interacting with this module directory must adhere to the following protocols:

* **Protocol 1 (SSOT Paramountcy):** Do not update content directly in Canvas. Markdown files and localized scripts in this directory must be updated first, and the HTML mirrored from them to preserve the GitOps ledger.
* **Protocol 2 (Artifact Segregation):** Do not duplicate master assignment files or lecture notes here. Use the "Associated Course Materials" cross-links provided above.
* **Protocol 3 (Deprecation):** Ensure any outdated code snippets in this module are formally tagged as `[ARCHIVED]` if updating for a new semester to prevent student confusion.

---

*Maintained by the [Knowledge Discovery in Databases (KDD) Lab](https://github.com/kddresearch) at Kansas State University.*