# Region and Tract Functions

> Use `--action=ana` to initiate tract analysis, region analysis, or export functions in [Step T3 Fiber Tracking].

## Examples of Tract Functions:

The `ana` action supports various post-tracking functions, including `delete_repeat`, `output`, `export`, `end_point`, `ref`, `connectivity`, and `connectivity_type`. Additionally, it allows tract-to-region connectivity calculations using built-in atlases or ROI files.

*(Available in versions after 2024/10)*: Calculate the tract-to-region connectivity using tracts and a specified atlas or ROI file.

```bash
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --connectivity=HCP-MMP,Brodmann
dsi_studio --action=ana --source=my.fz --tract=bundle1.tt.gz,bundle2.tt.gz,bundle3.tt.gz --connectivity=HCP-MMP+subcortical.nii.gz
dsi_studio --action=ana --source=my.fz --tract=*.tt.gz --connectivity=HCP-MMP
```

### Merging and Filtering Tracks
*Merge two trk files and save them to a text file:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz,Tract2.tt.gz --output=Tracts1.txt
```

*Filter a track file by ROIs and save as another trk file:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts.tt.gz --other_regions=roi.nii.gz,roi2.nii.gz --output=filtered_track.tt.gz
```

### Conversion and Statistics
*Convert a trk file to an ROI file:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --output=ROI.nii.gz
```

*Generate tract density imaging from a trk file:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --export=tdi
```

*Get track statistics from a track file:*
```bash
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --export=stat
```

*Get track DKI and ODI statistics from a track file:*
```bash
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --other_regions=DKI.nii.gz,ODI.nii.gz --export=stat
```

*Save tracks to a different slice space:*
```bash
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --ref=t1w.nii.gz --output=tract_in_t1w.tt.gz
```

---

## Examples of Region Functions:

*Get statistics of multiple native-space ROIs:*
```bash
dsi_studio --action=ana --source=my.fz --other_regions=native_space_roi.nii.gz
```

*Get statistics of multiple MNI-space ROIs:*
```bash
dsi_studio --action=ana --source=my.fz --other_regions=mni_space_roi.nii.gz
```

*Get statistics from Freesurfer segmented ROIs:*
```bash
dsi_studio --action=ana --source=my.fz --other_regions=aparc+aseg.nii.gz
```

*Get statistics for ROIs of a built-in atlas:*
```bash
dsi_studio --action=ana --source=*.fz --region=HCP842_tractography:Cingulum_L,HCP842_tractography:Cingulum_R
```

---

## Examples of Export Functions:

*Export diffusion metrics from FIB files:*
```bash
dsi_studio --action=ana --source=*.fz --export=qa,iso,dti_fa,rd,ad
```

*Convert an SRC file to a 4D nifti file:*
```bash
dsi_studio --action=ana --source=test.sz --export=4dnii
```

*Export all fiber orientations in the FIB file:*
```bash
dsi_studio --action=ana --source=test.fz --export=dirs
```

---

## Tract Functions Parameters

| **Parameter**  | **Description**                                                                 |
|-----------------|---------------------------------------------------------------------------------|
| `tract`        | Specifies the tract file (e.g., *.trk.gz, *.tt.gz). Include `mni` in the file name for MNI space tracts. |
| `output`       | Assign output file for conversion or ROI generation.                           |
| `export`       | Export tract-related data (e.g., `stat`, `tdi`, `tdi_color`, `tdi_end`).        |

---

## Region Functions Parameters

| **Parameter**    | **Description**                                                                 |
|-------------------|---------------------------------------------------------------------------------|
| `other_regions`  | Specifies a NIFTI file of single/multiple ROIs for region statistics. Add `mni` for MNI-space regions. |
| `atlas`          | Specify a built-in atlas name for region analysis.                             |

---

## Notes from the Source Code:
- **Connectivity**: The `ana` action supports tract-to-region mapping using the `connectivity` parameter.
- **Atlas Loading**: Atlases are loaded via `atl_load_atlas` and support region extraction using `load_region`.
- **Tract Statistics**: The `get_tract_statistics` function calculates metrics like FA and ADC along tracts.
- **Region Statistics**: The `get_regions_statistics` function aggregates quantitative data for regions.
