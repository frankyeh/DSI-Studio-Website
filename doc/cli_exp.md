# Export Metrics Data from FIB Files

> Use `--action=exp` to export data from a FIB file.

## Examples

**1. Export `dti_fa`, `ad`, and `md` from a FIB file:**
```bash
dsi_studio --action=exp --source=subject1.fz --export=dti_fa,ad,md
```

**2. Export `qa` and `iso` from all FIB files to the MNI space:** (supported after Oct/2025)
```bash
dsi_studio --action=exp --source=*.fz --export=qa,iso --export_to_mni 
```

---

## Core Functions

| **Parameter** | **Default** | **Description**                                                                 |
|---------------|-------------|---------------------------------------------------------------------------------|
| `source`      |             | Specify the path for the FIB file.                                              |
| `export`      |             | Specify the metrics to export (e.g., `qa`, `iso`, `dti_fa`, `ad`, `md`). Metrics are saved as `.nii.gz` files. |

to export image to the template space, specify --export_to_mni (supported after Oct/2025)

---

# Export Tract Data from TRK or TT Files

> Use `--action=exp` to export data from TRK or TT files.

## Examples

**1. Export TRK's tract data into a TT file:**
```bash
dsi_studio --action=exp --source=af.trk.gz --output=af.tt.gz
```

**2. Export all TT's tract data into TRK files:**
```bash
dsi_studio --action=exp --source=*.tt.gz --output=*.trk.gz
```

---

## Core Functions

| **Parameter** | **Default** | **Description**                                                                 |
|---------------|-------------|---------------------------------------------------------------------------------|
| `source`      |             | Specify the file name path for `.trk.gz` or `.tt.gz`.                           |
| `output`      |             | Specify the output file. The file name must end with `.tt.gz` or `.trk.gz`.     |

---

## Notes from the Source Code:

1. **Supported File Formats**:
   - `.fib.gz` / `.fz`: Metrics can be exported using the `export` parameter. Each metric will be saved as a `.nii.gz` file.
   - `.trk.gz`: Files can be converted to `.tt.gz` format using the `trk2tt` function.
   - `.tt.gz`: Files can be converted to `.trk.gz` format using the `tt2trk` function.

2. **Error Handling**:
   - If the FIB file is not a connectometry database or lacks demographics for matching, an error message is displayed.
   - Unsupported file formats will result in an error.

3. **Demographic Matching**:
   - Use the `match` parameter to save demographics-matched images. Example:
     ```bash
     dsi_studio --action=exp --source=database.fz --match=criteria.txt --output=matched_image.nii.gz
     ```

4. **Output Naming**:
   - Exported metrics are saved with the file name format: `<source>.<metric>.nii.gz`.
