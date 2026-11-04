# CIS 536/736: Computer Graphics and Animation

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **[Phase 4](../phases/phase_04/):** Cloud Rendering & Visualization | **[Module 09](../modules/module_09/):** Cloud Render Farms — AWS Deadline Cloud | **Lecture 29:** AWS Deadline Cloud Walkthrough | Wed 04 Nov 2026 |

---

## Slide 1: Introduction to AWS Deadline Cloud
### CIS 536/736 - Lecture 29
* **The Cloud Render Paradigm:** Moving beyond single-workstation rendering to scalable, elastic compute clusters.
* **AWS Deadline Cloud Overview:** A managed service for setting up and scaling render farms in AWS without managing the underlying EC2 instances manually.
* **The Control Plane:** We manage the Farm, Queues, and Fleets through the AWS Management Console or the CLI.
* **The Execution Plane:** The actual rendering happens in isolated worker nodes provisioned by the Fleet.
* **Today’s Objective:** A complete walkthrough of configuring an AWS Deadline Cloud environment, constructing a Job Bundle, and submitting a render.

**Speaker Notes:**
Welcome to Lecture 29. On Monday, we discussed the theory of distributed rendering and the critical cost/scale tradeoffs involved in moving away from monolithic local rendering. Today, we are getting hands-on with AWS Deadline Cloud. Deadline Cloud is a fully managed service that abstracts away the hardest parts of building a render farm—provisioning instances, managing network file systems, and scaling compute dynamically. We are going to walk through the entire lifecycle: from setting up your IAM identity to defining your queue, writing the YAML configuration for your job, and finally submitting a render task via the CLI.

---

## Slide 2: Environment & Identity Configuration
### Securing Access and Permissions
* **IAM (Identity and Access Management):** AWS security relies on strict role-based access control (RBAC).
* **AWS Educate/Academy Credentials:** You are operating under a constrained, temporary credential environment.
* **The Deadline Cloud Monitor:** A separate web interface (not the main AWS Console) used by artists to monitor job progress and logs.
* **Service Roles:** Deadline Cloud requires specific IAM roles to allow it to provision EC2 instances and access S3 buckets on your behalf.
* **CLI Authentication:** You must configure your local terminal with `aws configure sso` or static keys to interact with the Deadline Cloud API.

**Speaker Notes:**
Before we can render a single pixel, we have to establish security. AWS operates on the Principle of Least Privilege. By default, you have permission to do absolutely nothing. Because you are using AWS Academy accounts, your permissions are strictly scoped. You cannot just spin up a massive GPU cluster; you must operate within your budget. To use Deadline Cloud, we have to ensure the service has permission to access your S3 buckets (where your OpenUSD files live) and spin up EC2 fleets. Furthermore, while we will configure the infrastructure in the main AWS Console, the actual job monitoring happens in a separate dashboard called the Deadline Cloud Monitor. We will configure your CLI authentication first so you can submit jobs locally.

---

## Slide 3: Farms, Queues, and Fleets
### The Deadline Cloud Topology
* **The Farm:** The top-level logical container for your rendering infrastructure in a specific AWS Region.
* **The Queue:** The intake mechanism for render jobs. Jobs sit here waiting for available compute.
* **The Fleet:** The actual compute resources (worker nodes). Defines instance types (e.g., `c5.4xlarge`), OS (Linux/Windows), and scaling limits (min/max instances).
* **The Association:** Queues and Fleets are decoupled. You must explicitly associate a Queue with a Fleet to process jobs.
* **Queue Environments:** Define variables and software paths (e.g., where Blender or the USD renderer is installed on the worker node).

**Speaker Notes:**
Let's define the core topology of Deadline Cloud. Everything lives inside a "Farm." Inside the Farm, you create "Queues" and "Fleets." It is crucial to understand that these are decoupled. A Queue just holds jobs. A Fleet represents actual physical servers—the EC2 instances. If you submit a job to a Queue that has no Fleet associated with it, the job will sit there forever. You must create an association. This decoupled design allows you to route high-priority jobs in a "Rush Queue" to an expensive GPU Fleet, while sending standard jobs to a cheap CPU Fleet. Finally, we attach "Queue Environments" to tell the worker nodes where to find the rendering software.

---

