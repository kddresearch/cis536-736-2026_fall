# CIS 536 / 736: Computer Graphics and Animation

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 4:** Cloud Rendering & Visualization | **Module 9:** Cloud Render Farms — AWS Deadline Cloud | **Lecture 28:** Cloud Rendering Concepts (Distributed Rendering, Cost/Scale Tradeoffs) | Mon, Nov 02, 2026 |

---

## Slide 1: Welcome to Phase 4 — The Limits of Local Compute
### CIS 536 / 736 - Lecture 28
* **The Compute Wall:** We have spent 8 weeks optimizing rendering loops (Rasterization, RT/GI, Gaussian Splatting) to fit within an 8GB VRAM local workstation floor.
* **The Capstone Requirement:** High-fidelity physical simulations, path-traced final frames, and multi-thousand-frame animation sequences mathematically exceed single-machine limits.
* **Distributed Rendering Architectures:** Shifting from monolithic local rendering to elastic, horizontally scaled cloud render farms.
* **Module 9 Core Tooling:** We will utilize **AWS Deadline Cloud** and the **Open Job Description (OpenJD)** standard for distributed execution.
* **The Cost Mandate:** Cloud compute is bounded by finite financial constraints. MP 9 enforces strict AWS Educate/Academy credit caps.

**Speaker Notes:**
Welcome to Phase 4. Over the past two months, our primary constraint has been making things run efficiently on a single local GPU—fitting within the 8GB VRAM floor for real-time shaders and local Gaussian splatting. But in professional graphics, whether you are rendering a Pixar feature film or a massive OpenUSD architectural visualization, the compute requirements scale exponentially beyond a single machine. Today, we break out of the local workstation and introduce distributed cloud render farms. Over the next three lectures, we will learn how to decouple your scene from your laptop, wrap it in a standardized job package, and dispatch it across dozens of elastic cloud workers simultaneously using AWS Deadline Cloud. Crucially, we will learn how to do this without bankrupting your project budget.

---

## Slide 2: The Distributed Rendering Pipeline
### From Scene File to Final Frame Output
* **The Monolith (Local):** Your workstation loads the `.blend` or `.usd` file, holds all textures in memory, and sequentially renders Frame 1, Frame 2, Frame 3.
* **The Distributed Farm (Cloud):** The job is split into discrete "chunks" (e.g., 5 frames per task) and distributed across a massive pool of worker machines (a Fleet).
* **Shared Storage:** Worker nodes do not have your local hard drive. The entire scene graph, textures, and HDRI maps must be packaged and uploaded to shared cloud storage (e.g., Amazon S3).
* **Headless Execution:** Render engines (Cycles, Arnold, RenderMan) execute via Command Line Interface (CLI) in headless mode on worker nodes without any graphical UI.

**Speaker Notes:**
When you click "Render Animation" on your laptop, Blender acts as a monolith. It loads the scene into RAM once, then your CPU and GPU grind through every frame in sequence. If a 120-frame animation takes 5 minutes per frame, you are waiting 10 hours. In a distributed render farm, we split those 120 frames into 24 tasks of 5 frames each, hand them to 24 separate worker machines in the cloud, and get the entire animation back in roughly 6 minutes. But this requires a complete architectural shift. Those 24 cloud workers do not have access to your `C:\Users` folder. You must upload every texture, cache, and USD layer to shared storage, and the renderer must run entirely via command-line arguments. 

---

## Slide 3: Decoupling Queues from Fleets
### The AWS Deadline Cloud Architecture
* **The Queue:** The logical staging area where submitted jobs wait to be processed. It defines the priority, output locations, and the required software environment (e.g., "Requires Blender 4.2").
* **The Fleet:** The physical (or virtual) pool of worker machines (EC2 instances) that actually execute the tasks. 
* **The Decoupling Advantage:** Multiple queues can route to a single massive fleet, or a single queue can route to multiple specialized fleets (e.g., a fast GPU fleet for rendering, a cheap CPU fleet for video encoding).
* **Elasticity:** Fleets dynamically scale up (provisioning new instances) when the queue fills, and scale down to zero when idle to stop billing.

**Speaker Notes:**
In modern render management systems like AWS Deadline Cloud, we strictly separate the concept of a Queue from a Fleet. The Queue is the waiting room; it holds your job and knows what software the job needs. The Fleet is the actual muscle—the EC2 servers. This decoupling is incredibly powerful. You can have a high-priority "Final Render" queue and a low-priority "Test Render" queue both feeding into the same pool of machines, ensuring the hardware is always busy. Because cloud compute bills by the second, the Fleet is elastic. It watches the Queue; if the Queue is empty, the Fleet shuts down all instances so you aren't paying for idle machines.

---

