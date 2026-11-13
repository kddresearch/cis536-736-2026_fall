# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Visualization & Special Topics | **[Module 10](../modules/module_10/):** Color Theory & Information Visualization I | **Lecture 32:** Visual Encoding of Quantities & Information Visualization II | Wed 11 Nov 2026 | [Healy, *Data Visualization*, Ch. 3 & 4](https://socviz.co/); [GAMES101 Lecture 20](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html) |

---

## Slide 1: Lecture Overview & Agenda
### CIS 536/736 - Lecture 32
* **Core Focus:** Mapping quantitative, ordinal, and categorical data to visual channels; implementing runtime data visualization in real-time engines.
* **Assigned Reading:** [Healy, *Data Visualization*, Ch. 3 & 4](https://socviz.co/); [GAMES101 Lecture 20](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html)
* **Visual Channels & Aesthetics:** Position, length, area, angle, shape, and color channels.
* **Mackinlay's Channel Expressiveness & Effectiveness:** Ordering perceptual channels by quantitative accuracy.
* **Real-Time Data Visualization:** Building dynamic charts, graphs, and spatial data overlays in Unity 6.

**Speaker Notes:**
Good morning, everyone. Welcome to Lecture 32. Last time, we laid the biological and mathematical groundwork of color perception and radiometry. Today, we step up to high-level information visualization and visual encoding. We will explore how human perceptual accuracy dictates which visual channels we choose for quantitative data, and how we translate these principles directly into real-time visual engines.

---

## Slide 2: Data Types & Abstract Data Models
### Categorizing Information for Visual Mapping
* **[Quantitative / Continuous Data]:** Numerical values supporting continuous arithmetic operations (e.g., world-space coordinates, temperature, frame time).
* **[Ordinal Data]:** Categorical values with a meaningful inherent ordering but non-uniform step sizes (e.g., UI priority levels, LOD levels, survey ratings).
* **[Nominal / Categorical Data]:** Discrete, unordered categories without intrinsic magnitude (e.g., entity types, shader names, object classes).
* **[Relational & Network Structures]:** Topological graphs, hierarchies, and spatial trees (e.g., scene graphs, bounding volume hierarchies, skeletal rigs).

**Speaker Notes:**
Before drawing a single pixel or building a UI component, you must classify your data types. The mapping function between data and visual marks must respect the mathematical properties of the underlying data—mapping nominal categories to continuous color ramps creates false order, while mapping quantitative values to discrete shapes destroys numerical relationships.

---

## Slide 3: Visual Encodings & Marks
### Building Blocks of Graphic Representations
* **[Marks (Geometric Primitives)]:** Points, lines, areas, and volumes used as structural anchors in 2D and 3D scenes.
* **[Visual Channels (Aesthetics)]:** Graphical properties attached to marks, such as position ($x, y, z$), size/length, orientation, hue, saturation, and texture.
* **[Channel Capacity]:** Recognizing how many distinct values a human viewer can differentiate within a given channel before cognitive overload occurs.
* **[Channel Interdependence]:** Managing visual interference when combining channels (e.g., area estimation is distorted by extreme aspect ratios).

**Speaker Notes:**
In graphics and data presentation, we map abstract variables to geometric marks and visual channels. However, visual channels are not created equal. Some channels allow fine-grained, high-precision quantitative estimation, while others are strictly suited for broad categorical grouping.

---

## Slide 4: Mackinlay’s Expressiveness & Effectiveness Ranking
### Perceptual Hierarchy for Quantitative Data
* **[Expressiveness Principle]:** The visual encoding must express *all* the relationships in the data, and *only* the relationships in the data.
* **[Effectiveness Ranking (Quantitative)]:**
  1. Position along a common scale (Highest Accuracy)
  2. Position along unaligned scales
  3. Length / Distance
  4. Angle / Slope
  5. Area
  6. Volume / 3D Depth
  7. Color Luminance / Saturation
  8. Color Hue (Lowest Quantitative Accuracy)
* **[Cognitive Trade-Offs]:** Screen real estate is strictly bounded. We are forced to route secondary and tertiary data dimensions through lower-accuracy channels (like area or color) once spatial positioning ($x, y$) is exhausted.

**Speaker Notes:**
If you want users to compare two numbers accurately, map them to position on an aligned grid—like a bar chart or a scatter plot. As you move down Mackinlay's list to area, volume, or color hue, human error in quantitative estimation increases exponentially. This is why mapping critical numerical data to color hue without a spatial anchor is a dangerous design anti-pattern.

---

## Slide 5: Color Scales & Data Mapping
### Designing Perceptually Uniform Palettes
* **[Sequential Scales]:** Used for quantitative or ordered data progressing from low to high. Relies on strictly monotonic changes in luminance (lightness) rather than just hue.
* **[Diverging Scales]:** Used for quantitative data with a meaningful critical midpoint (e.g., zero, average). Employs two distinct sequential hues converging on a neutral central luminance.
* **[Qualitative (Categorical) Scales]:** Used for nominal data. Maximizes perceptual distance between distinct hues without implying magnitude or order.
* **[Color Vision Deficiency (CVD)]:** Ensuring palettes remain distinguishable under Deuteranopia (red-green), Protanopia, and Tritanopia using luminance variation.

**Speaker Notes:**
Color is deceptively complex. When mapping data to color, you cannot just pick colors that "look good" in the editor. You must match the mathematical structure of the data to the perceptual structure of the color scale. A sequential dataset requires a sequential luminance ramp, while categorical data requires a qualitative palette where no single color appears inherently "greater" or "heavier" than another.

---

## Slide 6: The Fallacy of the Rainbow Colormap
### Why "Jet" and "HSV" Ramps Damage Data
* **[Perceptual Non-Uniformity]:** Equal steps in the underlying data do not produce equal perceptual steps in the visual hue. 
* **[False Boundaries (Mach Bands)]:** Uncontrolled luminance spikes in rainbow scales create artificial gradients that trick the visual cortex into seeing edges where none exist in the data.
* **[Loss of Ordinality]:** Humans do not have an innate perceptual ordering for hues (e.g., is red mathematically higher than blue?).
* **[The Solution]:** Perceptually uniform colormaps (e.g., Viridis, Magma, Inferno, Cividis) derived from CIELAB color space, ensuring linear luminance mapping.

**Speaker Notes:**
The classic RGB rainbow colormap, often called "Jet," is mathematically dangerous for scientific and statistical visualization. It creates sharp, artificial yellow and cyan bands that look like data anomalies but are merely artifacts of how human vision processes luminance spikes. Always default to perceptually uniform scales like Viridis for continuous data.

---

## Slide 7: Hazards of 3D Information Visualization
### Spatial Distortion in Perspective Projection
* **[Occlusion]:** Front-facing geometric marks obscure rear data points, requiring complex transparency sorting or interactive camera controls to resolve.
* **[Perspective Foreshortening]:** Depth ($z$-axis) scaling visually shrinks distant objects, breaking the proportional mapping of data to visual area/volume.
* **[The 3D Pie Chart Anti-Pattern]:** Introducing arbitrary 3D extrusion distorts angle and area channels, deliberately violating Mackinlay's Expressiveness principle.
* **[Justified 3D]:** 3D visualization is highly effective when the underlying data is *inherently spatial* (e.g., fluid dynamics, bounding volume hierarchies, architectural stress maps).

**Speaker Notes:**
Just because we have a powerful 3D engine does not mean we should render abstract data in 3D. Rendering a bar chart in perspective projection ruins the viewer's ability to compare heights accurately because foreshortening alters the visual length based on depth. Save 3D rendering for data that actually possesses physical XYZ coordinates.

---

## Slide 8: Real-Time Data Vis in Unity 6
### Bridging Data Arrays and the Render Pipeline
* **[Procedural Mesh Generation]:** Constructing dynamic graphs and charts via scripts defining vertex buffers and triangle arrays at runtime.
* **[GPU Instancing (`Graphics.DrawMeshInstanced`)]:** Rendering thousands of data points (e.g., a massive 3D scatter plot) in a single draw call without overwhelming the CPU overhead.
* **[Compute Shaders for Big Data]:** Offloading data sorting, filtering, and position updates to the GPU for real-time visualization of millions of elements.
* **[Material Property Blocks]:** Updating colors and visual channels per-instance efficiently without breaking the scriptable render pipeline (SRP) batching.

**Speaker Notes:**
When moving from static data visualization (like matplotlib or ggplot) to real-time engine visualization, performance becomes the primary bottleneck. If you instantiate 50,000 GameObjects to represent a dataset, the engine's hierarchy overhead will kill your frame rate. We bypass this by mapping data arrays directly to Compute Buffers and using GPU instancing.

---

## Slide 9: Interactive Exploration & Querying
### Shneiderman’s Mantra: Overview First, Zoom and Filter, Then Details-on-Demand
* **[Brushing and Linking]:** Selecting a subset of data in one visual representation dynamically highlights the corresponding data in all other linked views.
* **[Interactive Filtering]:** Using runtime UI controls to cull data arrays passing through the compute shader, dynamically updating the active rendering buffer.
* **[Spatial Tooltips (Details-on-Demand)]:** Utilizing physics raycasts or screen-to-world point transformations to fetch exact metadata from a selected visual mark.
* **[Temporal Scrubbing]:** Animating data states over time by interpolating between buffered frames or calculating procedural transformations in the shader.

**Speaker Notes:**
The definitive advantage of rendering data in Unity rather than a static PDF is interactivity. We can deploy Shneiderman's Mantra natively. By utilizing raycasting against our instanced data, users can dynamically filter out noise and pull up exact metadata precisely when they need it, creating an exploratory sandbox rather than a static report.

---

## Slide 10: Summary & Next Steps
### Wrapping Up Module 10
* **[Core Takeaway]:** Effective visualization requires matching the mathematical type of the data to the perceptual accuracy of the chosen visual channel.
* **[Core Takeaway]:** Perceptually uniform color scales prevent artificial anomalies and ensure accurate magnitude estimation.
* **[Upcoming Lab]:** Lab 6 will cover writing a Compute Shader to visualize a large multi-dimensional dataset using GPU-instanced primitives in Unity 6.
* **[Next Phase]:** Next week, we enter Phase 5, diving into Non-Photorealistic Rendering (NPR) and stylized shading architectures.

**Speaker Notes:**
That concludes our dive into Information Visualization. Keep Mackinlay's hierarchy in mind whenever you are tasked with displaying analytics, telemetry, or debug data in your engine. Please review the Compute Shader documentation in the syllabus before your lab sections. I will see you all next week as we shift gears into NPR and stylized rendering.