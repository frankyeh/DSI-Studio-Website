# Reconstruction

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=215" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

The reconstruction step processes an SRC file from Step T1 to generate the FIB file, which can be used for advanced analyses like tractography or connectometry.

---

## Step T2: Open SRC File(s)

Click on the [**Step T2 Reconstruction**] button in the main project window to select one or multiple SRC files.

DSI Studio will present a reconstruction window as shown below:

![Reconstruction Window](https://user-images.githubusercontent.com/275569/147804658-3d2b3442-c0dd-4383-91cf-3718670b1413.png)

*Tip: You can batch-reconstruct multiple SRC files. Any preprocessing (e.g., flipping images or aligning to MNI space) applies uniformly to all selected files.*

---

## Visual Quality Inspection (Optional)

Switch to the [**Source Images**] tab and visually inspect the data for the following issues:

### 1. Eddy Current Distortion and Motion
- Use the keyboard's arrow keys to scroll through the b-table list to check for distortions or motion artifacts.

### 2. Bad Slices
- Click on the [**Show bad slices**] button to highlight bad slices in red. Use sagittal view for better visibility.

![Bad Slices](https://user-images.githubusercontent.com/275569/147804666-b75d4167-ce90-4722-816e-a3106046f6f0.png)

### 3. Other Problems
- Refer to the [video tutorial](https://www.youtube.com/watch?v=stL4GMeTC1I) for detailed guidance.

---

## Step T2a: Specify a Mask

A mask eliminates background signals, improving reconstruction efficacy. The mask selection window includes tools for:

- **Thresholding**: Generates an initial selection.
- **Smoothing**: Smooths the mask contour.
- **Expansion**: Expands the mask.
- **Erosion**: Shrinks the mask.
- **Defragment**: Removes small fragments.

Recommendation: Use thresholding, smoothing, and defragmentation in sequence.

Masks can be saved/loaded as `.txt` or `.nii.gz` files.

---

## Preprocessing Steps

Follow these steps in order to preprocess DWI data effectively:

### 1. TOPUP/EDDY/Motion Correction

#### For Reverse-Phase Encoding Acquisition:
1. Click [Corrections][TOPUP/EDDY].
2. Select the reverse-phase encoding file (e.g., `*.nii.gz` or `*.src.gz`).
3. DSI Studio will call FSL's `topup` and `eddy` tools for artifact correction.

#### Without Reverse-Phase Encoding:
1. Click [Corrections][EDDY].
2. FSL's `eddy` will correct for eddy current distortions.

> **Note:** This step can take hours. Save a new SRC/SZ file after corrections.

---

### 2. Correct Image Orientations (Animal Scans Only)

Animal scans often require orientation corrections. Use [Corrections][Volume Orientation Correction] to align the volume with the template.

Default template orientations for mice, marmosets, or rhesus are shown below:

![Template Orientation](https://user-images.githubusercontent.com/275569/149644623-ee22e1d3-d8a6-4650-b2ae-ce10d93e11f2.png)

If necessary, use [**Edit**][**Image Flip**] or [**Image Swap**] to match the template orientation.

---

### 3. Remove Background Signals or Crop Image Volume (Optional)

Eliminate background noise using [**Edit**][**Erase Background Signals**] and crop the volume using [**Edit**][**Crop Background**].

---

### 4. Check b-Table Orientation (Animal Scans)

Incorrect b-tables lead to flawed fiber orientations. Use [B-table][Check b-table] to verify b-table orientation. For low SNR datasets, manually test flip/swap configurations.

---

### 5. Make Isotropic (Optional)

Isotropic resolution is crucial for fiber tracking. Use [**Edit**][**Make Isotropic**] to interpolate non-isotropic data.

---

## Step T2b: Specify a Model

### Diffusion Tensor Imaging (DTI)
- Generates a single fiber orientation per voxel along with anisotropy and diffusivity measures.
- Use only if GQI fails.

### Generalized Q-Sampling Imaging (GQI) *(Recommended)*
- Model-free method for resolving fiber orientations.
- Optimize the diffusion sampling length ratio (`L`) using the following steps:
  1. Reconstruct FIB files with different `L` values (e.g., `0.3`, `0.4`, ..., `2.0`).
  2. Inspect crossing and non-crossing regions (e.g., corpus callosum) in [Step T3 Fiber Tracking].

The goal: **Select the highest `L` that minimizes spurious fibers.**

---

### Q-Space Diffeomorphic Reconstruction (QSDR)
- MNI-aligned version of GQI.
- Recommended for analyses in MNI space (e.g., connectometry).
- Ensure your data matches the selected template (e.g., human, monkey, rat).

To transform additional modalities (e.g., T1W) into MNI space, use the [**Attach Images...**] button.

---

## Step T2c: Specify Outputs

Specify output metrics (comma-separated) for the FIB file:

- `fa`: Fractional anisotropy
- `rd`: Radial diffusivity
- `md`: Mean diffusivity
- `tensor`: Tensor matrix (e.g., `txx`, `txy`, `txz`)
- `gfa`: Generalized fractional anisotropy
- `rdi`: Restricted diffusion
- `odf`: Export entire ODF vectors (increases `.fib` file size).

### Advanced Options
- **Ignore High b for DTI**: Use only b-values <1500 for tensor estimation.

---

This documentation captures all key features and workflows for Step T2 Reconstruction in DSI Studio. Let me know if further revisions are needed!
