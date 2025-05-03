# Region and Tract Functions

> Use `--action=ana` to initiate tract analysis, region analysis, or export functions in [Step T3 Fiber Tracking].

## Examples of Tract Functions:

The `ana` action can use any post-tracking functions listed under `--action=trk`, including `delete_repeat`, `output`, `export`, `end_point`, `ref`, `connectivity`, `connectivity_type`, and `connectivity_v[...]`.

*(Versions after 2024/10): Calculate the tract-to-region connectivity using the tracts and built-in atlas or ROI file.*

```
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --connectivity=HCP-MMP,Brodmann
dsi_studio --action=ana --source=my.fz --tract=bundle1.tt.gz,bundle2.tt.gz,bundle3.tt.gz --connectivity=HCP-MMP+subcortical.nii.gz
dsi_studio --action=ana --source=my.fz --tract=*.tt.gz --connectivity=HCP-MMP
```

*Merge two trk files and save them to a txt file:*
```
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz,Tract2.tt.gz --output=Tracts1.txt
```

*Read track trk file, filter it by ROIs, and output as another trk file:*
```
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts.tt.gz --other_regions=roi.nii.gz,roi2.nii.gz --output=filtered_track.tt.gz
```

*Convert trk file to ROI file:*
```
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --output=ROI.nii.gz
```

*Generate tract density imaging from a trk file:*
```
dsi_studio --action=ana --source=avg.mean.fz --tract=Tracts1.tt.gz --export=tdi
```

*Get track statistics from a track file:*
```
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --export=stat
```

*Get track statistics from a track file in MNI space (include "mni" in the file name):*
```
dsi_studio --action=ana --source=my.fz --tract=tract_mni.tt.gz --export=stat
```

*Get track DKI and ODI statistics from a track file:*
```
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --other_regions=DKI.nii.gz,ODI.nii.gz --export=stat
```

*Save tracks to a different slice space:*
```
dsi_studio --action=ana --source=my.fz --tract=tract.tt.gz --ref=t1w.nii.gz --output=tract_in_t1w.tt.gz
```

## Examples of Region Functions:

*Get the statistics of multiple native-space ROIs:*
```
dsi_studio --action=ana --source=my.fz --other_regions=native_space_roi.nii.gz
```

*Get the statistics of multiple MNI-space ROIs (include "mni" in the file name to indicate MNI space):*
```
dsi_studio --action=ana --source=my.fz --other_regions=mni_space_roi.nii.gz
```

*Get statistics from Freesurfer segmented ROIs:*
```
dsi_studio --action=ana --source=my.fz --other_regions=aparc+aseg.nii.gz
```

*Get statistics from ROIs of the atlas included in the DSI Studio package:*
```
dsi_studio --action=ana --source=*.fz --region=HCP842_tractography:Cingulum_L,HCP842_tractography:Cingulum_R,HCP842_tractography:Corpus_Callosum
```

*Get the statistics from multiple ROIs of an atlas:*
```
dsi_studio --action=ana --source=my.fz --atlas=HCP-MMP:L_V1+HCP-MMP:R_V1
```

## Examples of Export Functions:

*Get the diffusion metrics from FIB files (or .fz for newer versions):*
```
dsi_studio --action=ana --source=*.fz --export=qa,iso,dti_fa,rd,ad
```

*Export all fiber orientations in the FIB file:*
```
dsi_studio --action=ana --source=test.fz --export=dirs
```

*Convert an SRC file (or .sz for newer versions) to a 4D nifti file:*
```
dsi_studio --action=ana --source=test.sz --export=4dnii
```

*Export tracts from TT to TRK format:*
```
dsi_studio --action=exp --source=tracts.tt.gz --output=tracts.trk.gz
```

## Tract Functions

| Parameters  | Description                                                                 |
|:------------|:------------------------------------------------------------------------------|
| tract       | Specify the tract file. Tractography files (*.trk.gz *.tt.gz). If the tracts are in the MNI space, include `mni` in the file name (e.g., tract_mni.tt.gz). |
| output      | Use `--output=Tract.txt` to convert the trk file to another format or ROI (assigned output file as NIFTI file). |
| export      | Export additional information related to the fiber tracts. |

***Export tract density images***
- Use `--export=tdi` to generate a track density image in the diffusion space. To output in x2, x3, x4 resolution, use `tdi2`, `tdi3`, `tdi4`.
- Use `--export=tdi_color` to generate a track color density image.
- Use `--export=tdi_end` to output only the endpoint.

***Export tract profile***

- Use `--export=stat` to export tract statistics like along-tract mean fa, adc, or morphology indices like volume, length, etc.
- Use `--export=report:dti_fa:0:1` to export the tract reports on "fa" values with a profile style in the x-direction (0) and a bandwidth of 1.

The profile style can be the following:
  - `0`: x-direction
  - `1`: y-direction
  - `2`: z-direction
  - `3`: along tracts
  - `4`: mean of each tract

You can export multiple outputs separated by ",". For example:
```
--export=stat,tdi,tdi2
```
exports track statistics, tract density images (TDI), subvoxel TDI, along-tract qa values, and along-tract gfa values.

## Region Functions

***Do not specify `--tract` if you want to use the region analysis function.***

| Parameters        | Description                                                                 |
|:------------------|:------------------------------------------------------------------------------|
| other_regions     | Specify the NIFTI file of a single ROI (`--region`) or multiple ROIs (`--regions`) to export region statistics. If the regions are in the MNI space, add "mni" to the file name (e.g., mni_space_roi.nii.gz). |
| atlas             | Specify the built-in atlas name for export region statistics. |

Multiple files can be specified using "+" as the separator. The format can be a txt file, a nifti file, or from the atlas regions (e.g., `--region=HCP-MMP:L_V1+HCP-MMP:R_V1`).

The loaded regions can be modified using "smoothing", "erosion", "dilation", "defragment", "negate", "flipx", "flipy", "flipz", "shiftx" (shift the ROI by 1 in the x direction), or "shiftnx" (shift the ROI by -1 in x direction).

For example:
```
--region=region.nii.gz,dilate,smoothing --regions=multiple_regions.nii.gz,flipx
```

## Export Functions

If no tract or region is specified, then DSI Studio will export data from the FIB file or SRC file.

- Use `--export=qa,iso,dti_fa,ad,rd,md` to export the diffusion metrics as nifti files.
- Use `--export=dirs` to export all fiber orientations as a 4D nifti file. The fiber orientations are unit vectors stored in the 4th dimension. Since each voxel may have multiple fiber orientations, each orientation is stored sequentially.
- Use `--export=4dnii` to export DWI data and b-table from an SRC file. The output is a 4D nifti file and text files recording the b-table.

Example MATLAB code to extract directions:
```
I = load_nii('sample.dirs.nii.gz');
dir_x = squeeze(I.img(:,:,:,1));
dir_y = squeeze(I.img(:,:,:,2));
dir_z = squeeze(I.img(:,:,:,3));
```

- Use `--export=dir0` to export only the 1st (primary) fiber orientation as a 4D nifti file. The fiber orientations are stored in the 4th dimension. The sequence is [x of 1st fiber][y of 1st fiber].

You can combine multiple export targets. For example:
```
--export=dirs,dir0,dir1,fa0,fa1
```
