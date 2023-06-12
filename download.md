DSI Studio has a high version turning rate, and occasionally the computation outcome may be different between versions. I would recommend  keepping a local copy of DSI Studio for each research project and update DSI Studio every time if a new project is initiated.

# Packages

[![Build Release](https://github.com/frankyeh/DSI-Studio/actions/workflows/build_dsistudio.yml/badge.svg)](https://github.com/frankyeh/DSI-Studio/actions/workflows/build_release.yml)<a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio"></a><a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio"></a>

## DSI Studio

***[NOTICE (2020/08/02): Major revision on QA after August 2nd, 2022](https://groups.google.com/g/dsi-studio/c/t-kSFxXrGFU)***

***Download and unzip to run the executive. No installation needed***

| OS      | File     | Note      |
|---------|----------|-----------|
|  Windows (7+)  |  [GPU version (recommended)](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_win.zip)<br> [CPU version (if GPU does not work)](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_win_cpu.zip)<br> Unzip the file and click on the DSI Studio program to run it. | If missing DLL files, install the [VC package](https://aka.ms/vs/17/release/vc_redist.x64.exe).<br>To use GPU, install [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exe_network) and update your graphic drivers.|
|  Mac (11+)      |  [dsi_studio_mac-11.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_macos-11.zip)<br>[dsi_studio_mac-12.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_macos-12.zip)<br>To run it, you need to [enable permission](http://mac-how-to.wonderhowto.com/how-to/open-third-party-apps-from-unidentified-developers-mac-os-x-0158095/). |  |
|  Ubuntu (16.04+)   |  [dsi_studio_ubuntu1604.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_ubuntu1604.zip)<br>[dsi_studio_ubuntu1804.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_ubuntu1804.zip)<br>[dsi_studio_ubuntu2004.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_ubuntu2004.zip)<br>[dsi_studio_ubuntu2204.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_ubuntu2204.zip)<br> | If showing error related to *libQt6Charts*, run `sudo apt install libqt6charts6-dev` |
|  CentOS (7)   |  [dsi_studio_centos7.zip](https://github.com/frankyeh/DSI-Studio/releases/download/2022.08.03/dsi_studio_centos7.zip)<br> | |

*[previous versions](https://www.dropbox.com/sh/ectib64vhctkl8b/AADBRYp_aPLEuAOdNw393tO-a?dl=0)*

# Containers

**Docker**

```
docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $PWD:/data dsistudio/dsistudio:latest
```

**Singularity**
     
```
$ singularity pull docker://dsistudio/dsistudio:latest     # This creates a singularity container *.sif file from the latest release of DSI Studio. Make sure you keep a copy of .sif file for your results
$ singularity exec -B /var,/run -B /home/server/folder dsistudio_latest.sif dsi_studio  # Some cluster does not allow users to access host drive, and you may need to mount folder into singularity container
$ singularity exec dsistudio_latest.sif dsi_studio   #  This invoke the graphic interface of DSI Studio 
$ singularity exec dsistudio_latest.sif dsi_studio --action=rec --source=my.src.gz # call DSI Studio command line interface in the singularity container  
```

# License

DSI Studio is dual licensed. You can pick one license or the other. 

## Academic Usge

DSI Studio is free for academic or research purposes under [Attribution-***NonCommercial***-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode) license. 

## Commercial License

For commercial use, the DSI Studio software and source code require a paid license. It is important to note that the paid license does not cover third-party tools, resources, or patents owned by other entities, such as: 

1. [TinyFSL](https://github.com/frankyeh/TinyFSL) is free for academic use under [FSL License](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Licence).
2. [Alases](https://github.com/frankyeh/DSI-Studio-atlas): each atlas includes its own license or usage agreement. Most of them are free for both academic and commercial usage/
3. Certain algorithms and functions incorporated within DSI Studio may be protected by patents. Therefore, it is crucial to be aware of potential patent restrictions when using DSI Studio for commercial purposes. To avoid possible copyright or patent infringement, please consult Frank Yeh about the details.

Depending on the functions and resources used, the commercial use may require licenses from different parties. 
To obtain the DSI Studio license for commercial use, please email frank.yeh@gmail.com. 

# Hardware recommendation:

To achieve the best performance, I would recommend a computer with a multi-core CPU (e.g. http://store.hp.com/us/en/mdp/business-solutions/z840-workstation). Minimum memory of 16GB is needed. To handle a large number of tracks or image volumes, DSI Studio will need 32GB of memory or more. 

