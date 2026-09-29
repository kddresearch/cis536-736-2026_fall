# CIS 536/736: Computer Graphics / Game Engines

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Procedural Generation and Rigging | **Module 5:** Geometry Nodes Fundamentals & Patterns | **Lecture 14:** Geometry Nodes Fundamentals — Attributes, Fields, Instancing | Monday, September 28, 2026 |

---

## Slide 1: Welcome to Phase 3
### CIS 536/736 - Lecture 14
* **Transition:** From Real-Time Shading (Unity) to Procedural Generation (Blender 4.2 LTS)
* The Modifier Stack Evolution (Non-Destructive Workflows)
* The Field System (Data Flow vs. Evaluation)
* Attribute Math (Position, Normal, Index)
* Instancing on Points
* Node Group Encapsulation

**Speaker Notes:**
Welcome to Phase 3. For the last two weeks, we've lived in Unity 6, writing pixel and vertex shaders. Today, we pivot back to Blender 4.2 LTS to focus on the pipeline *before* the rendering engine: Procedural Geometry. We are going to look at Geometry Nodes, which allow you to generate entire cities, forests, or complex abstract structures non-destructively. This is the foundation for the procedural assets you will eventually export via OpenUSD back into Unity.

---

## Slide 2: The Modifier Stack Evolution
### Destructive vs. Non-Destructive Modeling
* **Destructive Modeling:** Extruding, beveling, and moving vertices by hand. Permanent topology changes.
* **The Traditional Stack:** Linear sequence of operations (e.g., Subdivision Surface -> Displace -> Decimate).
* **Geometry Nodes:** A node-based modifier that evaluates a directed acyclic graph (DAG) to generate or alter geometry.
* **The Advantage:** Iteration. You can change the base mesh or the mathematical rules at any time without losing work.

**Speaker Notes:**
In traditional 3D modeling, if you extrude a face and then realize 20 steps later it was the wrong face, you are hitting undo a lot. That is destructive modeling. The modifier stack introduced non-destructive operations—like applying a subdivision surface that you could toggle on and off. Geometry Nodes take this to the absolute extreme. It is a visual programming language embedded in a modifier. You build a mathematical rule set, and the geometry is generated on the fly. 

---

## Slide 3: The Field System
### Data Flow vs. Function Evaluation
* **Data Flow (Geometry Links):** The solid green lines. This is the actual mesh data (vertices, edges, faces) moving from one node to the next.
* **Fields (Diamond Sockets):** The dashed or solid diamond lines. These represent continuous functions, not single values.
* **The Concept:** A Field is a set of instructions evaluated *per element* (per vertex, per face) when required by a geometry node.
* **Why it matters:** You aren't passing an array of 10,000 vertex positions; you are passing the *rule* for how to calculate them.

**Speaker Notes:**
This is the hardest conceptual hurdle in Geometry Nodes: understanding Fields. Look at your node graph. Solid round sockets pass specific data, like a single vector or the actual 3D mesh. Diamond sockets represent Fields. A Field is not a number; it is a function. When you plug a Noise Field into a Set Position node, Blender doesn't calculate the noise once. It evaluates that noise function independently for every single vertex in the geometry stream. 

---

## Slide 4: Built-In Attribute Math
### Capturing and Manipulating State
* **Attributes:** Data attached to the geometry (Position, Normal, Radius, Index).
* **The Position Node:** Reads the current 3D coordinate of every element being evaluated.
* **The Normal Node:** Reads the facing direction of every face or vertex.
* **The Math Node:** Standard arithmetic (Add, Multiply, Sine) applied across the entire field simultaneously.

**Speaker Notes:**
To manipulate geometry, you have to read its current state. Geometry Nodes give you access to built-in attributes. The most common are Position and Normal. If you drop a Position node, run it through a Vector Math node to add +1 to the Z-axis, and plug it into a Set Position node, you have just lifted your entire mesh up by 1 unit. You didn't write a loop; the field evaluation inherently acts as a parallel loop over every vertex.

---

## Slide 5: Instancing on Points
### Massive Scale with Minimal Memory
* **The Concept:** Replacing points in space with references to a master object.
* **The Node:** `Instance on Points`
* **Inputs:** 
  * `Points`: The target geometry (where to spawn).
  * `Instance`: The object to duplicate (what to spawn).
* **The Memory Advantage:** 10,000 instanced trees cost roughly the same GPU memory as 1 tree. 

