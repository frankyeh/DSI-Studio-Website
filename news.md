
## Recent Update Log 

April 2026
- Major code revision to improve efficiency
- Adding UNet segmentation models for T1W/T2W structure images
- Revise atlas and template saving location

March 2026
- Revise autotrack function: reduce default TIP iteration count, adding debug features
- Adding comprehensive tract-to-region connectome quantification set

November 2025
- Fix a bug in tract statistics: the 1st and 4th quarter volume of the tracts reported in tract statistics may sometimes be flipped. This bug affect versions before 11/6/2025.
- Improve robustness and efficiency of image registration, adding more search count and estimate initial parameters
-
 
October 2025
- always apply bias field correction before reconstruction
- Improved mask generation at Step T2
- Fix registration bug when DWI has a large FOV.
- Fix export slice image to mni bug in the GUI
- Major revision on connectivity matrix computation. DSI Studio now support T2R results for all metrics.

September 2025

- revive --action=vis
- provide "vol" metrics
- fix a bug in computing "span" of the tracts. previously it computes only the voxel distance.

August 2025

- The Spearman correlation in correlational tractography updated to consider tie conditions. This should improve p-value.
- Julich Brain atlas updated to 3.1
- connectivity matrix in command line can output shape metrics and diffusion metrics at in one command specify --connectivity_value=all
- fix a bug in autotrack that failed to map pathway in large FOV scans

## Issues

Please check if your version has following issues and update accordingly

- [versions >= 4/15/2026 <= 4/24/2026] (MAJOR ISSUE) GPU-based nonlinear registration did not run correctly due to an error introduced during code revision.
- [versions >= 11/10/2025 <= 2/17/2026] (MAJOR ISSUE) NIFTI images displayed in the tracking window may appear flipped due to a bug in reading the NIFTI header.
- [versions >= 8/8/2025 <= 8/11/2025] A bug affected automatic fiber tracking and QSDR reconstruction, causing them to fail.
- [versions >= 6/4/2025 <= 6/20/2025] A bug may cause crashes when making SRC files isotropic. Another bug may create corrupted .sz and .fz files when saving. Any affected .sz and .fz files should be removed.
- [versions >= 4/15/2025 < 5/6/2025] Converting DICOM files to NIfTI may create left-right mirrored T1w images.
- [versions >= 2/1/2025 <= 3/10/2025] Autotract may produce incorrect results due to a normalization error.
- [versions < 5/1/2025] Bug fixes: (1) medial lemniscus tracts were incorrectly treated as MNI-space tracts, (2) a multithreading error may cause incorrect tract statistics in the connectometry database, and (3) a bug may cause alignment problems between .tck files and structural images.

## Past Updates

June 2025

- provide qa-iso ratio (QIR) in fiber tracking window
- support access to NDA restricted dataset using registery entity
- provide T1w tissue segmentation function at CLI
- allow using T2w to correct for phase distortion
- fix T1w normalization issues
- fix manual atlas normalization crash bug

May 2025

- add issue checking routine to alert users
- introduce multi-metrics database .dz format

April 2025

- add effect size funcion correlational tractography
- improved multi-thread efficiency
- improved differential tractography pipeline
- allow saving and loading empty tracts
- add bias field correction function
- add fib average function for creating population templates
- adding command history function

March 2025
- introduce new termination count system using tract-to-voxel ratio.
- built-in neonate template updated.

January 2025

- update t1w t2w to 0.5mm for icbm152 human
- enhance linear registration accuracy. 
- GUI: error message when loading template fz
- GUI: fix long text issue in tract or region statistics in Mac OS.


