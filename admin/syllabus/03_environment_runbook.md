<!--
---
yaml_schema_version: "2.2"
document_version: "1.0"
document_last_updated_date: "2026-11-01"
document_monotonicity: "REQUIRED"
routing_clearance: "PUBLIC - STUDENT FACING"
canvas_target_slug: "/courses/201513/pages/hw-1-and-mp-2-setup-walkthrough-docker-desktop-windows-11-+-wsl-2"
---
-->

<div align="center">
 <strong>🏫 Active Course Repositories:</strong> &nbsp;
 🏗️ <a href="https://github.com/kddresearch/course-architecture-base">Architecture Base</a> &nbsp;|&nbsp;
 🤖 CIS 530/730 &nbsp;|&nbsp;
 📊 <a href="https://github.com/kddresearch/cis531-731-2026_fall">CIS 531/731</a> &nbsp;|&nbsp;
 📈 CIS 732 &nbsp;|&nbsp;
 🎨 <a href="https://github.com/kddresearch/cis536-736-2026_fall">CIS 536/736</a> &nbsp;|&nbsp;
 👁️ CIS 798 X &nbsp;|&nbsp;
 🧠 CIS 830
</div>

<div align="center">
 <h1>🐳 CIS 531 / CIS 731 — Environment Runbook 1.0 &amp; Container Pinning Specification</h1>
 <p>
 <strong>Semester:</strong> Fall 2026 | <strong>Instructor:</strong> William H. Hsu<br>
 <strong>Document:</strong> <code>admin/syllabus/03_environment_runbook.md</code> | <strong>Walkthrough Mirror:</strong> <a href="../../docs/setup/hw1_mp2_windows11_wsl2_docker_walkthrough.md"><code>docs/setup/</code></a>
 </p>
</div>

<hr>

## ⚡ THE "ASK" BOX (ENVIRONMENT VERIFICATION SUMMARY)

* **Normative Course Baseline (CPU DevContainer):** Every homework (`HW`), machine problem (`MP`), and term-project milestone (`make verify` in [`HW 12`](../../assignments/homework/hw12_final_deliverables.md)) must build and run deterministically inside the course's pinned **CPU DevContainer** without requiring a GPU.
* **Required Verification Artifacts ([`HW 1`](../../assignments/homework/hw1_devcontainer_bringup_architecture.md) & [`MP 2`](../../assignments/machine_problems/mp2_devcontainer_deployment_manifest_verification.md)):**
  1. Build the pinned `Dockerfile` (`docker build -t cis531-runtime:mp2 .`).
  2. Run [`verify_env.py`](../../assignments/machine_problems/verify_env.py) inside the container (`docker run --rm cis531-runtime:mp2 python verify_env.py`) and confirm the exact SHA-256 runtime digest:
     `6f0648c69f69b1faeb96cfecaa7e2a904ed9210c8c07e0b5dd13d2f9df25ff75`
  3. Run `probe_compute.py` to log whether your environment is running on the **CPU-Only Fallback Baseline** or an optional **GPU Acceleration Overlay**.

---

## 1. Two-Tier Container Architecture & Digest Pinning Policy

To guarantee 100% reproducibility across Windows 11 laptops, Apple Silicon Macs, Linux workstations, and campus/national HPC clusters (Beocat / PSC Bridges-2), CIS 531/731 enforces a **Two-Tier DevContainer Policy** (checked by the grader by hand or automatically):

| Tier | Target Hardware | Role in Course & Grading Policy | Base Image & Pinning Requirement |
| :--- | :--- | :--- | :--- |
| **Tier 1: Canonical CPU DevContainer (Mandatory Baseline)** | All student laptops (Windows 11 WSL 2, macOS Intel/ARM64, Linux) | **Normative Grading Baseline:** 100% of assignments and term-project requirements (`make verify`) are solvable on CPU using `faiss-cpu` and Whisper `base` / `base.en`. | `python:3.10-slim-bullseye` with exact pinned package versions in [`term_project/docker/Dockerfile`](../../term_project/docker/Dockerfile). |
| **Tier 2: GPU Acceleration Profile (Optional Overlay)** | Beocat, PSC Bridges-2 (unlocked after [`PS 4`](../../assignments/homework/ps4_proposal.md) greenlight in Week 5+), or personal NVIDIA GPUs | **Optional Performance Acceleration:** Never required for grading; accelerates batch Whisper (`small.en` / `turbo`) or large embedding passes. | Official `pytorch/pytorch:<version>-cuda<version>-cudnn<version>-devel@sha256:<digest>` pinned by immutable SHA-256 digest. |

---

## 2. Host Prerequisites & Docker Desktop Setup (Windows 11 + WSL 2)

Ensure your host machine satisfies the following baseline before cloning the repository:

* **Operating System:** Windows 11 (Build `26200+`), macOS 13+, or Ubuntu 22.04+ Linux.
* **WSL 2 Kernel (Windows Hosts):** WSL 2 (`6.18+`) with hardware virtualization enabled in BIOS/UEFI. Check via PowerShell:
  ```powershell
  wsl --status
  ```
* **IDE & Extensions:** Visual Studio Code with the **Dev Containers** (`ms-vscode-remote.remote-containers`), **Docker**, and **Remote - SSH** extensions installed on the host. *(Avoid running unpinned host-side Python/Jupyter kernels for coursework; always execute inside the container).*

