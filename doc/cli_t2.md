
## Step T2 Reconstruction

> Use `--action=rec` to perform diffusion reconstruction (GQI/DTI/QSDR, etc.).

### Examples

**Reconstruct all SRC files with GQI** using a 1.25 sampling length and write out all `.fib` files into `fib/`  
```bash
dsi_studio --action=rec --source=*.sz --method=4 --param0=1.25 --output=fib/
```

**Reconstruct a single SRC file after EDDY correction**  
```bash
dsi_studio --action=rec --source=subject1.sz \
            --cmd="[Step T2][Corrections][EDDY]" \
            --method=4 --param0=1.25
```

**Reconstruct after TOPUP+EDDY** using reverse-PE b0 in a NIfTI  
```bash
dsi_studio --action=rec --source=*.sz \
            --rev_pe=*_rev_b0.nii.gz \
            --method=4 --param0=1.25
```

**Just apply TOPUP+EDDY (no reconstruction)** and save the corrected SRC  
```bash
dsi_studio --action=rec --source=raw_dwi.sz \
            --rev_pe=rev_b0.nii.gz \
            --save_src=raw_dwi_proc.sz
```

**QSDR** reconstruction with 1.25 sampling ratio, warping both T1w and T2w  
```bash
dsi_studio --action=rec --source=subj1.sz \
            --method=7 \
            --param0=1.25 \
            --other_image=t1w:my_t1w.nii.gz,t2w:my_t2w.nii.gz
```

**Bash loop** to QSDR‐reconstruct every `.sz` with ODF output  
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

## Core Options

| Option            | Default                      | Description                                                                                                                           |
|:------------------|:-----------------------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| `--source`        | *(required)*                 | path to the `.sz` (or `.src.gz`) file to reconstruct.                                                                                 |
| `--method`        | `4`                          | reconstruction algorithm:<br>`1`=DTI, `4`=GQI, `7`=QSDR.                                                                      |
| `--param0`        | `1.25` (in vivo) / `0.6` (ex vivo) | primary diffusion sampling length (for GQI/QSDR).                                                                                     |
| `--param1`        | *method-specific*             | second method parameter (e.g. order, regularization)—see paper/tool docs.                                                             |
| `--param2`        | *method-specific*             | third method parameter.                                                                                                               |
| `--other_output`  | `fa,rd,rdi`                  | comma‐list of diffusion metrics to compute. Add `odf` to write the ODF data; use `all` for every possible measure.                    |
| `--thread_count`  | system thread count          | number of threads used for reconstruction.                                                                                            |
| `--dti_ignore_high_b` | `1` for human, `0` for animal | ignore DWI volumes with b>1500 when computing tensors (only applies when `--method=1`).                                                |
| `--r2_weighted`   | `0`                          | use R²-weighted SDF estimation for GQI/QSDR.                                                                                            |
| `--template`      | `0`                          | QSDR template index (see printed list at startup—e.g. `0`=ICBM152, `1`=CIVM_mouse, …).                                                 |
| `--qsdr_reso`     | `2.0` mm                     | output resolution for QSDR (overridden if smaller than your voxel size).                                                              |

---

## Preprocessing Options

| Option                | Default | Description                                                                                                                       |
|:----------------------|:--------|:----------------------------------------------------------------------------------------------------------------------------------|
| `--rev_pe`            | —       | NIfTI/RSRC/SRC of the reverse-phase-encoding b0 for TOPUP/EDDY. Omit the value (`--rev_pe`) to auto-detect files in the folder.     |
| `--volume_correction` | `0`     | apply automatic volume orientation correction (swap/flip axes).                                                                    |
| `--check_btable`      | `0`     | run b-table QC and auto-flip/swaps to fix gradient directions.                                                                      |
| `--correct_by_t2`     | —       | perform susceptibility distortion correction by nonlinearly warping b0 → T2w.                                                      |
| `--motion_correction` | `0`     | rigid‐body align DWI volumes (and rotate b-table accordingly).                                                                     |
| `--make_isotropic`    | *(none)*| resample DWI to isotropic resolution; defaults to `2.0` mm for human, or your native slice‐thickness for non-human.               |
| `--align_acpc`        | —       | rotate/resample volume to align AC–PC; specify target isotropic resolution (e.g. `--align_acpc=1.5`).                             |

---

## Masking & Registration

