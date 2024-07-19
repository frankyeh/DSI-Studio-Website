# Generating SRC files

> use --action=`src` to generate SRC file from NIFTI or DICOM files

## Examples

*Convert one 4d NIFTI file and generate an SRC file subject001.nii.gz.src.gz*

```
dsi_studio --action=src --source=c:\subject001.nii.gz
```

*Convert one 4d NIFTI file and specify the location of bval and bvec (only needed when DSI Studio cannot find them)*

```
dsi_studio --action=src --source=c:\subject001.nii.gz --bval=bval --bvec=bvec
```

*Convert DWI NIFTI files in a BIDS folder and output all SRC files to a folder

```
dsi_studio --action=src --source=./bids_root_folder --output=/src_files
```

*Convert DWI NIFTI files in a BIDS session folder and output one SRC file or its reverse phase encoding RSRC file

```
dsi_studio --action=src --source=./sub-01/ses-01/dwi --bids=1
```


*Combine two 4d NIFTI files and generate one SRC file*

```
dsi_studio --action=src --source=HCA9992517_V1_MR_dMRI_dir98_AP.nii.gz --other_source=HCA9992517_V1_MR_dMRI_dir99_AP.nii.gz
```

*Search all DICOM files under the assigned directory and output SRC file*

```
dsi_studio --action=src --source=C:\20081006_11_00814348_DWI_WIP_DSI_203 --output=c:\20081006_11_00814348_DWI_WIP_DSI_203.src.gz
```

*Find all *98_AP.nii.gz file and combine its corresponding 99_AP.nii.gz file to generate one combined SRC file*

```
dsi_studio --action=rec --source=*98_AP.nii.gz --other_source=*99_AP.nii.gz
```

*Parse all 4d NIFTI files in a folder (each of them has a bval and bvec file that shares a similar file name) and generate corresponding SRC files to a new folder*

```
dsi_studio --action=src --source=*.nii.gz --output=/src_folder
```

*Windows batch file for creating src files from HCP datasets*

```bat
path=C:\dsi_studio_64
cd F:\HCP\
dir ?????? /b > file_list.txt
for /f "delims=" %%x in (file_list.txt) do (
call dsi_studio.exe --action=src --source=F:\HCP\%%x\T1w\Diffusion\data.nii.gz --output=F:\%%x.src.gz > F:\%%x.txt
)
```

*Windows batch script for creating src from NIFTI data*

```bat
path=C:\dsi_studio_64
dir ????? /b > file_list.txt
for /f "delims=" %%x in (file_list.txt) do (
call dsi_studio.exe --action=src --source=%%x\%%x_dwi_QCed.nii --bval=%%x\%%x_QC.bval --bvec=%%x\%%x_QC.bvec --output=%%x.src.gz
)
```

## Core Functions

| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| source | Specify a directory storing DICOM/NIFTI files or the filename of a 4D NIFTI file |


## Optional Functions

| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| other_source | Specify other files to be included in the SRC file. Multiple files can be assigned using comma separator, (e.g. --other_source=1.nii.gz,2.nii.gz) |
| output | Assign the output src file name (.src.gz) or the output folder |
| b_table | Assign the text file to replace b-table |
| bval | Specify the location of the FSL bval file<sup>a</sup> |
| bvec | Specify the location of the FSL bvec file<sup>a</sup> |



<sup>a</sup>for most cases, DSI Studio can automatically associate bval and bvec with NIFTI automatically.

## Accessory Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| recursive | `0` | Search all NIFTI or DICOM files under the directory specified in --source |
| up_sampling | `0` | Upsampling the DWI, `0`:no resampling, `1`:upsample by2, `2`:downsample by2, `3`:upsample by 4, `4`:downsample by 4 |
| bids | `0` not bids | Specify whether to use BIDS standard to parse a folder specified by --source  |
| overwrite | `0` no overwrite | Specify whether to overwrite existing files |

    
