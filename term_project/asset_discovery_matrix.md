# Architectural Feasibility Report: GitOps-Driven Canvas IMSCC Pipeline

This pipeline is architecturally feasible using documented Canvas APIs and IMS Common Cartridge v1.3 specifications, but requires careful handling of authentication, idempotency, and the distinction between SaaS Canvas (Instructure) and self-hosted Canvas LMS.[^1][^2][^3][^4]

---

# 1. API & Documentation Deep-Dive

## 1.1 Canvas LMS REST API Endpoints

The Instructure Canvas API provides full CRUD operations for the core course artifacts you need. All endpoints require OAuth2 bearer token authentication and are scoped per-resource.[^5][^6]

### Core Endpoints for Course Content

| Resource      | Create                                             | Update                                                       |
|---------------|-----------------------------------------------------|--------------------------------------------------------------|
| Modules       | POST /api/v1/courses/:course_id/modules            | PUT /api/v1/courses/:course_id/modules/:id                  |
| Module Items  | POST /api/v1/courses/:course_id/modules/:module_id/items | PUT /api/v1/courses/:course_id/modules/:module_id/items/:id |
| Pages         | POST /api/v1/courses/:course_id/pages              | PUT /api/v1/courses/:course_id/pages/:url_or_id             |
| Assignments   | POST /api/v1/courses/:course_id/assignments        | PUT /api/v1/courses/:course_id/assignments/:id              |
| Rubrics       | POST /api/v1/courses/:course_id/rubrics            | PUT /api/v1/courses/:course_id/rubrics/:id                  |
| Front Page    | —                                                   | PUT /api/v1/courses/:course_id/front_page                   |

### Authentication & Scopes

- Canvas uses OAuth2 bearer tokens via `Authorization: Bearer <token>`.[^1][^2][^3]
- Each endpoint requires a specific OAuth scope.
- Developer Keys (OAuth2 client ID/secret) are created in Canvas Admin UI and exchanged via `/login/oauth2/token`.[^6][^9]

---

## 1.2 IMS Common Cartridge (IMSCC) v1.3 Specification

IMS Global (now 1EdTech) Common Cartridge v1.3 defines a ZIP-based interchange format with a mandatory `imsmanifest.xml` at the root.[^4][^10]

### Required Package Structure

```
course-cartridge.imscc/
├── imsmanifest.xml
├── webcontent/
│   ├── index.html
│   └── assets/
└── content1/
    ├── descriptor.xml
    └── associated files...
```

- Manifest must declare `<schema>IMS Common Cartridge</schema>` and `<schemaversion>1.3.0</schemaversion>`.
- All resources defined with `<resource>` elements.[^4]
- Supports Learning Application Objects (QTI assessments, discussions, etc.).[^4]
- Relative paths required; shared content allowed in `webcontent/`.

### Content Types Supported in CC v1.3

| Type            | Description                         | Notes                                                         |
|-----------------|-------------------------------------|---------------------------------------------------------------|
| Web Content     | HTML, images, PDF                   | `intendeduse` attribute supported.[^4]                        |
| Assessment      | QTI v1.2.1 (6 question types)       | MCQ, T/F, essay, fill-in-blank, pattern match.[^4]           |
| Discussion      | Placeholder for LMS-native topic    | LMS creates native discussion on import.[^4]                 |
| LTI Link        | Basic LTI v1.0                      | OAuth-secured external tool.[^4][^5]                         |
| Question Bank   | QTI objectbank                      | Max 1; cannot reference questions in same cartridge.[^4]     |

---

# 2. Adversarial Interoperability Audit: GitHub Actions → Canvas

## 2.1 Feasibility Assessment

Verdict: **Production-ready with caveats.**

Pipeline steps:

1. Compile Markdown/LaTeX → HTML (Pandoc or custom scripts).  
2. Package as IMSCC (generate manifest, ZIP).  
3. Deploy to Canvas via API (pages, modules, assignments, rubrics).[^2][^3][^1]

### Critical Integration Bottlenecks

| Risk                     | Severity | Mitigation                                                                 |
|--------------------------|----------|----------------------------------------------------------------------------|
| API Rate Limiting        | Medium   | Implement batching + exponential backoff.[^5]                              |
| Idempotency              | High     | Use GET-before-POST; track IDs in `.canvas-state.json`.[^1][^3]            |
| Authentication Friction  | Medium   | Store tokens in GitHub Secrets; implement OAuth2 refresh.[^2][^6]          |
| IMSCC Import Limitations | High     | Canvas has **no IMSCC import API**; must use direct REST API.[^1][^12]     |
| Markdown→HTML Fidelity   | Medium   | Convert LaTeX to MathML or images; Canvas RCE supports MathJax/KaTeX.[^3]  |