**Speaker Notes:**
If you want to build a forest, you cannot model 10,000 unique trees. Your RAM will catch fire. You model one tree, scatter 10,000 points on a plane, and use the `Instance on Points` node. Instancing tells the renderer, "Draw this exact same mesh over here, just at a different location and scale." It is incredibly cheap computationally. This is how AAA games render massive crowds or dense foliage.

---

## Slide 6: Distributing Points on Faces
### Generating the Scatter Map
* **The Node:** `Distribute Points on Faces`
* **Function:** Converts a mesh surface into a point cloud.
* **Modes:**
  * `Random`: Fast, entirely stochastic.
  * `Poisson Disk`: Enforces a minimum distance between points (prevents overlapping).
* **Density:** Controlled by a float value or driven by a Field (like a texture mask).

**Speaker Notes:**
Before we can instance our trees, we need the points. The `Distribute Points on Faces` node takes any mesh and generates a point cloud on its surface. You have two main algorithms. `Random` is cheap but clusters heavily. `Poisson Disk` is slightly heavier but allows you to set a `Distance Min`, which is vital if you are instancing things like buildings or large trees that shouldn't clip into each other.

---

## Slide 7: Breaking Uniformity
### Randomizing Scale and Rotation
* **The Problem:** 10,000 identical trees facing the exact same way look synthetic.
* **The Node:** `Random Value` (set to Vector or Float).
* **Execution:** 
  * Plug a Random Float into the `Scale` input of `Instance on Points`.
  * Plug a Random Vector (mapped to Z-axis) into the `Rotation` input.
* **Result:** Every instance receives a uniquely seeded value upon evaluation.

**Speaker Notes:**
If you just plug your tree into the instance node, you get a forest of identical clones standing at attention. To make it organic, we introduce entropy. Drop a `Random Value` node. If you plug a random float between 0.5 and 1.5 into the Scale port, Blender evaluates that random function for every single point, giving you varied tree heights. Do the same for rotation around the Z-axis, and the uniformity vanishes.

---

## Slide 8: Node Group Encapsulation
### Building Reusable Tools
* **The Problem:** Complex graphs become visually unreadable "spaghetti."
* **The Solution:** Select a cluster of logical nodes and press `Ctrl+G` (Make Group).
* **The Interface:** Group Input and Group Output nodes allow you to define what data passes in and out of the black box.
* **Asset Library:** Node groups can be marked as Assets and reused across different .blend files.

**Speaker Notes:**
As your procedural generators grow, your graph will turn into an unreadable mess of wires. In software engineering, you write functions to encapsulate logic. In Geometry Nodes, you create Node Groups. Select the nodes that handle your random scattering, hit Ctrl+G, and collapse them into a single, clean node. You can define its inputs—like density and scale—and reuse that exact node group in any future project. 

---

## Slide 9: Exposing Parameters to the Modifier
### Art-Directable Tools
* **Group Input Node:** The gateway to the outside world.
* **The Workflow:** Drag an internal parameter (e.g., `Density`) into the blank socket of the `Group Input` node.
* **The Modifier Panel:** The parameter instantly appears in the Blender Properties panel.
* **The Goal:** An artist should be able to use your procedural tool without ever opening the Geometry Nodes editor.

**Speaker Notes:**
The ultimate goal of procedural generation is creating a tool for artists. If you build a procedural building generator, the artist shouldn't have to decipher your math to make the building taller. By dragging a parameter from a node into the `Group Input`, you expose it to the standard Modifier panel. The artist just drags a slider that says "Floors," and the math evaluates under the hood. 

---

## Slide 10: Preparation for Lab 3a
### Checkpoint & Deliverable Scope
* **Wednesday's Lecture:** Advanced scattering patterns, density masking, and curve-to-mesh operations.
* **Friday's Lab 3a:** You will build a parameterized procedural asset (e.g., a ruined pillar, a sci-fi crate, or scattered debris).
* **Requirement:** Must utilize Instancing, random fields, and at least 3 exposed parameters in the modifier stack.
* **Readings:** Review the Blender 4.2 Manual sections on Attributes and Fields before Wednesday.

**Speaker Notes:**
That covers the theoretical foundation. On Wednesday, we will look at more advanced patterns, including how to use noise to mask where instances spawn, and how to sweep profiles along curves to generate pipes or vines. On Friday, you will build your own procedural generator. Start thinking now about what you want to build. It needs to be something parameterized—something that generates a unique variation every time you change the seed value.
