# Region and Tract Functions

> use --action=`ana` to initiate tract analysis, region analysis, or export functions in [Step T3 Fiber Tracking]


## Examples of Tract Functions: 

The `ana` action can use any post-tracking functions listed under "--action=trk", including `delete_repeat`,`output`, `export`, `end_point`, `ref`, `connectivity`, `connectivity_type`, `connectivity_value`, and ROI related commands, such as `roi`, `roi2`, ...etc. Please check out `--action=trk` for details.

*Merge two trk files and save it to a txt file*
```
dsi_studio --action=ana --source=avg.mean.fib.gz --tract=Tracts1.tt.gz,Tract2.tt.gz --output=Tracts1.txt
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

*Get track statistics from a mni-space track file*
```
dsi_studio --action=ana --source=my.fib.gz --tract=tract_mni.tt.gz --export=stat    
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

*Get the statistics of multiple MNI-space ROIs (including 'mni' in the file name tells DSI Studio that this is an MNI space region)*
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

## Examples of Export Functions: 

*Get the diffusion metrics from fib files*
```
dsi_studio --action=ana --source=*.gqi.1.25.fib.gz --export=qa,iso,dti_fa,rd,ad
```

*Export all fiber orientations in the fib file*

```
dsi_studio --action=ana --source=test.fib.gz --export=dirs
```

*Convert an SRC file to a 4D nifti file*
```
dsi_studio --action=ana --source=test.src.gz --export=4dnii
```



## Core Functions

| Parameters   | Description                                                                 |
|:-------------|:------------------------------------------------------------------------------|
| source |  specify the fib.gz file  |

## Tract Functions
  
| Parameters  | Description                                                                 |
|:------------|:------------------------------------------------------------------------------|
| tract | Specify the tract file | Specify tractography file (*.trk.gz *.tt.gz). If the tracts are in the MNI-space, include `mni` in the file name (e.g. tract_mni.tt.gz) |
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

## Export Functions
If no tract or region is specified, then DSI Studio will export data from the FIB file or SRC file.

use "--export=qa,iso,dti_fa,ad,rd,md" to export the diffusion metrics as nifti files. 

use "--export=dirs" to export all fiber orientations as an 4D nifti file. The fiber orientations are unit vectors stored in the 4th dimension. Since each voxel may have multiple fiber orientations, each voxel can have, for example, 15 values, which is 5 fibers x 3 vector dimensions. The storage sequence is [x direction of 1st fiber][y direction of 1st fiber][z direction of 1st fiber][x direction of 2nd fiber][y direction of 2nd fiber]. You can also export the major fiber orientation only by --export=dir0 or the secondary by --export=dir1.

use "--export=4dnii" to export DWI data and b table from an SRC file. The output is a 4d nifti file and text files recording the b-table.

use the following matlab codes to get the values for x, y, and z directions:
```
I = load_nii('sample.dirs.nii.gz');
dir_x = squeeze(I.img(:,:,:,1));
dir_y = squeeze(I.img(:,:,:,2));
dir_z = squeeze(I.img(:,:,:,3)); 
```

use "--export=dir0" to export only the 1st (primary) fiber orientation as an 4D nifti file. The fiber orientations are stored in the 4th dimension. The sequence is [x of 1st fiber][y of 1st fiber]. To export the 2nd fiber, use "--export=dir1".
 
You can  combine multiple export targets. (e.g., --export=dirs,dir0,dir1,fa0,fa1)



