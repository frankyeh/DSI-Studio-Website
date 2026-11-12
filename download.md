
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio" alt="Latest source commit"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio" alt="Latest GitHub release"></a>

# Download DSI Studio

Choose your operating system and download the matching package. Unzip to run; no installation is needed.

<details class="dl6-notes"><summary>Versioning and reproducibility</summary><p>DSI Studio is updated frequently, and computational results may differ between versions. For reproducibility, keep a local copy of the DSI Studio version used for each research project. It is usually best to update DSI Studio when starting a new project, not in the middle of an ongoing analysis.</p></details>

## "Sun" Versions (2026-) — Recommended

Current release: **2026.7.25**.

<style>
/* Download v6 only: page-scoped styles, intentionally independent from site-wide CSS. */
.dl6-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:16px;margin:20px 0 28px}
.dl6-card{min-width:0;border:1px solid var(--line);border-radius:15px;padding:21px;background:#fff;scroll-margin-top:85px}
.dl6-card-head{display:flex;justify-content:space-between;align-items:baseline;gap:9px;margin-bottom:16px}
article .dl6-card h3{font:600 23px/1.2 var(--serif);margin:0}
.dl6-card-head>span{font-size:12px;color:var(--muted);text-align:right}
.dl6-buttons{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:9px}
.dl6-btn{display:flex;min-height:60px;justify-content:center;align-items:center;flex-direction:column;text-align:center;gap:4px;padding:9px 5px;border:1px solid #c6cfda;border-radius:10px;background:#f7f9fc;color:var(--ink)!important;font-size:14px;font-weight:650;text-decoration:none!important;overflow-wrap:anywhere}
.dl6-btn:hover{border-color:#5273a6;background:#eff4fc;box-shadow:0 2px 8px #1d36500c}
.dl6-btn:focus-visible,.dl6-extra summary:focus-visible{outline:2px solid #5273a6;outline-offset:3px}
.dl6-btn small{font-size:11px;color:var(--muted);font-weight:400}
.dl6-hint{font-size:12.5px;color:var(--muted);margin:13px 0 0!important}
.dl6-extra{margin-top:14px;border-top:1px solid var(--line);padding-top:11px}
.dl6-extra summary{cursor:pointer;color:#3c526d;font-size:13px;font-weight:600}
.dl6-extra-body{padding-top:12px}
article .dl6-extra-body p{margin:9px 0 0;font-size:13px;color:var(--muted)}
article .dl6-extra-body pre{margin:0 0 8px;font-size:12px}
.dl6-cluster-label{font-size:12px!important;font-weight:700!important;margin:12px 0 7px!important;color:var(--ink)!important}
.dl6-legacy>summary{cursor:pointer;font:600 17px/1.4 var(--serif);padding:15px 18px;border:1px solid var(--line);border-radius:12px;background:var(--soft)}
.dl6-legacy[open]>summary{margin-bottom:15px}
.dl6-notes>summary{cursor:pointer;color:var(--muted);font-weight:600}
.dl6-notes{margin:13px 0 20px}
.dl6-legacy-grid{margin-top:14px}
@media(max-width:780px){.dl6-grid{grid-template-columns:1fr}.dl6-card{padding:18px}.dl6-card-head{flex-wrap:wrap}}
@media(max-width:360px){.dl6-buttons{grid-template-columns:1fr}}
</style>


<div class="dl6-grid">
<section class="dl6-card" id="get-windows"><div class="dl6-card-head"><h3>Windows</h3><span>10 / 11 · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_cpu.zip"><span>CPU</span><small>Windows x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win.zip"><span>NVIDIA GPU</span><small>Windows x64 · CUDA</small></a></div><details class="dl6-extra"><summary>Windows ARM64</summary><div class="dl6-extra-body"><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_arm64.zip"><span>ARM64 CPU</span><small>Windows 11 on ARM</small></a></div></div></details><p class="dl6-hint">GPU requires a compatible NVIDIA card and driver.</p></section>
<section class="dl6-card" id="get-macos"><div class="dl6-card-head"><h3>macOS</h3><span>Apple Silicon / Intel</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-arm64_qt6.zip"><span>Apple Silicon</span><small>M-series Macs</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-intel_qt6.zip"><span>Intel</span><small>x64 Macs</small></a></div><details class="dl6-extra"><summary>Installation notes</summary><div class="dl6-extra-body"><p>Download the current release first. If macOS blocks an older build, see <a href="#troubleshooting">Troubleshooting</a>.</p></div></details></section>
<section class="dl6-card" id="get-linux"><div class="dl6-card-head"><h3>Linux</h3><span>Universal · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu.zip"><span>CPU</span><small>Universal x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda.zip"><span>NVIDIA GPU</span><small>Universal x64</small></a></div><details class="dl6-extra"><summary>ARM64, older Linux and Ubuntu builds</summary><div class="dl6-extra-body"><p class="dl6-cluster-label">Universal ARM64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu_arm64.zip"><span>ARM64 CPU</span><small>Universal</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda_arm64.zip"><span>ARM64 NVIDIA</span><small>Universal</small></a></div><p class="dl6-cluster-label">Older Linux</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_legacy_cpu.zip"><span>Legacy CPU</span><small>x64 · older distributions</small></a></div><p class="dl6-cluster-label">Ubuntu x64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804_cpu.zip"><span>Ubuntu 18.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804.zip"><span>Ubuntu 18.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004_cpu.zip"><span>Ubuntu 20.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004.zip"><span>Ubuntu 20.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu.zip"><span>Ubuntu 22.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204.zip"><span>Ubuntu 22.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu.zip"><span>Ubuntu 24.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404.zip"><span>Ubuntu 24.04 NVIDIA</span><small>x64</small></a></div><p class="dl6-cluster-label">Ubuntu ARM64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu_arm64.zip"><span>Ubuntu 22.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_arm64.zip"><span>Ubuntu 22.04 NVIDIA</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu_arm64.zip"><span>Ubuntu 24.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_arm64.zip"><span>Ubuntu 24.04 NVIDIA</span><small>ARM64</small></a></div><p>Universal supports Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10 and openSUSE 15.6. Legacy CPU supports older Linux distributions including Ubuntu 16.04, Debian 9–10, CentOS 7 and Amazon Linux 2. Linux ARM64 CUDA requires a compatible GPU and driver.</p></div></details><p class="dl6-hint">Universal CPU is the safest starting point; NVIDIA builds need compatible GPU hardware.</p></section>
<section class="dl6-card" id="get-docker"><div class="dl6-card-head"><h3>Docker</h3><span>x86_64 · containers</span></div><pre>docker pull dsistudio/dsistudio:latest</pre><pre>docker pull dsistudio/dsistudio:cuda</pre><p class="dl6-hint">Rolling Sun tags: <code>:latest</code> CPU · <code>:cuda</code> NVIDIA GPU.</p></section>
</div>
<details class="dl6-notes"><summary>Release details, AI Agent and architecture notes</summary>
<p>Sun includes AI Agent workflows through supported local CLI agents including Codex, Claude Code, Muse, Antigravity and Grok. Configure a supported agent locally and check availability in DSI Studio's AI Agent settings.</p>
<p>Windows and Linux GPU packages need compatible NVIDIA hardware and drivers; CPU packages do not. Linux Universal packages are recommended for most systems. Ubuntu-specific and legacy binaries remain available in each card's expandable options.</p>
</details>

### Troubleshooting

- **Windows:** if DLL files are missing, install the [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe). GPU builds require a compatible NVIDIA driver; the CUDA runtime is statically linked, so the CUDA Toolkit does not need to be installed.
- **macOS:** Older builds may be blocked by Gatekeeper; try the current signed release first. For an older copy you trust, you can remove the quarantine flag with `xattr -rd com.apple.quarantine /path/to/dsi_studio.app`. If the file-open dialog crashes with files on a cloud drive, copy the data to a local folder first.
- **Ubuntu:** if `libQt6Charts` is missing, run `sudo apt install libqt6charts6-dev`. For `xcb` errors, see this [forum solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI).



## "Hou" Versions (2024-2026) — Legacy

<details class="dl6-legacy"><summary>Hou 2025.04.16 — archived downloads and compatibility</summary>
<p>Hou introduced compact <code>*.fz</code>/<code>*.sz</code> formats, Data Hub, registration tools and a redesigned GUI. The pinned 2025.04.16 builds predate <code>.dz</code> database support; use current Sun for creating <code>.dz</code> databases.</p>
<div class="dl6-grid dl6-legacy-grid">
<section class="dl6-card"><div class="dl6-card-head"><h3>Windows</h3><span>10 / 11 · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win_cpu.zip"><span>CPU</span><small>Hou 2025.04.16</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win.zip"><span>NVIDIA GPU</span><small>Hou 2025.04.16</small></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>macOS</h3><span>Apple Silicon / Intel</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-14-arm64_qt6.zip"><span>Apple Silicon</span><small>macOS 14 ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-15-intel_qt6.zip"><span>Intel</span><small>macOS 15 x64</small></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>Ubuntu x64</h3><span>20.04 / 22.04 / 24.04</span></div><div class="dl6-buttons"></div><details class="dl6-extra"><summary>Select Ubuntu version</summary><div class="dl6-extra-body"><p class="dl6-cluster-label">Ubuntu x64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004_cpu.zip"><span>Ubuntu 20.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004.zip"><span>Ubuntu 20.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu.zip"><span>Ubuntu 22.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204.zip"><span>Ubuntu 22.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu.zip"><span>Ubuntu 24.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404.zip"><span>Ubuntu 24.04 NVIDIA</span><small>x64</small></a></div></div></details></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>Ubuntu ARM64</h3><span>22.04 / 24.04</span></div><div class="dl6-buttons"></div><details class="dl6-extra"><summary>Select Ubuntu version</summary><div class="dl6-extra-body"><p class="dl6-cluster-label">Ubuntu ARM64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu_arm64.zip"><span>Ubuntu 22.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_arm64.zip"><span>Ubuntu 22.04 NVIDIA</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu_arm64.zip"><span>Ubuntu 24.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_arm64.zip"><span>Ubuntu 24.04 NVIDIA</span><small>ARM64</small></a></div></div></details></section>
</div>
<p>China mirror (Hou only): <a href="https://pan.baidu.com/s/5GuYBQbLHTN_HvShnM3oQew">百度网盘</a>.</p>
</details>

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
