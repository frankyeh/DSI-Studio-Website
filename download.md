
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio" alt="Latest source commit"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio" alt="Latest GitHub release"></a>

# Download DSI Studio

Download and unzip the package to run DSI Studio. No installation is needed.

DSI Studio is updated frequently, and computational results may differ between versions. For reproducibility, keep a local copy of the DSI Studio version used for each research project. It is usually best to update DSI Studio when starting a new project, not in the middle of an ongoing analysis.

## "Sun" Versions (2026-) — Recommended

The current Sun release is **2026.7.25**.

**New in Sun: AI Agent.** DSI Studio supports local CLI agents including **Codex, Claude Code, Muse, Antigravity, and Grok** for natural-language workflows such as reconstruction, fiber tracking, image processing, and connectometry. Install and configure a supported agent on the same computer, then check its availability in DSI Studio's AI Agent settings.

Choose your processor architecture first. For Linux, the **Universal** packages are recommended for most systems; Ubuntu-specific builds are also provided.

<div class="dlv5-grid" aria-label="Recommended downloads by operating system">
<section class="dlv5-card" id="get-windows"><h3>Windows</h3><p class="dlv5-sub">Windows 10/11 · Intel / AMD x64</p><div class="dlv5-buttons"><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win.zip">NVIDIA GPU<small>Windows x64 · CUDA</small></a><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_cpu.zip">CPU<small>Windows x64</small></a></div><p class="dlv5-extra">Windows 11 ARM: <a href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_arm64.zip">ARM64 CPU download</a></p><p class="dlv5-tip">Choose NVIDIA GPU only with a compatible NVIDIA card and driver; otherwise select CPU. Unzip and run <code>dsi_studio.exe</code>.</p></section>
<section class="dlv5-card" id="get-macos"><h3>macOS</h3><p class="dlv5-sub">Apple Silicon or Intel</p><div class="dlv5-buttons"><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-arm64_qt6.zip">Apple Silicon<small>M-series Macs</small></a><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-intel_qt6.zip">Intel Mac<small>x86_64</small></a></div><p class="dlv5-tip">Download the current release first. If macOS blocks an older build, see <a href="#troubleshooting">Troubleshooting</a>.</p></section>
<section class="dlv5-card" id="get-linux"><h3>Linux</h3><p class="dlv5-sub">Universal packages · Intel / AMD x64</p><div class="dlv5-buttons"><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda.zip">NVIDIA GPU<small>Universal CUDA</small></a><a class="dlv5-button" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu.zip">CPU<small>Universal CPU</small></a></div><details class="dlv5-extra"><summary>ARM64 and legacy builds</summary><p>Universal ARM64: <a href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda_arm64.zip">NVIDIA GPU</a> · <a href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu_arm64.zip">CPU</a>. Older Linux x64: <a href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_legacy_cpu.zip">Legacy CPU</a>. Ubuntu-specific packages are in the complete table below.</p></details><p class="dlv5-tip">Universal packages support many Linux distributions. Choose GPU only if the machine has compatible NVIDIA hardware and drivers.</p></section>
<section class="dlv5-card" id="get-docker"><h3>Docker</h3><p class="dlv5-sub">Containers · x86_64</p><div class="dlv5-buttons"></div><pre>docker pull dsistudio/dsistudio:latest</pre><pre>docker pull dsistudio/dsistudio:cuda</pre><p class="dlv5-tip">Rolling tags serving the current Sun release: CPU <code>:latest</code> · CUDA <code>:cuda</code>.</p></section>
</div>
<p class="dlv5-matrix-heading"><strong>All Sun downloads</strong> — full architecture and distribution matrix</p>

| Architecture | OS | Downloads | Notes |
|---|---|---|---|
| **x86_64**<br>Intel / AMD | **Windows 10/11** | **[NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win.zip)** · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_cpu.zip) | Unzip and run `dsi_studio.exe`. |
| **x86_64**<br>Intel / AMD | **macOS** | [Intel Mac](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-intel_qt6.zip) | Intel Macs. |
| **x86_64**<br>Intel / AMD | **Linux** | **Universal:** [NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda.zip) · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu.zip) · [Legacy CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_legacy_cpu.zip)<br>**Ubuntu:** [18.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804_cpu.zip) · [20.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004_cpu.zip) · [22.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu.zip) · [24.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu.zip) | **Universal:** Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10, openSUSE 15.6.<br>**Legacy:** also Ubuntu 16.04, Debian 9–10, CentOS 7, Amazon Linux 2. |
| **ARM64** | **Windows 11 ARM** | [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_arm64.zip) | Windows on ARM. |
| **ARM64** | **macOS** | [Apple Silicon](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-arm64_qt6.zip) | M1/M2/M3/M4 and newer. |
| **ARM64** | **Linux** | **Universal:** [NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda_arm64.zip) · [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu_arm64.zip)<br>**Ubuntu:** [22.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_arm64.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu_arm64.zip) · [24.04 GPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_arm64.zip) / [CPU](https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu_arm64.zip) | **Universal CPU:** Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10, Amazon Linux 2023.<br>**CUDA:** Ubuntu 22.04–26.04, Debian 12–13, Rocky/AlmaLinux 10. |

