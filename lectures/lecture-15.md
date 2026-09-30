# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| --- | --- | --- | --- |
| **Phase 3:** Procedural Generation and Rigging | **Module 5:** Geometry Nodes Fundamentals & Patterns | **Lecture 15:** Geometry Nodes Patterns — Scatter, Distribution, Parametric Control | Wednesday, September 30, 2026 |

---

## Slide 1: Geometry Nodes Patterns — Advanced Procedural Control

### CIS 536/736 - Lecture 15

* **Stochastic vs. Structured Sampling:** Trade-offs between pure Monte Carlo and low-discrepancy point sets on Riemannian surfaces.
* **Surface Differential Geometry:** Extracting the Darboux frame $(\mathbf{T}, \mathbf{B}, \mathbf{N})$ and tangent spaces directly from mesh topology.
* **Density Attenuation & Masking:** Driving distribution probabilities via scalar fields, vertex attributes, and harmonic noise functions.
* **Parametric Domain Warping:** Transforming linear and spherical domains into organic variations using vector displacement.
* **Geometric Continuity ($G^0$ to $G^2$):** Reconstructing topological manifolds from parametric 1D splines.

**Speaker Notes:**
Welcome back. On Monday, we established the fundamental execution model of Blender's Geometry Nodes—specifically fields, attributes, and anonymous data propagation through lazy evaluation trees. Today, we advance from basic element manipulation to formal procedural design patterns.

Our goal is to understand the mathematical mechanics that prevent procedural assets from looking synthetic or repetitious. We will dissect point generation algorithms across non-Euclidean surfaces, derive rotation matrices to align coordinate spaces using surface normals, and use continuous 1D splines to construct watertight 2D manifolds via sweeping operations. These principles form the computational backbone of procedural environment art and engineering modeling.

---

## Slide 2: Spatial Point Processes — Random vs. Poisson Disk

### CIS 536/736 - Lecture 15

* **Independent and Identically Distributed (IID) Random:**
* Sample coordinates evaluated via barycentric coordinates $(\lambda_1, \lambda_2, \lambda_3)$ on triangular facets.
* Spatial Poisson Process: Variance equals mean $(\mathrm{Var}(N) = \mu)$, producing Poisson clumping and spatial clustering.


* **Poisson Disk Sampling (Blue Noise):**
* Enforces a hard minimum Euclidean boundary: $\forall \mathbf{p}_i, \mathbf{p}_j \in \mathcal{P}, \Vert{}\mathbf{p}_i - \mathbf{p}_j\Vert{} \ge r_{\min}$.
* Spatial distribution exhibits high-frequency noise characteristics without low-frequency clumps.


* **Algorithmic Complexity & Acceleration:**
* Naive dart throwing: $\mathcal{O}(N^2)$ worst-case convergence.
* Bridson's spatial grid acceleration: $\mathcal{O}(N)$ utilizing background uniform spatial hash grids with cell size $\delta = \frac{r_{\min}}{\sqrt{d}}$.



**Speaker Notes:**
When utilizing the `Distribute Points on Faces` node, your choice of distribution algorithm dictates both memory layout and visual realism. The default random scattering treats surface facets as independent probability bins using uniform barycentric sampling. However, independent uniform sampling naturally produces Poisson clustering—points randomly aggregate into dense clumps while leaving noticeable voids.

In biological and physical systems, entities compete for space, sunlight, or structural volume. Poisson disk sampling addresses this by enforcing a hard minimum distance metric, $r_{\min}$, around every candidate point, producing blue-noise spectral properties. In Geometry Nodes, this is implemented using an accelerated variant of Bridson's algorithm, leveraging spatial hash buckets to test candidate points in $\mathcal{O}(1)$ time. For instancing solid structures like foliage, debris, or architectural elements, Poisson disk distribution is mandatory to prevent self-intersection and physical clipping.

---

## Slide 3: Scalar Field Evaluation & Density Modulation

### CIS 536/736 - Lecture 15

* **Probability Weighting Functions:**
* Local point emission probability driven by normalized scalar intensity: $P(\mathbf{x}) \propto \phi(\mathbf{x}) \in [0, 1]$.
* Elimination of candidates via rejection sampling: accept if $\xi < \phi(\mathbf{x})$, where $\xi \sim \mathcal{U}(0, 1)$.


* **Continuous Noise Functions:**
* Fractional Brownian Motion (fBm): Multi-octave Perlin/Simplex lattices:

$$\phi(\mathbf{x}) = \sum_{k=0}^{M-1} \gamma^k f(2^k \mathbf{x})$$




* **Discrete Attribute Transfer:**
* Sampling continuous barycentric weights from discrete vertex color channels and vertex weight groups.


