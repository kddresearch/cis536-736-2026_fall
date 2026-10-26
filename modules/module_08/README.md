# CIS 536 / 736: Computer Graphics

| Phase | Module | Lecture(s) | Date(s) |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Generative Assets & Neural Rendering | **Module 8:** Generative Geometry & 3D Gaussian Splatting | **Lectures 25-27** | Mon 26 Oct - Fri 30 Oct 2026 |

---

## Lecture 25: Generative Geometry Survey: Shap-E, Point-E (Mon 26 Oct)

1. **Explicit vs. Implicit 3D Representations:** Understanding the difference between generating explicit meshes (vertices/faces) versus implicit functions (SDFs, NeRFs) for 3D asset creation.
2. **Point Cloud Generation (Point-E):** How diffusion models can be adapted to generate colored point clouds directly from text prompts without complex UV mapping.
3. **Implicit 3D Functions (Shap-E):** Exploring the mechanics of generating parameters for implicit functions that can be rendered as both textured meshes and neural radiance fields.
4. **Open-Weight & Local Execution:** The architectural and pipeline benefits of utilizing open-weight generative models that run locally within the bounds of a workstation (8GB VRAM floor).
5. **Integration with OpenUSD:** Establishing a pipeline for converting generated point clouds or implicit meshes into standard `.usd` assets for downstream use in Unity or Blender.

---

## Lecture 26: 3D Gaussian Splatting Fundamentals (Wed 28 Oct)

1. **Radiance Fields vs. Rasterization:** Bridging the gap between neural rendering (NeRFs) and real-time rasterization via 3D Gaussian representations.
2. **The Gaussian Splat Primitive:** Understanding the mathematical structure of a 3D Gaussian (position, covariance/scale/rotation, opacity, and spherical harmonics for color).
3. **Camera Pose Estimation (COLMAP):** The necessity of Structure-from-Motion (SfM) to extract accurate sparse point clouds and camera parameters from uncalibrated 2D video/images.
4. **Differentiable Tile-Based Rasterizer:** How the `gsplat` architecture achieves real-time rendering speeds (≥30fps at 1080p) by avoiding costly ray-marching.
5. **Optimization and Pruning:** The process of training a Gaussian Splat model, including adaptive density control (cloning and splitting Gaussians to fill detail gaps while pruning transparent splats).

---

## Lecture 27: Lab 5a - Capture & Local gsplat Reconstruction (Fri 30 Oct)

1. **Dataset Capture Best Practices:** Hands-on instruction for capturing effective video/image sequences (lighting consistency, overlap, coverage angles) for SfM reconstruction.
2. **Environment Bring-Up:** Configuring the local Python/CUDA environment to run `gsplat.studio` and COLMAP within the constraints of local hardware.
3. **Executing COLMAP:** Processing the raw capture dataset to generate the sparse point cloud and camera calibration metadata.
4. **Training the Splat:** Initializing and training the 3D Gaussian Splatting model locally, monitoring loss, and managing VRAM limits during optimization.
5. **Export and Visualization:** Exporting the trained `.ply` artifact and visualizing the real-time radiance field using standard viewers or Unity plugins.