---

## 2.2 GitHub Actions Workflow Skeleton

```yaml
name: Deploy to Canvas

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Convert Markdown to HTML
        run: |
          pandoc --mathjax -s content/*.md -o webcontent/

      - name: Generate imsmanifest.xml
        run: python scripts/generate_manifest.py

      - name: Package IMSCC
        run: zip -r course.imscc imsmanifest.xml webcontent/

      - name: Deploy to Canvas API
        env:
          CANVAS_API_TOKEN: ${{ secrets.CANVAS_API_TOKEN }}
          CANVAS_DOMAIN: ${{ secrets.CANVAS_DOMAIN }}
          COURSE_ID: ${{ secrets.COURSE_ID }}
        run: |
          python scripts/deploy_canvas.py \
            --token "$CANVAS_API_TOKEN" \
            --domain "$CANVAS_DOMAIN" \
            --course "$COURSE_ID"
```

---

# 3. Self-Hosted Constraint: SaaS Canvas vs. Open-Source Canvas LMS

## 3.1 Deployment Comparison

| Dimension        | SaaS Canvas                                | Self-Hosted Canvas LMS (AGPLv3)                                  |
|------------------|---------------------------------------------|------------------------------------------------------------------|
| Hosting          | AWS multi-tenant (99.9% SLA)                | Your infra (VPS/on-prem/private cloud)[^11][^13]                 |
| Code Parity      | Same core codebase                          | Same except proprietary plugins missing.[^12]                    |
| API Compatibility| Full REST API                               | Same API; rate limits may differ.[^5][^12]                       |
| Authentication   | OAuth2 Developer Keys                       | Same flow; you manage lifecycle.[^2][^6]                         |
| Data Sovereignty | Instructure-controlled                      | Full control.[^14][^15]                                          |
| Updates          | Automatic                                   | Manual upgrades; you manage patches.[^11][^16]                   |
| Cost Model       | $8–50/learner/year                          | Hosting + IT labor ($5K–50K/year).[^^17][^18]                    |
| Missing Features | N/A                                         | New Quizzes, MasteryPaths, SCORM, Analytics, etc.[^12]           |

---

## 3.2 Strategic Implications

| Consideration     | SaaS Canvas                          | Self-Hosted Canvas                               |
|-------------------|---------------------------------------|--------------------------------------------------|
| API Stability     | High                                  | High; you control upgrade cadence.[^12]          |
| Customization     | Limited                               | Full code access.[^12]                           |
| Compliance        | Instructure handles SOC2/FERPA        | You handle audits + backups.[^13][^14]           |
| Pipeline Friction | Lower                                 | Higher (OAuth2 server, SSL, token rotation).[^^2][^5] |
| IMSCC Support     | UI-only                               | Same limitation.[^11][^12]                       |

---

# 4. Interoperability Matrices

## 4.1 Canvas API Endpoint Matrix

| Operation        | Endpoint                                      | Method | Scope                                             | Idempotent |
|------------------|------------------------------------------------|--------|---------------------------------------------------|------------|
| Create Module    | /api/v1/courses/:course_id/modules            | POST   | url:POST|/api/v1/courses/:course_id/modules               | No         |
| Update Module    | /api/v1/courses/:course_id/modules/:id        | PUT    | url:PUT|/api/v1/courses/:course_id/modules/:id           | Yes        |
| Create Page      | /api/v1/courses/:course_id/pages              | POST   | url:POST|/api/v1/courses/:course_id/pages                | No         |
| Update Page      | /api/v1/courses/:course_id/pages/:url_or_id   | PUT    | url:PUT|/api/v1/courses/:course_id/pages/:url_or_id     | Yes        |
| Create Assignment| /api/v1/courses/:course_id/assignments        | POST   | url:POST|/api/v1/courses/:course_id/assignments          | No         |
| Create Rubric    | /api/v1/courses/:course_id/rubrics            | POST   | url:POST|/api/v1/courses/:course_id/rubrics             | No         |
| Set Front Page   | /api/v1/courses/:course_id/front_page         | PUT    | url:PUT|/api/v1/courses/:course_id/front_page           | Yes        |

---

