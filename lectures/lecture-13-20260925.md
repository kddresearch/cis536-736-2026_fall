# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 2:** Shading and Lighting | **Module 4:** Shader Graph & Procedural Textures | **Lecture 13:** Lab 2b — Shader Graph Procedural Material Build | Friday, September 25, 2026 |

---

## Slide 1: Lab 2b — Procedural Material Build
### CIS 536/736 - Lecture 13
* Lab Initialization: HDRP Lit Shader Graph
* Blackboard Parameterization
* Time-Driven Procedural Noise
* Material Application (Emission & Displacement)
* Pipeline Integration with OpenUSD

**Speaker Notes:**
Welcome to Lab 2b. On Monday and Wednesday, we covered the theory behind visual scripting, procedural noise, and vertex displacement. Today, we are putting our hands on the keyboard. We are going to build a dynamic, time-driven procedural material from scratch using Unity 6's Shader Graph, and we will integrate this directly with the OpenUSD assets you imported in previous modules. 

---

## Slide 2: Lab Setup & Initialization
### Creating the HDRP Lit Shader Graph
* **Asset Creation:** `Right-Click -> Create -> Shader Graph -> HDRP -> Lit Shader Graph`
* **Material Instantiation:** Right-click the new shader asset and select `Create -> Material`.
* **The Master Stack:** Verify the Vertex and Fragment contexts are visible.
* **Surface Options:** Ensure the material surface type is set appropriately (Opaque vs. Transparent) for your intended effect in the Graph Inspector.

**Speaker Notes:**
Step one is establishing the asset pipeline. In your Project window, create a new HDRP Lit Shader Graph. Name it something descriptive, like `FX_PulsatingAlienSkin`. Immediately create a Material from that shader. This is crucial: the Shader Graph is the blueprint; the Material is the instance you actually apply to your 3D models. Open the graph and verify you have access to both the Vertex and Fragment blocks in your Master Stack.

---

## Slide 3: Configuring the Blackboard
### Exposing Art-Directable Properties
* **The Blackboard:** The interface for defining public variables.
* **Required Properties for MP 3:**
  * `Color` (HDR enabled for Emission).
  * `Float` (Noise Scale).
  * `Vector2` (Scroll Speed).
  * `Float` (Displacement Strength).
* **Reference Names:** Naming conventions matter for downstream C# scripting (e.g., `_NoiseScale`).

**Speaker Notes:**
Before we drop any logic nodes, we need to define our inputs. The Blackboard is how we expose parameters to the Unity Inspector, allowing artists (or yourselves) to tweak the material without opening the graph. For Machine Problem 3, you are required to expose your variables. Create an HDR Color property for your emission, a float for your noise scale, a Vector2 to control X/Y scroll speed, and a float for your displacement strength.

---

## Slide 4: Noise Implementation
### Constructing the Procedural Network
* **The Node:** Instantiate a `Voronoi` or `Simple Noise` node.
* **Scale Control:** Drag your `Noise Scale` property from the Blackboard and plug it into the Scale port.
* **Previewing:** Observe the node preview window to verify the scale matches your expectations.
* **Mathematical Foundation:** The node calculates the noise algorithm locally per fragment based on the mesh's UVs (or World Position if using Triplanar).

**Speaker Notes:**
Now we build the core math. Drop a Voronoi or Simple noise node onto the canvas. Drag your `Noise Scale` property from the Blackboard and connect it. You should immediately see the node's preview window update. This is the advantage of visual scripting: instant feedback. This node is now calculating a 0-to-1 grayscale value for every pixel on your mesh based on the underlying procedural algorithm.

---

## Slide 5: Driving Time & Animation
### Making the Noise Dynamic
* **The Time Node:** Provides a continuous, incremental float value based on application runtime.
* **Tiling And Offset Node:** Used to scroll UV coordinates over time.
* **The Math:** `Time * Scroll Speed (Vector2) = Offset`.
* **The Routing:** Plug the output of `Tiling And Offset` into the `UV` port of your Noise node.

**Speaker Notes:**
Static noise is boring. Let's make it move. Drop a `Time` node. We don't want to pipe Time directly into the noise, because we need to control the speed and direction. Drop a `Multiply` node, multiply `Time` by your `Scroll Speed` Vector2 property, and plug the result into the `Offset` port of a `Tiling And Offset` node. Finally, route that moving UV data into the UV port of your Noise node. Your noise should now be scrolling in the preview.

