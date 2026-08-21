# Generating SRC Files

> Use `--action=src` to generate SRC files from NIFTI or DICOM files.

The `src` action converts NIFTI or DICOM diffusion data into the current `.sz` SRC format. Legacy `.src.gz` files remain supported for compatibility.

---

## Examples

**1. Convert a single 4D NIFTI file to an SRC file:**
```bash
dsi_studio --action=src --source=c:\subject001.nii.gz
```

**2. Specify the location of bval and bvec files for a 4D NIFTI file (when DSI Studio cannot find them automatically):**
```bash
dsi_studio --action=src --source=c:\subject001.nii.gz --bval=bval --bvec=bvec
```

**3. Convert DWI NIFTI files in a BIDS folder and output all SRC files to a specified folder:**
```bash
dsi_studio --action=src --source=./bids_root_folder --output=/src_files
```

**4. Parse a BIDS session folder and generate an `.sz` file, with an `.rz` companion when reverse-phase data are present:**
```bash
dsi_studio --action=src --source=./sub-01/ses-01/dwi --bids=1
```

**5. Combine two 4D NIFTI files into one SRC file:**
```bash
dsi_studio --action=src --source=HCA9992517_V1_MR_dMRI_dir98_AP.nii.gz --other_source=HCA9992517_V1_MR_dMRI_dir99_AP.nii.gz
```

**6. Search all DICOM files under a directory and output an SRC file:**
```bash
dsi_studio --action=src --source=C:\DICOM_folder --output=C:\output.sz
```

**7. Find and combine specific files based on a pattern to create a combined SRC file:**
```bash
dsi_studio --action=src --source=*98_AP.nii.gz --other_source=*99_AP.nii.gz
```

**8. Parse 4D NIFTI files from a folder, each with associated bval and bvec files, and generate SRC files in a new folder:**
```bash
dsi_studio --action=src --source=*.nii.gz --output=/src_folder
```

**9. Batch process using a loop to create SRC files from zip archives and associated files:**
```bash
dsi_studio --action=src --loop=MGH_*_all.zip --source=mgh_*/diff/preproc/mri/diff_preproc.nii.gz --bval=mgh_*/diff/preproc/bvals.txt --bvec=mgh_*/diff/preproc/bvecs_fsl_moco_norm.txt --output=sub-*_dwi.sz
```

---

## Core Functions

| **Parameter**   | **Description**                                                                 |
|------------------|---------------------------------------------------------------------------------|
| `source`        | Specify the directory containing DICOM/NIFTI files or the filename of a 4D NIFTI file. |

---

## Optional Functions

| **Parameter**      | **Description**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| `other_source`      | Specify additional files to be included in the SRC file. Multiple files can be assigned using a comma-separated list (e.g., `--other_source=1.nii.gz,2.nii.gz`). |
| `output`           | Assign the output `.sz` file name or output folder. If not specified, output is written next to the input. Legacy `.src.gz` remains readable. |
| `b_table`          | Assign a text file to replace the b-table. The file must match the loaded images in size. |
| `bval`             | Specify the location of the FSL bval file. *(DSI Studio usually detects this automatically.)* |
| `bvec`             | Specify the location of the FSL bvec file. *(DSI Studio usually detects this automatically.)* |
| `intro`            | Specify a `README` file to include as an introduction for the SRC file. |

---

## Accessory Functions

| **Parameter**      | **Default**     | **Description**                                                                 |
|---------------------|-----------------|---------------------------------------------------------------------------------|
| `bids`             | Auto | BIDS parsing is selected automatically when a BIDS structure is detected. Use `--bids=1` to request BIDS handling explicitly. |
| `overwrite`         | `0` | Overwrite existing output files when set to `1`. |
| `topup_eddy`        | `0` | Run TOPUP and EDDY during directory/BIDS conversion when reverse-phase data are available. |
| `sort_b_table`      | `0` | Sort the b-table when creating an SRC file from explicitly supplied images. |

---

## Notes from the Source Code

1. **Directory and BIDS Handling**:
   - For a directory source, DSI Studio first looks for BIDS DWI data, then other NIFTI DWI files, and otherwise falls back to DICOM conversion.
   - `--bids=1` can be used to request BIDS handling explicitly.

2. **Output Directory**:
   - If the `--output` parameter is not provided, the resulting SRC files are saved in the same directory as the input files.

3. **Reverse Phase Encoding**:
   - Reverse-phase data detected during directory/BIDS conversion are saved as an `.rz` companion when TOPUP/EDDY is not run during SRC generation.

4. **Error Handling**:
   - The tool performs various checks for mismatched b-values, b-vectors, and file compatibility, displaying error messages when issues are encountered.

5. **Default Behavior**:
   - If no NIFTI diffusion data are found under a directory `--source`, DSI Studio attempts DICOM conversion.
