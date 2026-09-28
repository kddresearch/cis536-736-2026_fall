# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Procedural Generation and Rigging | **Module 5:** Geometry Nodes Fundamentals & Patterns | **Lecture 15:** Geometry Nodes Patterns — Scatter, Distribution, Parametric Control | Wednesday, September 30, 2026 |

---

## Slide 1: Geometry Nodes Patterns
### CIS 536/736 - Lecture 15
* **Point Distribution Algorithms:** Random vs. Poisson Disk.
* **Density & Selection Masking:** Restricting instance spawning.
* **Aligning Euler to Vector:** Normal-based orientation.
* **Parametric Randomization:** Breaking uniformity natively.
* **Curve-to-Mesh Operations:** Sweeping structural geometry.

**Speaker Notes:**
Welcome back. On Monday, we covered the basics of the Geometry Nodes modifier, attributes, and instancing. Today, we elevate those concepts into reusable procedural patterns. We are looking at the math that makes procedural generation feel organic instead of synthetic. We will cover advanced point scattering, rotation alignment, and generating complex shapes from simple curves. 

---

## Slide 2: Point Distribution Algorithms
### CIS 536/736 - Lecture 15
* **Random Scatter:** Places points completely stochastically across the mesh surface. 
* **The Clustering Problem:** Pure random distribution often results in instances clipping into one another.
* **Poisson Disk Scatter:** Introduces a `Distance Min` parameter to enforce spacing.
* **Performance Cost:** Poisson Disk is computationally heavier but visually superior for large, solid assets like buildings or trees.

**Speaker Notes:**
When you use the `Distribute Points on Faces` node, your first instinct is to leave it on "Random." If you are instancing grass or small debris, that's fine. But if you are instancing oak trees, random scattering will spawn trees inside of other trees. Changing the algorithm to "Poisson Disk" allows you to set a minimum radius around each point where no other point can spawn. It costs slightly more compute time, but it immediately solves mesh clipping.

---

## Slide 3: Density & Selection Masking
### CIS 536/736 - Lecture 15
* **The Selection Socket:** A boolean (True/False) input that dictates whether a point is allowed to spawn.
* **Noise Masking:** Piping a `Noise Texture` through a `ColorRamp` or `Math (Greater Than)` node into the Selection socket.
* **Vertex Groups:** Using painted vertex weights from the base mesh to drive the density factor.
* **Organic Clustering:** Creates natural clearings, paths, and biome transitions.

**Speaker Notes:**
You rarely want instances covering 100% of a surface. To create clearings in a forest, you use the Selection socket. If you plug a Noise Texture into a Math node set to "Greater Than 0.5", and plug that into Selection, your instances will only spawn in the white areas of the noise. Alternatively, you can paint a Vertex Group on your base mesh to define exactly where a road should be, and invert that data to spawn trees everywhere *except* the road.

---

## Slide 4: Aligning Euler to Vector
### CIS 536/736 - Lecture 15
* **The Alignment Problem:** By default, instanced geometry spawns pointing straight up (Global Z), ignoring the slope of the surface.
* **The Node:** `Align Euler to Vector` calculates the rotational offset needed to match a vector.
* **The Normal Attribute:** The vector pointing directly perpendicular away from the mesh face.
* **The Solution:** Plug the mesh `Normal` into the `Vector` socket, and output the Euler rotation to the `Instance on Points` node.

**Speaker Notes:**
If you scatter mushrooms across a bumpy terrain, they will all point straight up at the sky, regardless of the angle of the hill they are on. That looks broken. We need them to point away from the surface. We do this by capturing the `Normal` attribute of the terrain, and plugging it into an `Align Euler to Vector` node. This translates the normal vector into Euler rotation data, ensuring your instances respect the slope of the geometry.

---

## Slide 5: Parametric Randomization
### CIS 536/736 - Lecture 15
* **The Uniformity Trap:** Identical scaling and rotation instantly ruins organic procedural generation.
* **The `Random Value` Node:** Generates a unique float or vector per evaluated instance based on the node's Seed.
* **Scale Randomization:** Plug a Float Random Value (e.g., 0.8 to 1.2) into the Instance Scale socket.
* **Rotation Randomization:** Plug a Vector Random Value into the Z-axis rotation to spin instances randomly on their local up-axis.

**Speaker Notes:**
Even with perfect distribution and surface alignment, a forest of identical trees looks like a video game from 1998. You must introduce parametric entropy. Drop a `Random Value` node. Set it to output a float between 0.8 and 1.2, and plug it into the instance scale. Every single tree will evaluate that node differently, giving you a canopy of varying heights. Adding a random Z-rotation ensures that the branching patterns don't all face the exact same direction.