---

## Slide 6: Material Application — Emission
### Creating Dynamic Glow
* **HDR Color:** Multiply your scrolling noise output (0 to 1) by your `HDR Color` property.
* **The Fragment Context:** Plug the result of that multiplication directly into the `Emission` port on the Master Stack.
* **Bloom Interaction:** In HDRP, emission values exceeding 1.0 (HDR intensity) will automatically trigger the Post-Processing Bloom effect.

**Speaker Notes:**
Let's apply this to the Fragment context. Take the output of your scrolling noise—which is just a stream of numbers between 0 and 1—and multiply it by your HDR Color property. Plug the result into the Emission port on the Master Stack. Because the noise fluctuates between 0 and 1, your emission intensity will pulse and flow. If your HDR color has an intensity greater than 1, it will trigger HDRP's Bloom effect, giving you a proper, cinematic glow.

---

## Slide 7: Material Application — Vertex Displacement
### Deforming the Silhouette
* **Vertex Context:** Moving to the top half of the Master Stack.
* **Normal Vector Node:** Retrieves the facing direction of the vertex (Object Space).
* **The Math:** `Normal Vector * (Noise Output * Displacement Strength)`.
* **Position Node:** Add the result to the current `Position` (Object Space) and plug it into the Master Stack's `Position` port.

**Speaker Notes:**
Now let's physically warp the mesh. Move up to the Vertex context on the Master Stack. We need to push the vertices outward. Drop a `Normal Vector` node to get the direction the vertex is facing. Multiply that Normal by your scrolling Noise output, and multiply *that* by your `Displacement Strength` property. Add this entire calculation to the current Vertex Position, and plug it in. Your mesh is now physically boiling or breathing based on the procedural noise.

---

## Slide 8: Pipeline Integration
### Applying the Material to OpenUSD Assets
* **Scene Setup:** Drag an imported OpenUSD Stage or asset into your Unity hierarchy.
* **Material Assignment:** Drag and drop your compiled Material instance onto the Mesh Renderer of the USD asset.
* **Validation:** The asset should immediately reflect the scrolling emission or displacement.
* **Debugging:** If the USD asset's UVs are broken, you may need to implement the Triplanar mapping technique discussed on Wednesday.

**Speaker Notes:**
The shader is compiled; now we integrate it into the pipeline. Find the OpenUSD asset you imported during Module 3. Drag your Material instance onto the asset in the scene. You should immediately see the results. If your displacement looks jagged, ensure your USD mesh has enough subdivisions (vertex density) to support displacement. If the noise looks stretched, the USD asset might have bad UVs, and you will need to swap your UV input for a Triplanar projection.

---

## Slide 9: Iteration & Art Direction
### Tuning from the Inspector
* **The Power of the Blackboard:** You do not need to open the Shader Graph to iterate.
* **Runtime Tuning:** Enter Play Mode. Select your material in the Project window.
* **Live Tweaking:** Adjust the Scroll Speed, Noise Scale, and HDR Color intensity directly in the Inspector to see real-time updates.

**Speaker Notes:**
This is where the pipeline pays off. Because we took the time to expose our properties to the Blackboard, we can now art-direct the material without touching the node math. Enter Play Mode, select your material, and start tweaking sliders. Change the scroll speed, crank up the emission intensity, or adjust the noise scale. This rapid iteration is the core advantage of visual scripting in a modern game engine.

---

## Slide 10: MP 3 Deliverable Prep
### Verification Checklist
* **Deliverable Requirement:** Submit a Unity Package containing your custom Shader Graph and Material.
* **Verification 1:** Does the material use procedural noise (no bitmap textures)?
* **Verification 2:** Is the noise parameterized by Time?
* **Verification 3:** Are Scale, Speed, and Intensity exposed to the Blackboard?
* **Verification 4:** Is it successfully applied to your OpenUSD asset?

**Speaker Notes:**
Before we wrap up, let's verify your progress against Machine Problem 3. MP 3 requires you to submit this custom Shader Graph. Check your work: Are you using pure procedural math, or did you accidentally use a texture sample? Is it moving over time? Did you expose your variables to the Blackboard? If you can check all these boxes and it looks correct on your OpenUSD asset, your lab work is complete and ready for packaging.

