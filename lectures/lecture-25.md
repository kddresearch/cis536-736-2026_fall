# CIS 536 / 736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Generative Assets & Neural Rendering | **Module 8:** Generative Geometry & 3D Gaussian Splatting | **Lecture 25:** Generative Geometry Survey: Shap-E, Point-E (open-weight, local) | Mon, October 26, 2026 |

---

## Slide 1: Generative Geometry Survey
### CIS 536 / 736 - Lecture 25
* **Module 8 Initiation:** Transitioning from deterministic procedural generation to AI-driven asset generation.
* **The 3D Generation Problem:** Why standard 2D diffusion models do not translate natively to 3D topologies.
* **Point-E:** Generating colored 3D point clouds via diffusion.
* **Shap-E:** Generating implicit 3D functions and extracting textured meshes.
* **Production Pipelining:** Moving generated assets to OpenUSD for Unity/Blender integration.

**Speaker Notes:**
Welcome to Module 8. Today we are crossing the bridge from the deterministic procedural generation we explored with Geometry Nodes into stochastic, AI-driven asset generation. We are going to examine the fundamental problem of generating 3D structures from text, and then break down two open-weight models from OpenAI: Point-E and Shap-E. Importantly, we are focusing on architectures you can run locally on an 8GB VRAM GPU, ensuring you are not tethered to expensive cloud API quotas.

---

## Slide 2: Explicit vs. Implicit 3D Representations
### The 3D Generation Problem
* **Explicit Representations:** Directly defining vertices, edges, and faces (e.g., standard polygonal meshes). Difficult for neural networks to generate due to strict topological constraints (e.g., non-manifold geometry, overlapping faces).
* **Voxel Grids:** 3D pixels. Easier for ML models to output (like 3D images), but incredibly memory-intensive ($O(n^3)$ scaling) and computationally expensive at high resolutions.
* **Implicit Representations:** Defining a 3D surface as a continuous mathematical function (e.g., Signed Distance Fields). 
* **The NeRF Approach:** Neural Radiance Fields map 3D coordinates and viewing angles to color and density.

**Speaker Notes:**
Before we can generate a 3D model, we have to decide how the AI will represent it. 2D diffusion works because images are just flat grids of pixels. If we try to generate explicit meshes directly, the neural network usually produces broken geometry—faces intersecting, vertices collapsing. Voxels solve the topology issue but explode in memory cost. Therefore, modern 3D generative AI relies heavily on implicit functions and point clouds to bypass the rigid constraints of traditional polygons.

---

## Slide 3: Point Cloud Generation via Diffusion
### Point-E Architecture
* **Two-Stage Pipeline:** Point-E does not go directly from text to 3D. It goes from Text $\to$ Image, and then Image $\to$ 3D Point Cloud.
* **The Image Prior:** Leverages a pre-trained GLIDE model to generate a synthetic 2D view of the requested object.
* **The Point Cloud Diffusion Model:** A transformer-based diffusion model conditioned on the generated image, outputting a 3D cloud of colored points.
* **Execution Speed:** Highly efficient. Generates point clouds in seconds on a single local GPU.

**Speaker Notes:**
Point-E stands for Point Cloud Diffusion for 3D Model Synthesis. It circumvents the mesh topology problem entirely by just throwing colored points into 3D space. It's a two-stage process: first it hallucinates a 2D image of what you asked for, and then it feeds that image into a second model that extrudes and diffuses a point cloud. Because it avoids complex surface calculations, it is incredibly fast and runs easily on local hardware.

---

## Slide 4: Point Clouds in Production
### Advantages and Limitations
* **Advantage - No UV Mapping:** Color is baked directly into the point data, eliminating the need for texture atlases.
* **Advantage - Computational Cost:** Extremely low barrier to entry for generating basic shapes and bounding volumes.
* **Limitation - Surface Topology:** There is no actual "surface," making traditional physics collisions and lighting calculations problematic.
* **Limitation - Rigging & Animation:** Standard skeletal animation (skinning) does not apply natively to point clouds without conversion or specialized shaders.

**Speaker Notes:**
While Point-E is fast, a point cloud is not a production-ready game asset out of the box. Because there are no vertices connected by faces, Unity's physics engine doesn't know where the surface actually is. Furthermore, you cannot easily attach a control rig to a cloud of points. Point clouds are excellent for environmental background assets, LOD proxies, or abstract VFX, but they require meshing algorithms if you want to use them as standard interacting props.

---

## Slide 5: Generating Implicit 3D Functions
### Shap-E Architecture
* **Beyond Point Clouds:** Shap-E generates parameters for implicit functions rather than discrete points or pixels.
* **Encoder-Decoder Structure:** Maps 3D assets into the parameters of an implicit function, then trains a diffusion model on those parameters.
* **Dual Output Modalities:** The generated implicit function can be rendered directly as a Neural Radiance Field (NeRF) or converted into a textured 3D mesh (STF).
* **Computational Cost:** Slower to train and infer than Point-E, but yields continuous surfaces.

**Speaker Notes:**
Shap-E is the logical evolution of Point-E. Instead of outputting discrete points, Shap-E outputs the parameters of a mathematical function that describes a 3D object. Because it generates a continuous implicit function, we can query that function at any resolution. This allows us to extract actual surfaces, effectively bridging the gap between generative AI and traditional 3D graphics pipelines.

