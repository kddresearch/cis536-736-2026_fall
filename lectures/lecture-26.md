# CIS 536 / 736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Generative Assets & Neural Rendering | **Module 8:** Generative Geometry & 3D Gaussian Splatting | **Lecture 26:** 3D Gaussian Splatting Fundamentals | Wed, October 28, 2026 |

---

## Slide 1: 3D Gaussian Splatting Fundamentals
### CIS 536 / 736 - Lecture 26
* **Bridging the Gap:** Neural Radiance Fields (NeRFs) vs. Real-Time Rasterization.
* **The Primitive:** Mathematical structure of a 3D Gaussian.
* **Camera Pose Estimation:** Structure-from-Motion (SfM) via COLMAP.
* **The Differentiable Rasterizer:** Fast, tile-based rendering (`gsplat`).
* **Optimization & Pruning:** Adaptive density control during training.

**Speaker Notes:**
Welcome to Lecture 26. On Monday, we looked at generating 3D assets from text. Today, we are focusing on reconstructing real-world environments from 2D images. 3D Gaussian Splatting represents a massive paradigm shift in computer graphics. It takes the photorealism of Neural Radiance Fields (NeRFs) but replaces the computationally crushing volumetric ray-marching with highly optimized, explicit rasterization. Today, we will deconstruct the math behind the primitive, how we initialize the scene, and how the rasterizer trains it.

---

## Slide 2: Radiance Fields vs. Rasterization
### The Computational Bottleneck of NeRFs
* **Implicit Neural Representations:** Traditional NeRFs encode a scene's density and color inside a Multi-Layer Perceptron (MLP).
* **Volumetric Ray-Marching:** To render a single pixel, a ray is cast through the scene, requiring dozens or hundreds of neural network queries along the ray.
* **The Cost:** Millions of MLP queries per frame. Inherently hostile to real-time $\ge 30$fps rendering without massive cloud GPU compute.
* **The Explicit Solution:** We need a representation that stores spatial data explicitly so we can leverage traditional GPU rasterization pipelines.

**Speaker Notes:**
The problem with NeRFs is that they are implicit. To know what is at coordinate X, Y, Z, you have to ask a neural network. To render a 1080p image, you cast a ray for every pixel and sample it hundreds of times. That is millions of neural network inferences for a single frame. It is beautifully accurate but completely unscalable for real-time engines like Unity without heavy caching. Gaussian Splatting solves this by returning to explicit geometry: discrete objects in 3D space that we can sort and draw instantly.

---

## Slide 3: The Gaussian Splat Paradigm
### Explicit Volumetric Rendering
* **The Scene Representation:** Instead of a polygon mesh or an implicit MLP, the scene is represented by millions of overlapping, semi-transparent 3D Gaussian blobs.
* **Continuous but Explicit:** Gaussians have no sharp edges (continuous), avoiding the aliasing artifacts of voxels or point clouds, but they possess defined coordinates in space (explicit).
* **Bypassing the MLP:** During inference (rendering), there is no neural network running. The scene is just a data structure of mathematical blobs that the GPU projects onto the 2D screen.

**Speaker Notes:**
Think of Gaussian Splatting as a point cloud where every point is a fuzzy, 3D ellipse. Because these blobs overlap and blend together, they create a continuous volumetric appearance, capturing things like smoke, reflections, and hair perfectly. But because they are just explicit blobs with XYZ coordinates, we can feed them directly to a GPU rasterizer. The neural network is only used to *train* the positions and colors; rendering them is pure, blazing-fast linear algebra.

---

## Slide 4: The Gaussian Splat Primitive
### Mathematical Structure (Position & Covariance)
* **Position ($\mu$):** The 3D center coordinate of the Gaussian in world space.
* **3D Covariance ($\Sigma$):** Determines the shape and orientation of the ellipsoid. 
* **Scaling ($S$) & Rotation ($R$):** To ensure the covariance matrix remains valid (positive semi-definite) during optimization, $\Sigma$ is factored into independent Scale ($S$) and Rotation ($R$) matrices.
* **Opacity ($\alpha$):** A scalar value determining how transparent the blob is, enabling volumetric blending.

**Speaker Notes:**
Let's look at the data structure of a single splat. It is defined by a 3D mean—its position—and a 3D covariance matrix. The covariance matrix defines how the blob stretches and rotates. Because gradient descent can easily break a covariance matrix by pushing it into negative values, the architects cleverly separated it into a Scaling vector and a Rotation quaternion. Add an opacity multiplier, and you have a fully differentiable 3D blob. 

---

## Slide 5: View-Dependent Color
### Spherical Harmonics (SH)
* **The Specular Problem:** Real-world surfaces change color depending on the viewing angle (e.g., reflections on a window, specular highlights on metal).
* **Spherical Harmonics:** A set of basis functions defined on the surface of a sphere. 
* **SH Coefficients:** Instead of storing a single RGB color, each Gaussian stores coefficients for these functions.
* **Dynamic Lighting:** The rasterizer calculates the final pixel color by evaluating the SH functions based on the camera's current view vector.

**Speaker Notes:**
If we just assigned a static RGB color to each blob, the scene would look flat and matte. A shiny car doesn't look the same from the front as it does from the side. To capture reflections and view-dependent lighting, Gaussians use Spherical Harmonics. By storing a matrix of SH coefficients, the blob can compute exactly what color it should emit based on the angle your virtual camera is looking at it from.

