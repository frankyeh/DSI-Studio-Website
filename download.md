
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio" alt="Latest source commit"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio" alt="Latest GitHub release"></a>

# Download DSI Studio

<details class="dl6-notes"><summary>Versioning and reproducibility</summary><p>DSI Studio is updated frequently, and computational results may differ between versions. For reproducibility, keep a local copy of the DSI Studio version used for each research project. It is usually best to update DSI Studio when starting a new project, not in the middle of an ongoing analysis.</p></details>

## "Sun" Versions (2026-) — Recommended

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
article .dl6-extra-body pre{margin:0 0 8px;font-size:12px;white-space:pre-wrap;overflow-wrap:anywhere}
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
<section class="dl6-card" id="get-windows"><div class="dl6-card-head"><h3>Windows</h3><span>10 / 11 · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_cpu.zip"><span>CPU</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win.zip"><span>NVIDIA GPU</span><small>Windows x64 · CUDA</small></a></div><details class="dl6-extra"><summary>Windows ARM64</summary><div class="dl6-extra-body"><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_win_arm64.zip"><span>ARM64 CPU</span><small>Windows 11 on ARM</small></a></div></div></details><details class="dl6-extra"><summary>Troubleshooting</summary><div class="dl6-extra-body"><p>If DLL files are missing, install the <a href="https://aka.ms/vs/17/release/vc_redist.x64.exe">Microsoft Visual C++ Redistributable</a>. GPU builds need a compatible NVIDIA driver. The CUDA runtime is statically linked; you do not need the CUDA Toolkit.</p></div></details></section>
<section class="dl6-card" id="get-macos"><div class="dl6-card-head"><h3>macOS</h3><span>Apple Silicon / Intel</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-arm64_qt6.zip"><span>Apple Silicon</span><small>M-series Macs</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_macos-intel_qt6.zip"><span>Intel</span><small>x64 Macs</small></a></div><details class="dl6-extra"><summary>Troubleshooting</summary><div class="dl6-extra-body"><p>If Gatekeeper blocks an older build, try the current signed release first. For an older copy you trust, remove quarantine using:</p><pre><code>xattr -rd com.apple.quarantine /path/to/dsi_studio.app</code></pre><p>If the file-open dialog crashes with a cloud-drive file, copy the data to a local folder first.</p></div></details></section>
<section class="dl6-card" id="get-linux"><div class="dl6-card-head"><h3>Linux</h3><span>Universal · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu.zip"><span>CPU</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda.zip"><span>NVIDIA GPU</span></a></div><details class="dl6-extra"><summary>ARM64, older Linux and Ubuntu builds</summary><div class="dl6-extra-body"><p class="dl6-cluster-label">Universal ARM64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cpu_arm64.zip"><span>ARM64 CPU</span><small>Universal</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_cuda_arm64.zip"><span>ARM64 NVIDIA</span><small>Universal</small></a></div><p class="dl6-cluster-label">Older Linux</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_linux_universal_legacy_cpu.zip"><span>Legacy CPU</span><small>x64 · older distributions</small></a></div><p class="dl6-cluster-label">Ubuntu x64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804_cpu.zip"><span>Ubuntu 18.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu1804.zip"><span>Ubuntu 18.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004_cpu.zip"><span>Ubuntu 20.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2004.zip"><span>Ubuntu 20.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu.zip"><span>Ubuntu 22.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204.zip"><span>Ubuntu 22.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu.zip"><span>Ubuntu 24.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404.zip"><span>Ubuntu 24.04 NVIDIA</span><small>x64</small></a></div><p class="dl6-cluster-label">Ubuntu ARM64</p><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_cpu_arm64.zip"><span>Ubuntu 22.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2204_arm64.zip"><span>Ubuntu 22.04 NVIDIA</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_cpu_arm64.zip"><span>Ubuntu 24.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/latest/download/dsi_studio_ubuntu2404_arm64.zip"><span>Ubuntu 24.04 NVIDIA</span><small>ARM64</small></a></div><p>Universal supports Ubuntu 18.04–26.04, Debian 11–13, Rocky/AlmaLinux 8–10 and openSUSE 15.6. Legacy CPU supports older Linux distributions including Ubuntu 16.04, Debian 9–10, CentOS 7 and Amazon Linux 2.</p></div></details><details class="dl6-extra"><summary>Troubleshooting</summary><div class="dl6-extra-body"><p>For Ubuntu builds missing <code>libQt6Charts</code>:</p><pre><code>sudo apt install libqt6charts6-dev</code></pre><p>For <code>xcb</code> errors, see the <a href="https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI">forum solution</a>.</p></div></details></section>
<section class="dl6-card" id="get-docker"><div class="dl6-card-head"><h3>Docker</h3><span>x86_64 · containers</span></div><pre>docker pull dsistudio/dsistudio:latest</pre><pre>docker pull dsistudio/dsistudio:cuda</pre><p class="dl6-hint">Rolling Sun tags: <code>:latest</code> CPU · <code>:cuda</code> NVIDIA GPU.</p></section>
</div>
<details class="dl6-notes"><summary>Release details, AI Agent and architecture notes</summary>
<p>Sun includes AI Agent workflows through supported local CLI agents including Codex, Claude Code, Muse, Antigravity and Grok. Configure a supported agent locally and check availability in DSI Studio's AI Agent settings.</p>
<p>Linux Universal packages are recommended for most systems.</p>
</details>

