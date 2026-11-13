# Reconstruction CLI

> Use `--action=rec` to reconstruct diffusion MRI (GQI, DTI, QSDR) or 2D histology data into `.fz` FIB files, or to run DWI preprocessing and export `.sz` / `.nii.gz` datasets.

-----

## Examples

**1. Batch GQI reconstruction** on all `.sz` files using a `1.25` sampling length ratio and writing `.gqi.fz` files to `fib/`:

```bash
dsi_studio --action=rec --source=*.sz --method=4 --param=1.25 --output=fib/
```

**2. Reconstruct a single `.sz` file after EDDY correction:**

```bash
dsi_studio --action=rec --source=subject1.sz \
            --cmd=eddy \
            --method=4 --param=1.25
```

**3. Batch TOPUP+EDDY + GQI** using matched reverse-phase-encoding b0 NIfTI files:

```bash
dsi_studio --action=rec --source=*.sz \
            --rev_pe=*_rev_b0.nii.gz \
            --method=4 --param=1.25
```

**4. Run TOPUP+EDDY only (no reconstruction)** and save the preprocessed `.sz` file:

```bash
dsi_studio --action=rec --source=raw_dwi.sz \
            --rev_pe=rev_b0.nii.gz \
            --save_src=raw_dwi_proc.sz
```

**5. QSDR reconstruction warping native T1w and T2w images to template space:**

```bash
dsi_studio --action=rec --source=subj1.sz \
            --method=7 \
            --param=1.25 \
            --other_image=t1w:my_t1w.nii.gz,t2w:my_t2w.nii.gz
```

To also drive QSDR nonlinear registration with an auxiliary modality pair (`<subject_nii>,<template_nii>`), add `--reg`:

```bash
dsi_studio --action=rec --source=subj1.sz \
            --method=7 --param=1.25 \
            --reg=my_t1w.nii.gz,mni_icbm152_t1_tal_nlin_asym_09c.nii.gz \
            --other_image=t1w:my_t1w.nii.gz
```

**6. Bash loop to QSDR-reconstruct every `.sz` file with full ODF vectors saved (`.odf.qsdr.fz`):**

```bash
#!/bin/bash
exec 1>log_qsdr.out 2>&1
for sub in *.sz; do
  dsi_studio --action=rec \
              --source="$sub" \
              --method=7 \
              --param=1.25 \
              --other_output=fa,md,rd,rdi,odf
done
```

**7. B-table quality check and manual b-vector correction:**

Inspect b-table orientation with `--action=qc` before applying corrections, especially on low-SNR scans:

```bash
dsi_studio --action=qc --source=*.sz --check_btable=1
```

In `--action=qc`, `--check_btable=1` uses the template-aware check when `--template=<name>` is also supplied, and the no-template check otherwise. In `--action=rec`, `--check_btable=1` runs the template-aware check and `--check_btable=2` runs the no-template check.

When a representative scan confirms a consistent acquisition export convention (e.g., `021fx`: swap `y`/`z` to `012`, then flip `x`), apply the exact b-vector correction via `--cmd`:

```bash
dsi_studio --action=rec --source=subject.sz \
            --cmd="swap_bybz+flip_bx" \
            --method=4 --param=1.25
```

-----

## Core Reconstruction Options

| **Option** | **Default** | **Description** |
|---|---|---|
| `--source` | *(required)* | Input `.sz`, `.src.gz`, 4D `.nii.gz` (with matching `.bval`/`.bvec`), 2D histology `.jpg`/`.tif` (writes `.hist.fz`), or QSDR `.fz` previously saved with `dwi` in `--other_output`. Wildcards (e.g., `*.sz`) automatically run in batch mode. |
| `--method` | `4` | Reconstruction method: `1` = DTI (`.dti.fz`), `4` = GQI (`.gqi.fz`; handles single-shell, multi-shell, DSI grid, and single-DWI HGQI), `7` = QSDR (`.qsdr.fz`). |
| `--param` | `1.25` (`max(b) < 5000`); `80/sqrt(max_b)` (`max(b) >= 5000`) | Diffusion sampling length ratio for GQI and QSDR (`--method=4` or `7`). Typical explicit values are `1.25` (in-vivo) and `0.6` (ex-vivo). |
| `--dti_ignore_high_b` | `1` (human); `0` (non-human) | Exclude shells with `b > 1750` when calculating tensor metrics if at least 10 DWIs have `100 < b <= 1750`. |
| `--r2_weighted` | `0` | Set `1` to weight GQI/QSDR SDF integration by squared diffusion displacement (writes `.gqi2.fz` or `.qsdr2.fz`). |
| `--template` | Auto-matched by volume (`human` = `0`) | Species/template name under `atlas/` (e.g., `human`, `human-neonate`, `chimpanzee`, `rhesus`, `marmoset`, `rat`, `mouse`) or single-digit index (`0`–`9`), used by QSDR, U-Net/template masking, and `--check_btable=1`. |
| `--qsdr_reso` | Human: `2.0` (`max(vx,vz) > 1.75`), `1.5` (`>= 1.5`), else `1.0`; non-human: `vz` | Output isotropic voxel size in mm for QSDR (`--method=7`). |
| `--other_output` | `fa,md,rd,rdi` | Comma-separated output tokens: tensor metrics (`fa`, `md`, `ad`, `rd`, `rd1`, `rd2`, `helix`, `tensor`), ODF/RDI metrics (`gfa`, `rdi`, `odf`), `dwi` (stores template-space balanced DWI in QSDR when `< 5` shells), `fixel` (stores refined `fixel0`..`fixelN` fiber vectors), `debug` (writes intermediate QSDR registration NIfTIs), or `all` (enables all metrics except `dwi`, `fixel`, and `debug`). Including `odf` writes `.odf.gqi.fz` / `.odf.qsdr.fz`. |

