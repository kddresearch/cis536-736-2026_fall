# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date | Assigned Reading |
| :--- | :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Visualization & Special Topics | **[Module 10](../modules/module_10/):** Color Theory & Information Visualization I | **Lecture 31:** Color Theory & Perception | Mon 09 Nov 2026 | [Healy, *Data Visualization*, Ch. 1](https://socviz.co/); [GAMES101 Lecture 20](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html) |

---

## Slide 1: Lecture Overview & Agenda
### CIS 536/736 - Lecture 31
* **Core Focus:** Establishing the physiological and mathematical foundations of color in computer graphics and visual representation.
* **Assigned Reading:** [Healy, *Data Visualization*, Ch. 1](https://socviz.co/); [GAMES101 Lecture 20](https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html)
* **The Human Visual System:** Photoreceptors, trichromacy, and opponent processing.
* **Color Spaces & Gamuts:** Transforming between device-dependent RGB and perceptually uniform models.
* **Perceptual Design:** Why physical luminance does not equal perceived brightness in digital displays.

**Speaker Notes:**
Good morning, everyone. Welcome to Module 10 and Lecture 31. Today we examine color not just as an artistic choice, but as a rigorous physical, physiological, and computational pipeline. We will explore how human visual perception dictates how we must structure color spaces in both real-time rendering and information visualization.

---

## Slide 2: The Human Visual System & Photoreceptors
### Biological Foundations of Color Vision
* **[Photoreceptor Specialization]:** Distinguishing between scotopic rod cells (luminance, motion, low-light) and photopic cone cells (color, high-acuity).
* **[Trichromatic Theory]:** The three cone classes (L, M, S) sensitive to Long (red), Medium (green), and Short (blue) wavelengths of visible light.
* **[Spectral Sensitivity Curves]:** Overlapping absorption spectra demonstrating why human eyes are most sensitive to green-yellow wavelengths.
* **[Neural Adaptation]:** Dynamic gain control allowing the visual cortex to operate across phenomenal luminance ranges (from starlight to direct sunlight).

**Speaker Notes:**
Our perception of color begins in the retina. Because our L, M, and S cones have overlapping spectral sensitivities, any given color sensation can be triggered by infinitely many physical wavelength combinations—making color a construct of our neural architecture rather than a pure physical measurement.

---

## Slide 3: Color Spaces & Mathematical Models
### Quantifying Color for Computation
* **[Additive RGB Space]:** Device-dependent primary color mixing used natively by emissive display panels and real-time framebuffers.
* **[Perceptual Linearity (CIELAB)]:** Designing color coordinates where Euclidean distance matches perceived color difference.
* **[HSV / HSL Models]:** Cylindrical transformations optimized for intuitive human color selection rather than physical accuracy.
* **[Gamut Boundaries]:** Understanding color space volume limitations when converting assets across sRGB, AdobeRGB, and DCI-P3 profiles.

**Speaker Notes:**
Computers store colors as RGB triples, but RGB is perceptually non-linear. Equal steps in RGB values do not yield equal steps in perceived brightness or color distance. For scientific visualization and realistic rendering, we rely on perceptually uniform spaces like CIELAB.

---

## Slide 4: GAMES101 Color Science Principles
### Radiometry, Photometry & Tone Mapping
* **[Radiometric Quantities]:** Measuring physical light energy via radiant flux, intensity, irradiance, and radiance.
* **[Photometric Weighted Vision]:** Integrating radiant power against the standard luminous efficiency function $V(\lambda)$ to match human eye response.
* **[High Dynamic Range (HDR)]:** Managing floating-point light values that far exceed standard 0-to-1 display device limits.
* **[Tone Mapping Operators]:** Compressing HDR luminance ranges into LDR display spaces while preserving local contrast and color fidelity.

**Speaker Notes:**
Following GAMES101 Lecture 20, we remember that computer graphics deals with physical light energy (radiometry), which must be weighted by human visual response curves (photometry). Handling HDR values and applying correct tone mapping is essential before color ever reaches the screen.

---

## Slide 5: Kieran Healy Chapter 1 — Look at Data
### The Philosophy of Visualization & Perception
* **[Data as Artifact]:** Recognizing that datasets are human constructs requiring careful visual framing to reveal underlying truths.
* **[The Power of Graphs]:** Leveraging human pre-attentive visual processing to spot trends, clusters, and outliers instantly.
* **[Mapping Information to Form]:** Transforming abstract numbers into geometric and chromatic properties that align with cognitive strengths.
* **[Avoiding Clutter]:** Eliminating chart junk and decorative distraction that impedes rapid visual comprehension.

**Speaker Notes:**
In Chapter 1 of *Data Visualization*, Kieran Healy reminds us that looking at data visually is not just an aesthetic exercise—it is an epistemological necessity. Our visual cortex can process spatial and chromatic patterns in parallel, far outperforming serial table reading.

---

## Slide 6: Opponent Process Theory
### Cortical Color Encoding Channels
* **[Hering's Opponent Channels]:** Explaining why we perceive certain colors as mixtures (blue-green) but never others (red-green, blue-yellow).
* **[Red-Green Channel]:** Differential signals comparing L and M cone activations.
* **[Blue-Yellow Channel]:** Comparing S cone responses against combined L+M luminance signals.
* **[Achromatic Luminance Channel]:** Summing cone outputs to extract high-resolution spatial structure and shading detail.

**Speaker Notes:**
Beyond trichromacy at the receptor level, the visual cortex processes color via opponent channels. This biological architecture directly inspires advanced graphics compression formats and perceptual color spaces like oRGB, which we will examine next lecture.

---

## Slide 7: Color Constancy & Visual Illusions
### Contextual Adaptation in Rendering
* **[Chromatic Adaptation]:** The visual system's automatic white-balancing under varying illumination spectra (e.g., tungsten vs. daylight).
* **[Simultaneous Contrast]:** The illusion where identical physical colors appear radically different depending on surrounding background luminance.
* **[Lighting Context in Games]:** Why real-time PBR engines must simulate environmental bounce light to maintain believable color perception.
* **[Perceptual Artifacts]:** Guarding against optical illusions that cause players to misinterpret spatial depth or material roughness.

**Speaker Notes:**
Our eyes are remarkably adaptive instruments. Color constancy means we recognize a white sheet of paper as white whether it is indoors under yellow incandescent bulbs or outside under blue sky. Renderers must account for this adaptation to prevent visual uncanny valley effects.

---

## Slide 8: Color Vision Deficiency (CVD) & Accessibility
### Designing Inclusive Graphics
* **[Deuteranopia & Protanopia]:** Understanding red-green color blindness affecting approximately 8% of males and 0.5% of females.
* **[Tritanopia]:** Rare blue-yellow color vision deficiency.
* **[Isolating Luminance]:** Ensuring information remains legible and distinguishable even when color saturation or hue is completely removed.
* **[Palette Selection Tools]:** Utilizing color-blind-safe palettes (such as ColorBrewer or Viridis) in both game UI and scientific figures.

**Speaker Notes:**
As graphics engineers and visualization designers, accessibility is a hard constraint, not an afterthought. If critical information in a simulation or chart relies solely on a red-versus-green hue distinction, millions of viewers will miss the signal entirely.

---

## Slide 9: Practical Palettes for Graphics & Vis
### Categorical, Sequential & Diverging Scales
* **[Qualitative (Categorical) Palettes]:** Distinct hues for unordered groups; must maintain equal visual weight and high mutual separability.
* **[Sequential Palettes]:** Single-hue or luminance-graded ramps (e.g., light-to-dark blue) representing ordered quantitative data.
* **[Diverging Palettes]:** Two contrasting sequential ramps meeting at a neutral midpoint, ideal for highlighting positive vs. negative deviations.
* **[Perceptually Uniform Ramps]:** Avoiding artifacts by strictly employing mathematically linear ramps like Viridis or Magma.

**Speaker Notes:**
Choosing the wrong color scale destroys data integrity. A rainbow colormap introduces false perceptual boundaries and visual distortion. Matching the data type—categorical, sequential, or diverging—to the correct perceptually uniform palette is a fundamental graphics skill.

---

## Slide 10: Summary & Next Lecture Preview
### Preparing for Encoding Quantities & Lab 6a
* **[Key Takeaways]:** Color is an interaction between physical light, biological photoreceptors, and cortical opponent channels.
* **[Readings for Next Time]:** Review Healy, *Data Visualization*, Chapters 3 and 4, focusing on visual encoding of quantities.
* **[Upcoming Lab Preview]:** In Lecture 32 and Lab 6a, we transition from theory to practice, building runtime visualization pipelines and charts in Unity 6.

**Speaker Notes:**
To summarize: color is neither purely objective physics nor purely subjective art—it is a measurable perceptual science. Keep these physiological principles in mind as we move into Lecture 32, where we will translate quantitative data into precise visual encodings. See you Wednesday!
