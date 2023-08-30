# Region and Tract Functions

> use --action=`ana` to initiate region or tract functions in [Step T3 Fiber Tracking]

The `ana` action can use any post-tracking functions listed under "--action=trk", including `delete_repeat`,`output`, `export`, `end_point`, `ref`, `connectivity`, `connectivity_type`, `connectivity_value`, and ROI related commands, such as `roi`, `roi2`, ...etc. Please check out `--action=trk` for details.

## Examples of Tract Functions: 

*Convert trk file to txt file*
```
dsi_studio --action=ana --source=avg.mean.fib.gz --tract=Tracts1.tt.gz--output=Tracts1.txt
```

*Read track trk file, filter it by ROIs, and output as another trk file*
```
dsi_studio --action=ana --source=avg.mean.fib.gz --tract=Tracts.tt.gz --roi=roi.nii.gz --roi2=roi2.nii.gz --output=filtered_track.tt.gz
```

*Convert trk file to ROI file*
```
dsi_studio --action=ana --source=avg.mean.fib.gz --tract=Tracts1.tt.gz --output=ROI.nii.gz
```

*Generate tract density imaging from a trk file.*
```
dsi_studio --action=ana --source=avg.mean.fib.gz --tract=Tracts1.tt.gz --export=tdi
```

*Get track statistics from a track file*
```
dsi_studio --action=ana --source=my.fib.gz --tract=tract.tt.gz --export=stat    
```

*Get track DKI and ODI statistics from a track file*
```
dsi_studio --action=ana --source=my.fib.gz --tract=tract.tt.gz --other_slices=DKI.nii.gz,ODI.nii.gz --export=stat    
```

*Calculate the connectivity using the tractography and ROI files*
```
dsi_studio --action=ana --source=my.fib.gz --tract=tract.tt.gz --connectivity=FreeSurferDKT,my_roi.nii.gz,another_roi.nii.gz --connectivity_value=qa,count,ncount --connectivity_type=pass,end
```

## Examples of Region Functions: 

*Get the statistics of multiple native-space ROIs*
```
dsi_studio --action=ana --source=my.fib.gz --regions=native_space_roi.nii.gz
```

*Get the statistics of multiple MNI-space ROIs*
```
dsi_studio --action=ana --source=my.fib.gz --regions=mni_space_roi.nii.gz
```

*Get statistics from Freesurfer segmented ROIs using subjects T1W to guide the registration*
```
dsi_studio --action=ana --source=my.fib.gz --regions=aparc+aseg.nii.gz --t1t2=subject_t1w.nii.gz
```

*Get statistics from ROIs of the atlas included in the DSI Studio package*
```
dsi_studio --action=ana --source=*.fib.gz --regions=HCP842_tractography:Cingulum_L,HCP842_tractography:Cingulum_R,HCP842_tractography:Corpus_Callosum
```

*Get the statistics from multiple ROIs of an atlas*
```
dsi_studio --action=ana --source=my.fib.gz --atlas=FreeSurferDKT
```

## Core Functions

| Parameters   | Description                                                                 |
|:-------------|:------------------------------------------------------------------------------|
| source |  specify the fib.gz file  |

## Tract Functions
  
| Parameters  | Description                                                                 |
|:------------|:------------------------------------------------------------------------------|
| tract | Specify the tract file |
| output | Use"--output=Tract.txt" to convert the trk file to another format or ROI (assigned output file as NIFTI file) |
| export | Export additional information related to the fiber tracts |

***Export tract density images***
- Use `--export=tdi` to generate a track density image in the diffusion space. To output in x2, x3, x4 resolution, use `tdi2`, `tdi3`, `tdi4` (also applied to the following commands).
- Use `--export=tdi_color` to generate a track color density image. 
- Use `--export=tdi_end` to output only the endpoint.

***Export tract profile***

- Use `--export=stat` to export tract statistics like along tract mean fa, adc, or morphology index such as volume, length, ... etc.
- Use `--export=report:dti_fa:0:1` to export the tract reports on "fa" values with a profile style at x-direction "0" and a bandwidth of "1"

The profile style can be the following:
  - `0`: x-direction
  - `1`: y-direction
  - `2`: z-direction
  - `3`: along tracts
  - `4`: mean of each tract 
  
You can export multiple outputs separated by ",". For example, 

`--export=stat,tdi,tdi2` exports track statistics, tract density images (TDI), subvoxel TDI, along tract qa values, and along tract gfa values.

## Region Functions

***Do not specify `--tract` if you want to use the region analysis function ***

| Parameters        | Description                                                                 |
|:------------------|:------------------------------------------------------------------------------|
| region or regions | specify the NIFTI file of a single ROI (--region) or multiple ROIs (--regions) to export region statistics. If the regions are in the MNI space, add "mni" to the file name (e.g. mni_regions.nii.gz) |
| t1t2 | if the region is derived from a T1W, then specify the t1w image using --t1t2=t1w.nii.gz for registration with the DWI.|
| atlas | Specify the built-in atlas name for export region statistics | 

Multiple files can be specified using "+" as the separator. The format can be a txt file or nifti file or from the atlas regions (e.g. --region=FreeSurferDKT:right_precentral+FreeSurferDKT:left_precentral)

The loaded regions can be modified using "smoothing", "erosion", "dilation",  "defragment", "negate", "flipx", "flipy", "flipz", "shiftx" (shift the roi by 1 in x direction), "shiftnx" (shift the roi by -1 in x direction), "shifty", "shiftny", "shiftz", "shiftnz". 

For example, --region=region.nii.gz,dilate,smoothing --regions=multiple_regions.nii.gz,flipx