---

## Slide 6: Curve-to-Mesh Operations
### CIS 536/736 - Lecture 15
* **The Concept:** Extruding a 2D profile (circle, square) along a 3D path (spline) to generate tubular geometry.
* **The Node:** `Curve to Mesh`.
* **Inputs:** 
  * `Curve`: The guiding path/spline.
  * `Profile Curve`: The cross-section shape.
* **Applications:** Instantly generating pipes, cables, vines, or structural beams.

**Speaker Notes:**
Point distribution is great for scattering, but what about continuous geometry like vines or cyberpunk cables? For this, we use the `Curve to Mesh` node. It takes two inputs: a guiding curve, and a profile curve. If your guide is a sprawling bezier curve, and your profile is a small circle, Blender will sweep that circle along the entire path, instantly generating a 3D pipe. 

---

## Slide 7: Advanced Curve Deformation
### CIS 536/736 - Lecture 15
* **Curve Radius:** The thickness of the curve at any given control point.
* **The `Set Curve Radius` Node:** Allows procedural manipulation of the profile thickness before it becomes a mesh.
* **Spline Parameter:** A 0-to-1 gradient from the start of the curve to the end.
* **Tapering:** Passing the Spline Parameter through a `Float Curve` node to smoothly taper a vine to a point at its tip.

**Speaker Notes:**
A standard pipe is boring. A vine needs to be thick at the root and taper to a point at the end. We achieve this by manipulating the Curve Radius before converting it to a mesh. We can read the `Spline Parameter`—which gives us a 0 at the start of the curve and a 1 at the end—and plug that into a Float Curve graph. We can visually draw a tapering curve, and the generated mesh will instantly respect that thickness profile.

---

## Slide 8: Preparing for Lab 3a
### CIS 536/736 - Lecture 15
* **Lab 3a Goal:** Build a fully parameterized, non-destructive procedural asset.
* **Component 1:** A base geometry structure (Curve-to-mesh or primitive math).
* **Component 2:** An instancing system with Poisson Disk scattering.
* **Component 3:** Organic randomization (scale/rotation noise).
* **Component 4:** At least 3 art-directable parameters exposed to the Modifier Blackboard.

**Speaker Notes:**
This Friday is Lab 3a. You are going to build your own procedural generator. I want you to combine the techniques from Monday and today. You will need a base structure, you will need to scatter detail geometry on top of it, and you must break the uniformity using random fields. Most importantly, you must expose your core variables—like density and scale limits—to the modifier panel so an artist can tweak them without opening your node graph.

---

## Slide 9: The OpenUSD Export Horizon
### CIS 536/736 - Lecture 15
* **The Engine Boundary:** Unity 6 cannot read Blender Geometry Node math. 
* **The Translation Layer:** OpenUSD requires explicit, realized geometry to render.
* **The `Realize Instances` Node:** Converts mathematically cheap instancing references into actual vertices and polygons prior to export.
* **The Tradeoff:** File size and export time will increase massively; this must be done selectively.

**Speaker Notes:**
Keep in mind where this asset is going. Unity 6 does not understand Geometry Nodes. If you try to export a node tree to OpenUSD, Unity will see nothing. Before we export, we have to bake the math into actual polygons. The `Realize Instances` node does exactly this. It takes your cheap, memory-efficient instances and turns them into heavy, explicit geometry that OpenUSD can serialize. You will do this at the very end of your graph.

---

## Slide 10: Wrapping Up
### CIS 536/736 - Lecture 15
* **Summary:** Mastering procedural patterns requires combining distribution, alignment, and randomization.
* **Action Items:** 
  * Decide on your Lab 3a asset target (e.g., ruined wall, asteroid, coral reef).
  * Review the Blender 4.2 Manual on `Instance on Points` and `Curve to Mesh`.
* **Next Class:** Friday, Lab 3a — Hands-on Geometry Nodes Procedural Asset Build.

**Speaker Notes:**
We have covered the foundational patterns of procedural generation. Your homework before Friday is to decide what you are going to build for Lab 3a. Do not aim for a procedural planet; aim for a procedural prop. A ruined pillar, a cluster of crystals, or a sci-fi crate. Review the documentation on instancing and curve sweeping, and come to the lab ready to build your node trees.

---
---

# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Procedural Generation and Rigging | **Module 5:** Geometry Nodes Fundamentals & Patterns | **Lecture 16:** Lab 3a — Geometry Nodes Procedural Asset | Friday, October 2, 2026 |

---

## Slide 1: Lab 3a — Geometry Nodes Procedural Asset
### CIS 536/736 - Lecture 16
* **Lab Objective:** Construct a non-destructive, parameter-driven asset in Blender 4.2 LTS.
* **Core Requirements:**
  * Base Geometry manipulation.
  * Poisson Disk scattering and instancing.
  * Parameter exposure (The Art-Directable API).
  * Realization of instances for OpenUSD export.
  * Python pipeline validation.

**Speaker Notes:**
Welcome to Lab 3a. Today, you are executing the procedural generation patterns we studied all week. Your objective is to build a modular, non-destructive asset generator. You will construct the base shape, scatter detail elements across it, and expose the control parameters to the modifier panel. Finally, we will prepare the asset for its eventual journey into Unity 6 via OpenUSD. Let’s get started.

---

## Slide 2: Asset Foundation
### CIS 536/736 - Lecture 16
* **Workspace Setup:** Open Blender 4.2 LTS. Enter the Geometry Nodes workspace.
* **The Canvas:** Add a base primitive (Plane, Cylinder, or Icosphere).
* **The Modifier:** Click "New" to attach a Geometry Nodes modifier to the primitive.
* **Structural Nodes:** Construct the base shape using `Transform`, `Extrude Mesh`, or `Subdivision Surface` nodes.

**Speaker Notes:**
Step one is setting up the canvas. Open Blender, delete the default cube, and drop in a base primitive. Switch to the Geometry Nodes workspace and attach a new modifier. Your `Group Input` is passing your raw primitive into the node tree. Before we scatter anything, use nodes like `Extrude Mesh` or `Transform` to give your base primitive its macro-shape. This is the foundation upon which your instances will rest.

---

## Slide 3: Point Scatter Setup
### CIS 536/736 - Lecture 16
* **The Scatter Node:** `Distribute Points on Faces`.
* **The Algorithm:** Set to **Poisson Disk** to prevent geometry clipping.
* **The Instance Node:** `Instance on Points`.
* **The Detail Object:** Create a separate, hidden collection containing the objects you want to instance (e.g., rocks, rivets, leaves), and drag it into the node tree using a `Collection Info` node.

**Speaker Notes:**
Now we add the micro-details. Insert a `Distribute Points on Faces` node and set it to Poisson Disk. Next, drop an `Instance on Points` node. But what are we instancing? Create a separate object in your outliner—like a simple low-poly rock—and hide it. Drag that rock from the outliner straight into your Geometry Nodes graph to create an `Object Info` node. Plug its geometry output into the `Instance` socket. You should now have rocks scattered across your base shape.

---

## Slide 4: Exposing Parameters to the Modifier
### CIS 536/736 - Lecture 16
* **The Goal:** Create an art-directable tool, not a hardcoded script.
* **The Mechanism:** Route internal node sockets to the blank output sockets of the `Group Input` node.
* **Required Expositions:**
  1. **Seed:** (Integer) Cycles random variations.
  2. **Density:** (Float) Controls the Poisson Disk maximum density.
  3. **Scale Variation:** (Vector/Float) Defines the boundaries for instance randomization.
* **The Result:** Controls appear dynamically in the Blender Modifier Properties panel.

**Speaker Notes:**
Hardcoding values is a failure of procedural design. We need an API for the artist. Take the `Density Max` socket from your distribution node and drag a wire all the way back to the empty socket on your `Group Input` node. Press 'N' to open the side panel and rename that input to "Asset Density." Repeat this for your Random Seed and Scale constraints. If you look at your Modifier panel on the right, you now have a custom UI to control your asset.

---

## Slide 5: Material Assignment
### CIS 536/736 - Lecture 16
* **The Disconnect:** Materials assigned in the standard properties panel often break when geometry is procedurally generated or replaced.
* **The Node:** `Set Material`.
* **Placement:** Insert this node immediately before the `Group Output` node.
* **Execution:** Select a basic Principled BSDF material from the dropdown. (Advanced shading will be handled in Unity).

**Speaker Notes:**
Because we are generating new geometry on the fly, Blender sometimes loses track of standard material assignments. To guarantee that our exported USD file retains material slots, we must assign them explicitly in the node graph. Drop a `Set Material` node right before your final output. Select a basic material slot. We don't need complex shader math here—we will build the HDRP shader in Unity later. We just need the material *slot* to exist for the USD export.

