# CIS 536/736: Computer Graphics (Fall 2026)

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase [X]:** [Phase Name] | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 19:** Rigging Fundamentals: Skeletons, Skinning, Control Rig | Monday, October 12, 2026 |

---

## Slide 1: Agenda & Overview
### CIS 536/736 - Lecture 19
* **Armature and Skeleton Hierarchy:** Defining bone chains, root nodes, and parent-child transformation inheritance in 3D space.
* **Forward vs. Inverse Kinematics (FK/IK):** Mathematical and practical distinctions between driving bones via local rotations (FK) versus end-effector targets (IK).
* **Vertex Weight Painting & Skinning:** Binding structural geometry to armature bones using normalized weight influences to dictate mesh deformation.
* **Control Rig Construction (Blender):** Building non-deforming control curves and shapes to act as the animator's interface for the skeletal rig.
* **Bone Constraints:** Implementing basic structural limits (e.g., pole vectors, limit rotation, copy transforms) to prevent mesh tearing and unnatural deformation.

**Speaker Notes:**
Welcome back. Today we transition into the core concepts of rigging. We'll be moving from static geometries to models that can articulate and deform dynamically. We will cover the foundational building blocks: skeletons, kinematics, skinning, and control interfaces.

---

## Slide 2: The Armature Foundation
### Defining the Skeleton
* **What is an Armature?:** The skeletal foundation that provides an articulating structure for a 3D model.
* **Bone Chains:** Connected sequences of joints representing anatomical or mechanical segments.
* **Root Nodes:** The top-level anchor of the skeleton that dictates global translation and rotation.

**Speaker Notes:**
Before we can move a character, we have to define its skeleton, or "armature." Think of the armature as the structural framework. Every chain of bones ultimately traces back to a single root node, which positions the entire character in global space.

---

## Slide 3: Hierarchies and Inheritance
### Parent-Child Transformations
* **Hierarchical Chains:** How motion propagates down the skeletal structure.
* **Transformation Inheritance:** Children inherit position, rotation, and scale from their parent bone.
* **Space Duality:** The difference between global (world) space and local (bone) space coordinates.

**Speaker Notes:**
The concept of hierarchy is essential. When you rotate a shoulder joint, the elbow, wrist, and fingers must follow. This is parent-child transformation inheritance. We evaluate these movements in "local space" relative to the parent, which is then calculated into world space.

---

## Slide 4: Forward Kinematics (FK)
### Local Rotation Control
* **Forward Kinematics (FK):** Driving bones via direct local rotations.
* **Top-Down Control:** Motion starts at the root/parent and cascades down to the end of the chain.
* **Best Use Cases:** Fluid, arc-based movements like a swinging arm or a walking tail.

**Speaker Notes:**
Forward Kinematics is the most intuitive way a hierarchy moves. You rotate the upper arm, then the forearm, then the hand. It calculates the final position from the root down to the tip. It's mathematically straightforward and excellent for sweeping, overlapping arcs.

---

## Slide 5: Inverse Kinematics (IK)
### End-Effector Targeting
* **Inverse Kinematics (IK):** Driving bone chains via end-effector targets instead of local rotations.
* **Bottom-Up Solving:** The system calculates the required joint angles to reach a specific point in space.
* **Best Use Cases:** Planting feet on the ground, holding a steering wheel, or interacting with props.

**Speaker Notes:**
Inverse Kinematics solves the opposite problem. Instead of rotating the shoulder and elbow to place the hand, you tell the hand where it needs to be, and the IK solver computes the necessary shoulder and elbow angles. This is crucial for grounding characters so their feet don't slide.

---

## Slide 6: The Skinning Process
### Binding Geometry to Bones
* **Vertex Weight Painting:** The process of defining how much a bone influences specific vertices on a mesh.
* **Binding Structural Geometry:** Attaching the 3D mesh (the skin) to the underlying armature.
* **Deformation:** The resulting change in the mesh as the bones move.

**Speaker Notes:**
A skeleton moving on its own is invisible in the final render. We must bind the geometry to these bones. This process, called skinning, tells the engine which vertices move when a specific bone rotates, creating the illusion of a contiguous, flexible body.

---

## Slide 7: Weight Normalization
### Smooth Deformation
* **Normalized Weight Influences:** Ensuring that the sum of all bone weights on a single vertex always equals exactly 1.0 (or 100%).
* **Influence Blending:** Using soft gradients across joint areas (like elbows or knees) to create smooth, organic bends.
* **Troubleshooting:** Identifying and fixing stray weights that cause vertices to stretch improperly.

**Speaker Notes:**
To prevent the mesh from exploding, vertex weights must be normalized. If a vertex at the elbow is influenced 50% by the bicep bone and 50% by the forearm bone, it will smoothly bridge the gap as the joint bends.

---

## Slide 8: Control Rig Construction
### The Animator's Interface
* **Control Rig Construction (Blender):** Creating the actual interactive layer for animators.
* **Non-Deforming Curves and Shapes:** Utilizing distinct visual markers (circles, boxes, arrows) that do not render or deform the mesh directly.
* **Abstraction Layer:** Hiding the complex skeletal mechanics behind a clean, user-friendly interface.

**Speaker Notes:**
Animators rarely grab the actual bones to animate. Instead, we build a control rig using non-rendering shapes. These controls act as handles that drive the bones beneath, providing a clean, intuitive interface that prevents accidental destruction of the rig's internal logic.

---

## Slide 9: Bone Constraints
### Limiting Unnatural Motion
* **Bone Constraints:** Rules applied to joints to restrict their behavior.
* **Limit Rotation & Copy Transforms:** Preventing joints (like knees) from bending backward, or locking props to hands.
* **Pole Vectors:** specialized targets that dictate the direction a joint (like an elbow or knee) points during an IK solve.
* **Preventing Mesh Tearing:** Ensuring deformation stays within anatomically or mechanically logical limits.

**Speaker Notes:**
Constraints are the guardrails of our rig. An IK chain for a leg might accidentally bend the knee backward. By applying rotation limits or setting up pole vector targets, we force the math to respect anatomical realities, preventing unnatural deformation and mesh tearing.

---

## Slide 10: Summary & Next Steps
### Preparing for Unity
* **Foundation Solidified:** We now understand skeletons, FK/IK differences, skinning, and control rigs.
* **Next Lecture Preview:** We will take these concepts into Unity to explore runtime Animation Rigging.
* **Homework/Lab:** Review your Blender armature setups and ensure your weight painting is normalized before our next session.

**Speaker Notes:**
That covers the fundamentals of rigging inside a DCC like Blender. Make sure you are comfortable with these core concepts—especially the difference between FK and IK, and how constraints work—because on Wednesday, we will carry this logic directly into Unity for runtime solving.
