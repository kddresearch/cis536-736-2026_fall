# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 5](../phases/phase_05/):** Visualization, VFX & Capstone Synthesis | **[Module 11](../modules/module_11/):** Data/Info Visualization II, VFX & Interim Integration | **Lecture 34:** Data/Info Visualization II: Objects & Processes | Mon 16 11 2026 | Healy, *Data Visualization*, Ch. 5–6 (`socviz.co`) |

---

## Slide 1: Overview & Agenda — Visualizing Objects & Processes
### CIS 536/736 - Lecture 34
* **From Raw Quantities to Complex Structures:** Moving beyond single-variable bar and scatter charts (Module 10) to multi-layer declarative graphics, statistical model outputs, and spatial/relational processes.
* **Assigned Reading:** Kieran Healy, *Data Visualization: A Practical Introduction*, Chapters 5–6 (`socviz.co`).
* **Multi-Layer Grammar of Graphics:** Decoupling data transformations (`stats`), geometric marks (`geoms`), aesthetic mappings, and coordinate panels.
* **Multivariate & Distribution Encodings:** Comparing groups across continuous and categorical axes using boxplots, violin plots, Cleveland dot plots, and small multiples.
* **Modeling & Process Visualization:** Displaying statistical regressions, confidence intervals, residuals, and marginal effects alongside raw observations.
* **Graphics Pipeline Integration:** Translating declarative 2D/3D visual encodings into interactive Unity 6 scene graphs and UI Toolkit canvases.

**Speaker Notes:**
Welcome to Lecture 34, opening Module 11 in Phase 5. Last week in Module 10, we covered perceptual color spaces (oRGB, CIELAB), elementary visual channels, and building basic runtime charts in Unity 6 UI Toolkit. Today, grounded in Chapters 5 and 6 of Kieran Healy's *Data Visualization*, we scale up from plotting isolated quantities to visualizing structured objects, statistical processes, and multi-dimensional relationships—and examine how those declarative principles map directly onto 2D/3D computer graphics scene graphs.

---

## Slide 2: Multi-Layer Declarative Grammar of Graphics
### Separating Data, Transforms, Geoms & Scales
* **Orthogonal Pipeline Stages:** A declarative visualization separates (1) **Data**, (2) **Aesthetic Mappings** (`x`, `y`, `color`, `size`, `alpha`), (3) **Statistical Transformations** (`stat_summary`, `stat_smooth`, binning), (4) **Geometric Primitives** (`geom_point`, `geom_line`, `geom_ribbon`), and (5) **Coordinate Systems / Scales**.
* **Layer Composition (`+` Operator Semantics):** Complex plots are constructed by stacking independent layers that either inherit global data/mappings or override them with local data subsets (e.g., plotting full-corpus background points in gray beneath highlighted outlier callouts).
* **Graphics Engine Analogy:** Mirrors modern GPU rendering passes and Unity/OpenUSD scene graphs—separating underlying mesh/point geometry (`geoms`), shader material uniforms (`aesthetics`), compute/geometry-node modifiers (`stats`), and camera projection matrices (`coord_cartesian`, `coord_polar`, `coord_map`).

**Speaker Notes:**
Notice how closely Wilkinson and Healy's Layered Grammar of Graphics mirrors a modern graphics pipeline. In Blender Geometry Nodes or a Unity shader pipeline, you never hardcode pixel positions by hand; you bind attributes to fields, run procedural transformations, and project through a camera coordinate space. When building analytical overlays in Unity 6, keeping your statistical aggregation separate from your mesh or UI Toolkit rendering layer prevents visual clutter and makes interactive filtering trivial.

---

## Slide 3: Visualizing Grouped Comparisons & Distributions
### Preserving Granularity Without Overplotting
* **Summary-Only Traps:** Bar charts of group means ("dynamite plots") conceal sample size, skewness, and multimodality while wasting ink below the baseline.
* **Distributional Geoms:** Combining **boxplots** or **violin plots** (kernel density estimates) with jittered raw points (`geom_jitter`) or beeswarm offsets exposes both summary quantiles and underlying sample density.
* **Cleveland Dot Plots & Range Plots:** Replacing heavy bars with high-precision position encodings along a common aligned scale—ideal for comparing ranked categories or before/after deltas (`geom_segment` + `geom_point`).
* **Handling High-Density Overplotting:** Mitigating visual saturation via alpha-blending (`alpha`), 2D hexagonal/rectangular binning (`geom_hex`), and contour density estimation (`geom_density_2d`).