| Option            | Description                                                                                               |
|:------------------|:----------------------------------------------------------------------------------------------------------|
| `--mask`          | path to a brain mask (`.nii.gz`).<br>Use `--mask=unet` to run U-Net segmentation, or `--mask=template`.    |
| `--apply_mask`    | if set, applies the loaded mask during reconstruction (useful after reloading via `--save_src/--save_nii`).|
| `--rotate_to`     | rigid‐body rotate DWI into this T1w/T2w space (no scaling).                                                |
| `--align_to`      | affine align DWI into this T1w/T2w space (includes scaling/shearing).                                     |
| `--other_image`   | attach native space images for qsdr to bring to template space: e.g. `--other_image=t1w:/path/t1w.nii.gz,t2w:/path/t2w.nii.gz`|

---

## I/O & Miscellaneous

| Option         | Description                                                                                   |
|:---------------|:----------------------------------------------------------------------------------------------|
| `--output`     | output folder or filename prefix for reconstructed files.                                     |
| `--save_src`   | write out the preprocessed SRC (no reconstruction).                                           |
| `--save_nii`   | write out the preprocessed 4D NIfTI instead of SRC.                                          |
| `--intro`      | load a text file (`.txt`/`.md`) as the “introduction” to embed in the reconstruction report.|
| `--remove`     | comma-list or ranges of DWI indices to drop—e.g. `--remove=0,5:10,end`.                        |
| `--export_r`   | write a `.rXX` file reporting registration R² (e.g. `.r85` for 0.85).                         |
| `--cmd`        | chain GUI steps for extra edits:<br>`--cmd="[Step T2][Edit][Resample]=1.0+[Step T2][File][Save Src File]=out.sz"`|

---

### Available `[Step …]` commands

Here is the **complete list** of recognized GUI commands you can pass via `--cmd="[…]+"` to DSI Studio’s `rec` action, as implemented in **image_model.cpp**:

---

### Step T2 (file & core ops) 

- `[Step T2][Reconstruction]`  
- `[Step T2][File][Save Src File]`  
- `[Step T2][File][Save 4D NIFTI]`  
- `[Step T2][File][Save B0]`  
- `[Step T2][File][Save DWI Sum]`  

---

### Step T2a (mask editing) 

- `[Step T2a][Open]`  
- `[Step T2a][Erosion]`  
- `[Step T2a][Dilation]`  
- `[Step T2a][Unet]`  
- `[Step T2a][Defragment]`  
- `[Step T2a][Slice Defragment]`  
- `[Step T2a][Smoothing]`  
- `[Step T2a][Fit]`  
- `[Step T2a][Negate]`  
- `[Step T2a][Template]`  
- `[Step T2a][Threshold]`  
- `[Step T2a][Remove Background]`  

---

### Step T2 Edit (image & voxel edits) 

- `[Step T2][Edit][Probablistic Masking]`  
- `[Step T2][Edit][Overwrite Voxel Size]`  
- `[Step T2][Edit][Smooth Signals]`  
- `[Step T2][Edit][Crop Background]`  
- `[Step T2][Edit][Image flip x]`  
- `[Step T2][Edit][Image flip y]`  
- `[Step T2][Edit][Image flip z]`  
- `[Step T2][Edit][Image swap xy]`  
- `[Step T2][Edit][Image swap yz]`  
- `[Step T2][Edit][Image swap xz]`  
- `[Step T2][Edit][Resample]`  
- `[Step T2][Edit][Align ACPC]`  

---

### Step T2 B-table (gradient QC) 

- `[Step T2][B-table][Check B-table]`  
- `[Step T2][B-table][Check B-table2]`  
- `[Step T2][B-table][flip bx]`  
- `[Step T2][B-table][flip by]`  
- `[Step T2][B-table][flip bz]`  
- `[Step T2][B-table][swap bxby]`  
- `[Step T2][B-table][swap bybz]`  
- `[Step T2][B-table][swap bxbz]`  

---

### Step T2 Corrections (TOPUP/EDDY/etc.) 

- `[Step T2][Corrections][TOPUP]`  
- `[Step T2][Corrections][TOPUP EDDY]`  
- `[Step T2][Corrections][EDDY]`  
- `[Step T2][Corrections][Motion Correction]`  
- `[Step T2][Corrections][Bias Field]`  
- `[Step T2][Corrections][By T2w]`  
- `[Step T2][Corrections][Volume Orientation Correction]`  

---

### Step T2b(2) (partial-FOV) 

- `[Step T2b(2)][Partial FOV]`  

---

You can chain any of these with `+`, and pass parameters with `=`, e.g.:  
```bash
--cmd="[Step T2][Corrections][EDDY]+[Step T2][File][Save 4D NIFTI]=out.nii.gz"
```

