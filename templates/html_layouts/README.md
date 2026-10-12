# Course Architecture: Golden HTML Templates
**Status:** ACTIVE | **Format:** Dehydrated HTML

## Overview
This directory contains the Single Source of Truth (SSOT) dehydrated HTML layouts for the KDD Lab course ecosystem. These templates enforce strict visual consistency across Canvas and GitHub Pages deployments.

## Hydration Protocol
These templates rely on inline CSS to survive Canvas/LMS sanitization filters. 
* **Primary Hydration Node:** `[Claude][Sov] Requirements-to-Spec`
* **Execution:** AI nodes must execute exact string replacement on `{{VARIABLE_NAME}}` tokens. Nodes are **STRICTLY FORBIDDEN** from altering inline `style="..."` attributes, modifying hex codes, or changing DOM nesting.

## Template Index
* `Golden-Template.html` - Base structural layout.
* `Golden_Template_Universal_Assignment_Page.html` - Standardized homework and machine problem specs.
* `Golden_Template_Universal_Lecture_Page.html` - Individual lecture landing pages with slide/video routing.
* `Golden_Template_Universal_Module_Page.html` - Weekly module wrappers with learning objectives.
* `Golden_Template_Universal_Phase_Page.html` - High-level course progression blocks.
* `Golden_Template_Universal_Term_Project_Proposal_Page.html` - Capstone scope and taxonomy definitions.