-----

## Preprocessing Options

| **Option** | **Default** | **Description** |
|---|---|---|
| `--bval` | *(none)* | Replace b-values from an FSL-format `.bval` file before `--remove` and `--cmd`. |
| `--bvec` | *(none)* | Replace b-vectors from an FSL-format `.bvec` file before `--remove` and `--cmd`. |
| `--remove` | *(none)* | Comma-separated 0-based DWI indices, inclusive ranges, or b-value conditions to drop before `--cmd` (e.g., `--remove=0,5:10,20:end`, `--remove="b>3000"`, or `--remove="b<100"`). |
| `--check_btable` | `0` | Automatic b-table check: `0` = off, `1` = template-aware check (`check_btable`), `2` (or any non-zero value `!= 1`) = no-template 24-candidate permutation/flip check (`check_btable2`). |
| `--rev_pe` | *(none)* | Reverse-phase-encoding `.nii.gz` or `.sz` file used to run `topup_eddy`. |
| `--volume_correction` | `0` | Set `1` to run automatic axis swap/flip volume orientation correction (`orientation_correction`). |
| `--motion_correction` | `0` | Set `1` to rigid-body align DWI volumes and rotate b-vectors accordingly (`motion_correction`). |
| `--bias_field_correction` | `0` (`--method=1`); `1` otherwise (unless already recorded in report) | Correct smooth DWI signal inhomogeneity. Pass `0` or `1` explicitly to override the method default. |
| `--make_isotropic` | Human (`--method!=7`, no `--save_src`): `2.0`/`1.5`/`1.0` mm when `vz > 1.25*vx`, else `max(vs)` | Resample non-QSDR data to isotropic resolution in mm. Pass `--make_isotropic=0` to disable automatic human resampling. Ignored when `--align_acpc`, `--rotate_to`, `--align_to`, or `--method=7` is active. |
| `--align_acpc` | `min(2.0, max(vs))` when passed without value | Rotate and resample DWI to AC-PC alignment at the specified isotropic resolution in mm (e.g., `--align_acpc=1.5`). Takes precedence over `--rotate_to`, `--align_to`, and `--make_isotropic`. |

`--action=rec` executes steps in this fixed order: `--bval`/`--bvec` → `--remove` → `--cmd` → `--check_btable` → `--rev_pe` → `--volume_correction` → `--motion_correction` → `--mask` → `--bias_field_correction` → spatial alignment/resampling (`--align_acpc`, else `--rotate_to`/`--align_to`, else `--make_isotropic`) → reconstruction (or early save via `--save_src`/`--save_nii`).

-----

## Masking

| **Option** | **Description** |
|---|---|
| `--mask` | Load a matching-dimension `.nii.gz` mask file, `--mask=unet` (species U-Net tissue segmentation), `--mask=template` (warp template mask), or `--mask=1` (cover full image volume without masking). |
| `--apply_mask` | Zero out-of-mask DWI signals. If the dataset is unmasked and `--mask` is omitted, `--apply_mask` (or `--save_src`) automatically generates a U-Net mask. |
| `--rotate_to` | Rigid-body transform DWI and b-table into the space and voxel size of a target `.nii.gz` (e.g., T1w/T2w; no scaling). Takes precedence over `--align_to` if both are supplied. |
| `--align_to` | Affine transform DWI and b-table into the space and voxel size of a target `.nii.gz` (includes scaling and shearing). |
| `--other_image` | Comma-separated `<name>:<file.nii.gz>` or `<file.nii.gz>` entries (wildcards supported) attached to the output `.fz`—warped to template space in QSDR or registered into DWI space otherwise (e.g., `--other_image=t1w:my_t1w.nii.gz,t2w:my_t2w.nii.gz`). |

