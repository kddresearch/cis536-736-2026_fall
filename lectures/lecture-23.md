# CIS 536/736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Character Animation & Motion | **Module 7:** Animation Graphs & Motion Matching | **Lecture 23:** Motion Matching: Data-Driven Animation Selection | Wed, October 21, 2026 |

---

## Slide 1: Motion Matching Overview
### CIS 536/736 - Lecture 23
* **The State Machine Wall:** Why deterministic blend trees fail to scale.
* **The Data-Driven Paradigm:** Replacing manual transitions with database queries.
* **The Cost Function:** Trajectory, Pose, and Velocity matching mathematics.
* **Trajectory Generation:** Predicting future player movement.
* **Data Curation:** Tagging and structuring raw MoCap for the solver.

**Speaker Notes:**
Good morning, everyone. On Monday, we covered standard animation graphs and blend trees. You built state machines that dictate exactly when a character goes from idle, to walk, to run. But you also probably noticed that as soon as you want your character to turn 45 degrees, sprint, stop suddenly, or transition between uneven terrain, the number of explicit transitions you have to author explodes. It becomes a rat's nest. Today, we look at the solution to that scaling problem: Motion Matching. Instead of writing rules for every possible transition, we are going to throw an entire database of motion capture at an algorithm and ask it to find the best frame in real-time.

---

## Slide 2: The Limits of Traditional Animation Graphs
### The "Rat's Nest" Problem
* **Combinatorial Explosion:** Every new animation state (e.g., crouch-walk) requires manual $N \times M$ transition rules to existing states.
* **Authoring Friction:** Animators spend massive amounts of time tweaking blend durations rather than creating good motion.
* **Rigidity:** Characters feel "stiff" or "floaty" if the player's input doesn't perfectly match the authored blend tree logic.

**Speaker Notes:**
Let's define the problem. Traditional state machines are deterministic. If I am in state A, and parameter X crosses a threshold, go to state B over 0.2 seconds. This works great for simple games. But in modern AAA titles—like *The Last of Us* or *Assassin's Creed*—the player can change direction at any angle, at any speed, at any moment. If you try to build a blend tree for every possible turning angle, you get what animators call the "rat's nest"—a state machine so complex it is impossible to debug. When it fails, you get foot sliding, where the character glides across the floor instead of stepping.

---

## Slide 3: What is Motion Matching?
### The Data-Driven Paradigm
* **Continuous Search:** The system continuously searches a massive database of raw animation frames for the "best fit."
* **No Explicit Transitions:** The algorithm jumps to a new frame in the database whenever a better match is found, blending over a tiny window.
* **Data over Logic:** The quality of the animation relies entirely on the quality and volume of the underlying motion capture data, not the complexity of a state machine.

**Speaker Notes:**
Motion Matching flips the paradigm. We throw out the explicit transition logic. Instead, we take a massive database of raw motion capture—maybe 30 minutes of an actor just jogging, turning, stopping, and starting. Every single frame in that database is a potential next frame for our character. The engine continuously asks: "Based on what the player is doing right now, and what they want to do next, what is the mathematically optimal frame in my entire database to play next?" If it finds a frame that matches better than the one currently playing, it simply jumps to it and blends. It's a continuous, real-time database query.

---

## Slide 4: The Core Mathematics: The Cost Function
### Evaluating the "Best Fit"
* **Cost Minimization:** The solver seeks the frame in the database that minimizes a specific mathematical cost function: $C = W_p P + W_v V + W_t T$.
* **Pose Alignment ($P$):** Does the database frame match the character's current skeletal pose (e.g., left foot down)?
* **Bone Velocity ($V$):** Are the limbs moving at the same speed and direction as the current frame?
* **Trajectory ($T$):** Does the future path of the database frame match where the player wants to go?

**Speaker Notes:**
How does the algorithm know which frame to pick? It uses a cost function. We are trying to minimize the mathematical difference between our character's current state and the candidate frame in the database. The equation has three main parts. First, Pose Alignment: if my character's left foot is currently planted, I cannot jump to a frame where the right foot is planted, or the animation will pop. Second, Bone Velocity: if my arm is swinging forward, the next frame must also have forward momentum. Third, Trajectory: where is the player pushing the joystick? We weight these components ($W_p, W_v, W_t$) to tune the system. If we weight pose too highly, the character is unresponsive. If we weight trajectory too highly, the character ignores physics to instantly turn.

---

## Slide 5: Trajectory Generation
### Predicting the Future
* **Past, Present, Future:** Trajectory is a curve defining where the character was (past), where they are (present), and where they are going (future).
* **Input-Driven Prediction:** The future trajectory is generated by projecting the player's current joystick/keyboard input forward in time (e.g., 0.5s, 1.0s, 1.5s).
* **Matching the Database:** The solver compares the player's generated trajectory against the actual, recorded trajectories of the actor in the MoCap database.