## Slide 4: Infrastructure Initialization (Demo)
### Provisioning the Render Farm
* **Step 1: Create the Farm:** Name the farm and accept the default KMS encryption keys.
* **Step 2: Create the Fleet:** Select "Customer Managed" or "Service Managed." We will use Service Managed for automatic scaling.
* **Step 3: Define Compute:** Select CPU instances (e.g., `c5.xlarge`). Set Maximum capacity to limit costs.
* **Step 4: Create the Queue:** Name the queue and attach it to the Fleet created in Step 2.
* **Step 5: Storage:** Define the Amazon S3 bucket used for staging input assets and receiving output frames.

**Speaker Notes:**
I am going to switch over to the AWS Console now for a live demo. We will start by creating the Farm. Next, we create the Fleet. We are choosing "Service Managed" fleets because we want AWS to handle the EC2 provisioning and auto-scaling. If we select "Customer Managed," we would have to configure the worker nodes manually, which is overkill for this lab. We will select a modest CPU instance type and, crucially, set a strict maximum capacity to protect our AWS Academy budget. Once the Fleet is provisioned, we create the Queue and link the two together. Finally, we must define an S3 bucket—this is where your local workstation will upload the OpenUSD files, and where the worker nodes will write the final `.exr` or `.png` frames.

---

## Slide 5: Open Job Description (OpenJD)
### Agnostic Job Definition
* **What is OpenJD?** An open-source, YAML-based standard for defining compute jobs, agnostic of the specific render farm software.
* **The Philosophy:** Decoupling the job definition from the proprietary API of the render manager.
* **Job Bundles:** A directory containing all the files necessary to define an OpenJD job.
* **Key Components:**
  * `template.yaml`: The core logic, steps, and tasks of the render.
  * `parameter_values.yaml`: Variables injected into the template (e.g., frame range, output path).
* **Portability:** An OpenJD bundle can theoretically run on AWS Deadline, a local render farm, or another supported scheduler.

**Speaker Notes:**
Now that the infrastructure is up, how do we tell it what to do? Historically, every render farm (like standard Deadline, Tractor, or Qube) had its own proprietary scripting language. AWS is pushing a new standard called Open Job Description, or OpenJD. OpenJD uses plain YAML files to define what the render job actually is, completely independent of the farm running it. To submit a job to AWS Deadline Cloud, we create a "Job Bundle." This is just a folder on your laptop containing a `template.yaml` file (the instructions) and a `parameter_values.yaml` file (the variables). This declarative approach means your render definition is portable and can be version-controlled in Git, perfectly aligning with our reproducible infrastructure goals.

---

## Slide 6: Authoring the `template.yaml`
### Defining the Render Steps
* **The Template Structure:** Contains `name`, `parameterDefinitions`, and `steps`.
* **Parameter Definitions:** Declare variables the template needs (e.g., `SceneFile`, `Frames`).
* **Steps:** Logical groupings of work. A single job might have a "Simulation" step and a "Render" step.
* **Task Parameterization:** How a step is chunked. (e.g., splitting a 100-frame animation into 10 tasks of 10 frames each).
* **The Action:** The actual bash or shell command executed by the worker node (e.g., `usdrecord --frames {{Task.Param.Frame}} {{Param.SceneFile}}`).

**Speaker Notes:**
Let's look at the `template.yaml` file. This is the heart of the Job Bundle. We start by defining parameters—variables that we will pass in later, like the path to the OpenUSD scene file. Then we define the `steps`. For this lab, we just have one step: Rendering. Inside the step, we define "Task Parameterization." This is where the magic of distributed rendering happens. If you are rendering 100 frames, you don't want one machine to render all 100. You parameterize the step so that Deadline creates 100 separate tasks, one for each frame. Finally, we define the Action—the literal command-line instruction the worker node will run, injecting the parameters using double curly braces.

---

## Slide 7: Authoring `parameter_values.yaml`
### Injecting Variables
* **Separation of Logic and Data:** The template defines *how* to render; the parameter values define *what* to render.
* **The Syntax:** A simple YAML key-value map mapping parameter names from the template to actual values.
* **Example:**
  ```yaml
  parameterValues:
    - name: SceneFile
      value: "s3://my-bucket/assets/scene.usda"
    - name: Frames
      value: "1-50"
    - name: Resolution
      value: "1920x1080"
  ```
* **Dynamic Overrides:** You can override these values via the CLI at submission time without editing the file.

