## Recent Update Log

July 2026

- Released the **Sun** generation of DSI Studio. The current release is **2026.7.25**.
- Added **AI Agent integration** for Codex and Claude Code. The agent can inspect the current DSI Studio state and operate workflows such as reconstruction, fiber tracking, image processing, and connectometry from natural-language requests.

May 2026

- Added U-Net based segmentation in DSI Studio for structural MRI.
- Added support for compact segmentation models for T1w/T2w images, allowing users to run brain MRI segmentation directly in DSI Studio without installing Python, PyTorch, or external segmentation packages.
- Improved the segmentation workflow so that model selection, inference, and output label generation can be handled within the DSI Studio interface.

April 2026

- Major code revision to improve computational efficiency.
- Added U-Net segmentation models for T1w/T2w structural images.
- Revised atlas and template storage locations.

March 2026

- Revised the automatic fiber tracking function by reducing the default TIP iteration count and adding debugging features.
- Added a comprehensive set of tract-to-region connectome quantification outputs.

## Issues

Please check whether your version is affected by the following issues and update accordingly.

- [versions >= 08/08/2025 <= 08/11/2025] A bug affected automatic fiber tracking and QSDR reconstruction, causing them to fail.
- [versions >= 06/04/2025 <= 06/20/2025] A bug may cause crashes when making SRC files isotropic. Another bug may create corrupted `.sz` and `.fz` files when saving. Any affected `.sz` and `.fz` files should be removed.
- [versions >= 04/15/2025 < 05/06/2025] Converting DICOM files to NIfTI may create left-right mirrored T1w images.
- [versions >= 02/01/2025 <= 03/10/2025] AutoTrack may produce incorrect results due to a normalization error.
- [versions < 05/01/2025] Bug fixes include: (1) medial lemniscus tracts were incorrectly treated as MNI-space tracts, (2) a multithreading error may cause incorrect tract statistics in the connectometry database, and (3) a bug may cause alignment problems between `.tck` files and structural images.

## Past Updates

2025

- Added issue checking, the multi-metric database `.dz` format, QA-ISO ratio (QIR), command-line T1w tissue segmentation, and support for NDA restricted datasets.
- Improved differential tractography, connectivity matrix computation, image registration, mask generation, multithreading efficiency, FIB averaging, bias field correction, and phase-distortion correction using T2w images.
- Updated correlational tractography p-value estimation, the Julich Brain atlas, built-in neonate template, and ICBM152 T1w/T2w templates.
- Added command history, empty-tract save/load support, expanded tract-to-region metrics, and restored `--action=vis`.

2023-2024

- Introduced the Hou generation of DSI Studio with new `.fz`/`.sz` formats, the DSI Studio Data Hub, updated registration tools, and a revised GUI.
- Updated tractography atlas organization and automated fiber tracking support for multiple tractography atlases.

2020-2022

- Major revision of QA calculation; nQA replaced QA.