## Slide 4: Cost/Scale Tradeoffs (The Billing Physics)
### Why Cloud Rendering is Not "Free Speed"
* **Compute-Hour Billing:** You are billed for the exact time a worker node is active, multiplied by the hourly rate of the specific EC2 instance type (e.g., $1.21/hour for a `g5.2xlarge` GPU node).
* **The Myth of Linear Speedup:** Rendering 120 frames on 1 machine (10 hours) costs exactly the same in compute-hours as rendering 120 frames on 120 machines (5 minutes). You are paying to compress time.
* **Worker Startup Overhead:** Every time a cloud worker boots up to take a task, there is a fixed time penalty (container initialization, downloading the scene from S3 into local RAM, building the BVH).
* **Chunking Inefficiency:** If you allocate 1 frame per task (chunk size = 1), the worker overhead (loading the 2GB scene) might take longer than rendering the actual frame, wasting massive budget on I/O.

**Speaker Notes:**
Let's talk about the physics of billing. Cloud rendering gives you immense speed, but it is not magic; you are paying to compress time. In a perfect vacuum, rendering for 10 hours on one machine costs the same as renting 120 machines for 5 minutes. But we do not live in a perfect vacuum. Every time a new cloud worker accepts a task, it has to wake up, pull your massive 3GB OpenUSD scene down from Amazon S3, parse the geometry, and build the Bounding Volume Hierarchy (BVH) before it renders a single pixel. If you configure your job to assign only 1 frame per worker, you might spend 4 minutes downloading data to render a 30-second frame. That overhead will bankrupt your capped credit allocation immediately. 

---

## Slide 5: AWS Compute Instance Types
### On-Demand vs. Spot Pricing
* **On-Demand Instances:** You pay a fixed, predictable premium rate. When you request the machine, AWS guarantees it will not interrupt your job. Best for final, critical deadline deliveries.
* **Spot Instances:** You bid on spare, unused AWS compute capacity at a massive discount (often 60–80% cheaper than On-Demand).
* **The Spot Interruption Risk:** If AWS suddenly needs that capacity for a full-paying On-Demand customer, they will forcefully terminate (interrupt) your Spot instance with only a 2-minute warning.
* **Idempotent Rendering:** To survive Spot interruptions, render jobs must save intermediate progress (writing out completed PNGs). When the interrupted task is retried on a new machine, it skips the finished frames.

**Speaker Notes:**
When you define your Fleet, you have to choose how you want to pay for the EC2 instances. On-Demand pricing is safe and guaranteed, but it is expensive. Spot pricing lets you rent Amazon's unused servers for pennies on the dollar. The catch? The moment a bank or enterprise customer wants that server at full price, Amazon evicts you. Your render node simply dies. Therefore, using Spot fleets requires your render job to be idempotent. If a worker is rendering frames 1 through 10, and gets killed on frame 6, the system will eventually restart that task on a new machine. That new machine must look at the output folder, see that frames 1 through 5 already exist, and pick up at frame 6, rather than starting over and wasting your money.

---

## Slide 6: Scene Complexity Reduction
### Staying Under the MP 9 Credit Cap
* **The MP 9 Invariant:** In Lab 5b and MP 9, you have a strictly capped AWS Educate/Academy credit allocation (e.g., $10.00). There are no supplemental credits. Exhausting your budget means failing the render requirement.
* **Resolution Control:** Reducing render resolution from 4K to 1080p reduces the pixel count (and render time) by a factor of 4.
* **Path-Tracing Sample Counts (SPP):** Halving the Samples Per Pixel (e.g., from 512 to 256) halves the compute time.
* **Denoising (OptiX / OIDN):** Utilize AI denoisers to achieve clean images at drastically lower sample counts. 128 samples + OptiX is often indistinguishable from 1024 raw samples but renders 8x faster.
* **Level of Detail (LOD):** Use OpenUSD variant sets to swap high-poly procedural geometry for baked proxies when rendering background assets.

**Speaker Notes:**
Because we are operating on capped AWS Academy credits in this module, you cannot simply throw hardware at a heavy scene. If you exhaust your budget, you cannot submit the assignment. You must act as a Technical Director and optimize your scene before it hits the farm. Resolution and sample count are your biggest levers—they scale linearly. Do not brute-force 1024 samples; use 128 samples and turn on the Intel Open Image Denoiser or NVIDIA OptiX. Furthermore, use the OpenUSD Variant Sets we learned in Phase 2 to swap out your heavy 4-million-polygon geometry nodes for low-poly proxies if the object is far away from the camera.

---

## Slide 7: The Open Job Description (OpenJD) Standard
### Cloud-Agnostic Render Definitions
* **What is OpenJD?** An open-source, YAML-based specification for defining render jobs, tasks, and environments, adopted by AWS Deadline Cloud.
* **Agnostic Portability:** OpenJD allows you to define a render job once, and run it locally on your laptop, on an on-premise university cluster, or on AWS, without changing the script.
* **The Job Template (`template.yaml`):** Defines the name, the software environment required (e.g., Blender, Maya, Nuke), the input/output paths, and the exact CLI command to execute the render.
* **Parameter Values (`parameter_values.yaml`):** Stores the specific variables for the current run (e.g., exact frame range `1-120`, resolution `1920x1080`), keeping the main template reusable.