**Docker (x86_64):** CPU image `dsistudio/dsistudio:latest` · CUDA image `dsistudio/dsistudio:cuda`

### Troubleshooting

- **Windows:** if DLL files are missing, install the [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe). GPU builds require a compatible NVIDIA driver; the CUDA runtime is statically linked, so the CUDA Toolkit does not need to be installed.
- **macOS:** Older builds may be blocked by Gatekeeper; try the current signed release first. For an older copy you trust, you can remove the quarantine flag with `xattr -rd com.apple.quarantine /path/to/dsi_studio.app`. If the file-open dialog crashes with files on a cloud drive, copy the data to a local folder first.
- **Ubuntu:** if `libQt6Charts` is missing, run `sudo apt install libqt6charts6-dev`. For `xcb` errors, see this [forum solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI).



## "Hou" Versions (2024-2026) — Legacy

Hou introduced the compact `*.fz`/`*.sz` formats, DSI Studio Data Hub, new linear/nonlinear registration tools, and the redesigned graphical user interface.

> **Compatibility:** The pinned **2025.04.16 Hou downloads below predate `.dz` database support**. Some later Hou builds added `.dz` creation but had issues that were fixed in later revisions. For creating `.dz` databases, use the current Sun version above.

| OS | File | Notes |
|----|------|-------|
| Windows 10/11 x64 | [GPU version for NVIDIA GPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win.zip)<br>[CPU version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win_cpu.zip) | Unzip and run `dsi_studio.exe`. |
| macOS 13+ | [Apple Silicon version (M1/M2/M3/M4)](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-14-arm64_qt6.zip)<br>[Intel version](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-15-intel_qt6.zip) | Unzip and run `dsi_studio.app`. |
| Ubuntu x86_64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 20.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004.zip)<br>[Ubuntu 22.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204.zip)<br>[Ubuntu 24.04](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404.zip)<br><br>CPU versions:<br>[Ubuntu 20.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004_cpu.zip)<br>[Ubuntu 22.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu.zip)<br>[Ubuntu 24.04 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu.zip) | Choose the package closest to your Ubuntu version. |
| Ubuntu arm64 | GPU versions for NVIDIA GPU:<br>[Ubuntu 22.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_arm64.zip)<br>[Ubuntu 24.04 arm64](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_arm64.zip)<br><br>CPU versions:<br>[Ubuntu 22.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu_arm64.zip)<br>[Ubuntu 24.04 arm64 CPU](https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu_arm64.zip) | For Linux arm64 desktops or workstations. |

**China mirror (Hou only):** [百度网盘](https://pan.baidu.com/s/5GuYBQbLHTN_HvShnM3oQew)

## Previous Versions

- ["Chen" versions (2022-2024)](https://github.com/frankyeh/DSI-Studio/releases)
- [Pre-"Chen" versions (2008-2022)](https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0)
- Older CentOS 8 builds, if needed, can be found in previous GitHub releases.

## License

DSI Studio offers dual licensing options for academic and commercial users.

## Academic License

DSI Studio is free for academic users. Read the [academic license agreement](https://dsi-studio.labsolver.org/academic_license.html). DSI Studio is not FDA-cleared or FDA-approved. It is provided for research, education, and adjunct visualization. Any clinical use must be reviewed and approved under the user’s institutional policies. Results should not be used as the sole basis for diagnosis, treatment, or surgical decision-making.

## Commercial License

Please contact frank.yeh@gmail.com about the commercial license.

## Hardware Recommendations

DSI Studio runs on a regular desktop or laptop. For large tractography or connectomics workflows, 64 GB RAM or more, a fast SSD, and a modern multi-core CPU are recommended. An NVIDIA GPU can accelerate GPU-enabled functions.
