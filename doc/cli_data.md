# DSI Studio File Formats

DSI Studio uses compact intermediate formats for diffusion MRI processing. For normal analysis, use DSI Studio to read, write, and export these files rather than modifying their internal matrices directly.

## Current formats

| Format | Purpose |
|:--|:--|
| `.sz` | Source diffusion data used for reconstruction. Stores processed DWI signals, the b-table, image geometry, mask, and related metadata. |
| `.rz` | Reverse-phase-encoding source data used with TOPUP/EDDY workflows. |
| `.fz` | Reconstructed fiber information used for tractography, diffusion metrics, connectomes, and other analyses. |
| `.dz` | Population/connectometry database. Current `.dz` databases can store multiple diffusion indices in one file together with subject information and optional demographics. |
| `.tt.gz` | DSI Studio TinyTrack tractography file. |
| `.nii` / `.nii.gz` | NIfTI exchange format for images and exported scalar maps. |

Legacy files such as `.src.gz`, `.fib.gz`, `.db.fz`, and `.db.fib.gz` remain supported in compatible workflows, but new analyses should normally use `.sz`, `.fz`, and `.dz`.

## `.sz` source files

An `.sz` file is the current source-data container used before reconstruction. It contains the diffusion-weighted signals and b-table needed to reconstruct an `.fz` file.

Common matrices include:

| Matrix | Description |
|:--|:--|
| `dimension` | Image dimensions. |
| `voxel_size` | Voxel size in millimeters. |
| `b_table` | Diffusion b-values and gradient directions. |
| `mask` | Spatial mask used by the compact representation. |
| `image0`, `image1`, ... | Diffusion-weighted image data. |
| `report` | Processing/report information when available. |

Compact matrices may store values only inside the mask. Scaling parameters can be stored as `matrix_name.slope` and `matrix_name.inter`, with restored values calculated as:

```text
value = stored_value * slope + inter
```

Because compact files can use matrix names containing periods, they should not be treated as ordinary MATLAB files. Python tools such as `scipy.io` can inspect the decompressed MAT v4 container, but DSI Studio is the preferred interface for conversion and export.

### Export DWI to NIfTI

Use DSI Studio rather than reconstructing the 4D volume manually:

```bash
dsi_studio --action=rec --source=data.sz --save_nii=data.nii.gz
```

## `.fz` fiber files

An `.fz` file contains reconstructed diffusion information used by Step T3 and command-line tractography. Depending on the reconstruction and requested outputs, it can include:

- fiber orientations and anisotropy values;
- orientation indices or directional vectors;
- DTI-derived measurements;
- QA and other GQI-derived measurements;
- additional scalar maps requested during reconstruction;
- image geometry, transformation, and reconstruction metadata.

Compact `.fz` matrices use the same masked/scaled storage concept described above.

### Export a metric from `.fz`

For example:

```bash
dsi_studio --action=exp --source=data.fz --export=dti_fa
```

Use [Export Files](/doc/cli_exp.html) for current export options.

## `.dz` connectometry databases

`.dz` is the current population/connectometry database format. A database is created from subject FIB files and stores subject measurements in a common template space.

Unlike older single-metric database files, a current `.dz` database can store multiple available diffusion indices in one file.

Example:

```bash
dsi_studio --action=atl --cmd=db --source=*.fz --output=study.dz
```

See [Correlational Tractography](/doc/gui_cx.html) and [Connectometry CLI](/doc/cli_cnt.html) for database creation and analysis.

## `.tt.gz` tractography files

TinyTrack (`.tt.gz`) is DSI Studio's compact tractography format. It stores streamline coordinates together with the image geometry needed to interpret them.

For interoperability, DSI Studio can export tractography and tract-derived measurements through the GUI and command line. Prefer those exports over parsing the binary track matrix unless direct format access is specifically required.

## Inspecting `.sz` or `.fz` with Python

The following example safely decompresses a file to a temporary MAT file and lists its matrices without overwriting the original file:

```python
from pathlib import Path
import gzip
import shutil
import tempfile
import scipy.io


def list_dsi_matrices(filename):
    filename = Path(filename)
    temp_name = None
    try:
        with tempfile.NamedTemporaryFile(suffix=".mat", delete=False) as temp:
            temp_name = temp.name
            with gzip.open(filename, "rb") as src:
                shutil.copyfileobj(src, temp)
        return scipy.io.whosmat(temp_name)
    finally:
        if temp_name:
            Path(temp_name).unlink(missing_ok=True)


for name, shape, data_type in list_dsi_matrices("data.fz"):
    print(name, shape, data_type)
```

Internal matrices are implementation details and can evolve between versions. For reproducible pipelines, use DSI Studio's documented import/export commands whenever possible.

## Loading MRtrix3 FOD peaks

Convert an MRtrix3 FOD image to peak directions:

```bash
sh2peaks fod.mif peak.mif
mrconvert peak.mif -stride 1,2,3,4 peak.nii.gz
```

The resulting `peak.nii.gz` can be opened in **Step T3: Fiber Tracking**.
