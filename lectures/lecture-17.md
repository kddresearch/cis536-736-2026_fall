# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase [X]:** [Phase Name] | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 17:** OpenUSD Interchange for Procedural Assets — Variant Sets, Layer Referencing | Monday, October 05, 2026 |

---

## Slide 1: Welcome to Module 6
### CIS 536/736 - Lecture 17
* **Transition:** From Procedural Generation (Blender) to Pipeline Interchange (OpenUSD)
* The Asset Portability Problem
* Universal Scene Description (USD) Overview
* Non-Destructive Composition
* Layer Referencing
* Variant Sets

**Speaker Notes:**
Welcome to Module 6. Over the last week, we built procedural assets in Blender using Geometry Nodes. But a procedural generator living exclusively in a `.blend` file doesn't help a team of artists working in Maya, Houdini, or Unity. Today, we bridge that gap. We are covering OpenUSD—Universal Scene Description. USD is not just a file format like FBX or OBJ; it is a framework for non-destructive pipeline interchange. It allows multiple artists to compose and override complex scenes without ever overwriting each other's work.

---

## Slide 2: The Asset Portability Problem
### Why FBX and OBJ Are Not Enough
* **Destructive Bakes:** Exporting an `.fbx` permanently collapses modifiers and bakes procedural data into static triangles.
* **Monolithic Files:** Traditional formats store the entire scene in one heavy file. If two artists edit the file, you get a merge conflict.
* **Loss of Metadata:** Custom attributes (like the density fields you built in Geometry Nodes) are often stripped or mangled during export.
* **The Goal:** A format that retains hierarchical structure, supports non-destructive overrides, and scales to massive scenes.

**Speaker Notes:**
Think about the workflow without USD. You have a procedural sci-fi crate in Blender. To get it into Unity, you export an FBX. The exporter applies the modifiers, destroys your procedural controls, and spits out a static mesh. If the art director says, "make the crate taller," you have to go back to Blender, change the parameter, and overwrite the FBX. If someone else was tweaking the materials on that FBX in Unity, their work might break. The traditional pipeline is linear, brittle, and highly destructive.

---

## Slide 3: Universal Scene Description (USD)
### The Pixar Standard
* **Origin:** Developed by Pixar Animation Studios to handle the massive complexity of feature film pipelines.
* **Open Source:** Now managed by the Alliance for OpenUSD (AOUSD).
* **The Primitives (Prims):** The fundamental building blocks of USD. A Prim can be a mesh, a light, a material, or an empty transform group.
* **The Stage:** The composed result of the USD file hierarchy loaded into memory.

**Speaker Notes:**
Pixar developed USD because they had scenes with billions of polygons, handled by hundreds of artists across different software packages. They needed a way for a lighter to adjust the lighting while an animator was still tweaking a character's pose, without locking files or destroying data. The core concept in USD is the 'Prim'—a primitive. Everything in USD is a Prim organized in a hierarchical tree. When you open a USD file, the system compiles all these Prims into a 'Stage', which is what you actually see on screen.

---

## Slide 4: Non-Destructive Composition
### The Core Philosophy of USD
* **Composition Arcs:** The rules USD uses to combine multiple files into a single Stage.
* **Overrides, Not Edits:** When you change something in USD, you don't overwrite the original data. You write an "override" instruction in a new file.
* **Sparse Data:** A USD file only contains the data that has changed (the deltas), making it incredibly lightweight.
* **Collaboration:** An environment artist and a lighting artist can work on the exact same Stage simultaneously by writing to different layers.

**Speaker Notes:**
This is the magic of USD: Non-Destructive Composition. In a standard workflow, if you want a red version of a blue car, you duplicate the car model and change the material. Now you have two heavy models to maintain. In USD, you create a new, tiny text file that says, "Reference the blue car, but override the paint color to red." The original blue car file is completely untouched. You are layering instructions, not duplicating geometry.

---

## Slide 5: Layer Referencing
### Stacking the Scene
* **The SubLayer Arc:** Stacking USD files on top of each other like layers in Photoshop.
* **Higher Layers Win:** If Layer A (top) and Layer B (bottom) both define the color of a Prim, Layer A overrides Layer B.
* **Typical Structure:** 
 * `shot_layout.usd` (Bottom)
 * `shot_animation.usd` (Middle)
 * `shot_lighting.usd` (Top)
* **Result:** The final `shot_composed.usd` is just a list pointing to the sublayers.

