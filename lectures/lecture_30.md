# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Cloud Rendering & Visualization | **[Module 09](../modules/module_09/):** Cloud Render Farms — AWS Deadline Cloud | **Lecture 30:** Lab 5b - Capped AWS Educate/Academy Credit Render Task | Fri 06 Nov 2026 |

---

## Slide 1: Welcome to Lab 5b
### CIS 536/736 - Lecture 30
* **The Goal:** Execute a complete cloud render workflow on AWS Deadline Cloud while strictly managing a capped financial budget.
* **The Environment:** You are operating within the AWS Educate/Academy environment with a hard cap on available credits.
* **The Stakes:** Quota exhaustion means failure. You must mathematically prove your render will not exceed your budget before dispatching.
* **Cross-Pillar Audit:** Ensuring your OpenUSD assets from previous labs are portable and cloud-ready.
* **Post-Mortem Analysis:** Comparing your pre-flight cost estimate against the actual AWS billing telemetry.

**Speaker Notes:**
Welcome to Lecture 30 and Lab 5b. Today, you are the technical directors of your own miniature studio. You are going to take the OpenUSD scenes you built in earlier phases and dispatch them to an AWS Deadline Cloud render farm. But there is a catch. You are operating on AWS Academy accounts. You have a hard, unyielding financial limit. In the real world, if you submit a sloppy, unoptimized render job to the cloud, you can bankrupt a small studio overnight. Today, you must calculate the cost of your job mathematically *before* you hit submit. If your job burns through your capped credits and crashes because it ran out of money, you fail the lab.

---

## Slide 2: Budget Estimation (Pre-Flight)
### The Physics of Cloud Economics
* **The Cost Equation:** Total Cost = (Frames) × (Time per Frame in hours) × (Compute Cost per hour) + (Data Transfer/Storage Costs).
* **The Baseline Test:** You MUST render a single, representative frame locally or via a highly restricted test queue to establish the "Time per Frame."
* **Instance Pricing:** Be aware of the exact per-hour cost of the EC2 instance type defined in your Deadline Cloud Fleet (e.g., `c5.xlarge` vs. `g4dn.xlarge`).
* **The 80% Rule:** Your estimated compute cost must not exceed 80% of your remaining AWS Academy credit balance, leaving a 20% buffer for startup overhead and storage.
* **The Deliverable:** Your lab report must include your explicit mathematical budget calculation.

**Speaker Notes:**
Before any studio sends a sequence to the farm, they run a budget calculation. You cannot guess. The equation is straightforward: how many frames, multiplied by how long a frame takes, multiplied by the cost of the server per hour. How do you know how long a frame takes? You render one frame as a baseline test. Look up the exact on-demand pricing for the EC2 instances powering your Deadline Cloud Fleet. If you are rendering 100 frames, and each takes 10 minutes on a server that costs ten cents an hour, calculate the total. You must leave a 20% buffer for overhead, because AWS also charges you for the time it takes the node to boot up and download your assets from S3.

---

## Slide 3: OpenUSD Cross-Pillar Audit
### Preparing Assets for the Cloud
* **The Absolute Path Trap:** `C:\Users\Name\Textures\diffuse.png` will fail instantly on a Linux worker node.
* **The Audit:** You must meticulously audit your `.usda` or `.usdc` files. Every texture, payload, and reference MUST use a relative path (e.g., `./textures/diffuse.png`).
* **Asset Packaging:** Ensure all required assets (geometry, textures, HDRIs) are contained within the root directory of your Job Bundle.
* **The `usdchecker` Utility:** Run the command-line `usdchecker` tool against your root scene file locally to identify missing references before uploading to S3.
* **The Consequence:** A missing texture in the cloud will either cause the job to fail (wasting startup time credits) or render with missing materials (wasting compute credits).

**Speaker Notes:**
The most common point of failure in cloud rendering is broken asset paths. The Linux servers in the AWS cloud do not have access to your laptop's hard drive. If you used absolute paths for your textures or geometry payloads, the render will fail. Before you submit your job bundle, you must perform a Cross-Pillar Audit. Open your `.usda` files and verify that every single file path is relative to the scene file itself. Package all your textures and models into the same directory structure as your OpenJD bundle. I highly recommend running the `usdchecker` command-line utility on your local machine; it acts as a linter and will immediately flag any broken links before you waste cloud credits trying to render them.

---

