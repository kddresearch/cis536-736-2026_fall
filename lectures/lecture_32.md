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
* **[Cognitive Trade-Offs