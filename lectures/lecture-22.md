# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Character Animation & Motion | **Module 7:** Animation Graphs & Motion Matching | **Lecture 22:** Animation Graphs: State Machines, Blend Trees (Unity 6) | Monday, October 19, 2026 |

---

## Slide 1: Animation Graphs: State Machines & Blend Trees
### CIS 536/736 - Lecture 22
* **Animator Controllers:** Node-based architecture and execution flow.
* **State Machines:** Discrete animation states and conditional logic.
* **Blend Trees:** Continuous blending via dynamic parameters.
* **Transition Parameters:** Interfacing user input with the animation graph.
* **Animation Layers & Avatar Masks:** Targeted skeletal hierarchies and overriding.

**Speaker Notes:**
Welcome back. Today we transition from rigging and skeletons—which give our models the mechanical capacity to move—into the actual logic of movement. In Unity 6, this is handled by the Animator Controller. We are going to look at the structural architecture of State Machines, how we evaluate conditions to switch states, and how we use Blend Trees to seamlessly interpolate between clips. By the end of this lecture, you'll understand how to bridge your C# input scripts to the visual output of the character.

---

## Slide 2: Animator Controllers: The Brain of the Character
### Node-Based Execution Flow
* **The Animator Component:** The behavior script attached to the GameObject that binds the skeleton (Avatar) to the logic.
* **The Animator Controller:** The state machine asset that dictates which animation clip plays and when.
* **Data Binding:** Separating the raw animation data (`.anim` or `.fbx` takes) from the control logic.

**Speaker Notes:**
The most important concept here is the separation of concerns. The animation clip itself is just raw data—a timeline of translation and rotation values for specific bones. The Animator Controller is a directed graph that evaluates at runtime. It doesn't store the animation; it routes the execution token. This allows you to reuse the exact same Animator Controller across different characters, provided their skeletons map to the same Avatar definition.

---

## Slide 3: State Machines: Discrete Animation States
### Defining Character Actions
* **Nodes as States:** Each node in the graph represents a distinct, looping or non-looping animation clip (e.g., "Idle", "Jump", "Reload").
* **The Default State:** The entry point of the graph, visually highlighted in orange; where the token originates on initialization.
* **The 'Any State' Node:** A global interrupt node used to transition to a specific state regardless of where the execution token currently is.

**Speaker Notes:**
A standard Finite State Machine (FSM). The character is in exactly one primary state at a time per layer. You use discrete states for actions that have a definitive start and end—like swinging a sword, taking damage, or jumping. The 'Any State' node is a crucial architectural shortcut for global interrupts. Instead of drawing a transition from every single node to "Die," you just draw one transition from "Any State" to "Die."

---

## Slide 4: Conditional Logic and Transitions
### Moving Between States
* **Transition Arrows:** Directed edges in the graph that define allowable paths between states.
* **Exit Time vs. Condition-Driven:** Transitions can fire automatically when a clip finishes (Exit Time) or instantaneously when a variable changes (Condition).
* **Transition Duration (Blending):** The crossfade window where Unity interpolates between the outgoing pose and the incoming pose.

**Speaker Notes:**
How do we navigate the graph? Transitions. If an animation must finish completely—like a heavy attack animation—you rely on "Has Exit Time." The engine waits until the clip is at 90% or 100% before crossing over. But for responsive player movement, like breaking out of an idle into a run, you disable Exit Time and rely entirely on Conditions. If you don't turn off Exit Time for movement, your character will feel sluggish and unresponsive, waiting for the idle loop to finish before moving.

---

## Slide 5: Transition Parameters
### Connecting Input to Animation
* **Floats & Integers:** Continuous or discrete numeric values (e.g., `Speed`, `Health`, `Direction`).
* **Booleans:** True/False toggles for sustained states (e.g., `isGrounded`, `isCrouching`).
* **Triggers:** Ephemeral boolean flags that reset automatically after being consumed by a transition (e.g., `Jump`, `Attack`).

**Speaker Notes:**
These parameters are your API. As a programmer, you shouldn't be hardcoding animation logic in your character controller script. Your C# script simply reads the physics engine and the player's joystick, calculates the magnitude of the velocity, and sets a Float parameter called "Speed" in the Animator. The State Machine listens to that parameter and handles the actual visual state change. Triggers are particularly useful for fire-and-forget actions because they prevent the character from getting stuck in an attack loop.

