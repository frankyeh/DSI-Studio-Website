# Rename DICOM Files

> Use `--action=ren` to rename and optionally convert DICOM files.

The `ren` action allows users to rename DICOM files in a specified directory, search subdirectories, and optionally convert files to SRC and NIFTI formats.

---

## Examples

**1. Rename DICOM files under the `raw_data` directory:**
```bash
dsi_studio --action=ren --source=d:/raw_data
```

**2. Rename DICOM files under the `dicom` directory and create SRC and NIFTI files:**
```bash
dsi_studio --action=ren --source=d:/dicom --to_src_nii=1
```

**3. For each directory, rename DICOM files using a batch script:**
```bash
for /f "delims=" %%x in ('dir * /b') do (
    call dsi_studio.exe --action=ren --source="D:\MRI\CA\%%x" > %%x.log.txt 
)
```

---

## Core Functions

| **Parameter**   | **Default** | **Description**                                                                 |
|------------------|-------------|---------------------------------------------------------------------------------|
| `source`        |             | Specify the path to the directory containing the DICOM files. Subdirectories will be scanned automatically. |
| `output`        | Same as `source` | Specify the directory to output the renamed DICOM files. If not provided, the renamed files are saved in the same directory as the `source`. |
| `to_src_nii`    | `0`         | Specify whether to convert DICOM files to SRC and NIFTI formats. Set to `1` to enable conversion. |

---

## Notes from the Source Code
- **Subdirectory Scanning**: The tool automatically scans subdirectories in the provided `--source` path.
- **Conversion**: When `--to_src_nii=1` is specified, the renamed DICOM files are converted to SRC and NIFTI formats using the `dicom2src_and_nii` function.
- **Default Behavior**: If the `--output` parameter is not specified, renamed files are saved in the same directory as the `--source`.

This documentation provides detailed guidance on using the `ren` action for renaming and converting DICOM files. Let me know if further refinements are needed!