**Speaker Notes:**
So, how do we actually tell AWS what to do? We use the Open Job Description standard, or OpenJD. It's a YAML-based language that defines the exact recipe for your render. The beauty of OpenJD is that it is cloud-agnostic. You define your job in a `template.yaml` file, specifying that you need Blender 4.2, where your input scene is, and the exact command-line arguments to run Cycles. You keep all the variables—like the frame range or resolution—in a separate `parameter_values.yaml` file. This means you can test the job bundle locally on your own machine using the OpenJD CLI, and when it works perfectly, you ship that exact same bundle to AWS Deadline Cloud.

---

## Slide 8: Anatomy of an OpenJD Step
### Task Parameterization and Chunking
* **Steps vs. Tasks:** A job is made of *Steps* (e.g., "Render Beauty Pass", "Composite Output"). A Step is divided into parallel *Tasks*.
* **The Parameter Space:** We define a `Frame` parameter mapped to a range (e.g., `1-120`). OpenJD automatically generates 120 individual Tasks.
* **Chunking (`chunkSize`):** To avoid the I/O overhead from Slide 4, we group Tasks into chunks. A `chunkSize` of `5` means a worker machine pulls down the scene once, renders 5 sequential frames, and then returns to the queue.
* **The Execution Command:** `blender -b {{Param.SceneFile}} -o {{Param.OutputDir}}/frame_#### -f {{Task.Param.Frame}}`

**Speaker Notes:**
Inside the `template.yaml`, the real work happens inside a "Step". When you define a Step, you give it a Parameter Space—usually the frame range. If you specify frames 1 through 120, OpenJD automatically spins up 120 separate Tasks. But remember the I/O overhead penalty we discussed? To fix that, we use the `chunkSize` setting. By setting chunk size to 5, we tell OpenJD to bundle 5 frames into a single unit of work. A cloud worker will boot up, download your scene file once, execute the headless Blender command for frame 1, 2, 3, 4, and 5, and only then disconnect. It amortizes the heavy startup cost across multiple frames.

---

## Slide 9: Path Hygiene and Relative Asset Linking
### The "Broken Link" Cloud Render Trap
* **The Absolute Path Failure:** If your `.blend` or `.usda` file references a texture at `C:\Users\Student\Desktop\wood_diffuse.png`, the cloud worker (running Linux) will immediately fail because that path does not exist on its hard drive.
* **Relative Paths Only:** All texture maps, OpenUSD references, and payload layers must be defined relative to the root project directory (e.g., `./textures/wood_diffuse.png`).
* **S3 Job Attachments:** AWS Deadline Cloud automatically packages your scene file and all its relative dependencies into an S3 bucket and reconstructs that exact folder hierarchy on the worker node.
* **Pre-Flight Auditing:** Always audit your scene graph for external or absolute file paths before submitting a cloud job to avoid paying compute hours for crashed tasks.

**Speaker Notes:**
The number one reason student cloud renders fail—and burn credits for nothing—is path hygiene. If you drag and drop a texture from your Windows Desktop into Unity or Blender, the software might save the absolute path starting with `C:\Users`. When that scene goes to AWS, it lands on a Linux server. The server tries to look for `C:\Users`, fails instantly, and crashes the render task. Because AWS Deadline Cloud automatically zips up your project folder and sends it to Amazon S3, you must ensure every single texture, cache file, and OpenUSD layer in your project uses a relative path, like `./textures/albedo.png`. We will run a script in Lab 5b to explicitly audit your files for absolute paths before we upload.

---

## Slide 10: Wrap-Up & Wednesday's AWS Console Walkthrough
### Preparing for Lab 5b
* **Review Cost Estimation:** We have established how instance types, chunk sizes, resolution, and sampling dictate your compute-hour burn rate.
* **OpenJD Portability:** You understand how to decouple job parameters from execution steps using standard YAML files.
* **Wednesday's Demo (Lecture 29):** We will log into the AWS Console live, spin up a Queue, configure a Spot Fleet, and submit a job bundle from the CLI.
* **Friday's Lab 5b Action Item:** Ensure you have access to your AWS Educate/Academy credentials and have a clean, optimized scene (from your PBR or Geometry Nodes assignments) ready to bundle and upload. 

**Speaker Notes:**
To wrap up: moving to the cloud isn't just about faster computers; it's a completely different production architecture. You have to think about I/O overhead, Spot instance interruptions, strict budget caps, and headless execution via OpenJD. On Wednesday, I will walk you through the actual AWS Deadline Cloud console. I will provision a fleet, write an OpenJD template, and submit a job live. For Friday's Lab 5b, you will need to do this yourselves using your own AWS Academy credentials. Pick a scene you built in Phase 2 or 3, optimize it to run cheaply, and we will dispatch it to the farm. See you Wednesday.
