# Step T2 Reconstruction

> Use `--action=rec` to perform diffusion reconstruction (e.g., GQI, DTI, QSDR).

This action reconstructs diffusion data (SRC/SZ format) into FIB files, with preprocessing corrections, masking, and registration options.

---

## Examples

**1. Reconstruct all SRC files with GQI** using a 1.25 sampling length and write `.fib` files to `fib/`:
```bash
dsi_studio --action=rec --source=*.sz --method=4 --param0=1.25 --output=fib/
```

**2. Reconstruct a single SRC file after EDDY correction**:
```bash
dsi_studio --action=rec --source=subject1.sz \
            --cmd="[Step T2][Corrections][EDDY]" \
            --method=4 --param0=1.25
```

**3. Reconstruct after TOPUP+EDDY** using a reverse-PE b0 in a NIfTI file:
```bash
dsi_studio --action=rec --source=*.sz \
            --rev_pe=*_rev_b0.nii.gz \
            --method=4 --param0=1.25
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
            --param0=1.25 \
            --other_image=t1w:my_t1w.nii.gz,t2w:my_t2w.nii.gz
```

**6. Bash loop to QSDR-reconstruct every `.sz` file with ODF output:**
```bash
#!/bin/bash
exec 1>log_qsdr.out 2>&1
method=7            # 7 = QSDR
param0="1.25"
for sub in *.sz; do
  echo
  dsi_studio --action=rec \
              --source="$sub" \
              --method=$method \
              --param0=$param0 \
              --odf_resolving=1
  echo
done
```

---

## Core Reconstruction Options

| **Option**           | **Default**                      | **Description**                                                                                                                           |
|-----------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `--source`           | *(required)*                     | Path to the `.sz` (or `.src.gz`) file to reconstruct.                                                                                     |
| `--method`           | `4`                              | Reconstruction algorithm:<br>`1`=DTI, `4`=GQI, `7`=QSDR.                                                                                 |
| `--param0`           | `1.25` (in vivo) / `0.6` (ex vivo) | Primary diffusion sampling length (for GQI/QSDR).                                                                                         |
| `--param1`           | *method-specific*                | Second method parameter (e.g., order, regularization).                                                                                     |
| `--param2`           | *method-specific*                | Third method parameter.                                                                                                                   |
| `--odf_resolving`    | `0`                              | Enable ODF resolving during reconstruction.                                                                                               |
| `--thread_count`     | System thread count              | Number of threads used for reconstruction.                                                                                                |
| `--template`         | `0`                              | QSDR template index (see printed list at startup—e.g. `0`=ICBM152, `1`=CIVM_mouse, etc.).                                                 |
| `--qsdr_reso`        | `2.0` mm                         | Output resolution for QSDR (overridden if smaller than your voxel size).                                                                  |
| `--r2_weighted`      | `0`                              | Use R²-weighted SDF estimation for GQI/QSDR.                                                                                              |
| `--other_output`     | `fa,rd,rdi`                      | Comma-separated list of diffusion metrics to compute. Add `odf` for ODF data; use `all` for every possible measure.                        |

---

## Preprocessing Options

| **Option**           | **Default**                      | **Description**                                                                                                                       |
|-----------------------|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `--rev_pe`           | *(auto-detect)*                  | NIfTI, RSRC, or SRC of the reverse-phase-encoding b0 for TOPUP/EDDY.                                                                  |
| `--volume_correction`| `0`                              | Apply automatic volume orientation correction (swap/flip axes).                                                                       |
| `--check_btable`     | `0`                              | Run b-table QC and auto-flip/swaps to fix gradient directions.                                                                        |
| `--correct_by_t2`    | *(none)*                         | Perform susceptibility distortion correction by nonlinearly warping b0 → T2w.                                                         |
| `--motion_correction`| `0`                              | Rigid-body align DWI volumes (and rotate b-table accordingly).                                                                        |
| `--make_isotropic`   | *(none)*                         | Resample DWI to isotropic resolution; defaults to `2.0` mm for human or your native slice thickness for non-human.                   |
| `--align_acpc`       | *(none)*                         | Rotate/resample volume to align AC–PC; specify target isotropic resolution (e.g., `--align_acpc=1.5`).                                |

---

## Masking 

