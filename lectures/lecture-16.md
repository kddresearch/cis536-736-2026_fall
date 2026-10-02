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