## Registration Options

| **Option** | **Default** | **Description** |
|---|---|---|
| `--reg` | *(none)* | Comma-separated pair `<subject_modality.nii.gz>,<template_modality.nii.gz>` used as an auxiliary registration channel in QSDR (`--method=7`). |
| `--reg_resolution` | `2` | Relative grid factor for QSDR nonlinear registration (`2` uses 2× lower resolution; `1` uses native template grid at higher computation cost). |
| `--reg_speed` | `0.3` | QSDR CDM deformation step speed per iteration. |
| `--reg_smoothing` | `0.05` | Smoothing factor applied to the QSDR deformation field (`0`–`1`; higher enforces smoother warps). |

-----

## I/O & Miscellaneous

| **Option** | **Default** | **Description** |
|---|---|---|
| `--output` | *(source directory & stem)* | Output folder or `.fz`/`.fib.gz` path. When omitted or set to an existing directory, appends `.dti.fz`, `.gqi.fz` (`.gqi2.fz`), `.qsdr.fz` (`.qsdr2.fz`), `.odf.<method>.fz`, or `.hist.fz` to the stripped source name. |
| `--overwrite` | `0` | Overwrite existing `--output`, `--save_src`, or `--save_nii` target (`1` = overwrite; `0` = skip and exit `0` if target exists). |
| `--save_src` | *(none)* | Save preprocessed DWI as `.sz`, `.src.gz`, or `.rz` and exit without running reconstruction. Unmasked inputs automatically receive a U-Net mask unless `--mask=1` is passed. |
| `--save_nii` | *(none)* | Save preprocessed 4D DWI to `<name>.nii.gz` along with `<name>.bval` and `<name>.bvec`, then exit without reconstructing (path must end with `nii.gz`). |
| `--intro` | *(none)* | Embed a text/Markdown file into the `intro` report field of the saved `.sz`/`.fz`. |
| `--export_r` | `0` | Set `1` to write an empty `<source>.r<int(R2*100)>` marker file reporting QSDR registration goodness-of-fit (e.g., `subj.sz.r85` for `R² = 0.85`). |
| `--cmd` | *(none)* | `+`-separated command sequence run after `--remove` and before `--check_btable` (e.g., `--cmd="eddy+resample=1.25"`). |
| `--thread_count` | Max CPU threads | Worker thread count used across preprocessing and reconstruction. |
| `--loop` | Auto when `--source` contains `*` | Explicit wildcard pattern to iterate over multiple files in batch mode. |
| `--continue_on_error` | *(off)* | Keep processing remaining batch files if one file fails. |

-----

## Available `--cmd` commands

`--action=rec --cmd` splits on `+` and dispatches each `command` or `command=parameter` directly:

```text
--cmd="command1=param1+command2=param2"
```

Always use the short command names below on the command line. Legacy GUI step strings such as `[Step T2][Corrections][EDDY]` are not translated when passed directly via `--cmd` (they are translated only when replaying saved `steps` histories).

### Parameters

- `set_param=<name>=<value>`: Set one reconstruction parameter (runs after initial CLI flag parsing, overriding earlier values).
- `set_params=<name>=<value>&<name>=<value>...`: Set multiple parameters in one step (quote the `--cmd` string in shells where `&` is special).
- `list_param` or `list_param=<name>`: Print all parameters (`all` or omitted) or one named parameter.

Supported parameter names: `method`, `thread_count`, `other_output`, `dti_ignore_high_b`, `r2_weighted`, `param`, `template` *(0-based integer template index)*, `qsdr_reso`, `reg_resolution`, `reg_speed`, `reg_smoothing`, `hist_downsampling`, `hist_raw_smoothing`, `hist_tensor_smoothing`, and `hist_resolution`.

### File Operations

- `save_src=<file.sz|.src.gz|.rz>`: Save the current DWI dataset to an SRC/SZ/RZ file.
- `save_nifti=<file.nii.gz>`: Save the current 4D DWI dataset to `.nii.gz` plus matching `.bval` and `.bvec` files.
- `save_b0=<file.nii.gz>`: Save the first b0 volume as a 3D NIfTI file.
- `save_dwi_sum=<file.nii.gz>`: Save the summed DWI volume as a 3D NIfTI file.
- `save_b_table=<file.txt>`: Save the 4-column DSI Studio b-table (`b bx by bz`).
- `save_bval=<file.bval>`: Save b-values in FSL `.bval` format.
- `save_bvec=<file.bvec>`: Save b-vectors in FSL `.bvec` format.

