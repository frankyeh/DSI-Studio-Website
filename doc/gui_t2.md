# Reconstruct: SRC to FIB

> **Before reconstruction:** Start with [Import: generate SRC files](/doc/gui_t1.html) to create an `.sz` file and review the diffusion images and b-table. Run [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control) before reconstructing a study cohort. This page begins with an existing SRC file and produces a tractography-ready `.fz` file.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=215" title="SRC reconstruction tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Reconstruction processes an SRC file (`.sz`; legacy `.src.gz`) and creates an FIB file (`.fz`) for tractography, diffusion measurements, connectome analysis, and other downstream workflows.

## Open SRC File(s)

In the current main window, choose <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Reconstruct</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">SRC File(s) (*.sz)</span></span> and select one or more SRC files. The <span class="gui-action">▸ More…</span> section contains <span class="gui-menu-path"><span class="gui-menu-item">Batch SRC</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">FIB</span></span> and <span class="gui-menu-path"><span class="gui-menu-item">SRC quality control...</span></span>. The reconstruction window opens after selecting SRC data. Older videos may show the previous main-window layout.

Multiple SRC files can be reconstructed as a batch. Processing steps configured on the first file can be applied to the remaining selected files.

## Reconstruction window menus

The reconstruction window has five top-level menus. Their actions operate on the loaded SRC image and its reconstruction settings; inspect changes before saving or reconstructing.

| Top menu | What it does |
|:--|:--|
| **File** | Saves the SRC dataset or exports 4D NIfTI, b0, and diffusion-weighted summary images. |
| **Edit** | Changes image orientation and geometry (flip/swap, AC–PC alignment, rotation, resampling), attaches other images, and offers signal/background or cropping operations. |
| **B-table** | Loads, saves, regenerates, checks, or flips/swaps diffusion-gradient directions. Incorrect changes alter estimated fiber orientations. |
| **Corrections** | Runs TOPUP/EDDY, T1w-guided distortion correction, motion/orientation correction, or bias-field correction as appropriate for the acquisition. |
| **QSDR** | Provides template-registration options, including manual alignment and partial-field-of-view controls for template-space reconstruction. |

These are the menus **inside the SRC reconstruction window**, not the main window's <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Reconstruct</span></span> entries.

## Visual quality inspection

Before reconstruction, inspect the raw DWI/source-image view and b-table for obvious problems.

### Motion and distortion

Use the b-table/image list to move through the diffusion volumes and look for motion, distortion, signal dropout, or inconsistent slices.

### Bad slices

Use <span class="gui-action">Show bad slice</span> to highlight detected problematic slices. Sagittal and coronal views can make slice artifacts easier to identify.



