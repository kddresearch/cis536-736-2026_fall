# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Visualization & Special Topics | **[Module 10](../modules/module_10/):** Color Theory & Information Visualization I | **Lecture 33:** Opponent Color Spaces (oRGB) & Unity UI Toolkit Runtime Charts | Fri 13 Nov 2026 | ["oRGB: A Practical Opponent Color Space" (Bratkova et al.)](https://graphics.stanford.edu/~boulos/papers/orgb_sig.pdf); [Unity UI Toolkit Runtime Charts Documentation](https://docs.unity3d.com/) |

---

## Slide 1: Lecture Overview & Agenda
### CIS 536/736 - Lecture 33
* **Core Focus:** Overcoming the limitations of standard color spaces using opponent color theory, and implementing data-driven UIs in Unity.
* **Assigned Reading:** ["oRGB: A Practical Opponent Color Space" (Bratkova et al.)](https://graphics.stanford.edu/~boulos/papers/orgb_sig.pdf); Unity UI Toolkit documentation.
* **Biological Grounding:** Hering's Opponent Process Theory of human color vision.
* **Mathematical Color Models:** The transition from RGB/HSV to CIELAB, and the optimization into the `oRGB` space.
* **Engine Implementation:** Architecting runtime charts and data dashboards using Unity's UI Toolkit (UXML/USS).

**Speaker Notes:**
Good morning. In our last lecture, we covered the visual encoding of quantitative data and why the standard RGB "rainbow" colormap is perceptually hazardous. Today, we bridge the gap between color science and engine architecture. We will analyze Bratkova's `oRGB` space to see how we can computationally mimic human opponent-color perception, and then we will shift to the engine side to render these perceptually uniform data visualizations using the Unity UI Toolkit.

---

## Slide 2: The Biology of Color Perception
### Hering’s Opponent Process Theory
* **[Trichromatic Foundation]:** The retina contains three types of cone cells (S, M, L) responding roughly to blue, green, and red wavelengths.
* **[Opponent Processing]:** Post-receptor neural pathways compress these signals into three opposing channels rather than transmitting raw RGB values to the brain.
* **[The Three Channels]:** 
  1. Light vs. Dark (Achromatic Luminance)
  2. Red vs. Green
  3. Blue vs. Yellow
* **[Mutual Exclusivity]:** Humans can perceive yellowish-red (orange) or bluish-red (purple), but we cannot biologically perceive "reddish-green" or "yellowish-blue."

**Speaker Notes:**
Standard RGB color spaces directly model the hardware of our monitors, which conceptually mirrors the three cone types in our eyes. However, our brains do not process raw RGB data. The neurological wiring in our visual cortex processes color in opposing channels. This biological reality—Opponent Process Theory—explains why standard RGB math often produces visual interpolations that look "muddy" or unnatural to the human eye. 

---

## Slide 3: Limitations of RGB and HSV/HSL
### Why Hardware-Centric Spaces Fail for Visualization
* **[RGB Non-Uniformity]:** A Euclidean distance of 10 units between two RGB colors in the red spectrum looks completely different than a 10-unit distance in the green spectrum.
* **[HSV/HSL Deceptions]:** Hue, Saturation, and Value/Lightness attempt to provide an intuitive interface, but they completely fail at maintaining uniform luminance.
* **[The Yellow/Blue Problem]:** In HSV, fully saturated blue ($H=240, V=1$) and fully saturated yellow ($H=60, V=1$) share the exact same "Value," even though the yellow is perceptually vastly brighter.
* **[Interpolation Artifacts]:** Linear interpolation ($lerp$) through RGB or HSV space often passes through gray, muddy, or mathematically unintended perceptual zones.

**Speaker Notes:**
If you write a shader that simply `lerps` between two RGB colors or two HSV values to represent a change in temperature or pressure, you are lying to your user. Because these spaces are not perceptually uniform, the viewer will perceive rapid changes in data where none exist, or miss subtle shifts entirely because the luminance curve collapses. 

---

## Slide 4: CIELAB and the Quest for Uniformity
### The Standard of Perceptual Color Spaces
* **[CIELAB ($L^*a^*b^*$)]:** Designed by the International Commission on Illumination (CIE) to be perceptually uniform, modeling human opponent vision.
* **[Luminance ($L^*$)]:** Represents perceived lightness, perfectly decoupled from color information.
* **[Chromaticity ($a^*, b^*$)]:** $a^*$ represents the Green-Red opponent axis; $b^*$ represents the Blue-Yellow opponent axis.
* **[The Computational Cost]:** Converting from RGB to CIELAB requires transitioning through the XYZ color space using non-linear cube-root transformations.

**Speaker Notes:**
CIELAB solves our perceptual mapping problems. In $L^*a^*b^*$, a Euclidean distance of $1.0$ is exactly one "Just Noticeable Difference" (JND) to the human eye, anywhere in the space. However, CIELAB is computationally heavy, making it expensive to evaluate per-pixel in a real-time shader, and its axes do not perfectly align with intuitive primary colors, making it difficult for artists to select palettes.

---

## Slide 5: The oRGB Color Space
### Bratkova et al.'s Practical Opponent Space
* **[The Objective]:** Create a color space that preserves the perceptual uniformity and opponent axes of CIELAB, but with the intuitive, fast transformations of HSV.
* **[The oRGB Model]:** Composed of three channels: $L$ (Luminance), $C_1$ (Red-Green), and $C_2$ (Blue-Yellow).
* **[Primary Alignment]:** Unlike CIELAB, the $C_1$ and $C_2$ axes in oRGB are mathematically rotated and scaled so that fundamental primary colors (pure red, green, blue, yellow) align perfectly with the major axes.
* **[Performance]:** Transformations between sRGB and oRGB avoid the heavy cube-roots of CIELAB, relying instead on fast linear matrix multiplications.

**Speaker Notes:**
This is where the Bratkova paper comes in. oRGB gives us the best of both worlds. It operates on the biological Red-Green and Blue-Yellow opponent axes, guaranteeing that our data-visualizations maintain uniform luminance. But because it is designed specifically for computer graphics, the math is optimized for the GPU, and the axes are aligned so that artists can easily generate intuitive color palettes without a PhD in colorimetry.

---

## Slide 6: Utilizing oRGB in the Render Pipeline
### Application in Shaders and Procedural Generation
* **[Color Picking]:** Allowing users or artists to select UI themes or data categories by moving along independent opponent axes.
* **[Perceptual Lerping]:** Implementing `lerp(colorA, colorB)` inside the oRGB space in a fragment shader guarantees smooth, mud-free gradients.
* **[Transfer Functions]:** In volume rendering (e.g., medical MRI data), oRGB allows precise mapping of scalar densities to independent luminance and chromaticity channels.
* **[Color Harmonies]:** Procedurally generating complementary, triadic, or tetradic color palettes at runtime by executing basic geometric rotations in the $C_1/C_2$ plane.

**Speaker Notes:**
When you build your data visualization tools in Lab 6, you will use oRGB transfer functions. By holding the $L$ channel constant and rotating through the $C_1/C_2$ plane, you can procedurally generate a categorical colormap where every distinct class is easily identifiable, but no single class aggressively dominates the screen due to a false luminance spike.

---

## Slide 7: Transitioning to UI Architecture
### Unity UI Toolkit vs. Immediate Mode / UGUI
* **[The Legacy Systems]:** `IMGUI` (Immediate Mode) is CPU-heavy and terrible for layout; `UGUI` (GameObjects/Canvas) pollutes the scene hierarchy and causes massive batch-breaking overhead when data updates.
* **[Unity UI Toolkit]:** A retained-mode UI framework modeled directly on web standards (HTML/CSS), operating entirely outside the standard GameObject hierarchy.
* **[UXML (Unity Extensible Markup Language)]:** Defines the structural hierarchy and DOM (Document Object Model) of the dashboard.
* **[USS (Unity Style Sheets)]:** Defines the visual encoding, layout constraints, and colors, fully decoupling presentation from data logic.

**Speaker Notes:**
Now that we have the math for our visual encoding, we need a surface to render it on. For years, rendering complex charts in Unity meant instantiating thousands of UI GameObjects, which destroyed the CPU via transform hierarchy updates. The modern standard is the UI Toolkit. By utilizing a web-style DOM, we can render complex, data-dense dashboards with minimal overhead.

---

## Slide 8: Architecting Runtime Charts
### Data Binding and Custom Visual Elements
* **[Custom `VisualElement`]:** Inheriting from Unity's base UI class to create dedicated `BarChart`, `LineGraph`, or `HeatMap` nodes within the UXML DOM.
* **[The `generateVisualContent` Callback]:** Overriding the `MeshGenerationContext` to draw raw vertices, lines, and polygons directly into the UI canvas without GameObjects.
* **[Data Binding]:** Linking C# arrays or generic lists directly to the custom UI element; when the underlying quantitative data mutates, the UI automatically invalidates and redraws.
* **[Painter2D API]:** Using Unity's vector drawing API to stroke lines and fill paths inside the UI Toolkit, enabling procedural pie charts and sparklines.

**Speaker Notes:**
The power of the UI toolkit for information visualization lies in the `MeshGenerationContext`. Instead of spawning prefabs, you write a script that says "take this array of 500 floats, calculate their spatial positions on the screen based on Mackinlay's hierarchy, and draw 500 vector lines directly to the canvas." It is extremely fast and keeps your scene completely clean.

---

## Slide 9: Synthesizing oRGB with UI Toolkit
### Data-Driven Aesthetics in Dashboards
* **[Dynamic Styling]:** Using C# to dynamically inject oRGB-calculated colors into the USS style variables of your custom charts at runtime.
* **[Diverging Chart Implementation]:** Building a runtime bar chart where positive/negative values branch from zero, mapping the quantitative magnitude to physical length, and mapping the polarity to the $C_1$ (Red-Green) opponent axis.
* **[Categorical Legend Generation]:** Procedurally instantiating UXML label elements and assigning them qualitative oRGB background colors based on unique data identifiers.
* **[Resolution Independence]:** UI Toolkit charts scale perfectly across 4K displays and VR headsets because the data is mapped to abstract layout flex-boxes rather than rigid screen pixels.

**Speaker Notes:**
This is the synthesis of Module 10. You will parse a dataset, categorize the data types, map the quantitative values to spatial properties in a custom `VisualElement`, and then calculate a perceptually uniform color ramp using the oRGB math to color the vector meshes. This ensures your dashboard is both highly performant and scientifically accurate.

---

## Slide 10: Summary & Next Steps
### Wrapping up Color and UI Visualization
* **[Core Takeaway 1]:** Opponent color spaces (like oRGB) align with biological perception, providing uniform luminance and artifact-free interpolation for shaders.
* **[Core Takeaway 2]:** Unity UI Toolkit allows for high-performance, retained-mode data visualization by drawing direct vector meshes decoupled from the scene hierarchy.
* **[Lab Assignment]:** Lab 6 - You will construct a dynamic UXML dashboard that ingests a CSV dataset, draws a custom vector chart, and colorizes it using a procedural oRGB shader.
* **[Next Lecture]:** We will transition into Special Topics, beginning with Non-Photorealistic Rendering (NPR) pipelines.

**Speaker Notes:**
That concludes Module 10. The bridge between raw math, human neurobiology, and real-time engine architecture is what separates a basic graphics programmer from a systems architect. Please have the UI Toolkit documentation open and ready for Lab 6. I will be in my office hours tomorrow if you need help debugging the oRGB matrix transformations. See you next week.
