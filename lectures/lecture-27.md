# CIS 536 / 736: Computer Graphics

| Phase | Module | Lecture | Date |
| :--- | :--- | :--- | :--- |
| **Phase 3:** Generative Assets & Neural Rendering[cite: 48] | **Module 8:** Generative Geometry & 3D Gaussian Splatting[cite: 48] | **Lecture 27:** Lab 5a - Capture & Local gsplat Reconstruction (8GB VRAM floor)[cite: 48, 50] | Fri 30 Oct 2026[cite: 48, 49, 50] |

---

## Slide 1: Lab 5a Overview & Agenda — Capture & Local gsplat Reconstruction
### CIS 536 / 736 - Lecture 27
* **Dataset Capture Best Practices:** Hands-on instruction for capturing effective video and image sequences—maintaining lighting consistency, frame overlap, and coverage angles—for Structure-from-Motion (SfM) reconstruction[cite: 48].
* **Environment Bring-Up (8GB VRAM Floor):** Configuring the local Python/CUDA environment to run `gsplat.studio` and COLMAP within the constraints of local workstation hardware (8GB VRAM floor)[cite: 48, 50].
* **Executing COLMAP:** Processing the raw capture dataset to extract camera calibration metadata and generate the sparse point cloud[cite: 48].
* **Training the Splat:** Initializing and training the 3D Gaussian Splatting model locally, monitoring loss, and managing VRAM limits during optimization[cite: 48].
* **Export, Visualization & Assigned Readings:** Exporting the trained `.ply` artifact and visualizing the real-time radiance field using standard viewers or Unity plugins, grounded in the `gsplat` GitHub README/Quickstart and COLMAP documentation[cite: 48, 49, 50].

**Speaker Notes:**
Welcome to Lecture 27 and Lab 5a, the hands-on culmination of Module 8 on Generative Geometry and 3D Gaussian Splatting[cite: 48]. Over the past two sessions, we surveyed open-weight 3D generative models (`Point-E` and `Shap-E`) on Monday and unpacked the mathematics of 3D Gaussian Splatting and differentiable tile-based rasterization on Wednesday[cite: 48, 49, 50]. Today, we put those principles into practice by executing a complete local reconstruction pipeline—from raw multi-view photo/video capture through COLMAP Structure-from-Motion pose estimation, local `gsplat` optimization within an 8GB VRAM floor, and `.ply` export for real-time visualization in standard viewers or Unity 6[cite: 48, 50].

---

## Slide 2: Bridging Theory to Practice — From Lectures 25 & 26 to Lab 5a
### Radiance Fields, Gaussian Primitives, and Local Execution
* **From Generative Surveys to Real-Time Splatting:** Whereas Lecture 25 examined text-to-3D point clouds (`Point-E`) and implicit functions (`Shap-E`) and Lecture 26 established the Kerbl et al. 3D Gaussian Splatting formulation, Lab 5a executes the full reconstruction loop from real-world imagery[cite: 48, 49, 50].
* **The 3D Gaussian Splat Primitive in Practice:** Every Gaussian primitive we optimize today is defined by its 3D position $\mu$, 3D covariance $\Sigma$ (factored into scale and rotation), opacity $\alpha$, and spherical harmonics (SH) coefficients for view-dependent color[cite: 48].
* **Local Workstation Execution (8GB VRAM Floor):** By combining COLMAP with the `gsplat` differentiable tile-based rasterizer, we achieve real-time rendering speeds ($\ge 30\text{ fps}$ at 1080p) and full local training within an 8GB GPU memory envelope[cite: 48, 50].

**Speaker Notes:**
Before we open the terminal, let's connect today's lab steps directly to the mathematical pipeline from Lecture 26[cite: 48]. Unlike Neural Radiance Fields (NeRFs) that rely on expensive volumetric ray-marching, 3D Gaussian Splatting projects explicit 3D ellipsoids onto the 2D image plane and composites them via a fast, differentiable tile-based rasterizer[cite: 48]. However, that rasterizer requires two crucial inputs before a single gradient step can run: accurate camera intrinsics/extrinsics and an initial sparse point cloud[cite: 48]. That is why our pipeline today is divided into two tightly coupled halves: geometric calibration via COLMAP, followed by photometric optimization and adaptive density control in `gsplat`[cite: 48, 50].

---