## Slide 4: Cost-Aware Job Execution
### Defining Constraints in OpenJD
* **Resolution and Sampling:** Lowering resolution (e.g., from 4K to 1080p) and samples per pixel (SPP) directly reduces render time and cost.
* **`parameter_values.yaml`:** Use this file to enforce strict bounds on your render job for the lab.
  ```yaml
  parameterValues:
    - name: Frames
      value: "1-24"  # Limit to 1 second of animation for the lab
    - name: Resolution
      value: "1920x1080"
  ```
* **Task Chunking:** For short frames, group them into larger tasks (e.g., 5 frames per task) to minimize the container startup overhead on the worker nodes.
* **The CLI Submission:** Dispatch the job using the `deadline bundle submit` command from your configured DevContainer terminal.

**Speaker Notes:**
Once your assets are clean, we optimize the job parameters. You are not rendering the next Pixar movie today; you are proving the pipeline works. We will constrain the job heavily in the `parameter_values.yaml` file. Limit your render to 24 frames—just one second of animation. Stick to 1080p resolution and a conservative sample count. If your frames are rendering very quickly (under a minute), you should configure your OpenJD template to "chunk" the tasks. Instead of spinning up a worker node for a 30-second frame, tell Deadline to process 5 frames per task. This reduces the overhead penalty of booting up the container and downloading the assets from S3 for every single frame.

---

## Slide 5: The Execution Phase
### Monitoring and Intervening
* **The Deadline Cloud Monitor:** Log into the web dashboard immediately after submitting your job via the CLI.
* **Watch the "Starting" Phase:** Verify that tasks are successfully moving from `Pending` to `Starting` to `Running`.
* **The CloudWatch Logs:** Click into the first running task. Open the CloudWatch logs and watch the stdout stream.
* **The Kill Switch:** If you see "File Not Found" errors or endless memory warnings in the first task's logs, **CANCEL THE JOB IMMEDIATELY**.
* **Failing Fast:** Do not let a doomed job run through 24 frames. Cancel, fix the paths locally, and resubmit.

**Speaker Notes:**
After you hit enter on the `deadline bundle submit` command, the clock starts ticking on your budget. Do not walk away. Open the Deadline Cloud Monitor web interface immediately. Watch your tasks. When the first task transitions to "Running," click on it and open the CloudWatch logs. Look at the raw terminal output from the AWS worker node. If it is complaining about missing files or failing to load plugins, hit the Cancel button on the entire job immediately. This is called "failing fast." It is much better to waste five cents on a broken startup sequence, fix your relative paths, and try again, rather than walking away and letting the farm burn through twenty dollars rendering black frames.

---

## Slide 6: Local vs. Cloud Render Parity
### Ensuring Visual Consistency
* **The Goal:** A frame rendered on an AWS Linux node should be mathematically identical to a frame rendered on your local Windows/Mac machine.
* **The Download:** Use the AWS CLI or the S3 Console to download the rendered frames from your output bucket.
* **Visual Inspection:** Check for missing textures (magenta/black surfaces), broken lighting (HDRI failures), or rendering artifacts.
* **MSE / PSNR:** For advanced validation, you can compute the Mean Squared Error (MSE) or Peak Signal-to-Noise Ratio (PSNR) between a local test render and a cloud render.
* **The Artifact:** You must include a side-by-side comparison of your local test frame and the cloud-rendered frame in your lab report.

**Speaker Notes:**
Once your job reaches 100% completion, your frames are sitting in an S3 bucket. Download them back to your local machine. The ultimate test of your pipeline is Render Parity. Does the frame rendered on a random server in Virginia look exactly like the frame rendered on your laptop? Check carefully. Did your HDRI environment map load correctly? Did your normal maps come through? Sometimes differing OS architectures or missing fonts can cause subtle differences. In professional studios, they use mathematical image comparison tools to check for Mean Squared Error to guarantee absolute parity. For your lab report, you must provide a side-by-side visual comparison proving that your cloud pipeline successfully reproduced your local artistic intent.

---

## Slide 7: Cost Discrepancy Analysis (Post-Mortem)
### Reconciling Estimate vs. Reality
* **The Final Bill:** Check the AWS Billing Dashboard (Cost Explorer) or the Deadline Cloud usage metrics.
* **The Discrepancy:** Your actual billed cost will almost certainly differ from your Slide 2 estimate. Why?
* **Hidden Overhead:**
  * **S3 Storage:** You pay for the GBs of data resting in the bucket.
  * **S3 PUT/GET Requests:** You pay per API call when the workers download assets.
  * **EC2 Boot Time:** You pay for the time the instance spends initializing before rendering starts.
