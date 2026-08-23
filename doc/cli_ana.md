# Region and Tract Analysis

Use `--action=ana` to analyze existing tractography or regions with a FIB file. It supports tract statistics, tract density imaging, ROI filtering, connectivity analysis, and region-based quantitative measurements.

## Tract Analysis Examples

### Tract statistics

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --export=stat
```

Load additional scalar maps, such as DKI or NODDI outputs, and include them in tract statistics:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --other_slices=DKI.nii.gz,ODI.nii.gz \
  --export=stat
```

### Merge tract files

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract1.tt.gz,tract2.tt.gz \
  --output=merged_tracts.tt.gz
```

### Filter a tract by ROI

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tracts.tt.gz \
  --roi=roi.nii.gz \
  --output=filtered_tract.tt.gz
```

### Export tract density imaging

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract.tt.gz \
  --export=tdi
```

### Convert tractography to a NIFTI region/density map

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract.tt.gz \
  --output=tract_region.nii.gz
```

## Connectivity Analysis

Calculate connectivity using built-in atlases or NIFTI parcellations:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --connectivity=HCP-MMP,Brodmann
```

Use endpoint-based connectivity instead of the default pass-through definition:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --connectivity=HCP-MMP \
  --connectivity_type=end
```

| Parameter | Description |
|:--|:--|
| `--tract` | Input tractography files (`.tt.gz` or `.trk.gz`). Multiple files can be comma-separated. |
| `--output` | Output tractography or NIFTI filename, depending on the requested operation. |
| `--export` | Tract output such as `stat`, `tdi`, `tdi_color`, or `tdi_end`. |
| `--connectivity` | Comma-separated built-in atlas names or NIFTI parcellations used for connectivity calculation. |
| `--connectivity_type` | `pass` (default) or `end`. |
| `--other_slices` | Additional NIFTI maps sampled for tract/region statistics. |
| `--ref` | Reference image defining the output grid for TDI or NIFTI output. |

## Region Analysis

Analyze one or more native-space regions:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --region=roi1.nii.gz,roi2.nii.gz
```

Analyze regions from a labeled NIFTI file or built-in atlas:

```bash
dsi_studio --action=ana --source=my.fz --region=labels.nii.gz:Hippocampus

dsi_studio --action=ana --source=my.fz --region=AAL2:Hippocampus_L
```

Common `--region` forms include:

```text
--region=mask.nii.gz
--region=labels.nii.gz
--region=labels.nii.gz:Hippocampus
--region=AAL2:Hippocampus_L
--region=roi1.nii.gz,roi2.nii.gz
```

| Parameter | Description |
|:--|:--|
| `--region` | NIFTI region files or atlas-qualified region names used for quantitative statistics. |
| `--atlas` | Built-in atlas selection for region analysis when required by the workflow. |

If a NIFTI region filename contains `mni`, DSI Studio treats it as an MNI-space image and maps it to native diffusion space when the required transformation is available. A labeled NIFTI can use a matching `.txt` or `.json` label file; FreeSurfer `aparc`/`aseg` files use the built-in FreeSurfer lookup table.

When a native-space region has a different image geometry from the FIB file, use `--other_slices` to load its anatomical reference so DSI Studio can apply the corresponding registration.

## Exporting Whole-Volume Metrics

Whole-volume diffusion metrics are exported with [`--action=exp`](/doc/cli_exp.html), for example:

```bash
dsi_studio --action=exp --source=subject.fz --export=qa,iso,dti_fa,rd,ad
```
