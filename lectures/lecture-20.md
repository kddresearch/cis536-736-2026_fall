# CIS 536/736: Computer Graphics (Fall 2026)

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase [X]:** [Phase Name] | **Module 6:** OpenUSD Interchange & Rigging Fundamentals | **Lecture 20:** Unity Animation Rigging: Constraints & IK Solvers | Wednesday, October 14, 2026 |

---

## Slide 1: Agenda & Overview
### CIS 536/736 - Lecture 20
* **Animation Rigging Package Overview:** Integrating Unity's procedural Animation Rigging package to augment static, baked animations at runtime.
* **Two-Bone IK Solvers:** Setting up procedural inverse kinematics for limbs (arms/legs) to dynamically reach targets in the Unity engine.
* **Multi-Aim & Look-At Solvers:** Configuring dynamic aim constraints for heads, eyes, and weapons to track targets independently of the animation clip.
* **Rig Transform Constraints:** Utilizing parent, copy, and damp constraints within Unity to dynamically attach objects (e.g., equipment, IK targets) to the skeletal hierarchy.
* **Procedural Rig Weighting & Blending:** Controlling the influence of procedural IK solvers versus baked animation data via script and animator parameters.

**Speaker Notes:**
Welcome everyone. On Monday we covered the foundational theory of rigging, FK/IK, and constraints within a DCC like Blender. Today, we are taking those concepts into the runtime environment. We will look at Unity's Animation Rigging package, which allows us to overlay procedural, dynamic bone adjustments on top of our static, baked animation clips.

---

## Slide 2: Animation Rigging Package Overview
### Runtime Procedural Animation
* **The Limitation of Baked Animation:** Pre-rendered clips (walk, run, idle) cannot adapt to dynamic environmental changes (e.g., uneven terrain, moving targets).
* **Unity's Animation Rigging Package:** A framework for setting up procedural motion and constraints that evaluate at runtime.
* **The Rig Builder Component:** The central hub added to your character's root that manages and evaluates the active constraint layers.

**Speaker Notes:**
Traditionally, game animation relied entirely on baked clips. If a character walked up a hill, their feet would clip through the geometry because the animation didn't know the hill was there. Unity's Animation Rigging package solves this by running a secondary pass over the skeleton at runtime, allowing constraints and solvers to dynamically alter the pose before it renders to the screen.

---

## Slide 3: Two-Bone IK Solvers
### Dynamic Limb Placement
* **Procedural Inverse Kinematics:** Calculating limb angles on the fly based on a moving target.
* **Target and Hint (Pole Vector):** Defining the end-effector's destination and the joint's bending direction.
* **Common Use Cases:** Foot placement on uneven terrain (Foot IK), hands gripping a moving steering wheel, or interacting with a dynamic prop.

**Speaker Notes:**
The most common solver you will use is the Two-Bone IK Solver. This operates exactly like the IK chains we discussed on Monday, but it calculates every frame in Unity. By moving a target transform to where the ground actually is, the IK solver will automatically bend the knee and ankle to plant the foot perfectly, regardless of what the baked animation clip is doing.

---

## Slide 4: Multi-Aim & Look-At Solvers
### Tracking Dynamic Targets
* **Multi-Aim Constraints:** Forcing specific bones (like the head or spine) to orient toward a target transform.
* **Weight Distribution:** Spreading the rotation across multiple spine bones to prevent unnatural neck snapping.
* **Look-At Solvers:** Specifically driving eye bones to focus on points of interest or the player camera.

**Speaker Notes:**
To make characters feel alive, they need to react to their environment. If a player walks past an NPC, that NPC should turn its head to look. We use Multi-Aim constraints for this. Instead of animating a specific "look left" clip, we just pass the player's coordinate to the solver, and it procedurally rotates the head and spine to track them.

---

## Slide 5: Rig Transform Constraints
### Dynamic Hierarchy Attachments
* **Parent & Copy Constraints:** Linking transforms dynamically without permanently altering the scene hierarchy.
* **Damp Constraints:** Introducing procedural lag or springiness to secondary motion (e.g., a heavy backpack or antenna).
* **Prop Interaction:** Attaching a weapon to a hand, or attaching an IK target to a moving vehicle.

**Speaker Notes:**
Sometimes you need bones or objects to follow each other without permanently nesting them in the hierarchy. Transform constraints allow a flashlight to perfectly follow the rotation of a hand, or an IK target for a hand to perfectly follow a moving door handle. Damp constraints are fantastic for adding overlapping action—making heavy objects feel like they have momentum.

---

## Slide 6: Procedural Rig Weighting & Blending
### Mixing Data with Solvers
* **Rig Weight:** A 0.0 to 1.0 multiplier that determines how strongly a procedural rig overrides the underlying animation clip.
* **Blending Transitions:** Smoothly fading IK targets in and out to prevent snapping.
* **Script Control:** Using C# to dynamically adjust rig weights based on game state (e.g., turning off Aim IK when the character starts sprinting).

**Speaker Notes:**
You rarely want your procedural solvers running at 100% all the time. If a character is aiming a gun, the Multi-Aim constraint should be at full weight. But if they put the gun away to sprint, you need to fade that constraint out smoothly back to the baked sprint animation. We manage this by modulating the Rig Weights via our C# game logic.

---

## Slide 7: Summary & Next Steps
### Preparing for Lab 4a
* **Runtime Flexibility:** Procedural rigging bridges the gap between static assets and dynamic environments.
* **Next Class (Friday):** Lab 4a - Control Rig + Unity Animation Rigging Hands-On.
* **Preparation:** Ensure your Blender rigs export correctly to FBX/USD with clean hierarchies, ready to accept Unity Rig Builders.

**Speaker Notes:**
Today was about understanding the tools available in the Unity Animation Rigging package. On Friday, we will get hands-on in Lab 4a. You will export your skeletons from Blender, set up the Rig Builder in Unity, and implement your own IK targets and constraints. Have your base rigs ready to go!