* **The Reflection:** Analyze where your math was wrong. Did you underestimate the startup time? Did you use an overly expensive instance type?

**Speaker Notes:**
The final part of Lab 5b is the Post-Mortem. Now that the render is done, check your actual AWS usage. Compare the real compute-hours billed against the estimate you calculated before the lab. They will rarely match perfectly. Your job is to analyze the discrepancy. Usually, students forget to calculate overhead. You don't just pay for the time the CPU is actively rendering pixels; you pay for the time the server takes to boot up, pull the Docker image, and download your massive HDRI files from S3. You also pay a tiny fraction of a cent every time a worker node makes a GET request to S3. Analyzing this overhead is how you learn to architect truly cost-effective cloud pipelines.

---

## Slide 8: Automating with CI/CD (Forward Looking)
### Integrating Rendering into GitOps
* **The Next Step:** Right now, you are submitting jobs manually from your terminal.
* **Continuous Integration:** In a modern studio, rendering can be tied to Git commits.
* **GitHub Actions + Deadline:** You can configure a GitHub Action that triggers whenever you push a new `.usd` file to the `main` branch.
* **Automated Dispatch:** The CI/CD pipeline packages the bundle and submits it to Deadline Cloud automatically.
* **The Benefit:** Ensures that the Single Source of Truth (the Git repo) is always perfectly synchronized with the rendered output.

**Speaker Notes:**
Looking beyond this lab, I want you to think about how this fits into the broader GitOps philosophy of the course. Right now, you are running `deadline bundle submit` by hand. But because OpenJD uses plain YAML files, and the CLI is just a command-line tool, we can automate this. In modern animation studios, rendering is tied directly to version control. You can set up a GitHub Action so that whenever an artist pushes an updated OpenUSD file to the `main` branch, GitHub automatically packages the bundle and submits it to Deadline Cloud in the background. The renders just show up on a Slack channel an hour later. This ensures your final output is always perfectly synchronized with your code repository.

---

## Slide 9: Lab 5b Submission Requirements
### What You Need to Turn In
* **1. Budget Estimate:** The mathematical calculation of your expected costs (Slide 2).
* **2. The Job Bundle:** A zip or repo link containing your `template.yaml`, `parameter_values.yaml`, and your relative-pathed `.usd` assets.
* **3. Telemetry Proof:** A screenshot of the Deadline Cloud Monitor showing your job `Succeeded`.
* **4. Render Parity:** Side-by-side images of the local test render vs. the downloaded cloud render.
* **5. Post-Mortem:** A brief paragraph analyzing the difference between your estimated budget and actual telemetry overhead.

**Speaker Notes:**
Here is your explicit checklist for Lab 5b. I need to see the math for your budget estimate. I need access to your OpenJD Job Bundle so I can verify you used proper parameterization and relative paths. I need a screenshot of the Deadline monitor proving you successfully executed a job on the AWS infrastructure. I need the visual parity comparison to prove your textures didn't break. And finally, I need your brief post-mortem analysis on the cost discrepancy. Ensure all of this is committed to your repository in a structured Markdown report.

---

## Slide 10: Wrap-Up & Module 09 Conclusion
### Mastering Cloud Infrastructure
* **Summary:** You have moved from local rendering to distributed cloud pipelines, learning to manage decoupled Queues and Fleets, author OpenJD bundles, and strictly manage financial constraints.
* **Module 09 Complete:** This concludes our study of AWS Deadline Cloud.
* **Next Week:** We will shift focus back to the rendering algorithms themselves, utilizing this new cloud infrastructure to explore advanced path-tracing optimizations and volumetric rendering techniques.
* **Action Item:** Complete Lab 5b. Monitor your AWS Academy credit balance closely. If you exhaust your budget, you will not be able to complete future assignments.

**Speaker Notes:**
This concludes Module 9. You have successfully navigated the transition from a single-machine paradigm to a scalable, distributed cloud infrastructure. You now possess the architectural knowledge to provision compute fleets, author agnostic render jobs, and manage the harsh realities of cloud billing. This is a massive leap in technical capability. With this infrastructure now in place, we will spend the next module diving deeper into the actual rendering algorithms, using Deadline Cloud to throw massive compute power at advanced path-tracing and volumetric simulations. Complete Lab 5b this weekend, and please, watch your credit balances. Have a great weekend.