### Mask Operations

- `mask_open=<mask_file>`: Load a mask file whose dimensions match the DWI volume.
- `mask_from_unet`: Generate a mask using the species tissue U-Net model for the active template.
- `mask_from_template`: Generate a mask by nonlinearly warping the active template.
- `mask_threshold=<int_threshold>`: Set the mask to `dwi_sum > threshold` (always supply an integer value in headless CLI runs).
- `mask_erosion`: Erode the current mask by 1 voxel.
- `mask_dilation`: Dilate the current mask by 1 voxel.
- `mask_defragment`: Retain the main connected component of the mask.
- `mask_slice_defragment`: Fill 2D holes slice by slice.
- `mask_smoothing`: Smooth the current mask boundary.
- `mask_fit`: Fit the current mask boundary to the DWI-sum image.
- `mask_negate`: Invert the current mask.
- `mask_remove_background`: Zero DWI voxels outside the current mask and set `apply_mask=true`.
- `probabilistic_masking=<prob_map.nii.gz>`: Multiply all DWI volumes by a matching-dimension probability map and set the mask to `prob > 0`.

### Image Editing

- `resample=<mm>`: Resample DWI volumes and mask to a positive isotropic voxel size in mm.
- `align_acpc` or `align_acpc=<mm>`: Align to AC-PC at `<mm>` isotropic resolution (defaults to current `vx` when `<mm>` is omitted).
- `crop_background` or `crop_background=<border>`: Trim empty background around the mask with an optional non-negative voxel margin (default `0`).
- `set_voxel_size="<vx> <vy> <vz>"`: Overwrite voxel spacing in mm without resampling image data.
- `smooth_signals`: Apply Gaussian smoothing across DWI volumes.
- `flip_x`, `flip_y`, `flip_z`: Flip the image volume along `x`, `y`, or `z` and negate the corresponding b-vector component.
- `swap_xy`, `swap_yz`, `swap_xz`: Swap two image axes, their voxel sizes, and the corresponding b-vector components.
- `remove=<spec>`: Drop DWI volumes by 0-based index/range or b-value filter (`0,5:10,20:end`, `b>3000`, `b<100`).
- `add_other_image=<file.nii.gz>` or `add_other_image=<name>:<file.nii.gz>`: Attach an auxiliary 3D NIfTI volume for reconstruction.

### B-table Operations

- `load_bval=<file.bval>`: Replace b-values from an FSL `.bval` file.
- `load_bvec=<file.bvec>`: Replace b-vectors from an FSL `.bvec` file (flips `by` to match DSI Studio convention).
- `regenerate_btable` or `regenerate_btable=<reference.sz>`: Re-estimate shell b-vectors by signal mutual information against the active template's reference `.sz` (or `<reference.sz>`).
- `scheme_balance`: Interpolate shell DWI data (`< 5` shells) onto an evenly distributed icosahedral sampling scheme.
- `check_btable`: Run the template-aware 24-orientation b-table check/auto-correction.
- `check_btable2`: Run the no-template 24-orientation b-table check/auto-correction.
- `flip_bx`, `flip_by`, `flip_bz`: Negate only the specified b-vector component (image volume unchanged).
- `swap_bxby`, `swap_bybz`, `swap_bxbz`: Swap only the specified b-vector components (image volume unchanged).

### Corrections

- `topup` or `topup=<rev_pe_file>`: Run FSL TOPUP susceptibility correction (auto-searches for a matching reverse-PE file if omitted).
- `topup_eddy` or `topup_eddy=<rev_pe_file>`: Run TOPUP followed by EDDY (skips TOPUP with a warning and still runs EDDY if no reverse-PE file is found).
- `eddy`: Run TinyFSL/FSL EDDY distortion and motion correction.
- `motion_correction`: Rigid-body register DWI volumes and rotate b-vectors accordingly.
- `bias_field_correction`: Estimate and remove smooth DWI bias field inhomogeneity.
- `correct_by_t1w=<t1w.nii.gz>` or `correct_by_t1w=<t1w.nii.gz>|<mm>`: Correct AP–PA susceptibility distortion by nonlinearly warping an inverted b0 to `<t1w.nii.gz>`, optionally resampling to `<mm>` isotropic (`0` or omitted keeps native resolution; `correct_by_t2w=<file>` is accepted as an alias).
- `orientation_correction`: Automatically swap/flip volume axes to standard orientation using symmetry heuristics.

### Reconstruction Geometry

- `partial_fov="<xmin> <ymin> <zmin> <xmax> <ymax> <zmax>"`: Set partial-FOV bounding coordinates used during QSDR registration.
