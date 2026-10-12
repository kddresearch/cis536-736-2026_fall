# CIS 536/736: Computer Graphics (Fall 2026)

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase [X]:** [Phase Name] | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 21:** Lab 4a: Control Rig + Unity Animation Rigging Hands-On | Friday, October 16, 2026 |

---

## Slide 1: Agenda & Overview
### CIS 536/736 - Lecture 21 (Lab 4a)
* **Lab 4a Objectives & Deliverables:** Outlining the requirements for building and exporting a functional rig to Unity.
* **Blender to Unity Skeleton Validation:** Verifying that bone axes, scales, and hierarchies remain intact across the `.fbx` or `.usd` export bridge.
* **Applying Unity Constraints to Imported Rigs:** Hands-on setup of the Rig Builder component and associating procedural constraints with the imported skeleton.
* **Debugging IK Targets and Pole Vectors:** Troubleshooting common runtime issues, such as joint flipping, hyperextension, and incorrect pole vector alignment.
* **Interactive Rig Finalization:** Tuning weights and solver parameters to ensure smooth transitions and robust interactive behavior within the Unity 6 scene.

**Speaker Notes:**
Welcome to Lab 4a. Today is a hands-on session where we put the theory from Monday and Wednesday into practice. We are going to take the rigs you've been working on, push them across the pipeline into Unity, and wire them up with procedural constraints so they can interact dynamically with the scene.

---

## Slide 2: Lab 4a Objectives & Deliverables
### The Assignment Pipeline
* **Objective 1:** Clean up and prepare your DCC armature for engine export.
* **Objective 2:** Successfully establish the import pipeline (FBX/USD) into Unity 6.
* **Objective 3:** Implement at least one Two-Bone IK solver and one Multi-Aim constraint that function at runtime.
* **Deliverable:** A functional Unity scene demonstrating interactive constraint behavior.

**Speaker Notes:**
Your goal for this lab is end-to-end functionality. A beautiful rig in Blender is useless if the skeleton shatters upon import. We are focusing heavily on pipeline discipline today—ensuring that what you author in the DCC survives the trip into the engine and can be successfully driven by Unity's Animation Rigging package.

---

## Slide 3: Blender to Unity Skeleton Validation
### Crossing the Export Bridge
* **Transform Freezing:** Applying all scale and rotation data before export (`Ctrl+A` in Blender).
* **Axis Conversion:** Managing the difference between Blender's Z-up coordinate system and Unity's Y-up system.
* **Hierarchy Verification:** Ensuring the root node and bone chains import without inserted "dummy" transforms.

**Speaker Notes:**
The number one cause of broken runtime rigs is unapplied scale or rotation in the DCC. If your rig has a scale of 0.01 on import, the Unity solvers will struggle with the math. Before you export, freeze all transforms. Pay close attention to your export settings to ensure the axes are converted cleanly from Z-up to Y-up without introducing empty parent nodes.

---

## Slide 4: Applying Unity Constraints to Imported Rigs
### Initializing the Rig Builder
* **The Rig Builder Component:** Adding the manager script to your character's root GameObject.
* **Creating Rig Layers:** Organizing constraints into logical groups (e.g., `Arm_IK_Rig`, `Spine_Aim_Rig`).
* **Assigning Targets:** Creating empty GameObjects to serve as the runtime targets for hands, feet, and eyes.

**Speaker Notes:**
Once your model is in Unity and properly configured as a Humanoid or Generic rig, you will attach the Rig Builder component. Think of the Rig Builder as the conductor. We will create separate Rig layers for different body parts so we can easily toggle them on or off via script later. Your next step will be creating empty GameObjects to act as the IK targets that the solvers will reach for.

---

## Slide 5: Debugging IK Targets and Pole Vectors
### Taming the Solvers
* **Joint Flipping:** When a knee or elbow suddenly snaps backwards due to an ambiguous solve.
* **Pole Vector Alignment:** Placing the hint target correctly along the hinge axis to guide the bend direction.
* **Hyperextension:** Preventing limbs from stretching awkwardly when the target moves too far away.

**Speaker Notes:**
As soon as you turn on a Two-Bone IK solver, you will likely see limbs twisting into horrifying shapes. This is normal. It almost always means your Pole Vector is misaligned. The Pole Vector tells the elbow or knee which way to point. If it's placed behind the character instead of in front, the leg will snap backward to reach it. We'll spend time today walking through how to properly align these targets.

---

## Slide 6: Interactive Rig Finalization
### Polishing Runtime Behavior
* **Tuning Solver Weights:** Adjusting the 0-1 blend values to mix baked animation with procedural IK.
* **Parenting Targets:** Attaching your IK targets to scene objects (like a moving platform or a prop) to test dynamic attachment.
* **Play Mode Verification:** Moving targets around during runtime to ensure the rig holds together under stress.

**Speaker Notes:**
The final step of today's lab is stress-testing. A rig might look fine standing still, but what happens when you grab the hand target in Play Mode and drag it above the character's head? Does the shoulder deform correctly? Does the weight blending feel smooth? We want to tune these parameters so the interactions feel grounded and robust.

---

## Slide 7: Summary & Next Steps
### Wrapping Up Module 6
* **Lab Completion:** Ensure your scene is saved and your Unity project is committed to your repository.
* **Module 6 Concluded:** You now have a complete workflow from procedural geometry through to interactive runtime rigging.
* **Looking Ahead:** Next week, we will shift focus into integration, tying these character systems into our larger scene compositions.

**Speaker Notes:**
By the end of this lab, you should feel confident in your ability to bridge characters from a DCC into Unity and make them dynamically react. Make sure your repository is fully synced. Next week, we start pulling all these pillars together into more complex scenes. Great work today.
