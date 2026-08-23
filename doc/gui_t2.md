# Reconstruction

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=215" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Reconstruction processes an SRC file (`.sz`; legacy `.src.gz`) and creates an FIB file (`.fz`) for tractography, diffusion measurements, connectome analysis, and other downstream workflows.

## Step T2: Open SRC File(s)

Click **[Step T2 Reconstruction]** in the main window and select one or more SRC files.

![Reconstruction Window](https://user-images.githubusercontent.com/275569/147804658-3d2b3442-c0dd-4383-91cf-3718670b1413.png)

Multiple SRC files can be reconstructed as a batch. Processing steps configured on the first file can be applied to the remaining selected files.

## Visual quality inspection

Before reconstruction, inspect the raw DWI/source-image view and b-table for obvious problems.

### Motion and distortion

Use the b-table/image list to move through the diffusion volumes and look for motion, distortion, signal dropout, or inconsistent slices.

### Bad slices

Use **Show bad slice** to highlight detected problematic slices. Sagittal and coronal views can make slice artifacts easier to identify.

![Bad Slices](https://user-images.githubusercontent.com/275569/147804666-b75d4167-ce90-4722-816e-a3106046f6f0.png)

For additional examples, see the [quality-control video](https://www.youtube.com/watch?v=stL4GMeTC1I).

## Step T2a: Specify a mask

The reconstruction mask limits processing to the tissue of interest. The mask editor provides thresholding and common morphological operations including smoothing, expansion, erosion, and defragmentation.

A typical starting sequence is **Threshold → Smoothing → Defragment**, followed by visual inspection and manual correction if needed.

Masks can be saved or loaded as text/NIfTI files.

## Corrections and preprocessing

Only apply corrections that are appropriate for the acquisition and study design. Keep the processing consistent across subjects in a group analysis.

### TOPUP/EDDY and motion correction

For reverse-phase-encoding acquisitions, use **[Corrections][TOPUP/EDDY]**. DSI Studio first looks for the matching `.rz` companion file. If one is not found, it can accept another reverse-phase-encoding `.sz`, `.rz`, legacy `.src.gz`, or NIfTI file.

For acquisitions without a reverse-phase pair, use the applicable EDDY/motion-correction workflow.

These corrections may take substantial processing time. Save the corrected source data as a new `.sz` file if you want to preserve the corrected dataset for later reconstruction.

### Volume orientation correction for animal scans

Animal datasets may need orientation adjustment before template-based analysis. Use **Corrections → Volume Orientation Correction** to align the volume orientation with the selected template.

<img width="800" alt="template orientation image" src="https://github.com/user-attachments/assets/d64d99dc-33f9-45ec-ad84-1431473bf6ab" />

When the slice-position slider moves toward the superior end of the volume, the displayed anatomy should follow the orientation expected by the selected template. Use image flip/swap operations only when necessary, and verify the result visually after each change.

### Remove background or crop the image volume

Use the image-editing tools to remove irrelevant background signal or crop excessive empty space when appropriate.

### Check b-table orientation

An incorrect b-table produces incorrect fiber orientations. Use **[B-table][Check b-table]** and inspect the result anatomically. Low-SNR or unusual animal acquisitions may require manual verification.

### Make isotropic

If needed, resample the source volume to isotropic resolution before reconstruction. Resampling does not add anatomical information, so use it for a specific processing or tracking requirement rather than routinely.

## Step T2b: Specify a reconstruction method

### Diffusion Tensor Imaging (DTI)

DTI estimates a diffusion tensor and tensor-derived measures such as FA, MD, AD, and RD. It provides one principal tensor direction per voxel and is appropriate when tensor measurements are the intended analysis.

### Generalized Q-Sampling Imaging (GQI)

GQI is the standard DSI Studio reconstruction for resolving one or more fiber orientations and is commonly used for native-space tractography.

The **diffusion sampling length ratio** controls GQI sampling. The appropriate value depends on acquisition and tissue properties. When changing it, verify the resulting fiber orientations and whole-brain tractography rather than selecting a value from the appearance of a single pathway.

### Q-Space Diffeomorphic Reconstruction (QSDR)

QSDR reconstructs diffusion information in a selected template space and is commonly used for population, atlas, and connectometry analyses.

Select the template appropriate for the species and study. Inspect registration quality before including a subject in population analysis.

Additional images can be attached when they are needed by the reconstruction/template workflow.

## Step T2c: Specify outputs

Select only the additional diffusion measurements needed for the analysis. Available outputs depend on the reconstruction method and DSI Studio version and can include tensor measures, GQI-derived measures, restricted-diffusion measures, and ODF-related outputs.

Saving unnecessary outputs increases `.fz` file size and processing time.

### DTI high-b option

**Ignore High b for DTI** restricts tensor estimation to lower b-values when appropriate for the acquisition. Use the same rule across subjects in a group study.

## Reconstruct

After checking the mask, corrections, reconstruction method, template settings, and requested outputs, start reconstruction. The resulting `.fz` file can be opened in **Step T3: Fiber Tracking** or used by the corresponding command-line and population-analysis workflows.

For command-line reconstruction, see [Reconstruction CLI](/doc/cli_t2.html).