## Slide 3: Dataset Capture Best Practices — Photometric & Geometric Discipline
### Lighting Consistency, Exposure Lock, and Static Scene Constraints
* **Lighting & Exposure Consistency:** Lock auto-exposure, auto-white-balance, and focus on your capture device before recording so that surface radiance remains consistent across viewing angles[cite: 48].
* **Eliminating Motion Blur:** Use a fast shutter speed and smooth, steady camera translation when recording video sequences so extracted frames contain crisp high-frequency corners for SIFT feature detection[cite: 48].
* **Static Scene Assumption:** Avoid moving subjects, wind-blown foliage, or shifting shadows cast by the camera operator, as transient changes violate multi-view epipolar consistency during Structure-from-Motion[cite: 48].

**Speaker Notes:**
Your reconstruction quality in Lab 5a is upper-bounded by the quality of your input capture[cite: 48]. Structure-from-Motion in COLMAP assumes a static scene under fixed illumination[cite: 48]. If your phone or camera dynamically shifts exposure or white balance as you walk around an object, COLMAP may still match geometric features, but the downstream Gaussian Splat optimizer will hallucinate floating colored splats ("floaters") in mid-air to explain away the sudden brightness shifts between views[cite: 48]. Lock your exposure and focus on the primary subject, keep yourself out of the light path so you don't cast a moving shadow, and prioritize sharp, blur-free frames[cite: 48].

---

## Slide 4: Dataset Capture Best Practices — Overlap & Coverage Angles
### Multi-Elevation Orbits and Video-to-Frame Sampling for SfM
* **Dense View Overlap:** Maintain high spatial overlap (70–80%+ shared scene content between consecutive frames) so COLMAP can track 2D keypoints continuously across the trajectory[cite: 48].
* **Multi-Angle Hemisphere Coverage:** Capture full $360^\circ$ loops at multiple elevation angles (a low-angle pass, an eye-level pass, and a high-angle downward pass) rather than rotating the camera in place from a single center[cite: 48].
* **Frame Extraction Budgeting:** When sampling frames from an input video via `ffmpeg`, extract 60 to 150 sharp, evenly spaced frames to provide strong parallax baselines while staying within the 8GB VRAM training budget[cite: 48, 50].

**Speaker Notes:**
A common beginner mistake is standing in one spot and panning the camera like a tripod panorama. Pure rotation provides zero parallax, which means COLMAP cannot triangulate depth[cite: 48]! Instead, translate the camera around the target object or scene in two to three concentric rings at different heights—eye level, slightly above looking down, and slightly below looking up—ensuring each frame overlaps heavily with its neighbors[cite: 48]. If you record a 30-to-60-second video at 30 or 60 fps, extract roughly 2 to 3 frames per second (`ffmpeg -i input.mp4 -vf fps=2 frames/frame_%04d.jpg`) so you end up with roughly 80 to 120 crisp, well-baselines views[cite: 48].

---

## Slide 5: Environment Bring-Up — Local Python/CUDA, COLMAP & `gsplat.studio`
### Configuring the Toolchain Within Local Hardware Constraints (8GB VRAM Floor)
* **Python, PyTorch & CUDA Verification:** Activate your local environment and verify CUDA availability (`torch.cuda.is_available()`) alongside matching CUDA toolkit compilers required by `gsplat`[cite: 48, 49, 50].
* **Installing `gsplat` and `gsplat.studio`:** Follow the `gsplat` GitHub README and Quickstart to install the differentiable rasterization kernels and the interactive `gsplat.studio` / `viser` training viewer[cite: 48, 49, 50].
* **COLMAP Binary & VRAM Pre-Check:** Confirm `colmap -h` executes cleanly from your terminal and inspect baseline GPU memory usage (`nvidia-smi`) to ensure your 8GB VRAM floor is clear of background workloads[cite: 48, 49, 50].

**Speaker Notes:**
Step 2 of Lab 5a is bringing up our local Python/CUDA environment[cite: 48]. Per our course hardware policy, every workflow in Module 8 is engineered to run within an 8GB VRAM floor[cite: 48, 50]. Make sure you close heavy background GPU applications (such as an open Unity Editor or extra browser tabs with hardware acceleration) before starting training[cite: 48, 50]. In your terminal, verify that PyTorch sees your CUDA device, that `colmap` is accessible on your system `PATH`, and that `gsplat` imports without compilation errors as detailed in the assigned `gsplat` GitHub README and Quickstart guide[cite: 48, 49, 50].

