
## Recent Update Log

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

November 2025

- Fixed a bug in tract statistics: the first- and fourth-quartile tract volumes reported in tract statistics may sometimes be flipped. This bug affects versions before 11/6/2025.
- Improved the robustness and efficiency of image registration by adding more search counts and better initial parameter estimation.

October 2025

- Bias field correction is now always applied before reconstruction.
- Improved mask generation at Step T2.
- Fixed a registration bug when DWI has a large field of view.
- Fixed a GUI bug when exporting slice images to MNI space.
- Major revision of connectivity matrix computation. DSI Studio now supports tract-to-region results for all metrics.

September 2025

- Restored `--action=vis`.
- Added the `vol` metric.
- Fixed a bug in computing the `span` of tracts. Previously, it only computed voxel distance.

August 2025

- Updated Spearman correlation in correlational tractography to handle tied ranks, improving p-value estimation.
- Updated the Julich Brain atlas to version 3.1.
- Added command-line support for outputting shape metrics and diffusion metrics in one connectivity matrix command using `--connectivity_value=all`.
- Fixed a bug in automatic fiber tracking that failed to map pathways in large-FOV scans.

## Issues

Please check whether your version is affected by the following issues and update accordingly.

- [versions >= 4/15/2026 <= 4/24/2026] **Major issue:** GPU-based nonlinear registration did not run correctly due to an error introduced during code revision.
- [versions >= 11/10/2025 <= 2/17/2026] **Major issue:** NIfTI images displayed in the tracking window may appear flipped due to a bug in reading the NIfTI header.
- [versions >= 8/8/2025 <= 8/11/2025] A bug affected automatic fiber tracking and QSDR reconstruction, causing them to fail.
- [versions >= 6/4/2025 <= 6/20/2025] A bug may cause crashes when making SRC files isotropic. Another bug may create corrupted `.sz` and `.fz` files when saving. Any affected `.sz` and `.fz` files should be removed.
- [versions >= 4/15/2025 < 5/6/2025] Converting DICOM files to NIfTI may create left-right mirrored T1w images.
- [versions >= 2/1/2025 <= 3/10/2025] Autotrack may produce incorrect results due to a normalization error.
- [versions < 5/1/2025] Bug fixes include: (1) medial lemniscus tracts were incorrectly treated as MNI-space tracts, (2) a multithreading error may cause incorrect tract statistics in the connectometry database, and (3) a bug may cause alignment problems between `.tck` files and structural images.

## Past Updates

June 2025

- Added the QA-ISO ratio (QIR) in the fiber tracking window.
- Added support for access to NDA restricted datasets using a registry entity.
- Added T1w tissue segmentation in the command line interface.
- Added support for using T2w images to correct phase distortion.
- Fixed T1w normalization issues.
- Fixed a crash in manual atlas normalization.

May 2025

- Added an issue-checking routine to alert users about known affected versions.
- Introduced the multi-metric database `.dz` format.

April 2025

- Added effect size calculation in correlational tractography.
- Improved multithreading efficiency.
- Improved the differential tractography pipeline.
- Added support for saving and loading empty tracts.
- Added bias field correction.
- Added FIB averaging for creating population templates.
- Added command history.

March 2025

- Introduced a new termination count system using the tract-to-voxel ratio.
- Updated the built-in neonate template.

January 2025

- Updated ICBM152 human T1w/T2w templates to 0.5 mm resolution.
- Enhanced linear registration accuracy.
- GUI: added an error message when loading a template `.fz` file.
- GUI: fixed long-text display issues in tract and region statistics on macOS.