---

## Slide 6: Realizing Instances
### CIS 536/736 - Lecture 16
* **The Engine Constraint:** OpenUSD and Unity do not natively interpret Blender's procedural instancing logic.
* **The Node:** `Realize Instances`.
* **Placement:** Insert at the end of the instancing chain, before joining it with the base geometry.
* **The Effect:** Converts memory-efficient instance references into explicit, serialized vertex and face data.

**Speaker Notes:**
This is the most critical step for the pipeline. If you export this file right now, Unity will load your base shape, and the instances will be entirely missing. You must add a `Realize Instances` node. This tells Blender to stop treating the rocks as cheap mathematical references, and bake them into real, heavy polygons. Your polycount will spike, but this is required for the OpenUSD serializer to capture the data.

---

## Slide 7: Validation (Stress Test)
### CIS 536/736 - Lecture 16
* **The Test:** Rapidly manipulate the exposed Seed, Density, and Scale parameters in the Modifier panel.
* **Check 1 (Clipping):** Do instances intersect unnaturally? (Adjust Poisson Disk Distance Min).
* **Check 2 (Orphans):** Do instances float in mid-air? (Check Normal alignment and origin points).
* **Check 3 (Performance):** Does the viewport frame rate drop below acceptable thresholds when Realize Instances is active?

**Speaker Notes:**
Before we export, we stress-test the generator. Go to your Modifier panel. Scrub the Seed slider rapidly. Scrub the Density slider from 0 to maximum. Does the generator break? Do rocks suddenly float in the air, or clip into each other? If they do, your math is flawed, and you need to adjust your minimum distances or normal alignments. A good procedural tool must remain structurally sound regardless of the parameter inputs.

---

## Slide 8: Exporting to OpenUSD
### CIS 536/736 - Lecture 16
* **Preparation:** Ensure the object scale is applied (`Ctrl+A -> Scale`).
* **Execution:** `File -> Export -> Universal Scene Description (.usd / .usda)`.
* **Export Settings:**
  * Check `Export UVs` and `Export Normals`.
  * Ensure `Evaluation Mode` is set to `Render` or `Viewport` to capture the modifier stack output.
* **File Naming:** Use structural naming conventions (e.g., `PROP_ProceduralAsset_v1.usda`).

**Speaker Notes:**
The asset is validated. Now we push it across the boundary. Select your object, ensure its scale is applied, and navigate to File -> Export -> Universal Scene Description. In the export settings on the right, ensure UVs and Normals are checked, and confirm that the exporter is evaluating the modifier stack. I recommend exporting as `.usda` (ASCII format) initially, so you can read the file structure in a text editor if debugging is required.

---

## Slide 9: Python Validation Script
### CIS 536/736 - Lecture 16
* **The Problem:** Silent export failures resulting in empty transform nodes.
* **The Solution:** We use a Python Jupyter notebook to validate the USD topology.
* **The Library:** `from pxr import Usd, UsdGeom`.
* **The Execution:** The script traverses the stage and counts `UsdGeom.Mesh` primitives and their vertex counts to prove `Realize Instances` functioned correctly.

**Speaker Notes:**
Trust, but verify. A common pipeline error is exporting a `.usd` file that is just 5 kilobytes of empty transform nodes because the modifier stack failed to evaluate during export. Open the Jupyter Notebook provided in the Lab 3a assignment module. Run the Python `pxr` validation script against your newly exported file. It will traverse the USD stage and print out the vertex count of your mesh. If the vertex count matches Blender, the export is successful.

---

## Slide 10: Final Submission Checklist
### CIS 536/736 - Lecture 16
* [ ] Geometry Nodes graph utilizes fields and parameters (no destructive modifiers).
* [ ] Seed, Density, and Scale are exposed to the Modifier Panel.
* [ ] `Realize Instances` is implemented prior to output.
* [ ] Asset exports cleanly to `.usd` format without errors.
* [ ] Python validation confirms `UsdGeom.Mesh` presence and vertex counts.
* [ ] The `.blend` source and `.usd` export are committed to your `cis536_736_hw` GitHub repository.

**Speaker Notes:**
This is your exit criteria for Lab 3a. Verify that your graph is fully procedural, your parameters are exposed, and your instances are realized. Run the Python validation script to guarantee the `.usd` file is topologically sound. Once verified, commit both your `.blend` source file and the exported `.usd` artifact to your GitHub homework repository, and paste the commit URL into Canvas. Great work today. Next week, we bring these assets into Unity.
