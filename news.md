
## Update log 

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

- [versions >= 6/10/2025 < 6/15/2025] A bug in saving file that creates corrupted .sz and .fz files. (also need to remove .sz .fz files)
- [versions >= 4/15/2025 < 5/6/2025] Converting DICOM to Nifti file may create left-right mirrow T1w images
- [versions < 2/1/2025] DSI Studio treats MedialLemniscus tracts as an mni space tract
- [versions < 2/1/2025] bug fix: an error in multi-thread may cause estimation of tract statistics errors in connectometry db.
- [versions >= 2/1/2025 <= 3/10/2025 ] autotract may give incorrect results
- [versions < 5/1/2025] DSI Studio cannot align tck with structure images correctly
