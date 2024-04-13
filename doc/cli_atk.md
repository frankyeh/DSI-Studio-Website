# Automatic Fiber Tracking

> Use --action=`atk` to initiate automatic fiber tracking

Automatic fiber tracking maps individual bundles using deterministic fiber tracking and tract recognition. The implementation is detailed in Yeh, Fang-Cheng. "Shape analysis of the human association pathways." Neuroimage 223 (2020): 117329.

## Examples

*Run fiber tracking on all fib.gz files to map all association pathways
```
dsi_studio --action=atk --source=*.fib.gz
```

*Run fiber tracking on all fib.gz files to map corticospinal tracts and optic radiation
```
dsi_studio --action=atk --source=*.fib.gz --track_id=Corticos,Optic
```


*Run fiber tracking on all fib.gz files and output tracts to the template space
```
dsi_studio --action=atk --source=*.fib.gz --export_template_trk=1
```


## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source |  | Specify fib.gz files for automatic bundle tracking.  |
| track_id | `Arcuate,Cingulum,Aslant,InferiorFronto,InferiorLongitudinal,SuperiorLongitudinal,Uncinate,Fornix,Corticos,ThalamicR,Optic,Lemniscus,Reticular,Corpus`| Specify the name or the bundles separated by commas. A partial name is accepted. The complete list of the available pathways can be found [here](https://github.com/frankyeh/DSI-Studio-atlas/blob/main/ICBM152_adult/ICBM152_adult.tt.gz.txt)<br>    example:<br>   for tracking left and right arcuate fasciculus, assign --track_id=arcuate    (DSI Studio will find bundles with names containing 'arcuate', case insensitive) <br>    example:<br>   for tracking left and right arcuate and cingulum, assign --track_id=arcuate,cingulum|
| template | 0 | Specify the template. Current DSI Studio only has atk for ICBM152,INDI_rhesus,Pitt_Marmoset :<br>`0`:ICBM152<br>`1`:CIVM_mouse<br>`2`:Neonate<br>`3`:INDI_rhesus<br>`4`:Pitt_Marmoset<br>`5`:WHS_SD_rat |
| tolerance | `22,26,30` | the tolerance for bundle recognition. The unit is in mm. Multiple values can be assigned using a comma separator. A larger value may include larger track variation but also subject to more false results. |
| track_voxel_ratio | `2.0` | the track-voxel ratio for the total number of the streamline count. A larger value gives better mapping at the expense of computation time. 
| check_ending | `1` and `0` for cingulum | remove tracts if they terminate in high anisotropy locations. |
| thread_count | hardware max | Specify the number of CPU cores used in computation |
| yield_rate | '0.00001' | This rate will be used to terminate tracking early if DSI Studio finds the fiber trackings are not generating results |
| default_mask | `0` | Specify whether the default mask is used. |
| overwrite | `0` | Specify whether to overwrite existing files. |
| export_stat | `1` | Specify whether to output track statistics. |
| export_trk | `1` | Specify whether to output the tractography file. |
| export_template_trk | `0` | Specify whether to output tractography in the template space. |
| trk_format | `tt.gz` | Specify the postfix and the output format of the tractography. Supported formats include tt.gz trk trk.gz tck txt mat nii nii.gz. It also allows for changing the postfix of the filename |
| stat_format | `stat.txt` | Specify the postfix for the statistics text file (has to be in text format).  |
| output | the directory of the first file specified in --source | Specify the output directory | 
  
Fiber tracking parameters such as --otsu_threshold, --fa_Threshold, --turning_angle, --step_size, --smoothing, --tip_iteration are also supported. 
(--min_length and --max_length are not supported because the length constraint will be automatically determined from the atlas)

  