* **Selection Sockets vs. Density Inputs:**
* Density Max modulates global sampling frequency; Selection enforces binary boolean pruning after spatial generation.



**Speaker Notes:**
Distributing points uniformly across an entire mesh surface is rarely the desired outcome in production. We modulate spatial point processes using scalar probability fields. In Geometry Nodes, the `Density` input on the distribution node modulates the sampling probability before points are instantiated. Conversely, the `Selection` boolean socket performs an explicit cull on already evaluated points.

When using procedural textures, such as Perlin or Voronoi noise, we sample continuous scalar fields directly in object coordinate space. By piping these procedural fields through color ramps or step functions, we establish threshold masks. For hand-authored art direction, vertex groups provide discrete weights across the topology. The node engine interpolates these vertex weights across polygon faces using standard barycentric coordinates, allowing artist-painted boundary maps to control procedural asset placement.

---

## Slide 4: Coordinate Space Alignment — Align Euler to Vector

### CIS 536/736 - Lecture 15

* **Surface Normals & The Vector Field:**
* Face normals provide the directional pole: $\mathbf{n} = \frac{\frac{\partial \mathbf{S}}{\partial u} \times \frac{\partial \mathbf{S}}{\partial v}}{\left\Vert{}\frac{\partial \mathbf{S}}{\partial u} \times \frac{\partial \mathbf{S}}{\partial v}\right\Vert{}}$.


* **Computing the Transformation Operator:**
* Rotation from source unit vector $\mathbf{v}_{\text{src}}$ to target surface normal $\mathbf{n}$.
* Axis of rotation: $\mathbf{a} = \frac{\mathbf{v}_{\text{src}} \times \mathbf{n}}{\Vert{}\mathbf{v}_{\text{src}} \times \mathbf{n}\Vert{}}$, Rotation angle: $\theta = \arccos(\mathbf{v}_{\text{src}} \cdot \mathbf{n})$.


* **Rodrigues' Rotation Formula & Quaternion Equivalence:**

$$\mathbf{R}(\theta, \mathbf{a}) = \mathbf{I} + (\sin\theta)\mathbf{K} + (1 - \cos\theta)\mathbf{K}^2$$


* **Gimbal Lock Avoidance:**
* Geometry Nodes calculates alignment internally via unit quaternions $\mathbf{q} = \left[\cos\left(\frac{\theta}{2}\right), \mathbf{a}\sin\left(\frac{\theta}{2}\right)\right]$ before outputting Euler angles.



**Speaker Notes:**
When geometry is instanced onto points, the instances inherit the global coordinate space by default, leaving their local $+Z$ axes oriented along $(0, 0, 1)$ regardless of surface curvature. To orient instances along the terrain, we must calculate a rotation transformation that maps the instance's canonical up-vector to the face normal.

The `Align Euler to Vector` node computes this rotation operator. Mathematically, it calculates the cross product between the source axis and the surface normal to find the orthogonal axis of rotation, and the dot product to derive the rotation angle. While the node outputs an Euler vector to interface with standard transform sockets, the internal mathematics are computed using unit quaternions. This prevents gimbal lock and handles edge cases where target normals approach antipodal directions relative to the source vector.

---

## Slide 5: Parametric Entropy — Stochastic Transform Variation

### CIS 536/736 - Lecture 15

* **Combating Visual Repetition:**
* Human visual perception is highly sensitive to repeated patterns, collinear orientations, and identical scale matrices.


* **Decoupled Randomization Layers:**
* **Scale:** Uniform vs. Non-Uniform affine transformations:

$$\mathbf{S} = \begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & s_z \end{bmatrix}$$


* Uniform scaling preserves aspect ratio; non-uniform anisotropic scaling varies physical dimensions.


* **Constrained Rotation Decomposition:**
* Fixing surface normal alignment along $+Z$ while sampling yaw randomly: $\psi \sim \mathcal{U}(0, 2\pi)$.


* **Deterministic Pseudo-Randomness:**
* PCG (Permuted Congruential Generator) hashing driven by stable integer `ID` attributes combined with an external `Seed`.



**Speaker Notes:**
Visual repetition breaks the illusion of natural procedural generation. To mitigate this, we introduce controlled parametric entropy across the transform matrices of the instanced collection. We decouple this variation into explicit scale and rotation passes.

For scaling, drawing from a uniform distribution between bounds, such as $[0.85, 1.15]$, scales instances proportionally without introducing distortion. For elements like foliage or debris, however, you may want independent $Z$-axis scaling to vary height while maintaining footprint width.

For rotation, we rarely want full 3D rotation, which would overturn our terrain-aligned normals. Instead, we compose rotations: we retain the normal-aligned base transform and inject a randomized local yaw offset around the local $Z$-axis.