**Speaker Notes:**
Healy Chapter 5 emphasizes showing the distribution behind the summary. When you visualize telemetry in your term project—such as frame-time distributions across hardware tiers or particle-count benchmarks—never show just a mean bar. Pair a violin or boxplot summary with translucent jittered observations, or use a Cleveland dot plot when comparing ordered categorical metrics so the viewer's visual system uses position along a common scale rather than area.

---

## Slide 4: Small Multiples & Faceting (`facet_wrap` / `facet_grid`)
### Trading Spaghetti Plots for Aligned Coordinate Panels
* **The Spaghetti Plot Failure Mode:** Overlaying >5 time-series curves or trajectories on a single axis overloads color discrimination and creates severe occlusion.
* **Small Multiples Principle (Tufte / Healy):** Partitioning data by one (`facet_wrap`) or two (`facet_grid`) categorical variables into an array of small, identically scaled panels.
* **Cognitive Efficiency:** Shared axes allow the viewer's eye to scan structural differences across conditions, cohorts, or simulation runs without repeatedly re-reading legends.
* **Background Context Layering:** Plotting the pooled dataset as a faint silhouette in every facet panel anchors each subset against the global distribution.

**Speaker Notes:**
Whenever a multi-line chart turns into an unreadable tangle of crossing colors, decompose it into small multiples. By holding the x- and y-axis scales fixed across a grid of panels, you shift the cognitive burden from legend lookup to rapid spatial pattern recognition. In a 3D viewport or Unity HUD, small multiples translate cleanly into synchronized multi-camera viewports or tiled UI Toolkit panels.

---

## Slide 5: Direct Labeling & Annotation Hygiene
### Eliminating Legend-Lookup Friction
* **Direct Text & Callout Labels:** Placing category labels directly adjacent to terminal data points or curves (`geom_text_repel`) eliminates horizontal eye saccades back and forth to a detached legend.
* **Selective Outlier Highlighting:** Using `subset()` filters inside `geom_text` or `geom_label` to annotate only statistically notable points (e.g., top-5 residuals or threshold violations) rather than every vertex.
* **Repulsion & Collision Avoidance:** Algorithmic label placement (force-directed spring repulsion) prevents overlapping typography in dense 2D screen space and 3D billboarded HUDs.

**Speaker Notes:**
Detached legends force the viewer to hold color-to-name mappings in working memory. In Healy Chapter 5, direct labeling with collision-avoiding repulsion (`ggrepel`) keeps the reader's gaze inside the data region. For your Unity 6 visualization pipelines, apply the exact same rule: project 3D world-space anchor coordinates to screen space and run a lightweight screen-space bounding-box repulsion pass before drawing text labels.

---

## Slide 6: Modeling & Statistical Process Visualization (Healy Ch. 6)
### Plotting Models as First-Class Geometric Objects
* **Models Are Not Separate From Data:** Statistical models (OLS, GLM, LOESS smoothers, survival curves) generate fitted values, standard errors, and residuals that should be rendered directly over empirical observations.
* **Explicit Uncertainty Ribbons:** Rendering 95% confidence bands (`geom_ribbon`) or error bars (`geom_errorbar` / `geom_pointrange`) alongside point estimates prevents viewers from treating noisy estimates as deterministic constants.
* **Comparing Multiple Model Specifications:** Using **coefficient plots** (forest plots) and **marginal effects curves** to compare parameter estimates and uncertainty intervals across competing models on a shared baseline.

**Speaker Notes:**
Moving into Healy Chapter 6, the core insight is that statistical models and simulation processes are themselves objects to be visualized—not just tables of p-values. Whether you are displaying a regression trend over empirical sensor readings or plotting predicted trajectories in a simulation, always render the uncertainty envelope (`geom_ribbon` or `pointrange`) explicitly so the viewer sees where the model is well-constrained versus extrapolating.

---

## Slide 7: Residual Diagnostics & Simulation Trajectory Processes
### Visualizing Error Structure & Time-Varying State
* **Residual-vs-Fitted & Q–Q Diagnostics:** Plotting model residuals against fitted values exposes heteroscedasticity, non-linear bias, and structural outliers invisible in raw $R^2$ summaries.
* **Process & State-Transition Trajectories:** Encoding temporal processes (e.g., optimization convergence, kinematic state paths, or frame-budget drift) using directional paths (`geom_path` ordered by time $t$ rather than sorted by $x$) with color/size encoding velocity or age.
* **Bootstrapped & Ensemble Spaghetti:** Rendering an ensemble of simulated paths at low opacity behind a bold median/quantile trajectory communicates stochastic variance directly.

