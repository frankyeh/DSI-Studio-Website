# Step T2 Reconstruction

> Use `--action=rec` to perform diffusion reconstruction (e.g., GQI, DTI, QSDR).

This action reconstructs diffusion data (SRC/SZ format) into FIB files, with preprocessing corrections, masking, and registration options.

-----

## Examples

**1. Reconstruct all SRC files with GQI** using a 1.25 sampling length and write `.fz` files to `fib/`:

```bash
dsi_studio --action=rec --source=*.sz --method=4 --param=1.25 --output=fib/
```

**2. Reconstruct a single SRC file after EDDY correction**:

```bash
dsi_studio --action=rec --source=subject1.sz \
            --cmd=eddy \
            --method=4 --param=1.25
```

**3. Reconstruct after TOPUP+EDDY** using a reverse-PE b0 in a NIfTI file:

```bash
dsi_studio --action=rec --source=*.sz \
            --rev_pe=*_rev_b0.nii.gz \
            --method=4 --param=1.25
```

**4. Apply TOPUP+EDDY only (no reconstruction)** and save the corrected SRC:

```bash
dsi_studio --action=rec --source=raw_dwi.sz \
            --rev_pe=rev_b0.nii.gz \
            --save_src=raw_dwi_proc.sz
```

**5. Perform QSDR reconstruction with a 1.25 sampling ratio, warping both T1w and T2w images:**

```bash
dsi_studio --action=rec --source=subj1.sz \
            --method=7 \
            --param=1.25 \
            --other_image=t1w:my_t1w.nii.gz,t2w:my_t2w.nii.gz
```

**6. Bash loop to QSDR-reconstruct every `.sz` file with ODF output:**

```bash
#!/bin/bash
exec 1>log_qsdr.out 2>&1
method=7            # 7 = QSDR
param="1.25"
for sub in *.sz; do
  echo
  dsi_studio --action=rec \
              --source="$sub" \
              --method=$method \
              --param=$param \
              --odf_resolving=1
  echo
done
```

**7. B-table Quality Check and Manual Correction:**
Applying automatic b-table correction uniformly is not recommended when SNR is poor. Run QC first and inspect the result:

```bash
dsi_studio --action=qc --source=*.sz --check_btable=1
```

For `--action=qc`, `--check_btable=1` enables b-table checking, while the presence of `--template=<template>` determines whether the template-aware check is used. Without `--template`, QC uses the no-template check. This differs from `--action=rec`, where the numeric `--check_btable` value selects the reconstruction-time checking mode described below.

If a representative good-quality acquisition indicates a consistent swap/flip convention (for example `021fx`), apply the corresponding **b-table-only** correction to scans known to share that acquisition/export convention. For `021fx`, swap y and z to obtain `012`, then flip x:

```bash
dsi_studio --action=rec --source=subject.sz \
            --cmd="swap_bybz+flip_bx" \
            --method=4 --param=1.25
```

Do not infer a group-wide b-table correction from a low-SNR or artifact-heavy outlier.

-----

## Core Reconstruction Options

| **Option** | **Default** | **Description** |
|-----------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `--source`           | *(required)* | Path to the `.sz` file to reconstruct. Legacy `.src.gz` files are also supported. |
| `--method`           | Current/SRC setting | Reconstruction algorithm: `1`=DTI, `4`=GQI, `7`=QSDR. Specify it explicitly for reproducible command-line workflows. |
| `--param`            | Data/current setting | Diffusion sampling length ratio for GQI/QSDR. Common explicit values are `1.25` for in-vivo and `0.6` for ex-vivo applications. |
| `--odf_resolving`    | `0` | Enable ODF resolving during reconstruction. |
| `--template`         | Current template | Select the QSDR template. Use the template list reported by the current DSI Studio version. |
| `--qsdr_reso`        | Data dependent | Output resolution for QSDR. |
| `--r2_weighted`      | `0` | Use R²-weighted SDF estimation for GQI/QSDR. |
| `--other_output`     | Current reconstruction setting | Comma-separated diffusion metrics to compute; specify explicitly when a particular output set is required. |

-----

## Preprocessing Options

