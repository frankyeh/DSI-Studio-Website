# Automatic Fiber Tracking

> use --action=`atk` to initiate automatic fiber tracking

Automatic fiber tracking maps individual bundles using deterministic fiber tracking and tract recognition. The implementation is detailed in Yeh, Fang-Cheng. "Shape analysis of the human association pathways." Neuroimage 223 (2020): 117329.

## Examples

*Run fiber tracking on all fib.gz files to map the left and arcuate fasciculus
```
dsi_studio --action=atk --source=*.fib.gz --track_id=Arcuate
```

*Run fiber tracking on all fib.gz files and output tracts to the template space
```
dsi_studio --action=atk --source=*.fib.gz --export_template_trk=1
```


## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source |  | specify fib.gz files for automatic bundle tracking.  |
| track_id | `Fasciculus,Cingulum,Aslant,Corticos,Thalamic_R,Reticular,Optic,Fornix,Corpus` | specify the id number or the name of the bundle. The id can be found in /atlas/ICBM152/HCP1065.tt.gz.txt . <p> This text file is included in DSI Studio package (For Mac, right-click on dsi_studio_64.app to find content). You can specify partial name of the bundle: <p>           example:<p>   for tracking left and right arcuate fasciculus, assign --track_id=0,1  or --track_id=arcuate    (DSI Studio will find bundles with names containing 'arcuate', case insensitive) <p>           example:<p>   for tracking left and right arcuate and cingulum, assign --track_id=0,1,2,3 or --track_id=arcuate,cingulum|
| tolerance | `22,26,30` | the tolerance for the bundle recognition. The unit is in mm. Multiple values can be assigned using comma separator. A larger value may include larger track variation but also subject to more false results. |
| track_voxel_ratio | `2.0` | the track-voxel ratio for the total number of streamline count. A larger value gives better mapping with the expense of computation time. 
| check_ending | `1` and `0` for cingulum | remove tracts if they terminate in high anisotropy locations. |
| thread_count | hardware max | Specify number of CPU cores used in computation |
| yield_rate | '0.00001' | This rate will be used to terminate tracking early if DSI Studio find the fiber trackings is not generating results |
| default_mask | `0` | Specify whether default mask is used. |
| overwrite | `0` | Specify whether to overwrite existing files. |
| export_stat | `1` | Specify whether to output track statistics. |
| export_trk | `1` | Specify whether to output tractography file. |
| export_template_trk | `0` | Specify whether to output tractography in the template space. |
| trk_format | `tt.gz` | Specify the postfix and the output format of the tractography. Supported formats include tt.gz trk trk.gz tck txt mat nii nii.gz. It also allows for changing the postfix of the filename |
| stat_format | `stat.txt` | Specify the postfix for the statistics text file (has to be in text format).  |
| output | the directory of the first file specified in --source | Specify the output directory | 
  
Fiber tracking parameters such as --otsu_threshold, --fa_Threshold, --turning_angle, --step_size, --smoothing, --tip_iteration are also supported. 
(--min_length and --max_length are not supported because the length constraint will be automaticalled determined from the atlas)

  