**Speaker Notes:**
The magic of motion matching lies in the trajectory. We aren't just looking at the present frame; we are looking at the future. The system projects a spline or a series of points representing where the player wants to go—say, half a second from now, and one second from now. In our database, every single frame of MoCap has already been processed to know where the actor went in the seconds *after* that frame. The solver compares the player's desired future path against the actor's recorded future path. If the player pushes the stick hard left, the solver finds a frame where the actor was about to plant their foot and cut hard left.

---

## Slide 6: Building the Pose Database
### Processing Raw MoCap
* **Feature Extraction:** Unity pre-processes the raw `.fbx` animation clips to extract the specific features needed for the cost function (foot positions, velocities, trajectory).
* **Dimensionality Reduction:** Searching every bone for every frame at 60FPS is too expensive. We only track key features (usually the root, feet, and hips).
* **The Kd-Tree / BVH:** The resulting feature data is baked into an optimized spatial data structure to allow lightning-fast, real-time querying.

**Speaker Notes:**
We can't just search raw animation files at runtime; it would kill our frame rate. We have to pre-process the data. Unity takes all your MoCap clips and extracts only the essential features—where are the feet, what is the velocity of the hips, and what is the trajectory. We throw away the finger and neck data for the search phase to save compute. This extracted data is then baked into a highly optimized search structure, like a Kd-Tree, so the engine can query millions of frames in less than a millisecond.

---

## Slide 7: Data Tagging & Curation
### Guiding the Solver
* **Garbage In, Garbage Out:** Motion matching is entirely reliant on having the *right* data. If you don't have a MoCap clip of a 180-degree turn, the solver cannot fake it.
* **Tagging:** Manually or automatically marking sections of the database (e.g., "Idle", "Sprint", "Combat").
* **Restricting the Search Space:** Using tags to prevent invalid jumps. You don't want the solver jumping from a relaxed walk into a combat crouch just because the foot positions happened to match.

**Speaker Notes:**
Motion matching is not magic AI; it cannot synthesize motion that doesn't exist. If your player tries to do a 180-degree turn, and your database only has 90-degree turns, the system will look broken. Furthermore, you have to tag your data. You might have a great animation of a character relaxing, and another of them in combat. If their foot positions happen to align perfectly, the solver might try to jump between them. We use tags to tell the solver, "Only search the 'Combat' tags right now," restricting the search space to maintain contextual logic.

---

## Slide 8: Unity 6 Integration
### The Native Motion Matching Package
* **Motion Matching Component:** The core Unity component that replaces or augments the traditional Animator.
* **Motion Sequence / Database:** The asset that holds the baked feature data from your animation clips.
* **Trajectory Generator:** The component that translates your C# input scripts into the predicted path for the solver.
* **Debugging Tools:** Unity provides visualizers to see the current pose, the predicted trajectory, and the chosen database frame in real-time.

**Speaker Notes:**
Unity 6 has integrated motion matching natively. You'll be using three main components. The Motion Sequence asset is where you drop your animations to be baked. The Trajectory Generator takes your player input and draws the future path. The Motion Matching component ties it all together, querying the database and driving the character. The most important part of this workflow is the visualizer—you can literally see a ghostly projection of the trajectory and the pose the solver *wants* to pick, which is crucial for debugging your weights.

---

## Slide 9: Hybrid Systems
### Blending Old and New
* **Motion Matching is for Locomotion:** It excels at fluid, continuous movement (walking, running, turning).
* **State Machines are for Discrete Actions:** State machines are still required for specific, timed actions (e.g., opening a door, climbing a ledge, or performing a specific attack).
* **The Hybrid Approach:** Using an Animator Controller to handle discrete states, while treating the entire Motion Matching system as a single "Locomotion" state within that graph.

**Speaker Notes:**
It is critical to understand that motion matching does not completely replace state machines. Motion matching is unparalleled for locomotion—moving around the world smoothly. But if your character needs to press a button, or open a door, or execute a specific sword swing, you don't want the solver guessing which frame to use. You want absolute deterministic control. Therefore, modern games use a hybrid approach. The motion matching system handles the walking and running, but when the player hits the "interact" button, we transition back to a traditional, rigid state machine to play that specific animation.

---

## Slide 10: Preparation for Lab 4b
### Next Steps (Friday)
* **Dataset Import:** We will provide a starter dataset of raw MoCap locomotion clips.
* **Baseline State Machine:** You will build a quick blend tree to verify your rig is working correctly.
* **Motion Matching Setup:** We will bake the provided MoCap into a database, configure the cost function weights, and replace the blend tree with the data-driven solver.
* **Review the Unity Documentation:** Please review the Unity 6 Motion Matching quickstart guide prior to Friday's lab.

**Speaker Notes:**
On Friday, we will put this into practice. I will provide a clean dataset of locomotion MoCap. You will first ensure your character rig works using the traditional blend trees we covered on Monday. Then, we will rip that out, bake the data, and set up the motion matching solver. You'll spend most of the lab tuning your cost function weights to see how changing the pose vs. trajectory bias affects the responsiveness of the character. Please glance at the Unity documentation before lab so you are familiar with the component names. See you Friday.
