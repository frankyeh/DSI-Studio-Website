
## Update log 

** New diffusion index added: QIR (QA-to-Iso Ratio), which is the ratio between QA and ISO. We found this provide a much reliable replacement for QA in correlational tractography

April 2025

- add QIR (QA-to-Iso Ratio) as the default database metrics
- add effect size funcion correlational tractography
- improved multi-thread efficiency
- improved differential tractography pipeline
- fix a bug in aligning tck with structure images
- allow saving and loading empty tracts
- add bias field correction function
- add fib average function for creating population templates
- adding command history function

March 2025

- introduce new termination count system using tract-to-voxel ratio.
- fix a bug in autotract in versions between 02/01/2025 and 03/10/2025
- built-in neonate template updated.

January 2025

- update t1w t2w to 0.5mm for icbm152 human
- enhance linear registration accuracy. 
- bug fix: fix a bug that treats MedialLemniscus tracts as mni space tract. 
- GUI bug fix: error message when loading template fz
- bug fix: an error in multi-thread may cause estimation of tract statistics errors in connectometry db.
- GUI : fix long text issue in tract or region statistics in Mac OS.
- bug fix: DSI Studio fails to update image resolution when flip or swap image volume.

