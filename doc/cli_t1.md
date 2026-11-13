# Generating SRC Files

Use `--action=src` to convert diffusion NIfTI (`.nii`, `.nii.gz`), DICOM (`.dcm`), Bruker `2dseq`, Varian/Agilent `.fdf`, or NRRD/NHDR (`.nhdr`, `.nrrd`) data into `.sz` source files. Legacy `.src.gz` files remain readable in downstream steps.

---

## Examples

**1. Convert a single 4D NIfTI file (auto-detecting sidecar `bval`/`bvec`):**
```bash
dsi_studio --action=src --source=subject001.nii.gz --output=subject001.sz
```

**2. Specify FSL `bval` and `bvec` files explicitly (both flags required together):**
```bash
dsi_studio --action=src --source=c:\subject001.nii.gz --bval=c:\bval.txt --bvec=c:\bvec.txt --output=c:\subject001.sz
```

**3. Replace the gradient table using a 4-column DSI Studio `b_table` file (`bval bx by bz` per volume):**
```bash
dsi_studio --action=src --source=subject001.nii.gz --b_table=b_table.txt --output=subject001.sz
```

**4. Convert a BIDS root directory (`sub-*` hierarchy) and save outputs to a folder:**
```bash
dsi_studio --action=src --source=./bids_root_folder --output=./src_files
```

**5. Convert a single BIDS `dwi` directory explicitly (saving `.rz` when reverse-phase data exist):**
```bash
dsi_studio --action=src --source=./sub-01/ses-01/dwi --bids=1
```

**6. Convert all 4D NIfTI DWI files in a directory into separate `.sz` files:**
```bash
dsi_studio --action=src --source=./nifti_folder --output=./src_folder
```
Pass the directory path (`./nifti_folder`) rather than `--source="*.nii.gz"` when generating one `.sz` per scan: `--action=src` does not auto-loop on `--source=*`, so a wildcard without `--loop` merges all matched files into a single `.sz`.

**7. Combine multiple NIfTI scans in the same image space into one `.sz` file:**
```bash
dsi_studio --action=src \
  --source=HCA9992517_V1_MR_dMRI_dir98_AP.nii.gz \
  --other_source=HCA9992517_V1_MR_dMRI_dir99_AP.nii.gz \
  --output=HCA9992517_AP.sz
```
Wildcard patterns also merge matched files when `--loop` is omitted (quote wildcards in POSIX shells):
```bash
dsi_studio --action=src --source="*98_AP.nii.gz" --other_source="*99_AP.nii.gz"
```

**8. Convert DICOM studies (parent folder vs. single series):**

Parent directory containing series subfolders (writes `<patient>_s<seq_num>_<sequence>.sz` or `.nii.gz`):
```bash
dsi_studio --action=src --source=C:\DICOM_study_root
```

Single DICOM series folder (using a `.dcm` wildcard):
```bash
dsi_studio --action=src --source="C:\DICOM_series\*.dcm" --output=C:\subject001.sz
```

**9. Batch loop across subjects or `.zip` archives (`--loop`):**
```bash
dsi_studio --action=src \
  --loop="MGH_*_all.zip" \
  --source="mgh_*/diff/preproc/mri/diff_preproc.nii.gz" \
  --bval="mgh_*/diff/preproc/bvals.txt" \
  --bvec="mgh_*/diff/preproc/bvecs_fsl_moco_norm.txt" \
  --output="sub-*_dwi.sz"
```

---

## Core Functions

| Option | Default | Description |
|:--|:--|:--|
| `--action` | *(required)* | Must be `src`. |
| `--source` | *(required)* | Directory path (BIDS root, NIfTI folder, or DICOM parent folder) or file/pattern (`.nii`, `.nii.gz`, `.dcm`, Bruker `2dseq`, `.fdf`, `.nhdr`, `.nrrd`). Multiple comma-separated files or wildcards merge into one `.sz` unless `--loop` is set. |

---

## Optional Functions

| Option | Default | Description |
|:--|:--|:--|
| `--output` | `<stem>.sz` in CWD (file mode) or input folder (directory NIfTI/BIDS mode) | **File/pattern mode:** Output `.sz` (or `.rz`) file path, or an existing directory (`<output>/<stem>.sz`); appends `.sz` if the path does not end in `.sz` or `.rz`. **Directory NIfTI/BIDS mode:** Output directory (created if missing); writes beside each input NIfTI when omitted. Ignored by DICOM directory fallback. |
| `--other_source` | *(none)* | *(File/pattern mode only)* Additional DWI file(s), comma-separated paths, wildcard pattern, or NIfTI directory to append into the same `.sz` file. |
| `--b_table` | *(none)* | *(File/pattern mode only)* Whitespace-delimited text file containing `4 * N` numbers (`bval bx by bz` per volume) that replaces the loaded b-table directly without flipping axes. |
| `--bval` | Auto-detected (NIfTI) | *(File/pattern mode only; must pair with `--bvec`)* FSL b-value file(s) (single path, comma-separated list, or wildcard). |
| `--bvec` | Auto-detected (NIfTI) | *(File/pattern mode only; must pair with `--bval`)* FSL b-vector file(s) (`3 x N` or `N x 3`); negates `bvec_y` from FSL convention and sets `bval = 0`, `bvec = (0,0,0)` where `bval < 100`. Overrides `--b_table` if both are supplied. |
| `--intro` | Auto `README` if present | *(File/pattern mode only)* Text file embedded as the `intro` metadata string inside the `.sz` file. |