---

## Slide 6: Camera Pose Estimation
### Structure-from-Motion (SfM) via COLMAP
* **The Cold Start Problem:** We cannot train the Gaussians if we do not know where the cameras were when the photos were taken.
* **Feature Extraction:** Algorithms (like SIFT) detect distinct keypoints across multiple 2D images of the same subject.
* **Triangulation:** COLMAP matches these keypoints across frames to calculate the extrinsic (position/rotation) and intrinsic (focal length) parameters of the cameras.
* **The Initialization Seed:** COLMAP outputs a Sparse Point Cloud, which serves as the starting XYZ positions ($\mu$) for our initial Gaussian blobs.

**Speaker Notes:**
Before we can optimize the Gaussians, we need a starting point. If you just take a video with your phone, the algorithm has no idea how the phone moved through space. We use an open-source tool called COLMAP to perform Structure-from-Motion. COLMAP analyzes your frames, tracks features moving across them, and reverse-engineers the exact 3D path your camera took. It also gives us a sparse, messy point cloud. We drop our initial Gaussian blobs exactly onto those points to start the training process.

---

## Slide 7: The Differentiable Rasterizer
### Tile-Based Sorting (`gsplat` architecture)
* **Projection:** 3D Gaussians are projected onto the 2D image plane, transforming the 3D covariance into a 2D covariance.
* **Screen Tiling:** The screen is divided into $16 \times 16$ pixel tiles.
* **Frustum Culling:** Gaussians outside the camera's view are immediately discarded.
* **Depth Sorting:** Gaussians are assigned to intersecting tiles and sorted front-to-back based on their depth from the camera.

**Speaker Notes:**
How do we render millions of blobs at 60 frames per second? Through a highly optimized, tile-based rasterizer. First, we project the 3D ellipsoids onto the 2D screen—they become 2D splats. Then, the GPU divides your screen into a grid of 16-by-16 pixel tiles. The rasterizer culls anything behind the camera, figures out which tile each splat covers, and sorts them by depth. This means the GPU threads only process the math for the specific blobs visible in their specific tile.

---

## Slide 8: Forward Pass & Alpha Blending
### Rendering the Image
* **Alpha Compositing:** For every pixel in a tile, the rasterizer accumulates color front-to-back.
* **Volumetric Accumulation:** $C = \sum_{i=1}^{N} c_i \alpha_i \prod_{j=1}^{i-1} (1 - \alpha_j)$
* **Early Out:** Once the accumulated opacity reaches $1.0$ (fully opaque), the GPU stops processing Gaussians further back in that pixel's depth queue, saving massive compute.
* **Differentiability:** Because the entire projection and blending math is fully differentiable, we can calculate the loss against the ground-truth photograph and backpropagate the errors.

**Speaker Notes:**
With the blobs sorted, the GPU blends them together to figure out the final pixel color. It uses standard alpha compositing, moving from the front blob to the back blob. The crucial optimization here is the "early out" rule: once the accumulated pixel color hits 100% opacity, the GPU stops doing math for anything behind it, because you wouldn't be able to see it anyway. Because all of this math is differentiable, we can compare the rendered output to the real photograph and flow the error gradients backward to update the blobs.

---

## Slide 9: Optimization and Pruning
### Adaptive Density Control
* **Gradient Descent:** Loss functions (L1 and D-SSIM) drive updates to position, scale, rotation, opacity, and SH color.
* **Cloning (Under-reconstruction):** If an area has high positional gradients but the Gaussians are small, the model clones them and moves them along the gradient to fill the gap.
* **Splitting (Over-reconstruction):** If a Gaussian is too large and covers high-variance detail, it is split into two smaller Gaussians to capture finer resolution.
* **Pruning:** Gaussians whose opacity ($\alpha$) drops below a threshold (effectively invisible) or that become excessively large are deleted to save memory.

**Speaker Notes:**
The true magic of 3DGS is Adaptive Density Control. As the model trains, it doesn't just change the colors of the blobs—it changes the number of blobs. If the neural network detects a highly detailed area but the blobs are too small, it clones them. If a blob stretches too big over a detailed texture, it splits it in half. And if a blob is pushed to 0% opacity by the gradients, the system deletes it. The geometry literally sculpts itself over 30,000 iterations to match the photographs.

---

## Slide 10: Lab 5a - Capture & Reconstruction
### Transitioning to Execution
* **Data Capture:** Friday’s lab requires a locally captured dataset. Ensure smooth camera movement, high overlap, and static lighting (no moving shadows).
* **Environment Setup:** We will utilize the `gsplat.studio` pipeline on our local lab workstations.
* **VRAM Constraints:** Be prepared to monitor `nvidia-smi`. Optimization scales with image resolution; we will cap training at $1920 \times 1080$ to respect the 8GB VRAM floor.
* **Next Steps:** Ensure your Python/Conda environments are configured and your raw video/image sets are ingested before Friday's lab period begins.

**Speaker Notes:**
This brings us to Friday's Lab. Your task is to capture a physical subject and run this entire pipeline on your local workstation. The quality of your 3D Gaussian Splat is 100% dependent on the quality of your input photos. Move slowly, maintain high overlap between shots, and do not film things that move. We will be using the `gsplat` open-source framework. Because we are operating on 8GB VRAM GPUs, we will be heavily capping our rendering resolutions to prevent Out-Of-Memory crashes. Bring your raw datasets on Friday.
