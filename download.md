
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio"></a>

# Download DSI Studio

Download and unzip the package to run DSI Studio. No installation is needed.

DSI Studio is updated frequently, and computational results may differ between versions. For reproducibility, keep a local copy of the DSI Studio version used for each research project. It is usually best to update DSI Studio when starting a new project, not in the middle of an ongoing analysis.

## Which file should I download?

- **Windows:** use the CPU version if you do not have an NVIDIA GPU. Use the GPU version for CUDA acceleration.
- **Mac:** choose **Apple Silicon** for M1/M2/M3/M4 Macs and **Intel** for older Intel Macs. Check **Apple menu > About This Mac** if unsure.
- **Ubuntu:** choose the package matching your Ubuntu version and CPU architecture. Use the CPU version if CUDA is not needed.
- **Docker:** useful for command-line workflows, reproducible pipelines, or Linux GUI use with X11 forwarding.

## "Sun" Versions (2026-)

The current Sun release is **2026.7.25**.

### New in Sun: AI Agent

- Built-in support for **Codex** and **Claude Code**.
- The agent can read the DSI Studio AI manuals, inspect the current data and windows, and operate DSI Studio from natural-language requests.
- Supports workflows including reconstruction, fiber tracking, image processing, and connectometry.

To use the AI Agent, install **Codex or Claude Code** on the same computer.

| OS | File | Notes |
|----|------|-------|
| Windows 10/11 x64 | [GPU version for NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_win.zip)<br>[CPU version](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_win_cpu.zip) | Unzip and run `dsi_studio.exe`. |
| macOS 13+ | [Apple Silicon version (M1/M2/M3/M4)](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_macos-14-arm64_qt6.zip)<br>[Intel version](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_macos-15-intel_qt6.zip) | Unzip and run `dsi_studio.app`. |
| Ubuntu x86_64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 20.04](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2004.zip)<br>[Ubuntu 22.04](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2204.zip)<br>[Ubuntu 24.04](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2404.zip)<br><br>CPU versions:<br>[Ubuntu 20.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2004_cpu.zip)<br>[Ubuntu 22.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2204_cpu.zip)<br>[Ubuntu 24.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2404_cpu.zip) | Choose the package closest to your Ubuntu version. |
| Ubuntu arm64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 22.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2204_arm64.zip)<br>[Ubuntu 24.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2404_arm64.zip)<br><br>CPU versions:<br>[Ubuntu 22.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2204_cpu_arm64.zip)<br>[Ubuntu 24.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2026.7.25/dsi_studio_ubuntu2404_cpu_arm64.zip) | For Linux arm64 desktops or workstations. |
| Docker | Docker Hub image:<br>`dsistudio/dsistudio:latest` | Command-line and Linux container workflows. |

## "Hou" Versions (2024-2026)

### Highlights

- Introduced compact FIB/SRC formats (`*.fz`, `*.sz`) for better storage efficiency.
- Added DSI Studio Data Hub for instant access to thousands of readily trackable datasets.
- Added linear and nonlinear registration tools with support for multi-modality registration and lesion-aware workflows.
- Introduced a redesigned graphical user interface.

> **Heads-up:** Hou versions have a known issue when creating `.dz` databases. If you need to create a `.dz` database, use the Sun version above.