For additional examples, see the [quality-control video](https://www.youtube.com/watch?v=stL4GMeTC1I).

## Specify a mask

The reconstruction mask limits processing to the tissue of interest. The mask editor provides thresholding and common morphological operations including smoothing, expansion, erosion, and defragmentation.

A typical starting sequence is **Thresholding → Smoothing → Defragment**, followed by visual inspection and manual correction if needed.

Masks can be saved or loaded as text/NIfTI files.

## Corrections and preprocessing

Only apply corrections that are appropriate for the acquisition and study design. Keep the processing consistent across subjects in a group analysis.

### TOPUP/EDDY and motion correction

For reverse-phase-encoding acquisitions, use <span class="gui-menu-path"><span class="gui-menu-item">Corrections</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">TOPUP/EDDY</span></span>. DSI Studio first looks for the matching `.rz` companion file. If one is not found, it can accept another reverse-phase-encoding `.sz`, `.rz`, legacy `.src.gz`, or NIfTI file.

For acquisitions without a reverse-phase pair, use the applicable EDDY/motion-correction workflow.

### T1w distortion correction

When there is no reverse-phase pair but a T1w image of the same subject is available, use <span class="gui-menu-path"><span class="gui-menu-item">Corrections</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">T1w Distortion Correction...</span></span> to correct susceptibility distortion. Select the T1w file, then choose the output resolution (**Native**, **1.5 mm isotropic**, or **1.0 mm isotropic**).

DSI Studio converts the first b0 image to a T1-like contrast and extracts the brain from the T1w using U-Net, followed by bias field correction. It then registers the T1w rigidly to the b0 and estimates a nonlinear deformation restricted to the anterior–posterior (y) axis. Every DWI volume is warped once from the original data using T1w-guided interpolation and Jacobian signal modulation, and the mask is regenerated from the corrected images. The correction is recorded in the methods report.

The correction assumes AP–PA phase encoding. Run EDDY or motion correction before it, because the same deformation is applied to all volumes. The selected output resolution replaces a separate resampling step.

These corrections may take substantial processing time. Save the corrected source data as a new `.sz` file if you want to preserve the corrected dataset for later reconstruction.

### Volume orientation correction for animal scans

Animal datasets may need orientation adjustment before template-based analysis. Use <span class="gui-menu-path"><span class="gui-menu-item">Corrections</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Volume Orientation Correction</span></span> to align the volume orientation with the selected template.

<img width="800" alt="template orientation image" src="https://github.com/user-attachments/assets/d64d99dc-33f9-45ec-ad84-1431473bf6ab" />

When the slice-position slider moves toward the superior end of the volume, the displayed anatomy should follow the orientation expected by the selected template. Use image flip/swap operations only when necessary, and verify the result visually after each change.

### Remove background or crop the image volume

Use the image-editing tools to remove irrelevant background signal or crop excessive empty space when appropriate.

### Check b-table orientation

An incorrect b-table produces incorrect fiber orientations. Use <span class="gui-menu-path"><span class="gui-menu-item">B-table</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Check b-table</span></span> and inspect the result anatomically. Low-SNR or unusual animal acquisitions may require manual verification.

### Make isotropic

If needed, resample the source volume to isotropic resolution before reconstruction. Resampling does not add anatomical information, so use it for a specific processing or tracking requirement rather than routinely.

## Choose a reconstruction method

### Diffusion Tensor Imaging (DTI)

DTI estimates a diffusion tensor and tensor-derived measures such as FA, MD, AD, and RD. It provides one principal tensor direction per voxel and is appropriate when tensor measurements are the intended analysis.

### Generalized Q-Sampling Imaging (GQI)

GQI is the standard DSI Studio reconstruction for resolving one or more fiber orientations and is commonly used for native-space tractography.

The **diffusion sampling length ratio** controls GQI sampling. The appropriate value depends on acquisition and tissue properties. When changing it, verify the resulting fiber orientations and whole-brain tractography rather than selecting a value from the appearance of a single pathway.

### Q-Space Diffeomorphic Reconstruction (QSDR)

QSDR reconstructs diffusion information in a selected template space and is commonly used for population, atlas, and connectometry analyses.

Select the template appropriate for the species and study. Inspect registration quality before including a subject in population analysis.

Additional images can be attached when they are needed by the reconstruction/template workflow.

## Choose outputs

Select only the additional diffusion measurements needed for the analysis. Available outputs depend on the reconstruction method and DSI Studio version and can include tensor measures, GQI-derived measures, restricted-diffusion measures, and ODF-related outputs.

Saving unnecessary outputs increases `.fz` file size and processing time.

### DTI high-b option

<span class="gui-menu-path"><span class="gui-menu-item">Ignore High b for DTI</span></span> restricts tensor estimation to lower b-values when appropriate for the acquisition. Use the same rule across subjects in a group study.

## Batch reconstruction

Use <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Reconstruct</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">▸ More…</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Batch SRC</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">FIB</span></span> to process multiple `.sz` files. <span class="gui-menu-path"><span class="gui-menu-item">SRC quality control...</span></span> is in the same expanded section. Batch execution applies the configured reconstruction to a group of subjects, but it does not replace inspecting representative images and evaluating [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control) first.

The resulting reconstructed files use `.fz`. For population studies, use **QSDR with the same template and output resolution** across subjects before preparing a [correlational tractography database](/doc/gui_cx.html#create-a-connectometry-database). For longitudinal or cross-sectional differential analysis, see [Differential Tractography](/doc/gui_t3_dt.html).

## Reconstruct

After checking the mask, corrections, reconstruction method, template settings, and requested outputs, start reconstruction. The resulting `.fz` file can be opened via <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">3 Track</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">FIB File (*.fz)</span></span> or used by the corresponding command-line and population-analysis workflows.

For command-line reconstruction, see [Reconstruction CLI](/doc/cli_t2.html).