**Speaker Notes:**
The most basic composition arc is SubLayering. Think of it exactly like Photoshop layers. You have a base layer with your geometry. The animator adds an animation layer on top. The lighter adds a lighting layer on top of that. If the lighter wants to move a light, they move it in their layer. It overrides the position defined in the base layer, but the base layer remains perfectly intact. If you mute the lighting layer, the light snaps back to its original position. 

---

## Slide 6: Variant Sets
### Packaging Options in a Single Asset
* **The Problem:** Games need asset variations (e.g., intact crate, damaged crate, destroyed crate).
* **The Solution:** A `VariantSet` allows you to package mutually exclusive variations inside a single USD Prim.
* **Selection:** The downstream software (like Unity) can simply flip a dropdown menu to switch between variants.
* **Efficiency:** Only the active variant is loaded into memory.

**Speaker Notes:**
If you have three versions of a prop—clean, dirty, and broken—you usually have three separate prefabs or FBX files. USD solves this with Variant Sets. You can package the clean, dirty, and broken meshes into a single USD asset. When you drop that asset into Unity, it appears as one object, but it has a dropdown menu in the inspector. You select "Broken," and the geometry swaps instantly. This keeps your asset libraries incredibly clean.

---

## Slide 7: Procedural Export Readiness
### Preparing Geometry Nodes for USD
* **The Reality Check:** Unity cannot evaluate Blender's Geometry Node math. 
* **The Translation:** When exporting to USD, procedural generated geometry must be realized (turned into real vertices and faces) at the moment of export.
* **Attribute Preservation:** The `Realize Instances` node is required to pass custom attributes (like vertex colors or density maps) across the USD bridge.
* **The Workflow:** Use Blender to generate the variations, export them as USD Variants, and compose them in the engine.

**Speaker Notes:**
Here is where the theory hits the pipeline reality. You built an amazing procedural generator in Geometry Nodes. Unity cannot read those nodes. The USD exporter will bake the output of your node graph into static meshes. If you instanced 10,000 leaves, you must use a `Realize Instances` node before export, or USD will just export 10,000 empty points. We use Blender as the procedural factory to manufacture the variations, and USD as the shipping container to deliver them to Unity.

---

## Slide 8: Authoring Variants from Geometry Nodes
### The Factory Workflow
1. **Set the Seed:** Set your Geometry Node seed to 1.
2. **Export `crate_varA.usd`.**
3. **Change the Seed:** Set your Geometry Node seed to 2.
4. **Export `crate_varB.usd`.**
5. **Compose:** Create a master `crate_asset.usd` that references both A and B within a VariantSet.
* **Result:** You have bridged procedural generation with real-time asset selection.

**Speaker Notes:**
To combine what we learned last week with what we are learning today: you use your procedural generator to rapidly author variants. You change the parameters on your modifier stack—make a tall building, export it. Make a short building, export it. Then, using Python or USD tools, you wrap those exports into a single USD VariantSet. You've just created a smart asset for your level designers in Unity.

---

## Slide 9: Python and USD
### The API Under the Hood
* **USD is an API:** While USD files can be human-readable text (`.usda`), USD is fundamentally a C++ and Python library.
* **Scripting Composition:** You do not have to compose USD files by hand. You can write Python scripts to automatically build VariantSets and SubLayers.
* **The NVIDIA Omniverse Ecosystem:** Heavily relies on Python scripts executing USD commands to bridge DCCs (Blender/Maya) with rendering engines.

**Speaker Notes:**
While you can open a `.usda` file in a text editor and read it, nobody does that for complex scenes. USD is actually a software library. It has a massive Python API. In a real studio pipeline, a Technical Artist writes a Python script that automatically takes your Blender exports, builds the SubLayers, constructs the VariantSets, and publishes the final asset to the server. You are manipulating the scene graph via code.

---

## Slide 10: Preparation for Wednesday
### Term Project Workshop & Export Lab
* **Wednesday's Lecture:** We will pivot to the Term Project Proposal Workshop. 
* **Lab 3b Checkpoint:** You will physically execute the export pipeline. You will take the procedural asset you built in Lab 3a, export it to OpenUSD, and successfully import it into Unity 6.
* **Troubleshooting:** We will cover missing materials, up-axis flipping (Z-up vs Y-up), and scale issues.
* **Readings:** Review the Unity 6 Manual on "USD Import" and the Blender 4.2 Manual on "USD Export".

**Speaker Notes:**
On Wednesday, we are shifting gears. Half the class will be a workshop on your Term Project proposals—how you plan to integrate these tools into a final capstone. The other half is Lab 3b, which is a critical pipeline checkpoint. You must prove you can get your procedural asset out of Blender and into Unity via USD without data loss. Read the export documentation carefully; scale and axis-orientation are the most common failure points. See you Wednesday.