Crucially, the `Random Value` node in Blender relies on stable hash functions evaluated per point index or ID attribute. Changing an asset elsewhere in the pipeline will not scramble the random transforms of existing instances as long as their unique IDs remain constant.

---

## Slide 6: Continuous Topological Sweeps — Curve to Mesh

### CIS 536/736 - Lecture 15

* **1D Manifolds in $\mathbb{R}^3$:**
* Parametric space curve: $\mathbf{r}(t) = \big(x(t), y(t), z(t)\big), \quad t \in [0, 1]$.


* **The Sweep Operation:**
* Extruding an arbitrary 2D cross-sectional planar profile curve $\mathcal{C}_P$ along the trajectory of a 3D guide curve $\mathcal{C}_G$.


* **The Construction of the Moving Frame (Frenet-Serret vs. Parallel Transport):**
* **Tangent:** $\mathbf{T}(t) = \frac{\mathbf{r}'(t)}{\Vert{}\mathbf{r}'(t)\Vert{}}$
* **Normal:** $\mathbf{N}(t) = \frac{\mathbf{T}'(t)}{\Vert{}\mathbf{T}'(t)\Vert{}}$
* **Binormal:** $\mathbf{B}(t) = \mathbf{T}(t) \times \mathbf{N}(t)$


* **Singularity Mitigation:**
* Transitioning from Frenet frames to rotation-minimizing (Bishop/Parallel Transport) frames prevents twisting at zero-curvature inflection points ($\kappa(t) = 0$).



**Speaker Notes:**
Instancing handles discrete objects, but continuous geometric elements—such as wiring, structural conduits, vines, or dynamic piping—require sweeping operations. The `Curve to Mesh` node takes a 1D parametric curve and extrudes an arbitrary 2D profile curve along its trajectory to generate a 2-manifold surface mesh.

To construct this surface, the engine must construct an orthogonal coordinate frame at every infinitesimal step along the curve to orient the 2D profile vertices. While classical differential geometry relies on the Frenet-Serret frame, that system collapses whenever a curve exhibits zero curvature, causing sudden $180^\circ$ surface inversions. Blender uses parallel transport and rotation-minimizing frames (RMF) to slide the profile along the curve trajectory smoothly, preserving consistent surface topologies without topological pinching or axial twisting.

---

## Slide 7: Non-Uniform Domain Profiling & Spline Parameterization

### CIS 536/736 - Lecture 15

* **Normalized Arc-Length Parameterization:**
* Normalized factor mapping along the curve:

$$s(t) = \frac{\int_0^t \Vert{}\mathbf{r}'(\tau)\Vert{} \, d\tau}{\int_0^1 \Vert{}\mathbf{r}'(\tau)\Vert{} \, d\tau} \in [0, 1]$$




* **The `Spline Parameter` Node:**
* Exposes local normalized evaluation parameter $s(t)$ and raw cumulative arc-length distances.


* **Direct Radius Manipulation:**
* `Set Curve Radius` maps scalar functions directly into control point attributes: $R(s) = f(s)$.


* **Transfer Functions via Analytic Curves:**
* Using cubic Bézier shaping nodes (`Float Curve`) to map input arc domain $s$ to radius scalars: $R(s) = \mathrm{BezierInterp}(s; \mathbf{P}_0, \mathbf{P}_1, \mathbf{P}_2, \mathbf{P}_3)$.



**Speaker Notes:**
Extruding a uniform circle along a curve yields a static cylinder. To generate organic or dynamic engineered forms, we must vary cross-sectional profiles continuously along the spline domain. The `Spline Parameter` node provides access to the normalized arc-length coordinate $s(t)$, which spans from $0.0$ at the origin terminal to $1.0$ at the termination point.

By routing the `Factor` output of the `Spline Parameter` into a transfer function—such as a mathematical power series or a visual `Float Curve` node—we author continuous radius profiles. The output drives the `Set Curve Radius` node prior to running the `Curve to Mesh` operator. This allows you to parameterize structural flares, functional tapers, architectural joints, and roots non-destructively, defining clean geometric silhouettes from raw mathematical functions.

---

## Slide 8: Lab 3a Implementation Architecture

### CIS 536/736 - Lecture 15

* **System Pipeline Design:**
* Construct a unified, parametric, non-destructive environmental prop generator.


* **Component 1: Structural Macro-Geometry:**
* Spline sweeps and procedural polyhedral extrusion forming the load-bearing framework.


* **Component 2: Constrained Sub-Element Instancing:**
* Surface-normal-aligned Poisson disk distributions targeting detail geometry collections.


* **Component 3: Stochastic Noise Displacement:**
* Bounded affine perturbations modulating local scale factors and rotational yaw.


