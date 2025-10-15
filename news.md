
## Update log 

October 2025
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

## Issues

Please check if your version has following issues and update accordingly

- [versions >= 8/8/2025 <= 8/11/2025] bug in automatic fiber tracking and qsdr reconstruction causing it not working
- [versions >= 6/4/2025 <= 6/20/2025] a bug causing a crash when making SRC files isotropic and a bug in saving file that creates corrupted .sz and .fz files. (also need to remove .sz .fz files)
- [versions >= 4/15/2025 < 5/6/2025] converting DICOM to Nifti file may create left-right mirrow T1w images
- [versions >= 2/1/2025 <= 3/10/2025] autotract may give incorrect results due to normalization error
- [versions < 5/1/2025] bug fix: (1) medialLemniscus tracts was wrongly treated as an mni space tract, (2) an error in multi-thread may cause estimation of tract statistics errors in connectometry db, (3) a abug causing alignment problem in tck with structure images. 