---

## Slide 6: Executing COLMAP (Part 1) — Feature Extraction & Matching
### Extracting SIFT Keypoints and Establishing Multi-View Correspondences
* **Step 1 — Feature Extraction (`colmap feature_extractor`):** Detect scale-invariant keypoints and descriptors across your image directory while specifying `--ImageReader.single_camera 1` and a camera model (e.g., `SIMPLE_RADIAL` or `OPENCV`)[cite: 48, 49, 50].
* **Step 2 — Feature Matching (`exhaustive_matcher` vs. `sequential_matcher`):** Run `colmap sequential_matcher` (with loop detection) for ordered video frames or `colmap exhaustive_matcher` for unordered photo sets of ~50–150 images[cite: 48, 49, 50].
* **Geometric Verification:** COLMAP filters raw descriptor matches using RANSAC and the fundamental/essential matrix to retain only geometrically consistent inlier correspondences in `database.db`[cite: 48, 49, 50].

**Speaker Notes:**
With our frames inside `data/<scene>/images/`, we execute COLMAP to solve the Structure-from-Motion problem[cite: 48, 49, 50]. First, `colmap feature_extractor` scans every image for SIFT features and stores them in a SQLite database (`database.db`)[cite: 48, 49, 50]. Because all frames in your capture come from the same camera lens at a fixed zoom, passing `--ImageReader.single_camera 1` forces all frames to share a single set of intrinsic parameters (focal length, principal point, and radial distortion), which dramatically improves calibration stability[cite: 48, 49, 50]. Second, we match those features across views using either sequential matching (for video sequences) or exhaustive matching (for small photo sets)[cite: 48, 49, 50].

---

## Slide 7: Executing COLMAP (Part 2) — Sparse Reconstruction & Image Undistortion
### Generating the Sparse Point Cloud and Pin-Hole Camera Metadata
* **Step 3 — Sparse Bundle Adjustment (`colmap mapper`):** Incrementally registers images, triangulates 3D points, and runs global bundle adjustment to output `cameras.bin`, `images.bin`, and `points3D.bin` under `sparse/0/`[cite: 48, 49, 50].
* **Step 4 — Pinhole Undistortion (`colmap image_undistorter`):** Removes radial lens distortion and converts camera intrinsics to an ideal pinhole model (`PINHOLE`) required by the 3D Gaussian projection math[cite: 48, 49, 50].
* **Diagnostic Sanity Check:** Load `sparse/0/` in the COLMAP GUI (`colmap gui`) to verify that nearly 100% of your input frames registered along a smooth camera trajectory with a clean sparse point cloud[cite: 48, 49, 50].

**Speaker Notes:**
Once matching completes, `colmap mapper` performs incremental Structure-from-Motion: it picks a strong initial image pair, triangulates 3D points, registers remaining cameras via Perspective-n-Point (PnP), and refines everything via bundle adjustment[cite: 48, 49, 50]. Always check your output folder `sparse/0/`! If COLMAP split your scene into `sparse/0/` and `sparse/1/`, or only registered 15 out of 100 frames, your capture had an overlap gap or motion blur[cite: 48, 49, 50]. Finally, because the fast 2D covariance projection in `gsplat` assumes an ideal pinhole camera, ensure your images and camera files are undistorted if you captured with a wide-angle lens[cite: 48, 49, 50].

---

## Slide 8: Training the Splat (Part 1) — Initialization & Loss Monitoring
### Seeding 3D Gaussians from COLMAP and Optimizing in `gsplat`
* **Sparse Point Cloud Initialization:** Each 3D point in COLMAP's `points3D.bin` seeds one initial 3D Gaussian at position $\mu_i$, with initial isotropic scale set to the mean distance to its three nearest neighbors[cite: 48].
* **Differentiable Tile-Based Training Loop:** `gsplat` projects Gaussians into 16×16 screen tiles, sorts them front-to-back on the GPU, alpha-composites the rendered image $\hat{I}$, and backpropagates the combined $\mathcal{L}_1 + \lambda \mathcal{L}_{\text{D-SSIM}}$ photometric loss[cite: 48, 50].
* **Live Telemetry in `gsplat.studio`:** Monitor training loss, PSNR, and Gaussian count in real time via the interactive web viewer while orbiting the reconstructing scene during optimization[cite: 48, 50].

