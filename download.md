
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio"></a>

# Download DSI Studio

Download and unzip the package to run DSI Studio. No installation is needed.

DSI Studio is updated frequently, and computational results may differ between versions. For reproducibility, keep a local copy of the DSI Studio version used for each research project. It is usually best to update DSI Studio when starting a new project, not in the middle of an ongoing analysis.

## "Sun" Versions (2026-) — Recommended

The current Sun release is **2026.7.25**.

**New in Sun: AI Agent.** DSI Studio can use **Codex** or **Claude Code** to understand natural-language requests and operate workflows including reconstruction, fiber tracking, image processing, and connectometry. To use the AI Agent, install Codex or Claude Code on the same computer.

Choose your processor architecture first. For Linux, the **Universal** packages are recommended for most systems; Ubuntu-specific builds are also provided.

| Architecture | OS | Downloads | Notes |
|---|---|---|---|
| **x86_64**<br>Intel / AMD | **Windows 10/11** | **[NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win.zip)** · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_cpu.zip) | Unzip and run `dsi_studio.exe`. |
| **x86_64**<br>Intel / AMD | **macOS** | [Intel Mac](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-15-intel_qt6.zip) | Intel Macs. |
| **x86_64**<br>Intel / AMD | **Linux** | **Universal:** [NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda.zip) · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu.zip) · [Legacy CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_legacy_cpu.zip)<br>**Ubuntu:** [18.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804_cpu.zip) · [20.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004_cpu.zip) · [22.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu.zip) · [24.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu.zip) | **Universal:** Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10, openSUSE 15.6.<br>**Legacy:** also Ubuntu 16.04, Debian 9–10, CentOS 7, Amazon Linux 2. |
| **ARM64** | **Windows 11 ARM** | [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_arm64.zip) | Windows on ARM. |
| **ARM64** | **macOS** | [Apple Silicon](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-14-arm64_qt6.zip) | M1/M2/M3/M4 and newer. |
| **ARM64** | **Linux** | **Universal:** [NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda_arm64.zip) · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu_arm64.zip)<br>**Ubuntu:** [22.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_arm64.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu_arm64.zip) · [24.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_arm64.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu_arm64.zip) | **Universal CPU:** Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10, Amazon Linux 2023.<br>**CUDA:** Ubuntu 22.04–26.04, Debian 12–13, Rocky/AlmaLinux 10. |

**Docker (x86_64):** CPU image `dsistudio/dsistudio:latest` · CUDA image `dsistudio/dsistudio:cuda`

## "Hou" Versions (2024-2026) — Legacy

Hou introduced the compact `*.fz`/`*.sz` formats, DSI Studio Data Hub, new linear/nonlinear registration tools, and the redesigned graphical user interface.

> **Heads-up:** Hou versions have a known issue when creating `.dz` databases. If you need to create a `.dz` database, use the Sun version above.

| OS | File | Notes |
|----|------|-------|
| Windows 10/11 x64 | [GPU version for NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win.zip)<br>[CPU version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win_cpu.zip) | Unzip and run `dsi_studio.exe`. |
| macOS 13+ | [Apple Silicon version (M1/M2/M3/M4)](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-14-arm64_qt6.zip)<br>[Intel version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-15-intel_qt6.zip) | Unzip and run `dsi_studio.app`. |
| Ubuntu x86_64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 20.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004.zip)<br>[Ubuntu 22.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204.zip)<br>[Ubuntu 24.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404.zip)<br><br>CPU versions:<br>[Ubuntu 20.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004_cpu.zip)<br>[Ubuntu 22.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu.zip)<br>[Ubuntu 24.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu.zip) | Choose the package closest to your Ubuntu version. |
| Ubuntu arm64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 22.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_arm64.zip)<br>[Ubuntu 24.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_arm64.zip)<br><br>CPU versions:<br>[Ubuntu 22.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu_arm64.zip)<br>[Ubuntu 24.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu_arm64.zip) | For Linux arm64 desktops or workstations. |
| Docker | Docker Hub image:<br>`dsistudio/dsistudio:latest` | Command-line and Linux container workflows. |

**China mirror (Hou only):** [百度网盘](https://pan.baidu.com/s/5GuYBQbLHTN_HvShnM3oQew)

## Troubleshooting

- **Windows:** if DLL files are missing, install the [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe). The GPU version may require an updated NVIDIA driver and/or [CUDA Toolkit 11.8](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local).
- **macOS:** if macOS reports that the app is damaged or cannot be opened because it is not notarized, run `xattr -rd com.apple.quarantine /path/to/dsi_studio.app`. If the file-open dialog crashes with files on a cloud drive, copy the data to a local folder first.
- **Ubuntu:** if `libQt6Charts` is missing, run `sudo apt install libqt6charts6-dev`. For `xcb` errors, see this [forum solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI).

# Previous Versions

- ["Chen" versions (2022-2024)](https://github.com/frankyeh/DSI-Studio/releases)
- [Pre-"Chen" versions (2008-2022)](https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0)
- Older CentOS 8 builds, if needed, can be found in previous GitHub releases.

# License

DSI Studio offers dual licensing options for academic and commercial users.

## Academic License

DSI Studio is free for academic users under the [Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode). View the full license agreement [here](https://github.com/frankyeh/DSI-Studio/?tab=License-1-ov-file#readme).

DSI Studio is not FDA-cleared or FDA-approved. It is provided for research, education, and adjunct visualization. Any clinical use must be reviewed and approved under the user’s institutional policies. Results should not be used as the sole basis for diagnosis, treatment, or surgical decision-making.

## Commercial License

Please contact frank.yeh@gmail.com about the commercial license.

# Hardware Recommendations

DSI Studio runs on a regular desktop or laptop. For large tractography or connectomics workflows, 64 GB RAM or more, a fast SSD, and a modern multi-core CPU are recommended. An NVIDIA GPU can accelerate GPU-enabled functions.