---

---

## Slide 11: USD Stage Validation Pipeline
### Bridging OpenUSD and Unity Shaders
* **The Goal:** Ensure our imported OpenUSD stage contains valid target primitives before attempting vertex displacement.
* **The Python Validation Layer:**
  * We use the `pxr` USD core library (specifically `Usd` and `UsdGeom`).
  * `stage.Traverse()`: Iterates through the entire scene graph.
  * `prim.IsA(UsdGeom.Mesh)`: Confirms the primitive has geometry that can be deformed.
* **The Threat:** Applying a vertex displacement shader to an empty transform or non-mesh primitive will result in zero visual output. Always validate the target path first.

**Speaker Notes:**
Before we push our new shader to the GPU, we have to make sure the target actually exists. If you drop a vertex displacement material onto an empty USD transform node, nothing happens, and you'll waste an hour debugging your math. In your Python environment, use the `pxr` library to traverse your stage. We are looking specifically for `UsdGeom.Mesh` primitives. Only those nodes have the actual vertices our shader can push around.

---

## Slide 12: Preparing for C# Parameterization
### Bridging the Graph to the CPU
* **The Blackboard as an API:** Exposing a property (e.g., `_NoiseScale`) creates an entry point for C# scripts.
* **Property IDs:**
  * Unity accesses shader properties via string lookup (e.g., `"_NoiseScale"`).
  * *Performance Optimization:* String lookups are slow. We cache them as integer hashes in `Start()` using `Shader.PropertyToID()`.
* **The Objective:** We are moving control of the animation from the autonomous `Time` node inside the shader to a dynamic C# controller on the CPU.

**Speaker Notes:**
You have exposed `NoiseScale`, `AnimationSpeed`, and `DisplacementStrength` to the Blackboard. Right now, you can tweak them in the Inspector. But games are dynamic. For Machine Problem 3, you must drive these values via code. The Blackboard properties act as an API. However, telling the GPU to update a property by its string name—like `"_NoiseScale"`—every single frame is computationally expensive. We avoid this by caching that string into an integer hash the moment the script boots up.

---

## Slide 13: Generating the C# Wrapper
### MP 3 Deliverable Code
* **The `Material.SetFloat()` Method:** The primary pipeline to push float values from CPU RAM to GPU VRAM.
* **Execution Flow in C#:**
  1. `Start()`: Cache integer hashes (e.g., `noiseScaleId = Shader.PropertyToID("_NoiseScale");`).
  2. `Update()`: Push the serialized inspector values to the GPU (e.g., `proceduralMaterial.SetFloat(noiseScaleId, noiseScale);`).
* **Dynamic Oscillation:**
  * Use `Mathf.PingPong(Time.time, displacementStrength)` in `Update()` to create a heartbeat or breathing effect programmatically.

**Speaker Notes:**
This is the C# boilerplate you will use for MP 3. In your `Start()` function, you grab your integer hashes. In your `Update()` function, which runs every frame, you use the `Material.SetFloat()` method to push the data across the bus to the GPU. For the extra mile on your deliverable, don't just pass static values. Use a mathematical function like `Mathf.PingPong()` combined with `Time.time` to make your displacement pulse dynamically from the C# side, rather than relying solely on the shader's internal time node.

---

## Slide 14: MP 3 Final Submission Checklist
### The Portcullis Requirements
* [ ] **Compilation:** HDRP Shader Graph compiles with 0 errors.
* [ ] **USD Application:** Material instance is successfully assigned to a verified `.usd` stage mesh.
* [ ] **Displacement Math:** Geometry physically deforms using a noise function piped into the Vertex Position node.
* [ ] **C# Controller:** The `ShaderGraphController.cs` script successfully manipulates the shader properties in real-time via Play Mode.
* [ ] **GitOps Commit:** Unity Project (excluding `Library/` and `Temp/`) pushed to your `cis536_736_hw` repo.

**Speaker Notes:**
This brings us to the final checklist for Machine Problem 3. If you followed the Python validation, your USD mesh will accept the shader. If you followed the C# generation, your controller will drive the Blackboard. Before you commit, test your scene in Play Mode. Ensure the sliders in your C# script actually alter the glowing or boiling effect on the mesh in real-time. Remember to clean your repository—do not commit your `Library` or `Temp` folders—and push to GitHub before the deadline.