## 4.2 IMSCC Packaging Steps

| Step | Action                         | Tool/Spec                         | Output                                      |
|------|--------------------------------|-----------------------------------|---------------------------------------------|
| 1    | Convert Markdown/LaTeX → HTML  | Pandoc, filters                   | HTML in `webcontent/`.[^4]                  |
| 2    | Generate `imsmanifest.xml`     | Python/Node script                | Valid manifest.[^4][^19]                    |
| 3    | Organize directory structure   | Shell/GitHub Action               | ZIP-ready hierarchy.[^4]                    |
| 4    | Package as `.imscc`            | `zip -r course.imscc`             | Valid IMSCC ZIP.[^4]                        |
| 5    | Deploy                         | Canvas API or UI import           | Course artifacts in Canvas.[^1][^2][^11]    |

**Critical Gap:** Canvas **does not** support IMSCC import via API.[^11][^12]

---

# 5. Recommendations & Risk Mitigation

## 5.1 Production-Ready Path

1. Prefer API-first deployment; IMSCC is UI-only.[^2][^12][^1][^11]  
2. Implement idempotency layer (`.canvas-state.json`).[^3][^1]  
3. Use OAuth2 refresh flow.[^9][^6]  
4. Prefer self-hosted Canvas for sovereignty.[^14][^15][^12]

## 5.2 Open Research Questions

- No IMSCC import API exists.[^12][^11]  
- No QTI import API; must use assignment/quiz endpoints.[^2]  
- Rate limits undocumented; empirical testing required.[^5]

---

# Footnotes

[^1]: https://developerdocs.instructure.com/services/canvas/resources/modules  
[^2]: https://developerdocs.instructure.com/services/canvas/resources/rubrics  
[^3]: https://developerdocs.instructure.com/services/canvas/resources/pages  
[^4]: https://www.imsglobal.org/cc/ccv1p3/imscc_Implementation-v1p3.html  
[^5]: https://developerdocs.instructure.com/services/canvas  
[^6]: https://github.com/instructure/canvas-Ims/blob/master/doc/api/developer keys.md  
[^7]: https://documentation.instructure.com/doc/api/all_resources.html  
[^8]: https://community.canvaslms.com/t5/Archived-Questions/ARCHIVED-Can-I-set-my-course-to-change-to-a-different-homepage/m-p/227132  
[^9]: https://github.com/instructure/canvas-lms/blob/master/doc/api/oauth.md  
[^10]: https://www.imsglobal.org/cc/ccv1p2/imscc_profilev1p2-Implementation.html  
[^11]: https://www.instructure.com/lms-learning-management-system  
[^12]: https://github.com/instructure/canvas-lms/wiki/FAQ  
[^13]: https://www.instructure.com/about/learning-platform/infrastructure  
[^14]: https://abstract-technology.de/blog/open-edx-vs-canvas  
[^15]: https://www.host-stage.net/case-study/top-self-hosted-lms/  
[^16]: https://www.instructure.com/en-au/resources/product-overviews/cloud-vs-open-source-canvas-lms-guide  
[^17]: https://openeducat.org/solutions/online-learning-platforms/  
[^18]: https://www.vendr.com/marketplace/canvas  
[^19]: https://github.com/instructure/common-cartridge-viewer/blob/master/public/test-cartridges/all-question-types/imsmanifest.xml  
[^20]: https://www.imsglobal.org/cc/index.html  
[^21]: https://www.imsglobal.org/cc/ccv1p1/imscc_profilev1p1-Implementation.html  
[^22]: https://www.imsglobal.org/cc/ccv1p0/imscc_profilev1p0.html  
[^23]: https://developerdocs.instructure.com/services/canvas/resources/submissions  
[^24]: https://documentation.instructure.com/doc/api/file.endpoint_attributes.html  
[^25]: https://tessl.io/registry/tessl/pypi-canvasapi/3.3.0/files/docs/content-management.md  
[^26]: https://community.canvaslms.com/t5/Developers-Group/Is-it-possible-to-identify-or-change-a-Course-Home-Page-through/td-p/156943  
[^27]: https://www.instructure.com/resources/blog/canvas-lms-v-competition-why-higher-ed-chooses-canvas  
[^28]: https://github.com/instructure/canvas-Ims/blob/master/app/controllers/wiki pages api controller.rb  
[^29]: https://www.canvas.instructure.com/doc/api/pages.html  
[^30]: https://www.umjicanvas.com/doc/api/all_resources.html