* **Component 4: Art-Directable Modifier API:**
* Group inputs exposed cleanly to the Blender UI modifier stack for high-level parameter control.



```
[Group Input: Density, Seed, Profile]
   │
   ├──> [Curve to Mesh] ──> [Transform / Displace] ──┐
   │                                                 ▼
   └──> [Distribute Points (Poisson)] ──> [Instance on Points] ──> [Join Geometry] ──> [Group Output]

```

**Speaker Notes:**
In Lab 3a this Friday, you will synthesize these distribution, alignment, and sweeping principles into a production-ready procedural asset generator. The diagram illustrates the core dataflow architecture you will implement.

Your node graph must decouple structural base generation from instanced detail passes. The base geometry—whether generated via procedural spline networks or transformed primitive lattices—supplies both the visible foundation and the surface manifold used to generate Poisson point fields. Details are instanced on those point sets, aligned to surface normals, and randomized within safe ranges.

Critically, you will not hardcode values inside internal nodes. You will wire operational controls back to the `Group Input` interface, authoring an explicit external API for technical artists.

---

## Slide 9: Engine Ingestion Boundaries — The OpenUSD Horizon

### CIS 536/736 - Lecture 15

* **Execution Boundary:**
* Real-time engines (Unity 6 / Unreal Engine 5) do not parse Blender Geometry Node trees natively; they consume standardized scene description graphs and polygon buffers.


* **Instancing Semantics in OpenUSD:**
* Native Point Instancer (`UsdGeomPointInstancer`): Stores single prototype mesh referenced across arrays of translation, orientation, and scale vectors.


* **The `Realize Instances` Node Operator:**
* Destructively collapses lightweight transformation matrices into explicit, monolithic vertex, edge, and polygon index lists.


* **Memory & Storage Pipeline Trade-offs:**
* Explicit Geometry: Ballooning file payloads, high draw calls, simplified parsing.
* Point Instancers: Minimal disk footprint, GPU hardware instancing preserved, strict prototype separation required.



```
Geometry Nodes Runtime                 OpenUSD Serialization
[Instances (Pointer + Matrix)] ───► UsdGeomPointInstancer (Prototypes + Arrays)
            │
    [Realize Instances]
            ▼
[Explicit Mesh Data (V, E, F)] ───► UsdGeomMesh (De-duplicated Polygon Topology)

```

**Speaker Notes:**
As computer scientists and pipeline engineers, you must always look downstream toward ingestion boundaries. A common point of failure occurs when students expect a game engine to evaluate dynamic DCC node graphs natively. Engines like Unity 6 evaluate compiled meshes and standardized scene graphs.

When authoring in Geometry Nodes, instances exist as lightweight memory pointers paired with $4 \times 4$ transformation matrices. When exporting through OpenUSD, if these remain instances, they translate cleanly into a `UsdGeomPointInstancer` schema. However, if your target engine integration fails to parse point instancers, or if instances must be unified into a single collision shell, the `Realize Instances` node collapses that relational data into explicit vertex arrays.

Be mindful of the performance trade-offs: realization destroys memory savings and increases payload sizes exponentially, whereas native instancing leverages hardware-accelerated instanced draw calls on the GPU.

---

## Slide 10: Summary & Lab 3a Deliverables

### CIS 536/736 - Lecture 15

* **Core Theoretical Concepts Mastered:**
* Spatial Poisson processes and elimination of stochastic clustering artifacts.
* Normal-vector coordinate re-orientation via rotational transformations.
* Smooth 1D-to-2D topological sweeps via parallel transport curve framing.
* Exposure of procedural modifier interfaces via discrete parameter extraction.


* **Preparation for Lab 3a:**
* Review the Blender 4.2 LTS Manual on `Distribute Points on Faces` and `Curve to Mesh`.
* Select your asset target: Parameterized architectural element, structural conduit array, or organic terrain formation.


* **Submission Protocol:**
* Source `.blend` file paired with downstream `.usda` export containing validated schema topology.



**Speaker Notes:**
To summarize today's lecture: high-fidelity procedural generation relies on mathematical control over spatial sampling, vector coordinate transformations, and topological continuous sweeps. Combining Poisson disk distributions with procedural noise fields produces structured, natural distributions. Aligning instance coordinate spaces to surface normals ensures clean integration with base geometry, and parallel-transport curve sweeping enables the generation of dynamic, watertight structural meshes.

For Lab 3a on Friday, review the manual pages covering point distribution and curve modeling. Select a focused, parameterized asset target—such as an industrial pipe system, a weathered stone balustrade, or a procedural tree branch. Come to lab prepared to translate these mathematical patterns into a robust, exportable Geometry Nodes graph. See you in the lab.
