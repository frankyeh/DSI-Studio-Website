
# Region and Tract Functions

The `--action=ana` command is used for analyzing fiber tracts, calculating region-based statistics, or performing connectivity analysis within the [Step T3 Fiber Tracking] workflow.

## Examples of Tract Functions

The `ana` action supports post-tracking operations including merging, filtering, and quantifying tracts. Key parameters include `connectivity`, `connectivity_type`, `other_slices`, and `export`.

### Connectivity Analysis
Calculate tract-to-region connectivity using built-in atlases or custom ROI files. 
*Note: `connectivity_type` defaults to `pass` (tracts passing through the region). Use `end` to only count tracts ending within the region.*

```bash
# Connectivity using built-in atlases
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --connectivity=HCP-MMP,Brodmann

# Combining atlases with a custom NIFTI ROI
dsi_studio --action=ana --source=my.fz --tract=bundle.tt.gz --connectivity=HCP-MMP+subcortical.nii.gz

# Using end-point connectivity logic
dsi_studio --action=ana --source=my.fz --tract=*.tt.gz --connectivity=HCP-MMP --connectivity_type=end
```

### Merging and Filtering Tracks
*Merge multiple tract files into one:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz,Tract2.tt.gz --output=merged_tracts.tt.gz
```

*Filter tracts by regions and save the result:*
```bash
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts.tt.gz --roi=roi.nii.gz --output=filtered_track.tt.gz
```

### Conversion and Statistics
*Convert a tract file to a density map or ROI mask:*
```bash
# Save as NIFTI ROI (binary mask or probability map)
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --output=ROI.nii.gz

# Generate tract density imaging (TDI)
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --export=tdi
```

*Get tract statistics with additional metrics:*
Use `--other_slices` to load external maps (like DKI or ODI) for sampling along the tracts.
```bash
# Standard statistics (FA, MD, etc.)
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --export=stat

# Statistics including external DKI/ODI metrics
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --other_slices=DKI.nii.gz,ODI.nii.gz --export=stat
```

---

## Examples of Region Functions

Region analysis calculates quantitative metrics within specific volumes of interest. If the ROI file contains `.mni.` in the filename, DSI Studio automatically performs the necessary transformation from MNI space to the subject's native diffusion space.

*Get statistics for native or MNI space ROIs:*
```bash
dsi_studio --action=ana --source=my.fz --regions=native_roi.nii.gz,mni_space.mni.nii.gz
```

*Get statistics for built-in atlas regions:*
```bash
dsi_studio --action=ana --source=my.fz --regions=HCP842_tractography:Cingulum_L,HCP842_tractography:Cingulum_R
```

---

## Examples of Export Functions

To export volume-wide metrics or convert source files, use `--action=exp`.

*Export diffusion metrics from FIB files:*
```bash
dsi_studio --action=exp --source=subject.fz --export=qa,iso,dti_fa,rd,ad
```

*Convert an SRC file to a 4D NIFTI file:*
```bash
dsi_studio --action=exp --source=test.sz --export=4dnii
```

---

## Parameter Definitions

### Tract Parameters (`--action=ana`)

| Parameter | Description |
| :--- | :--- |
| `tract` | Specifies input tract files (*.trk.gz, *.tt.gz). |
| `output` | Specifies the output filename. Use `.tt.gz` for tracts or `.nii.gz` for ROI/density maps. |
| `export` | Exports specific tract data (e.g., `stat`, `tdi`, `tdi_color`, `tdi_end`). |
| `connectivity` | Lists atlases or NIFTI files for connectivity matrix calculation. |
| `connectivity_type` | Defines connectivity logic: `pass` (default) or `end`. |
| `other_slices` | Loads external NIFTI maps to be sampled for tract/region statistics. |
| `ref` | Specifies a reference image to define the grid space for TDI or output ROIs. |

### Region Parameters (`--action=ana`)

| Parameter | Description |
| :--- | :--- |
| `regions` | Specifies NIFTI files or atlas-labeled regions for statistical analysis. |
| `atlas` | Specifies a built-in atlas to be used for region analysis. |

---

## Technical Notes
- **Metric Sampling**: The `get_tract_statistics` function extracts diffusion metrics (FA, ADC, etc.) specifically along the coordinates of the fibers.
- **ROI Labels**: When loading NIFTI ROIs, DSI Studio checks for a corresponding `.txt` file in the same directory to assign region names to the indices.
- **Automated Alignment**: The system uses the `mni` keyword in filenames as a trigger to apply MNI-to-native coordinate transformations.