**Speaker Notes:**
Now we launch `gsplat` training pointed at our COLMAP dataset directory[cite: 48, 49, 50]. At iteration 0, the model loads `points3D.bin` and places a 3D Gaussian at each sparse SfM point[cite: 48]. If you look at the live viewer in the first few hundred steps, the scene looks like a fuzzy watercolor painting of coarse ellipsoids[cite: 48]. As the Adam optimizer updates positions, quaternions, scales, opacities, and spherical harmonics against the $\mathcal{L}_1$ and D-SSIM loss, watch both the training loss curve and the total number of Gaussians—which brings us to adaptive density control and VRAM management[cite: 48].

---

## Slide 9: Training the Splat (Part 2) — Adaptive Density Control & 8GB VRAM Management
### Cloning, Splitting, Pruning, and Staying Within Hardware Bounds
* **Adaptive Density Control (Cloning & Splitting):** Every 100 iterations (during the densification window), Gaussians with large view-space positional gradients are **cloned** (if small, to fill under-reconstructed geometry) or **split** (if large, to resolve over-reconstructed regions)[cite: 48].
* **Pruning & Opacity Resets:** Transparent splats ($\alpha < \epsilon_\alpha$) and oversized floaters near the camera are pruned, with periodic opacity resets preventing irreversible floater accumulation[cite: 48].
* **8GB VRAM Survival Controls:** To prevent out-of-memory (OOM) errors on an 8GB GPU, use `--data_factor 2` or `4` (downsampling 4K frames to 1080p/720p), cap maximum splat count (e.g., 500k–1M Gaussians), or pause the interactive viewer during peak densification[cite: 48, 50].

**Speaker Notes:**
Adaptive density control is where 3D Gaussian Splatting turns a sparse 20,000-point COLMAP cloud into a photorealistic radiance field of hundreds of thousands of splats[cite: 48]. However, every time `gsplat` clones and splits Gaussians, VRAM consumption climbs[cite: 48]. On an 8GB VRAM workstation, trying to train on raw 4K phone photos while allowing unbounded densification up to 4 million Gaussians will trigger a CUDA Out-Of-Memory error around iteration 7,000 to 10,000[cite: 48, 50]. Keep your training resolution around 1080p (`--data_factor 2` or `4`), monitor peak allocated memory, and let pruning strip away transparent splats so your run finishes cleanly at 30,000 steps (or 7,000 steps for a fast diagnostic preview)[cite: 48, 50].

---

## Slide 10: Export, Visualization & Transition to Module 9
### Exporting `.ply` Radiance Fields and Previewing Cloud Rendering
* **Exporting the Trained `.ply` Artifact:** At the end of optimization, export the converged Gaussian attributes ($\mu$, $\text{scale}$, $\text{quat}$, $\text{opacity}$, and SH coefficients) as a standard binary `.ply` file[cite: 48].
* **Real-Time Visualization & Engine Integration:** Load your `.ply` artifact into `gsplat.studio`, web-based WebGL/WebGPU splat viewers, Blender add-ons, or Unity 6 Gaussian Splatting / OpenUSD scenes to verify real-time ($\ge 30\text{ fps}$) playback[cite: 48, 50].
* **Lab 5a Deliverable & Next Week's Transition (Module 9):** Commit your capture summary, COLMAP stats, training log, and exported `.ply` screenshot; on **Monday, Nov 2 (Lecture 28)**, we begin **Module 9: Cloud Rendering Concepts & AWS Deadline Cloud**[cite: 48, 49, 50].

**Speaker Notes:**
Once training converges, `gsplat` writes out your trained radiance field as a `.ply` file[cite: 48]. Load that `.ply` into the viewer or your Unity 6 project and test novel-view synthesis by flying the camera along paths that were never in your original video—notice how the spherical harmonics capture view-dependent highlights at 60+ frames per second[cite: 48]. Make sure you capture your COLMAP sparse view, your training loss/VRAM log, and your final real-time `.ply` render for your Lab 5a submission[cite: 48]. Next week, in Module 9 (Lectures 28–30), we will examine what happens when local compute isn't enough—moving into distributed Cloud Rendering Concepts and AWS Deadline Cloud with capped budget controls[cite: 49, 50]. Let's open our terminals and start the Lab 5a walkthrough[cite: 48]!
