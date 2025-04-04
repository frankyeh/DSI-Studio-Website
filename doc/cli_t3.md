# Fiber Tracking

use --action=`trk` to initiate fiber tracking in DSI Studio. 

The fiber tracking is a deterministic tracking approach doeumented in Yeh, Fang-Cheng, et al. "Deterministic diffusion fiber tracking improved by quantitative anisotropy." PloS one 8.11 (2013): e80713.

The pipeline also includes computations for connectivity matrices and tractometry.

For mapping individual bundle, please use [(automatic fiber tracking)](https://dsi-studio.labsolver.org/doc/cli_atk.html)

## Examples

*Whole brain tracking on all fib files and save tractograms*
```
dsi_studio --action=trk --source=*.fz --output=*.tt.gz
```
*Track the left arcuate fasciculus of a fib file*
```
dsi_studio --action=trk --source=subject01.fz --track_id=ArcuateFasciculusL --output=subject01.ArcuateFasciculusL.tt.gz
```
*(Versions after 2024/10) Tract-to-region connectivity between left arcuate fasciculus and HCP-MMP parcellation
```
dsi_studio --action=trk --source=subject01.fz --track_id=ArcuateFasciculusL --connectivity=HCP-MMP
```
*Perform fiber tracking with all fiber tracking parameters assigned by a parameter ID*
```
dsi_studio --action=trk --source=subject001.fz --parameter_id=c9A99193Fba3F2EFF013Fcb2041b96438813dcb
```
*Use the left and right precentral region from FreeSurferDKT atlas as the ROIs. Dilate them twice and smooth them for fiber tracking*
```
dsi_studio --action=trk --source=subject001.fz --roi=FreeSurferDKT_Cortical:left_precentral,dilate,dilate,smoothing --roi2=FreeSurferDKT_Cortical:right_precentral,dilate,dilate,smoothing
```
*Fiber tracking using two MNI-space ROIs (include MNI in the file name) and subject-space whole-brain seeding (from wholeBrain.nii)*
```
dsi_studio --action=trk --source=subject001.fz --seed=wholeBrain.nii --roi=mni_roi1.nii --roi2=mni_roi2.nii
```
*Perform fiber tracking and output connectivity matrix using HCP-MMP and AAL3 parcellation, respectively. Specify --output=no_file to skip saving the large tractography file*
```
dsi_studio --action=trk --source=subject001.fz --output=no_file --connectivity=HCP-MMP,FreeSurferDKT_Cortical --connectivity_value=count,qa,trk
```
*Perform fiber tracking and export track-specific statistics and TDI*
```
dsi_studio --action=trk --source=subject001.fz --output=tracks.tt.gz --export=stat,tdi
```
*Perform fiber tracking and recognize tracts
```
dsi_studio --action=trk --source=subject001.fz --output=tracks.tt.gz --recognize=cluster_info
```
*Perform fiber tracking and insert other metrics (DKI, FW_FA...etc) and export track-specific statistics*
```
dsi_studio --action=trk --source=subject001.fz --other_slices=./other/*.nii.gz --output=tracks.tt.gz --export=stat
```
*Perform differential tractography by comparing MNI-space FA map (include MNI in the file name) and subject's fa and tracking the decreased FA at 10%*
```
dsi_studio --action=trk --source=subject001.fz --other_slices=template_qa.nii.gz --dt_metric1=follow_up_qa --dt_meetric2=qa --dt_threshold=0.2 --seed_count=1000000 --min_length=30 --output=tracks.tt.gz
```
*(Windows System) Search for all fib files under the current directory and perform fiber tracking to get the FreeSurferDKT connectivity matrix*
```
path=C:\dsi_studio_64
for /f "delims=" %%x in ('dir *.fz /b /d /s') do (
call dsi_studio.exe --action=trk --source="%%x" --seed_count=1000000 --thread_count=8 --output=no_file --connectivity=FreeSurferDKT > "%%x.log.txt"
)
```

## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source         |  | specify the fib file |                  
| thread_count   | hardware max | specify the thread count. |

## Conventional Tracking
> Specify the tracking parameters below or replace them by using a single `--parameter_id`, which can be found at the method text under Step T3d after running fiber tracking.


**The following parameters better leave them as default (don't need to specify them) unless you have special purpose**

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| tract_count or seed_count | `0`| specify the number of tract or seed to be generated. The default (0) means let DSI Studio decide the number.  | 
| fa_threshold   | `0` | which means it is randomized): DSI Studio will select a random value between 0.5 Otsu and 0.7 Otsu threshold using a uniform distribution.
| turning_angle | `0` (randomized) | This threshold (in degrees) serves as a termination criterion. If two consecutive moving directions have a crossing angle above this threshold, the tracking will be terminated. <br> The default `0` will be a random selection of a value from 15 degrees to 90 degrees.|
| step_size | `0` (randomized) | Step size defines the moving distance in each tracking iteration. This unit is in millimeter-scale. The default value `0` will be a random selection of the step size from 1.0 to 3.0 voxel distance (for versions before June 2023, 0.5 to 1.5 voxels). |
| min_length | `30` (mm)| Length constraint that filters out the tracks that are either too short |
| max_length | `300` (mm) | Length constraint that filters out the tracks that are too long |

The following settings are also included in `--parameter_id` but  usually the default values are used.

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| method | `0` |  tracking methods 0:streamline , 1:rk4 |
| otsu_threshold | `0.6` | The default Otsu's threshold can be adjusted to any ratio. |
| tip_iteration | `0` | specify pruning iterations. If --track_id or --dt_threshold_index is specified, the default value is `16` |
| random_seed | `0` | specify the random number for fiber tracking. Specify a different interger to get different seed sequences. |

> Please note that parameter ID does not override ROI settings, dt_threshold_index, or threshold_index settings. You may still need to assign ROI/ROA...etc. in the command line.


## ROI Settings

| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| seed |  specify the seeding file. Supported file format includes text files and nifti files. |
| roi roi2 roi3 roi4 roi5 |  specify the roi files. Supported file format includes text files and nifti files. |
| roa roa2 roa3 roa4 roa5 |  specify the roa files. |
| end end2 |  specify the endpoint files. |
| ter ter2 ter3 ter4 ter5 |  specify the terminative region. |
| nend |  specify the file for non ending region. |
| lim |  specify the file for limiting region. |
| other_slices | specify t1w or t2w images as the ROI reference image |

> Multiple input is supported. You may use atlas regions as the roi, roa, end, or ter regions. Specify the atlas and region name (case sensitive!) as follows:
```
--roi=FreeSurferDKT:left_precentral  
```

> An ROI (and also other region types) can be modified by the following action code: "smoothing", "erosion", "dilation",  "defragment", "negate", "flipx", "flipy", "flipz", "shiftx" (shift the roi by 1 in x direction), "shiftnx" (shift the roi by -1 in x direction), "shifty", "shiftny", "shiftz", "shiftnz".
```
--roi=region.nii.gz,dilation,dilation,smoothing   #This dilates the region in the region.nii.gz file twice and smooth it
```
> You can assign the region value to be loaded
```
--roi=multiplre_region_nifti.gz:1    # only loads regions with value=1
```

> You can merge two regions, separated by a comma
```
--roi=region1.nii.gz+region2.nii.gz
```

> You can assign an MNI space NIFTI file. Just make sure to have MNI in the file name
```
--roi=mni_broca1.nii.gz
```

## Differential tracking settings

## Examples

*Differential fiber tracking on all patient's fib files against HCP1065 NQA MNI-space template to map pathways with 20% decreases (The QA nifti file needs to have mni in the file name to enable warping
```
dsi_studio --action=trk --source=*.fz --other_slices=HCP1065_nqa_mni.nii.gz --dt_metric2=qa --dt_metric1=HCP1065_nqa_mni --dt_threshold=0.2
```



| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| other_slices     | [option 1] specify the NIFTI file or to be inserted for differential tractography (e.g., --other_slices=pre.nii.gz,post.nii.gz)<br> [option 2]  specify a connectometry database with built-in demographics and specify subject's demographics using --subject_demo (e.g., --other_slices=study.db.fz --subject_demo=62,1)<br>If the image is in the MNI space, add `mni` in the filename (e.g. xxx.mni.nii.gz).<br>To disable registration, add `native` in the file name (e.g. qa.native.nii.gz). |
| dt_threshold | Assign percentage threshold for differential tractography. assign --dt_threshold=0.1 to detect more than 10% change. |
| dt_metric1 | Specify the first metrics for differential tracking |
| dt_metric2 | Specify the second metrics for differential tracking |
| dt_threshold_type | specify the calculation of the differences. 0: (m1-m2)/m1 1: (m1-m2)/m2 2:(m1-m2) | 

> To load slices as "MNI images", please include "mni" in the file name
```
--other_slices=mni_qa.nii.gz
```

## Automated fiber tracking settings

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| tractography_atlas | 0 | specify the atlas.<p>    0: association pathway<p>    1:cerebellum pathways<p>    2:commissure pathways<p>    3:cranial nerves<p>    4:long projection pathways<p>    5:short projection pathways  |
| track_id       | (not assigned) | Specify which tract to map using automatic fiber tracking. The complete list of tract can be found [here](https://github.com/frankyeh/data-atlas/blob/main/ICBM152_adult/ICBM152_adult.tt.gz.txt). partial name supported. |


## Tract-related post-processing and analysis

| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| delete_repeat  | assign the distance for removing repeat tracks (e.g. --delete_repeat=1 removes repeat tracks with distance smaller than 1 mm) |
| output | specify the output directory or output file name (tt.gz, trk.gz, or nii.gz ). Specify `no_file` to disable tractography output. <br> To output coordinates to the t1w space, specify `--ref=T1w.nii.gz`|
| ref | specify the file name (e.g. t1w.nii.gz) for output tracts in the that space |
| end_point | specify file name for output endpoint coordinates (.txt or .mat) |
| end_point1 | specify file name for output the first endpoint region in NIFTI format |
| end_point2 | specify file name for output the second endpoint region in the NIFTI format |
| template_track | specify file name for output tracts in the template space |
| other_slices | specify the file name of the other image modality for analysis. <br> Multiple files can be assigned by using comma separator (e.g. --other_slices=t1w.nii.gz,t2w.nii.gz). <br> Wildcard supported (e.g., --other_slices=*.nii.gz) <br> If the image is already registered, to avoid registration, the other image modality should have the same dimension as the DWI, and the file name should include "reg" (e.g. --other_slices=t1w_reg.nii.gz) |
| export |  export additional information related to the fiber tracts <br> use "--export=tdi" to generate track density image in the diffusion space. <br> use "--export=tdi2" to generate track density image in the subvoxel diffusion space. <br> use "--export=tdi_t1t2" to generate track density image in the t1w space specified by --ref=t1w.nii.gz.  <br> use "--export=tdi_color" or "--export=tdi2_color" to generate track color density image. <br> use "--export=stat" to export tracts statistics like along tract mean fa, adc, or morphology index such as volume, length, ... etc. <br> To export TDI endpoints, use tdi_end or tdi2_end. <br> use "--export=report:dti_fa:0:1" to export the tract reports on "fa" values with a profile style at x-direction "0" and a bandwidth of "1" <br> the profile style can be the following: <br> 0 x-direction <br> 1 y-direction <br> 2 z-direction <br> 3 along tracts <br> 4 mean of each tract <br> You can export multiple outputs separated by ",". For example, <br> --export=stat,tdi,tdi2 exports tract statistics, tract density images (TDI), subvoxel TDI, along tract qa values, and along tract gfa values. |
| recognize | Specify the output for the "recognize and cluster" function. Specify --recognize=cluster to save tract labels as cluster.label.txt and cluster.name.txt |

## Connectivity analysis

| Parameters   | Default         | Description                                                                 |
|:-------------|:-------|:------------------------------------------------------------------------------|
| connectivity |   |output connectivity matrix using ROIs or atlas as the matrix entry. |
| connectivity_threshold | `0.001` | specify the threshold ( in relative ratio to the max value) for calculating binarized graph measures and connectivity values. This means if the maximum connectivity count is 1000 tracks in the connectivity matrix, then at least 1000 x 0.001 = 1 track is needed to pass the threshold. Otherwise, the values will be set to zero.|
| connectivity_type | `pass` | specify whether to use "pass" or "end" to count the tracks. |
| connectivity_value | `count` | specify the way to calculate the matrix value. `count` outputs the number of tracks passing/ending in the regions. `ncount` outputs the number of tracks normalized by the median length. `mean_length` outputs the mean length of the tracks. `trk` outputs a trk file each connectivity matrix entry. `dti_fa` outputs mean FA  `qa` outputs mean QA. You can output any metrics or even metrics added using --other_slice. You can output multiple results by separating the parameters with ",". (e.g. `--connectivity_value=count,ncount,trk`) |
| connectivity_output | `matrix,connectogram,measure` | specify whether to output connectivity matrix (`matrix`), `connectogram`, or network measures (`measure`) |
   
> If you would like to use the built-in atlas, please specify the atlas name.
```
--connectivity=FreeSurferDKT_cortical,HCP-MMP
```
> If you would like to use subject space ROIs, specify the file name
```
--connectivity=/subject01/segmentation_in_dwi_space.nii.gz
```
> If you would like to use your customized MNI space atlas, specify the file name (must have `mni` in the file name)
```
--connectivity=mni_space_parcellations.nii.gz
```
> If the ROIs is segmented based on T1W, you will need to specify the original t1w file using --other_slices
```
--other_slices=subject_t1w.nii.gz --connectivity=parcellation_based_on_subject_t1w.nii.gz
```
> Please note that if the number of the connecting tracks is not enough (determined by connectivity_threshold), the connectivity value will not be calculated. This strategy is used to avoid the inclusion of accidental connections due to false tracks.

## Clustering settings
   
| Parameters   | Default         | Description                                                                 |
|:-------------|:-------|:------------------------------------------------------------------------------|
| cluster | optional | run track clustering after fiber tracking. 4 parameters separated by comma are needed: --cluster=METHOD_ID,CLUSTER_COUNT,RESOLUTION,OUTPUT FILENAME       METHOD_ID: 0=single-linkage clustering 1=k means 2=EM        CLUSTER_COUNT: The total number of clusters. In k-means or EM, this is the total number of clusters assigned. In single-linkage, it is the maximum number of clusters allowed to avoid over-segmentation (the remaining small clusters will be grouped in the last clusters).       RESOLUTION: only used in single-linkage clustering. The value is the mini meter resolution for merging the clusters.       OUTPUT FILENAME: the file name for output the cluster label (should be a text file). The file name should not contain a space. |

```
--cluster=0,500,2,label.txt  #run a single-linkage cluster with 500 maximum cluster count and 2-mm resolution, saved as label.txt
```