---

## Slide 6: Blend Trees: Continuous Animation Blending
### Mixing Clips Dynamically
* **The Blending Problem:** Discrete state transitions cannot handle continuous, analog input ranges (e.g., walking $\rightarrow$ jogging $\rightarrow$ sprinting).
* **1D Blending:** Interpolating linearly between clips based on a single parameter (e.g., `Speed` from 0.0 to 1.0).
* **2D Blending:** Interpolating across two axes (e.g., Cartesian `Velocity X` and `Velocity Z` for strafing and walking backward).

**Speaker Notes:**
State machines are terrible for analog movement. If you build a state machine with "Walk," "Jog," and "Sprint" nodes, you will get a chaotic spiderweb of transitions, and the character will pop between speeds. A Blend Tree replaces all of those nodes with a single state. Inside that state, Unity continuously computes a weighted average of the skeletal poses based on your parameters. 

---

## Slide 7: Blend Tree Architecture in Unity 6
### Under the Hood
* **Thresholds:** The specific parameter values where a clip's weight is strictly 1.0.
* **Compute Weights:** Unity uses barycentric coordinates (in 2D) to calculate the influence of surrounding clips.
* **Nested Blend Trees:** Embedding a 1D Blend Tree inside a 2D Blend Tree to manage complex states cleanly without graph bloat.

**Speaker Notes:**
When you set up a 2D Freeform Directional blend tree for a strafing system, you place your clips in a 2D coordinate space. Forward is (0, 1), Left is (-1, 0). Unity calculates the weight of each clip based on the parameter's distance to those thresholds. It's a continuous mathematical interpolation of the transforms. Nesting allows you to keep the top-level state machine clean.

---

## Slide 8: Animation Layers
### Managing Multiple State Machines
* **Base Layer vs. Additional Layers:** Running parallel state machines on the same character simultaneously.
* **Override Blending:** The upper layer completely replaces the animation data of the lower layer for its masked bones.
* **Additive Blending:** The upper layer adds its transform deltas to the lower layer (e.g., adding a breathing procedural offset to a rigid idle).

**Speaker Notes:**
What happens when you need to run and shoot at the same time? In the old days, animators had to create a "Run_Shoot" clip, a "Walk_Shoot" clip, an "Idle_Shoot" clip. This results in combinatorial explosion. Layers solve this. You create a Base Layer for locomotion (legs and root), and an Upper Body Layer for actions (aiming, reloading). 

---

## Slide 9: Avatar Masks
### Targeted Skeletal Animation
* **Defining the Mask:** A binary filter that flags which bones in the skeletal hierarchy are active or ignored.
* **Binding Mask to Layer:** Applying the mask to an Animation Layer restricts its influence exclusively to the active bones.
* **Kinematic Separation:** Allowing the Base Layer's run animation to control the legs while the Upper Body Layer completely hijacks the spine, arms, and head.

**Speaker Notes:**
Layers only work if you use Avatar Masks. If your "Reload" animation on the second layer includes keyframes for the legs (even if they are just standing still), an Override layer will overwrite the running legs from the Base layer, and your character will glide across the floor. You create an Avatar Mask, click off all the lower-body bones, and assign it to the Upper Body Layer. Now, the execution is strictly decoupled.

---

## Slide 10: Wrap Up & Transition to Motion Matching
### The Limits of Traditional State Machines
* **The Complexity Explosion:** The "Spiderweb" effect when adding turns, stops, starts, and pivots to a traditional FSM.
* **Rigidity:** State machines are deterministic but brittle; they struggle to adapt to dynamic, unpredicted player inputs smoothly.
* **Next Class: Data-Driven Animation:** Moving beyond explicit node graphs to Unity 6's Motion Matching—using pose-search algorithms across raw motion capture databases.

**Speaker Notes:**
We've covered the standard industry pipeline today. But there is a ceiling to this architecture. If you want a AAA-quality character that leans into turns, plants its feet, and transitions perfectly, a standard State Machine will quickly become a 500-node nightmare that is impossible to debug. Next class, we are going to look at the bleeding-edge solution: Motion Matching. We will discard the rigid graph entirely and learn how to use a cost function to search a database of raw motion capture frames in real-time. See you Wednesday.
