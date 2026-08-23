# Rename DICOM Files

Use `--action=ren` to rename and organize DICOM files and, optionally, convert the resulting studies to DSI Studio source files and NIfTI.

## Examples

### Rename DICOM files under a directory

```bash
dsi_studio --action=ren --source=d:/raw_data
```

DSI Studio scans subdirectories, reads the DICOM metadata, and organizes files by patient, sequence, and image name.

### Rename and convert to SRC/NIfTI

```bash
dsi_studio --action=ren --source=d:/dicom --to_src_nii=1
```

### Allow conversion outputs to be overwritten

```bash
dsi_studio --action=ren --source=d:/dicom --to_src_nii=1 --overwrite=1
```

### Windows batch example

```bat
for /f "delims=" %%x in ('dir * /b') do (
    call dsi_studio.exe --action=ren --source="D:\MRI\CA\%%x" > %%x.log.txt
)
```

## Options

| Parameter | Default | Description |
|:--|:--|:--|
| `--source` | required | Directory containing the DICOM files. Subdirectories are scanned recursively. |
| `--output` | same as `source` | Root directory for the renamed DICOM hierarchy. |
| `--to_src_nii` | `0` | Set to `1` to convert the renamed DICOM studies to DSI Studio source (`.sz`) and NIfTI outputs. |
| `--overwrite` | `0` | When conversion is enabled, set to `1` to allow existing conversion outputs to be overwritten. |

## Important

This action **renames and moves DICOM files**. Keep a backup of the original data or test the workflow on a small copy before applying it to a large archive.