### Step 2.1: Docker Desktop General Settings
1. Open **Docker Desktop** and click the **Settings (gear icon)** in the top-right header.
2. Select **General** and verify:
   * **Choose Virtual Machine Manager (VMM):** `WSL 2`
   * **Use containerd for pulling and storing images:** `On`

### Step 2.2: WSL Integration Enablement
1. In Settings, select **Resources $\rightarrow$ WSL integration**.
2. Toggle **Enable integration with my default WSL distro** to `On` (and enable your target Ubuntu/Debian distro under additional distros if applicable).
3. Click **Apply & Restart Engine**.

### Step 2.3: WSL 2 Terminal Client & Daemon Smoke Test
Open your WSL 2 terminal and verify communication with the Docker daemon:
```bash
docker --version
# Expected: Docker version 29.9.2, build a82e528 (or current stable release)

docker run --rm hello-world
# Expected: "Hello from Docker! This message shows that your installation appears to be working correctly."
```

---

## 3. Pinned Runtime Specification (`Dockerfile` & `devcontainer.json`)

All submissions must match the pinned package versions specified in [`term_project/fixed_docker_manifest_requirement.md`](../../term_project/fixed_docker_manifest_requirement.md); floating (`latest` or unpinned) dependency versions fail verification.

### 3.1 Canonical Course `Dockerfile` (`term_project/docker/Dockerfile`)
```dockerfile
# ==============================================================================
# CIS 531/731: Canonical CPU DevContainer Dockerfile (MP2 & Term Project Baseline)
# ==============================================================================
FROM python:3.10-slim-bullseye

# 1. Install System Dependencies (Java 11 for PySpark 3.5 LTS; ffmpeg for Whisper)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    openjdk-11-jre-headless \
    ffmpeg \
    git \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# 2. Configure Environment Variables
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV PYSPARK_PYTHON=python3
ENV PIP_NO_CACHE_DIR=1

# 3. Install Python Dependencies (Strict Pinned Versions per Course Manifest)
RUN pip install --upgrade pip && \
    pip install \
    pyspark==3.5.1 \
    torch==2.2.1 \
    transformers==4.38.2 \
    openai-whisper==20231117 \
    mlflow==2.11.1 \
    faiss-cpu==1.8.0

# 4. Create Non-Root User
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
WORKDIR /workspace
```

### 3.2 DevContainer Configuration (`.devcontainer/devcontainer.json`)
```json
{
  "name": "CIS 531/731 MP2 Runtime",
  "build": {
    "dockerfile": "../Dockerfile",
    "context": ".."
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python",
        "ms-toolsai.jupyter",
        "redhat.vscode-yaml"
      ]
    }
  },
  "remoteUser": "vscode",
  "updateRemoteUserUID": true
}
```

---

## 4. Deterministic Environment & Compute Verification Scripts

### 4.1 SHA-256 Manifest Verification (`assignments/machine_problems/verify_env.py`)
```python
import hashlib
import sys

def verify_environment():
    try:
        import torch
        import transformers
        import mlflow
        import faiss
        import whisper
    except ImportError as e:
        print(f"FAILED: Missing module - {e}")
        sys.exit(1)

    env_str = (
        f"torch-{torch.__version__}_"
        f"transformers-{transformers.__version__}_"
        f"mlflow-{mlflow.__version__}_"
        f"faiss-{faiss.__version__}"
    )
    digest = hashlib.sha256(env_str.encode("utf-8")).hexdigest()
    print(f"Environment String: {env_str}")
    print(f"SHA-256 Digest: {digest}")

if __name__ == "__main__":
    verify_environment()
```

Run the build and verification check from your terminal:
```bash
docker build -t cis531-runtime:mp2 .
docker run --rm cis531-runtime:mp2 python verify_env.py
```

**Required Deterministic Output (for the grader to check by hand or automatically):**
```text
Environment String: torch-2.2.1_transformers-4.38.2_mlflow-2.11.1_faiss-1.8.0
SHA-256 Digest: 6f0648c69f69b1faeb96cfecaa7e2a904ed9210c8c07e0b5dd13d2f9df25ff75
```

### 4.2 Compute Allocation & Fallback Probe (`probe_compute.py`)
```python
import torch

def probe_compute():
    if torch.cuda.is_available():
        device_name = torch.cuda.get_device_name(0)
        print(f"GPU Pool Active - Batch Embedding/Whisper Ready ({device_name})")
    else:
        print("CPU-Only Fallback Active")

if __name__ == "__main__":
    probe_compute()
```

---

## 5. Course API Quota Gateway & CPU Fallback Protocol

* **Team Virtual Keys (LiteLLM Proxy):** Any course-managed LLM or commercial ASR/embedding service is accessed exclusively via a team-scoped virtual key that enforces model allow-lists, RPM/TPM rate limits, and a fail-closed team budget ceiling. Upstream provider credentials are never distributed to students or committed to Git.
* **Automatic CPU Fallback ("Broke the Bank" Protocol):** If a team exhausts its external API or streaming quota (e.g., AssemblyAI streaming credits in [`MP 5`](../../assignments/machine_problems/mp5_structured_streaming.md)), the pipeline must automatically fall back to local CPU execution (`openai-whisper` `base`/`base.en` and `SentenceTransformer('all-MiniLM-L6-v2')` inside the CPU DevContainer). Quota exhaustion never grants a deadline extension.