| **Option** | **Default** | **Description** |
|-----------------------|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `--rev_pe`           | *(none)* | Reverse-phase-encoding NIfTI/SRC input used to run TOPUP/EDDY. |
| `--volume_correction`| `0` | Apply automatic volume orientation correction (swap/flip axes). |
| `--check_btable`     | `0` | For `--action=rec`, `1` calls the template-aware **Check B-table** routine (using the selected/current template when available); `2` calls the no-template 24-candidate b-vector permutation/flip check. Automatic inference can be unreliable with poor SNR or poor template registration, so inspect QC first. |
| `--motion_correction`| `0` | Rigid-body align DWI volumes and rotate the b-table accordingly. |
| `--bias_field_correction` | `0` for DTI; `1` for GQI/QSDR unless already corrected | Correct smooth DWI signal inhomogeneity. Specify explicitly when you need to override the reconstruction-method default. |
| `--make_isotropic`   | Data dependent | Override the isotropic resampling resolution. If omitted, DSI Studio may choose a resolution from the input data and reconstruction workflow. |
| `--align_acpc`       | *(none)* | Rotate/resample the volume to AC-PC alignment at the specified isotropic resolution (e.g., `--align_acpc=1.5`). |

When resampling is needed, keep the original DWI geometry while applying corrections that depend on the original acquisition (for example TOPUP/EDDY, motion, or distortion correction), then resample before reconstruction.

-----

## Masking

| **Option** | **Description** |
|-----------------------|--------------------------------------------------------------------------------------------------------------|
| `--mask`            | Path to a brain mask (`.nii.gz`).<br>Use `--mask=unet` to run U-Net segmentation, or `--mask=template`.        |
| `--apply_mask`      | If set, applies the loaded mask during reconstruction (useful after reloading via `--save_src` or `--save_nii`).|
| `--rotate_to`       | Rigid-body rotate DWI into the specified T1w/T2w space (no scaling).                                           |
| `--align_to`        | Affine align DWI into the specified T1w/T2w space (includes scaling/shearing).                                 |
| `--other_image`     | Attach native-space images for QSDR to bring to template space. Format: `t1w:/path/t1w.nii.gz,t2w:/path/t2w.nii.gz`. |

## Registration Options

| **Option** | **Default**| **Description** |
|---------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------|
| `--reg_resolution`  | `2`    | The relative resolution used in nonlinear registration. `2` uses 2× lower resolution; use `1` for the same resolution at higher computation cost. |
| `--reg_speed`       | `0.3`  | Deformation speed. Higher values allow larger changes per iteration. |
| `--reg_smoothing`   | `0.05` | Smoothing applied to the deformation field. Higher values enforce greater spatial continuity. |

-----

## I/O & Miscellaneous

| **Option** | **Description** |
|-----------------------|--------------------------------------------------------------------------------------------------------------|
| `--output`          | Output folder or filename prefix for reconstructed files. |
| `--save_src`        | Write out the preprocessed SRC (no reconstruction). |
| `--save_nii`        | Write out the preprocessed 4D NIfTI instead of SRC. |
| `--intro`           | Load a text file (`.txt`/`.md`) as the introduction embedded in the reconstruction report. |
| `--remove`          | Comma-separated DWI indices or ranges to drop, e.g. `--remove=0,5:10` or `--remove=20:end`. |
| `--export_r`        | Write a `.rXX` file reporting registration R² (e.g., `.r85` for 0.85). |
| `--cmd`             | Run one or more reconstruction commands before reconstruction. Separate commands with `+` and append parameters with `=`, e.g. `--cmd="resample=1.0+save_src=out.sz"`. |

-----

## Available `--cmd` commands

Direct `--action=rec --cmd` calls use the short command names below. Commands are chained with `+`:

```text
--cmd="command[=parameter]+command2[=parameter]"
```

Older DSI Studio versions recorded processing steps using GUI-path names such as `[Step T2][Corrections][EDDY]`. Current command-line workflows should use the short names below. Saved processing histories are translated internally when they are replayed.

### Parameters

- `set_param=<name>=<value>`: Set one reconstruction parameter.
- `set_params=<name>=<value>&<name>=<value>...`: Set multiple reconstruction parameters in one command. Quote the whole `--cmd` argument in shells where `&` has special meaning.
- `list_param[=<name>]`: Print one parameter, or all parameters when no name (or `all`) is given.