---

## Slide 6: Extracting Explicit Meshes from Shap-E
### Bridging Generative and Traditional Pipelines
* **Marching Cubes Algorithm:** The standard method for extracting a polygonal mesh from an implicit function or Signed Distance Field.
* **Textured Meshes:** Shap-E assigns color to the implicit surface, which can be baked out as vertex colors.
* **Resolution Trade-offs:** Querying the implicit function at higher resolutions yields better meshes but exponentially increases extraction time and VRAM usage.
* **Cleanup Required:** Extracted meshes often have high vertex counts and require retopology or decimation before real-time engine ingestion.

**Speaker Notes:**
Once Shap-E generates the implicit function, we have to get it into a format Unity or Blender can understand. We use an algorithm like Marching Cubes to sample the implicit function and wrap a polygonal mesh around it. The output will have vertex colors, meaning you get the texture baked right into the geometry. However, be aware that AI-generated meshes are usually extremely dense and "messy," requiring a decimation modifier in Blender to reduce the poly count before putting it in a game engine.

---

## Slide 7: Open-Weight & Local Execution
### The 8GB VRAM Constraint
* **Sovereignty in Infrastructure:** Avoiding reliance on paid cloud APIs (Midjourney, OpenAI) ensures reproducibility and zero-cost iteration.
* **Hardware Floor:** Both Point-E and Shap-E are small enough to infer locally on standard consumer GPUs (NVIDIA RTX 3060/4060 class with 8GB VRAM).
* **Local Python/CUDA Stack:** Requires a properly configured PyTorch environment with CUDA support to offload tensor operations to the GPU.
* **Data Privacy:** Local execution guarantees proprietary game IP or student concepts are not uploaded to corporate training datasets.

**Speaker Notes:**
In this lab, we enforce an open-weight, local-execution policy. We do not build dependencies on black-box SaaS APIs that can change their pricing, censor your prompts, or go offline. You will run these models locally. Shap-E and Point-E were specifically chosen because they fit within the 8GB VRAM floor of our lab workstations. This forces you to learn the actual MLOps of managing a local PyTorch and CUDA environment, which is a critical industry skill.

---

## Slide 8: Lab 5 Setup & Preparation
### Preparing the Local Environment
* **Environment Management:** Use Conda or Python `venv` to isolate dependencies. Do not install globally.
* **PyTorch Installation:** Ensure the correct version of PyTorch matching your local CUDA toolkit version (`nvcc --version`).
* **Repository Cloning:** You will clone the official OpenAI GitHub repositories for Shap-E and Point-E.
* **Weight Downloading:** The models will automatically download their `.pt` (PyTorch) weight files on the first run. Ensure adequate disk space (~3-5 GB).

**Speaker Notes:**
For this week's lab, your first hurdle is environment bring-up. Python dependency hell is real. Use a virtual environment. You will need to verify your CUDA drivers and install the GPU version of PyTorch. If you install the CPU version, generating a single mesh will take 20 minutes instead of 10 seconds. The model weights will download automatically during your first test script execution.

---

## Slide 9: Bridging AI and Production
### Exporting to OpenUSD
* **The SSOT Format:** Universal Scene Description (USD) remains our backbone for asset interchange.
* **Point Cloud to USD:** Saving `.ply` or `.pcd` point clouds into `.usd` formats while preserving vertex color arrays.
* **Mesh Export:** Converting Shap-E's extracted `.obj` or `.ply` meshes into standard `.usda` (ASCII) or `.usdc` (Binary) formats.
* **Pipeline Integration:** Importing the generated `.usd` asset into Unity 6 and applying HDRP vertex color shaders to visualize the result.

**Speaker Notes:**
An AI-generated asset is useless if it is trapped inside a Jupyter notebook. Your assignment will require you to generate a mesh with Shap-E, export it as a `.ply` or `.obj`, and then package it into our OpenUSD pipeline. Once it is a USD asset, you can reference it into your main scene assembly just like the procedural Geometry Nodes assets we built in Phase 2. You will need to ensure your Unity HDRP material is set up to read vertex colors, otherwise your model will just render flat white.

---

## Slide 10: Next Steps: 3D Gaussian Splatting
### Preparing for Lecture 26
* **From Generation to Reconstruction:** Shifting from text-to-3D (hallucination) to image-to-3D (real-world reconstruction).
* **The Splatting Paradigm:** Introduction to real-time radiance field rendering without ray-marching.
* **Required Reading:** Review the `gsplat` GitHub repository documentation and the COLMAP quickstart guide.
* **Pre-Lab Requirement:** Begin thinking about a physical object or small environment you can film with your smartphone for Friday's reconstruction lab.

**Speaker Notes:**
Today we covered how to hallucinate 3D geometry from text. On Wednesday, we pivot to reconstructing real-world geometry from video using 3D Gaussian Splatting. Gaussian Splatting is currently revolutionizing real-time rendering. Please review the gsplat documentation before Wednesday's lecture, and start thinking about a physical object—like a statue or a distinct prop on campus—that you can record a 360-degree video of for Friday's lab.