For NIfTI inputs without `--bval`/`--bvec`, DSI Studio automatically checks `<stem>.bval`, `<stem>.bvals`, `<stem>.bval.txt`, `<stem>.bvals.txt` and `<stem>.bvec`, `<stem>.bvecs`, `<stem>.bvec.txt`, `<stem>.bvecs.txt`. When present in the same directory as a 4D NIfTI, `nodif_brain_mask.nii.gz` is loaded as the mask and `grad_dev.nii.gz` (9-volume gradient-nonlinearity tensor; requires `b0` and `bval`/`bvec`) is applied automatically.

---

## Accessory Functions

| Option | Default | Description |
|:--|:--|:--|
| `--bids` | `1` if `sub-*` DWI found, else `0` | *(Directory NIfTI mode)* Enable (`1`) or disable (`0`) BIDS folder grouping and reverse-phase pairing. Set `--bids=1` when `--source` points directly to a `dwi` or session folder. |
| `--overwrite` | `0` | Skip existing outputs when `0`; replace existing `.sz` (and DICOM `.nii.gz`) files when `1`. |
| `--topup_eddy` | `0` | *(BIDS directory mode only, `bids=1`)* When `1`, runs TOPUP (if reverse-phase data exist) and EDDY before saving `.sz` instead of saving a companion `.rz`. Ignored when `bids=0` or in file mode. |
| `--sort_b_table` | `0` | *(File/pattern mode only)* When non-zero (`1`), sorts volumes by ascending `b-value` (then `b-vector`) and averages duplicate volumes with identical `b-value` and `b-vector`. |
| `--loop` | *(none)* | *(Global dispatcher)* Explicit file/archive wildcard pattern that runs `--action=src` once per match and substitutes `*` in other options. |
| `--continue_on_error` | *(not set)* | *(Global dispatcher)* Continue remaining `--loop` iterations after an error instead of stopping on the first failure. |
| `--thread_count` | `tipl::max_thread_count` | *(Global dispatcher)* Maximum CPU thread count used by parallel NIfTI directory conversion and gradient-deviation correction. |

---

## Directory behavior

When `--source` is an existing directory, `--other_source`, `--b_table`, `--bval`, `--bvec`, `--sort_b_table`, and `--intro` are not used. DSI Studio processes the directory in the following order:

1. **BIDS/NIfTI detection**:
   - Searches `sub-*/dwi` and `sub-*/ses-*/dwi` for NIfTI files with matching `bval`/`bvec` (`--bids` defaults to `1` when found).
   - If none are found, searches `--source` directly for `*.nii.gz` and `*.nii` (`--bids` defaults to `0` unless `--bids=1` is specified).

2. **BIDS phase-encoding pairing (`bids=1`)**:
   - Within each folder, NIfTI files (excluding `*.sz.*`) are sorted from largest to smallest volume count and grouped by 3D dimensions and `PhaseEncodingDirection` from `<stem>.json` (or filename markers `_ap`, `ap_`, `_pa`, `pa_`, `_lr`, `lr_`, `_rl`, `rl_`, `rev` when JSON metadata are absent).
   - Same-phase runs merge into `<main_stem>.sz` (embedding `README` from up to three parent directories or the working directory). Opposite-phase runs are saved as `<main_stem>.rz` when `--topup_eddy=0`, or used to run TOPUP + EDDY directly when `--topup_eddy=1` (a single reverse-phase NIfTI does not require `bval`/`bvec`).

3. **Plain NIfTI folder conversion (`bids=0`)**:
   - Converts each 4D NIfTI with matching `bval`/`bvec` in parallel to `<stem>.sz` inside `--output` (created if needed), or beside each input NIfTI when `--output` is omitted.

4. **DICOM directory fallback**:
   - If no NIfTI files are found, DSI Studio recursively scans subdirectories of `--source` for `*.dcm` series (point `--source` at the parent study folder containing series subfolders, or use `--source="series_dir\*.dcm"` for a single leaf series folder).
   - Diffusion series with a b-table are saved as `<series_parent>/<patient>_s<seq_num>_<sequence>.sz`; structural or non-b-table series are saved as `<series_parent>/<patient>_s<seq_num>_<sequence>.nii.gz`. Only `--overwrite` applies (`--output` is ignored).