Supported parameter names are `method`, `thread_count`, `other_output`, `dti_ignore_high_b`, `odf_resolving`, `r2_weighted`, `param`, `template`, `qsdr_reso`, `reg_resolution`, `reg_speed`, `reg_smoothing`, `hist_downsampling`, `hist_raw_smoothing`, `hist_tensor_smoothing`, and `hist_resolution`.

### File Operations

- `save_src=<filename>`: Save the current data as an SRC/SZ file. Use an SRC/SZ filename extension.
- `save_nifti=<filename>`: Save the current DWI data as a 4D NIfTI file. Use a NIfTI filename extension.
- `save_b0=<filename>`: Save the first B0 volume as a NIfTI file.
- `save_dwi_sum=<filename>`: Save the DWI-sum image as a NIfTI file.

### Mask Operations

- `mask_open=<mask_file>`: Load a mask whose dimensions match the source data.
- `mask_erosion`: Erode the current mask.
- `mask_dilation`: Dilate the current mask.
- `mask_unet`: Generate a mask using the U-Net model for the selected template.
- `mask_defragment`: Keep the connected mask component after defragmentation.
- `mask_slice_defragment`: Fill/defragment the mask slice by slice.
- `mask_smoothing`: Smooth the current mask.
- `mask_fit`: Fit the current mask to the DWI-sum image.
- `mask_negate`: Invert the current mask.
- `mask_from_template`: Generate a mask by warping the selected template.
- `mask_threshold=<threshold>`: Set the mask from `DWI sum > threshold`. Supply the threshold explicitly for noninteractive CLI use.
- `mask_remove_background`: Zero DWI voxels outside the current mask and enable mask application.
- `probabilistic_masking=<probability_map.nii.gz>`: Multiply DWI signals by a same-dimension probability map and set the mask to voxels with probability greater than zero.

### Image Editing

- `resample=<resolution>`: Resample to the specified positive isotropic resolution in mm.
- `align_acpc[=<resolution>]`: Align to AC-PC at the specified positive isotropic resolution; if omitted, the current x voxel size is used.
- `crop_background[=<border>]`: Crop background with an optional non-negative voxel border (default `0`).
- `set_voxel_size="<x> <y> <z>"`: Overwrite voxel size with three positive values.
- `smooth_signals`: Apply Gaussian smoothing to the DWI volumes.
- `flip_x`, `flip_y`, `flip_z`: Flip image data and corresponding b-vector axis.
- `swap_xy`, `swap_yz`, `swap_xz`: Swap image axes, voxel sizes, and corresponding b-vector axes.

### B-table Operations

- `check_btable`: Run the template-aware b-table check.
- `check_btable2`: Run the no-template b-table check.
- `flip_bx`, `flip_by`, `flip_bz`: Flip only the corresponding b-vector component.
- `swap_bxby`, `swap_bybz`, `swap_bxbz`: Swap only the corresponding b-vector components.

### Corrections

- `topup[=<reverse_phase_file>]`: Apply TOPUP. If the reverse-PE file is omitted, DSI Studio searches for a matching reverse-phase input.
- `topup_eddy[=<reverse_phase_file>]`: Apply TOPUP followed by EDDY. If no usable reverse-PE input is found, TOPUP is skipped and EDDY is still attempted.
- `eddy`: Apply FSL EDDY correction.
- `motion_correction`: Perform rigid-body DWI motion correction and rotate the b-table accordingly.
- `bias_field_correction`: Correct smooth DWI signal inhomogeneity.
- `correct_by_t2w=<T2_file>`: Correct distortion using a T2-weighted image.
- `orientation_correction`: Automatically correct volume orientation using image symmetry/axis heuristics.

### Reconstruction Geometry

- `partial_fov="<xmin> <ymin> <zmin> <xmax> <ymax> <zmax>"`: Set the partial-FOV bounds used by reconstruction.

Reconstruction itself runs after the `--cmd` sequence. The legacy saved-step label `[Step T2][Reconstruction]` is a history marker rather than a direct `--cmd` operation.