**Speaker Notes:**
The second file in our bundle is `parameter_values.yaml`. This file strictly separates our data from our logic. The template file shouldn't care if you are rendering scene A or scene B; it just knows how to run `usdrecord`. The parameter values file provides the specific details for this specific submission. We map the parameter names defined in the template to concrete values—like the S3 path to our `.usda` file, or the exact frame range we want to render today. If you need to render a test at 720p, you change the resolution value here. You can also override these values directly in the terminal when you submit the job, which is very useful for quick iterative testing.

---

## Slide 8: CLI Submission Workflow
### Dispatching the Job Bundle
* **The Prerequisite:** You must have the AWS Deadline Cloud CLI installed (`pip install deadline`).
* **The Submission Command:**
  `deadline bundle submit /path/to/bundle --farm-id <FARM_ID> --queue-id <QUEUE_ID>`
* **Asset Synchronization:** The CLI automatically parses your job bundle and uploads required local assets to the configured S3 bucket.
* **The Job ID:** A successful submission returns a unique Job ID (e.g., `job-1234567890abcdef`).
* **Monitoring:** You use this Job ID in the Deadline Cloud Monitor to track the progress of your render tasks.

**Speaker Notes:**
Once your bundle is authored, you submit it using the command line. You must have the `deadline` Python package installed. The command is `deadline bundle submit`, followed by the path to your folder, and the IDs for your Farm and Queue. What happens next is crucial: the CLI reads your bundle, identifies any local files you referenced, and automatically syncs them up to the S3 bucket attached to your queue. You don't have to manually upload your textures or USD files to S3 first; the CLI handles the staging. When it finishes, it prints a Job ID to your terminal. You will copy that ID and use it to track your render in the web monitor.

---

## Slide 9: Worker Telemetry and Logging
### Diagnosing Render Failures
* **The Deadline Cloud Monitor:** The primary visual dashboard for tracking task status (Pending, Starting, Running, Succeeded, Failed).
* **Task Logs:** Every worker node streams its standard output (stdout) and standard error (stderr) to Amazon CloudWatch.
* **Container Startup Overhead:** The "Starting" phase includes the time required for the worker node to download the Docker image and sync assets from S3.
* **Common Failures:**
  * Missing textures (broken relative paths).
  * Out of Memory (OOM) kills by the OS.
  * Incorrect software paths in the Queue Environment.
* **Iterative Debugging:** Always render a single test frame (e.g., `--parameter-values Frames=1`) before submitting a 500-frame sequence.

**Speaker Notes:**
Your first job will probably fail. When it does, you need to know how to read the telemetry. Open the Deadline Cloud Monitor and find your job. You will see a list of tasks. If a task fails, you can click on it and view the raw CloudWatch logs. This is exactly what the worker node saw in its terminal. The most common error is a "file not found" because you used an absolute path (like `C:/textures/`) instead of a relative path in your OpenUSD file, so the worker node couldn't find the asset on S3. Also, be aware of startup overhead. It takes a minute for the worker to download your assets. Always submit a single-frame test job to verify your paths and materials before you submit a massive, expensive animation sequence.

---

## Slide 10: Wrap-Up & Next Steps
### Preparing for Lab 5b
* **Summary:** We provisioned an AWS Deadline Cloud Farm, Queue, and Fleet, authored an OpenJD Job Bundle (`template.yaml` + `parameter_values.yaml`), and submitted a job via the CLI.
* **Next Lecture (Fri, 06 Nov):** Lab 5b. You will execute this workflow yourselves using your AWS Educate/Academy credentials.
* **The Constraint:** Lab 5b requires strict budget calculation. You must verify your OpenUSD paths and calculate your compute-hour estimate *before* hitting submit.
* **Action Item:** Verify that your AWS Academy SSO credentials are active in your local terminal (`aws sts get-caller-identity`) before Friday's lab.

**Speaker Notes:**
To wrap up: today we walked through the entire lifecycle of cloud rendering. We built the infrastructure in the AWS console, we defined our render logic using the agnostic OpenJD YAML standard, and we dispatched the job using the CLI, letting AWS handle the asset syncing to S3. On Friday, you will do this yourselves in Lab 5b. However, Friday's lab is a strictly constrained environment. You are operating on capped AWS Academy budgets. You will be required to mathematically estimate the cost of your render before you submit it, and you must ensure your OpenUSD assets use relative paths, or your job will fail and waste credits. Please ensure your AWS CLI is authenticated and working before class on Friday.