| OS | File | Notes |
|----|------|-------|
| Windows 10/11 x64 | [GPU version for NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win.zip)<br>[CPU version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win_cpu.zip) | Unzip the file and run `dsi_studio.exe`.<br>If DLL files are missing, install the [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe).<br>The GPU version may require an updated NVIDIA driver and/or [CUDA Toolkit 11.8](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local). |
| macOS 13+ | [Apple Silicon version (M1/M2/M3/M4)](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-14-arm64_qt6.zip)<br>[Intel version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-15-intel_qt6.zip) | Unzip the file and run `dsi_studio.app`.<br>If macOS reports that the app is damaged or cannot be opened because it is not notarized, run:<br>`xattr -rd com.apple.quarantine /path/to/dsi_studio.app`<br>You may drag `dsi_studio.app` into Terminal after typing the command to fill in the path.<br>Known issue: the file-open dialog may crash when opening files stored on some cloud drives. If this happens, copy the data to a local folder first. |
| Ubuntu x86_64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 20.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004.zip)<br>[Ubuntu 22.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204.zip)<br>[Ubuntu 24.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404.zip)<br><br>CPU versions:<br>[Ubuntu 20.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004_cpu.zip)<br>[Ubuntu 22.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu.zip)<br>[Ubuntu 24.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu.zip) | Choose the package closest to your Ubuntu version.<br>If there is an error related to `libQt6Charts`, run:<br>`sudo apt install libqt6charts6-dev`<br>If there is an error related to `xcb`, check this [forum solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI). |
| Ubuntu arm64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 22.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_arm64.zip)<br>[Ubuntu 24.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_arm64.zip)<br><br>CPU versions:<br>[Ubuntu 22.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu_arm64.zip)<br>[Ubuntu 24.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu_arm64.zip) | For Linux arm64 desktops or workstations.<br>If there is an error related to `libQt6Charts`, run:<br>`sudo apt install libqt6charts6-dev`<br>If there is an error related to `xcb`, check this [forum solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI). |
| Docker | Docker Hub image:<br>`dsistudio/dsistudio:latest` | Command-line use:<br>`docker run -ti --rm -v "$PWD":/data dsistudio/dsistudio:latest dsi_studio --help`<br><br>Linux GUI use with X11:<br>`xhost +local:docker`<br>`docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v "$PWD":/data dsistudio/dsistudio:latest` |

***[Alternate download in China: 百度网盘](https://pan.baidu.com/s/5GuYBQbLHTN_HvShnM3oQew)***



# Previous Versions

- ["Chen" versions (2022-2024)](https://github.com/frankyeh/DSI-Studio/releases)
- [Pre-"Chen" versions (2008-2022)](https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0)
- Older CentOS 8 builds, if needed, can be found in previous GitHub releases.

**Major Changes**

- 2020/08/02: Major revision on QA calculation. nQA is now replacing QA (https://groups.google.com/g/dsi-studio/c/t-kSFxXrGFU)
- 2023/06/28: Fiber tracking results will change because the default step size = 0 has a different implementation. Older versions will randomly select between 0.5 and 1.5 voxel spacing. The updated version will have 1.0 voxel spacing. To replicate older versions, set the step size to -1 in the command line. Fiber tracking with nonzero step size and correlation tracking is not affected.
- 2023/07/08: Tractography atlas is further separated into 5 sets of pathways. The GUI and command line interface for automated fiber tracking has been modified. The updated DSI Studio allows for the use of multiple tractography atlases.
- 2023/10/02: The Otsu's threshold was updated to ignore zero values in the background. As a result, the equivalent value will be slightly different if there are zeros in the background. The seeding region in automatic fiber tracking was updated to provide more comprehensive mapping. The previous version seeded within a more restricted region designated by the atlas, and some branches may not have been fully covered. The updated version uses a larger seeding region to improve coverage.

# License

DSI Studio offers dual licensing options for academic and commercial users.

## Academic License

DSI Studio is free for academic users under the [Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode). View the full license agreement [here](https://github.com/frankyeh/DSI-Studio/?tab=License-1-ov-file#readme).

DSI Studio is not FDA-cleared or FDA-approved. It is provided for research, education, and adjunct visualization. Any clinical use must be reviewed and approved under the user’s institutional policies. Results should not be used as the sole basis for diagnosis, treatment, or surgical decision-making.

## Commercial License

Please contact frank.yeh@gmail.com about the commercial license.

# Hardware Recommendations

DSI Studio can run on a regular desktop or laptop. Large tractography or connectomics workflows benefit from a workstation with:

1. A modern multi-core CPU.
2. 64 GB RAM or more for large datasets.
3. A fast NVMe SSD for image data and temporary files.
4. An NVIDIA GPU for CUDA acceleration if using GPU-enabled functions.
5. Windows 10/11 or Ubuntu 22.04/24.04 for the broadest compatibility.