**Speaker Notes:**
Notice the distinction between `geom_line` (which sorts observations left-to-right along the x-axis) and `geom_path` (which connects points in sequential time order $t_0 \to t_n$). Whenever you visualize a dynamic process—such as a particle trajectory, a camera path, or a phase-space state loop—you must preserve sequential ordering (`geom_path`) and encode progression through luminance or vector arrowheads.

---

## Slide 8: Spatial & Network Process Encodings
### Maps, Choropleths, Flows & Graph Topologies
* **Coordinate Reference Systems & Projections:** Transforming geographic coordinates $(\lambda, \phi)$ via explicit map projections (`coord_sf` / `coord_map`) while accounting for area distortion (e.g., Albers Equal-Area vs. Web Mercator).
* **Choropleths vs. Proportional Symbols:** Normalizing rates per capita before coloring polygons to avoid the "population map" fallacy, or overlaying graduated symbols / flow arcs (`geom_curve`) for origin–destination processes.
* **Force-Directed & Hierarchical Graph Layouts:** Visualizing relational objects (such as OpenUSD composition arcs or scene-graph dependencies) using Fruchterman–Reingold force layouts, radial trees, or adjacency matrices.

**Speaker Notes:**
In Healy Chapters 5 and 6, spatial and relational data introduce an explicit geometric transformation step before rendering. Never shade raw counts onto geographic regions of unequal area—always normalize to rates or use proportional point symbols. Similarly, when visualizing hierarchical scene graphs or dependency networks in your projects, choose deterministic tree layouts for strict DAGs and force-directed layouts only when community clustering is the primary analytical question.

---

## Slide 9: Bridging Declarative InfoVis to Unity 6 & OpenUSD Pipelines
### Real-Time 2D/3D Data Visualization Architecture
* **Data-to-Visual Pipeline Mapping:**
  * **Tabular / Time-Series Store (`JSON` / `CSV` / `SQLite`)** $\to$ C# `DataFrame` / ComputeBuffer (`stats` aggregation).
  * **Visual Channels (`aesthetics`)** $\to$ Shader uniforms (`Color`, `Metallic`, `Emission`, vertex scale) & UI Toolkit USS classes.
  * **Geometric Marks (`geoms`)** $\to$ GPU-instanced meshes (`Graphics.RenderMeshInstanced`), `LineRenderer` ribbons, or UI Toolkit `VisualElement` vector painters (`Painter2D`).
* **Perceptual Safety in 3D Scenes:** Applying colorblind-safe palettes (Okabe–Ito, Viridis) and avoiding unlit neon saturation so data encodings remain legible under scene lighting and tone mapping.
* **Lab 6a $\to$ Lab 6b Continuity:** Integrating your Module 10 Unity 6 visualization canvas directly into your Module 11 interim project scene.

**Speaker Notes:**
How does all of this connect to your Unity 6 code in Lab 6a and Lab 6b? Treat Unity's UI Toolkit `Painter2D` API or `Graphics.RenderMeshInstanced` as your `geom` backend. Keep your statistical aggregations—quantiles, confidence intervals, normalized scales—in clean C# data structures so your rendering loop only reads pre-mapped aesthetic attributes. If you render 3D data glyphs inside a lit HDRP scene, remember that post-processing bloom and directional shadows can distort hue and luminance, so use unlit or UI-overlay shaders for quantitative color channels.

---

## Slide 10: Summary & Transition to VFX, Particles & Lab 6b
### Key Takeaways & Next Steps
* **Declarative Composition:** Stack orthogonal layers (data, aesthetics, statistical transforms, geoms, coordinate scales) to visualize complex objects without clutter.
* **Distributions & Small Multiples:** Replace single-point summaries and tangled multi-line charts with distribution geoms, Cleveland dot plots, and faceted grids.
* **Uncertainty & Process First:** Render confidence intervals, residuals, and sequential paths (`geom_path`) as primary visual elements.
* **Next Lecture (Wed 18 Nov — Lecture 35):** Special Topics: VFX & Particle Systems Survey (Lagrangian particles, Eulerian fluid grids, GPU Gems Ch. 38, and Unity VFX Graph).
* **Friday Lab (Fri 20 Nov — Lecture 36):** Lab 6b Term Project Interim Integration Check-In—bring your unified scene graph and pipeline checklist before Fall Break.

**Speaker Notes:**
To wrap up: good visualization of objects and processes comes down to structural honesty—showing full distributions, aligning panels via small multiples, labeling directly, and rendering model uncertainty explicitly. On Wednesday in Lecture 35, we shift from static and statistical visualizations to dynamic physical simulation with VFX and particle systems (Lagrangian particles, Eulerian Navier–Stokes grids, and Unity VFX Graph), preparing you for Friday's Lab 6b Term Project Interim Integration Check-In before Fall Break.