| **Option**           | **Description**                                                                                               |
|-----------------------|--------------------------------------------------------------------------------------------------------------|
| `--mask`            | Path to a brain mask (`.nii.gz`).<br>Use `--mask=unet` to run U-Net segmentation, or `--mask=template`.        |
| `--apply_mask`      | If set, applies the loaded mask during reconstruction (useful after reloading via `--save_src` or `--save_nii`).|
| `--rotate_to`       | Rigid-body rotate DWI into the specified T1w/T2w space (no scaling).                                           |
| `--align_to`        | Affine align DWI into the specified T1w/T2w space (includes scaling/shearing).                                 |
| `--other_image`     | Attach native-space images for QSDR to bring to template space. Format: `t1w:/path/t1w.nii.gz,t2w:/path/t2w.nii.gz`. |


## Registration Options

| **Option**          | **Default**| **Description**                                                                                                                       |
|---------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------|
| `--reg_resolution`  | `2`    |   The relative resolution used in the nonlinear registration. `2` subsample to x2 lower resolution. Use `1` to the same resolution (higher computation time) |
| `--reg_speed`       | `0.3`  |   The deformation speed. Higher values handle larger distortions                                      |
| `--reg_smoothing`   | `0.05` |   The smoothing applied to the deformation field. Higher values ensure structural continuity |

---

## I/O & Miscellaneous

| **Option**           | **Description**                                                                                               |
|-----------------------|--------------------------------------------------------------------------------------------------------------|
| `--output`          | Output folder or filename prefix for reconstructed files.                                                     |
| `--save_src`        | Write out the preprocessed SRC (no reconstruction).                                                           |
| `--save_nii`        | Write out the preprocessed 4D NIfTI instead of SRC.                                                           |
| `--intro`           | Load a text file (`.txt`/`.md`) as the “introduction” for embedding in the reconstruction report.              |
| `--remove`          | Comma-list or ranges of DWI indices to drop—e.g., `--remove=0,5:10,end`.                                       |
| `--export_r`        | Write a `.rXX` file reporting registration R² (e.g., `.r85` for 0.85).                                        |
| `--cmd`             | Chain GUI steps for extra edits: e.g., `--cmd="[Step T2][Resample]=1.0+[Step T2][File][Save Src File]=out.sz"`. |

---

### Available `[Step …]` commands in `--cmd`

The following commands are defined in **libs/dsi/image_model.cpp** and can be chained using `--cmd`:

#### File Operations
- `[Step T2][File][Save Src File]=<filename>`: Save the current SRC file.
- `[Step T2][File][Save 4D NIFTI]=<filename>`: Save DWI data as a 4D NIfTI file.
- `[Step T2][File][Save B0]=<filename>`: Save the B0 volume as a NIfTI file.
- `[Step T2][File][Save DWI Sum]=<filename>`: Save the sum of all DWI volumes.

#### Corrections
- `[Step T2][Corrections][TOPUP]`: Apply TOPUP correction.
- `[Step T2][Corrections][TOPUP EDDY]=<reverse_phase_file>`: Apply TOPUP and EDDY corrections.
- `[Step T2][Corrections][EDDY]`: Apply EDDY correction.
- `[Step T2][Corrections][Motion Correction]`: Perform motion correction.
- `[Step T2][Corrections][Bias Field]`: Correct the DWI bias field.
- `[Step T2][Corrections][By T2w]=<T2_file>`: Correct distortions using a T2-weighted image.
- `[Step T2][Corrections][Volume Orientation Correction]`: Adjust volume orientation.

#### Editing
- `[Step T2][Edit][Resample]=<resolution>`: Resample the image to isotropic resolution.
- `[Step T2][Edit][Align ACPC]=<resolution>`: Align the volume to AC–PC.
- `[Step T2][Edit][Image flip x]`: Flip the image along the X-axis.
- `[Step T2][Edit][Image flip y]`: Flip the image along the Y-axis.
- `[Step T2][Edit][Image flip z]`: Flip the image along the Z-axis.

#### B-table Operations
- `[Step T2][B-table][Check B-table]`: Check and correct the b-table.
- `[Step T2][B-table][flip bx]`: Flip the X-gradient in the b-table.
- `[Step T2][B-table][flip by]`: Flip the Y-gradient in the b-table.
- `[Step T2][B-table][flip bz]`: Flip the Z-gradient in the b-table.
- `[Step T2][B-table][swap bxby]`: Swap X and Y gradients in the b-table.
- `[Step T2][B-table][swap bybz]`: Swap Y and Z gradients in the b-table.
- `[Step T2][B-table][swap bxbz]`: Swap X and Z gradients in the b-table.

---

This comprehensive documentation includes all available options and commands for `--action=rec`. Let me know if further refinements are needed!
