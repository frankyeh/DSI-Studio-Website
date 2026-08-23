# Export Data

Use `--action=exp` to export voxelwise metrics from FIB files or convert tractography between TT and TRK formats.

## Export Metrics from FIB Files

Export selected metrics from one FIB file:

```bash
dsi_studio --action=exp --source=subject.fz --export=dti_fa,ad,md
```

Export QA and ISO from all FIB files and transform the exported images to MNI space:

```bash
dsi_studio --action=exp --source=*.fz --export=qa,iso --export_to_mni
```

| Parameter | Description |
|:--|:--|
| `--source` | Input `.fz` or legacy `.fib.gz` file. Wildcards can be used for batch processing. |
| `--export` | Comma-separated metric names to export, such as `qa,iso,dti_fa,ad,md`. Each metric is saved as `<source>.<metric>.nii.gz`. |
| `--export_to_mni` | Transform exported images to MNI space. |

## Convert Tractography Files

Convert TRK to TT:

```bash
dsi_studio --action=exp --source=af.trk.gz --output=af.tt.gz
```

Convert TT to TRK:

```bash
dsi_studio --action=exp --source=af.tt.gz --output=af.trk.gz
```

The conversion direction is determined by the input and output suffixes. `--action=exp` accepts `.trk.gz` → `.tt.gz` and `.tt.gz` → `.trk.gz` conversion.

## Demographic-Matched Image Export

For a FIB-format connectometry database that includes demographics, `--match` can create a demographics-matched image:

```bash
dsi_studio --action=exp --source=database.fz --match=criteria.txt --output=matched_image.nii.gz
```

This function requires connectometry database data and demographics stored in the input file. If `--output` is omitted, DSI Studio uses `<source>.matched.nii.gz`.