### Troubleshooting

Troubleshooting instructions are available in the Windows, macOS and Linux download cards above.

## "Hou" Versions (2024-2026) — Legacy

<details class="dl6-legacy"><summary>Hou 2025.04.16 — archived downloads and compatibility</summary>
<p>Hou introduced compact <code>*.fz</code>/<code>*.sz</code> formats, Data Hub, registration tools and a redesigned GUI. The pinned 2025.04.16 builds predate <code>.dz</code> database support; use current Sun for creating <code>.dz</code> databases.</p>
<div class="dl6-grid dl6-legacy-grid">
<section class="dl6-card"><div class="dl6-card-head"><h3>Windows</h3><span>10 / 11 · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win_cpu.zip"><span>CPU</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_win.zip"><span>NVIDIA GPU</span></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>macOS</h3><span>Apple Silicon / Intel</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-14-arm64_qt6.zip"><span>Apple Silicon</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_macos-15-intel_qt6.zip"><span>Intel</span></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>Ubuntu x64</h3><span>20.04 / 22.04 / 24.04</span></div><div class="dl6-buttons"></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004_cpu.zip"><span>Ubuntu 20.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2004.zip"><span>Ubuntu 20.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu.zip"><span>Ubuntu 22.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204.zip"><span>Ubuntu 22.04 NVIDIA</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu.zip"><span>Ubuntu 24.04 CPU</span><small>x64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404.zip"><span>Ubuntu 24.04 NVIDIA</span><small>x64</small></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>Ubuntu ARM64</h3><span>22.04 / 24.04</span></div><div class="dl6-buttons"></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_cpu_arm64.zip"><span>Ubuntu 22.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2204_arm64.zip"><span>Ubuntu 22.04 NVIDIA</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_cpu_arm64.zip"><span>Ubuntu 24.04 CPU</span><small>ARM64</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_ubuntu2404_arm64.zip"><span>Ubuntu 24.04 NVIDIA</span><small>ARM64</small></a></div></section>
</div>
<p>China mirror (Hou only): <a href="https://pan.baidu.com/s/5GuYBQbLHTN_HvShnM3oQew">百度网盘</a>.</p>
</details>

## Previous Versions

<details class="dl6-legacy"><summary>Chen, Pre‑Chen and CentOS 8</summary>
<p><strong>Chen 2024.05.22 — archived downloads</strong></p>
<div class="dl6-grid dl6-legacy-grid">
<section class="dl6-card"><div class="dl6-card-head"><h3>Windows</h3><span>x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_win_cpu.zip"><span>CPU</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_win.zip"><span>NVIDIA GPU</span></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>macOS</h3><span>Archived builds</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_macos-13_qt6.zip"><span>macOS 13</span><small>Qt 6</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_macos-12_qt6.zip"><span>macOS 12</span><small>Qt 6</small></a></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_macos-11.zip"><span>macOS 11</span><small>Qt 5</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_macos-11_qt6.zip"><span>macOS 11</span><small>Qt 6</small></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_macos-12.zip"><span>macOS 12</span><small>Qt 5</small></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>Ubuntu</h3><span>16.04–22.04</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_ubuntu2204.zip"><span>Ubuntu 22.04</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_ubuntu2004.zip"><span>Ubuntu 20.04</span></a></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_ubuntu1804.zip"><span>Ubuntu 18.04</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_ubuntu1604.zip"><span>Ubuntu 16.04</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2023.12.06/dsi_studio_centos7.zip"><span>CentOS 7</span></a></div></section>
</div>
<p><a href="https://github.com/frankyeh/DSI-Studio/releases">Other Chen releases</a></p>
<div class="dl6-grid dl6-legacy-grid">
<section class="dl6-card"><div class="dl6-card-head"><h3>Pre-Chen</h3><span>2008–2022 · archive</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0"><span>Browse older builds</span><small>Dropbox archive</small></a></div></section>
<section class="dl6-card"><div class="dl6-card-head"><h3>CentOS 8</h3><span>Legacy Linux · x64</span></div><div class="dl6-buttons"><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2025.04.16/dsi_studio_centos8.zip"><span>Hou 2025.04.16</span></a><a class="dl6-btn" href="https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_centos8.zip"><span>Hou 2024.06.12</span></a></div></section>
</div>
</details>

## License



## Academic License

DSI Studio is free for academic users. Read the [academic license agreement](https://dsi-studio.labsolver.org/academic_license.html). DSI Studio is not FDA-cleared or FDA-approved. It is provided for research, education, and adjunct visualization. Any clinical use must be reviewed and approved under the user’s institutional policies. Results should not be used as the sole basis for diagnosis, treatment, or surgical decision-making.

## Commercial License

Contact frank.yeh@gmail.com.

## Hardware Recommendations

DSI Studio runs on a regular desktop or laptop. For large tractography or connectomics workflows, 64 GB RAM or more, a fast SSD, and a modern multi-core CPU are recommended. An NVIDIA GPU can accelerate GPU-enabled functions.
