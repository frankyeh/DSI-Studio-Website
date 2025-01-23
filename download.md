
[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio"></a>

# Download Links: "Hou" versions (2024-)

**Hou version is currently at the pre-release stage under frequent updates and testing**

Download and unzip to run the executive. No installation is needed. DSI Studio has a high version turning rate, and the computation outcome may differ between versions. I would recommend  keeping a local copy of DSI Studio for each research project and updating DSI Studio every time a new project is initiated.


| OS      | File     | Note      |
|---------|----------|-----------|
|  Windows (7+)  |  [GPU version (recommended)](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_win.zip)<br> [CPU version (if GPU does not work)](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_win_cpu.zip)| Unzip the file and click on the DSI Studio program to run it. <br> If missing DLL files, install the [VC package](https://aka.ms/vs/17/release/vc_redist.x64.exe).<br>To use GPU, install [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exe_network) and update your graphic drivers.|
|  Mac (13+)      |  [dsi_studio_mac-15.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_macos-15_qt6.zip)<br>[dsi_studio_mac-14.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_macos-14_qt6.zip)<br>[dsi_studio_mac-13.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_macos-13_qt6.zip) | make sure to [enable run permission](http://mac-how-to.wonderhowto.com/how-to/open-third-party-apps-from-unidentified-developers-mac-os-x-0158095/).<br> **On MacOS15, it may show the app is damaged (because it is not notarized, which requires $99 per year). Please right-click on dsi_studio.app and select [Open] to bypass Gatekeeper.** |
|  Ubuntu (20.04+)   |  [dsi_studio_ubuntu2004.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_ubuntu2004.zip)<br>[dsi_studio_ubuntu2204.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_ubuntu2204.zip)<br>[dsi_studio_ubuntu2404.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2024.06.12/dsi_studio_ubuntu2404.zip)<br> | If showing error related to *libQt6Charts*, run `sudo apt install libqt6charts6-dev`<br> If reporting error related to xcb, check out this [solution](https://groups.google.com/g/dsi-studio/c/b61uyoo0CuI). |
|  Container (Docker)   |  docker hub repository: `dsistudio/dsistudio:latest`  | `docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $PWD:/data dsistudio/dsistudio:latest` |

**Unique in Hou versions**

- 2024/08:
  - New FIB/SRC format (*.fz,*.sz): higher storage efficiency.
  - DSI Studio Data Hub: instant access to thousands of readily trackable data.
  - New linear and nonlinear registrations: support simultaneously multi-modality registration with greater accuracy dealing with lesions.
  - New GUI

# Previous Versions 

- ["Chen" versions (2022-2024)](https://github.com/frankyeh/DSI-Studio/releases)
- [Pre-"Chen" versions (2008-2022)](https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0)

**Major Changes**

- 2020/08/02: Major revision on QA calculation. nQA is now replacing QA (https://groups.google.com/g/dsi-studio/c/t-kSFxXrGFU)
- 2023/06/28: Fiber tracking results will change because the default step size = 0 has a different implementation. Older versions will randomly select between 0.5 and 1.5 voxel spacing. The updated version will have 1.0 voxel spacing. To replicate older versions, set the step size to -1 in the command line. Fiber tracking with nonzero step size and correlation tracking is not affected.
- 2023/07/08: Tractography atlas is further separated into 5 sets of pathways. The GUI and command line interface for automated fiber tracking has been modified. The updated DSI Studio allows for the use of multiple tractography atlases.
- 2023/10/02: The Otsu's threshold was updated to ignore zeros values in the background, as a result, the equivalent value will be slightly different if there are zeros in the background. The seeding region in automatic fiber tracking was updated to provide more comprehensive mapping. The previous version seeds within a more restricted region designated by the atlas and some branches may not be fully covered. The updated version will have a much larger seeding region to get better coverage.


# License

DSI Studio offers dual licensing options for academic and commercial users.

## Academic License

DSI Studio is free for academic users under [Attribution-***NonCommercial***-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode). View the full license agreement [here](https://github.com/frankyeh/DSI-Studio/?tab=License-1-ov-file#readme).

## Commercial License

[Purchasing the DSI Studio Commercial License](https://dsi-studio.labsolver.org/payment.html)

# Hardware recommendation:

Here is an example of a workstation for DSI Studio tractography:

1. Chassis: Dell Precision 7920 Tower
2. Processor: two CPUs of Intel Xeon Gold 6230 (2.1GHz, 3.9GHz Turbo, 20 Cores)
3. Memory: 128GB RAM
4. Graphics Card: NVidia Quadro RTX4000, 8GB
5. Hard drive: 2TB SSD
6. Operating System: Windows

