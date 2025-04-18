# Step T2 Reconstruction

> use --action=`rec` to initiate fiber tracking in DSI Studio

## Examples

*Reconstruct all SRC files with GQI using 1.25 mean diffusion distance and output all FIB files to a folder named fib*

```
dsi_studio --action=rec --source=*.sz --method=4 --param0=1.25 --output=/fib/
```

*Reconstruct all SRC files after EDDY correction 

```
dsi_studio --action=rec --source=subject1.sz --cmd="[Step T2][Corrections][EDDY]" --method=4 --param0=1.25
```

*Reconstruct SRC file after TOPUP/EDDY correction using reverse-phase encoding b0 stored in the nifti file.*

```
dsi_studio --action=rec --source=*.sz --rev_pe=*.rev_b0.nii.gz --method=4 --param0=1.25
```

*Apply TOPUP/EDDY to the src file using the corresponding reverse PE image and save the preprocessed data to a new SRC file (no reconstruction done).*

```
dsi_studio --action=rec --source=*_s04_dMRI_dir258_1_HDFT.sz --rev_pe=*_s09_dMRI_dir258_2_HDFT.nii.gz --save_src=*_proc.sz
```

*QSDR reconstruction with 1.25 sampling length ratio. The t1w and t2w were also warped with QSDR*

```
dsi_studio --action=rec --source=20081006_M025Y_1Shell.sz --method=7 --other_image=t1w:my_tiw.nii.gz,t2w:my_t2w.nii.gz
```

*A bash script that reconstructs all src.gz files in the directory*

```bash
#!/bin/bash
# Reconstruct all src.gz file in the directory
# Output logging
exec 1>log_qsdr.out 2>&1

# List all src.gz files
subs=$(ls *.sz)

# Reconstruction Parameters
method=7          # 7 for QSDR
param0="1.25"

for sub in $subs
do
    echo
        dsi_studio --action=rec --source=${sub} --method=${method} --param0=${param0} --record_odf=1
    echo
done
```


## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| source |  | specify the .sz file for reconstruction. |
| method | `4` | specify the reconstruction methods.<br> 0:DSI, 1:DTI, 4:GQI 7:QSDR.|
| param0 | `1.25` (in-vivo) or `0.6` (ex-vivo)| the diffusion sampling length ratio for GQI and QSDR reconstruction. |
| other_output | `fa,rd,rdi` | specify what diffusion metrics to calculate. add `odf` to output the odf data. use `all` to get all of possible metrics |
| qsdr_reso | `2.0` | specify output resolution for QSDR reconstruction |
| template | `0` | specify the template for QSDR reconstruction:<br>`0`:ICBM152<br>`1`:CIVM_mouse<br>`2`:Neonate<br>`3`:INDI_rhesus<br>`4`:Pitt_Marmoset<br>`5`:WHS_SD_rat |

## Preprocessing Operation

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| rev_pe | (empty) | Specify the NIFTI, RSRC, or SRC file of the reversed-phase encoding images for TOPUP/EDDY. specify empty parameter (i.e. '--rev_pe') to automatically search for RSRC or NIFTI files that can be used. |
| volume_correction | 0 (human) 1 (non-human) | Set --volume_correction=1 to apply automatic volume orientation correction by swapping or flipping xyz axis. |     
| check_btable | 0 (human) 1 (non-human) | Set --check_btable=1 to test b-table orientation and apply automatic flipping |
| motion_correction | 0 | Set --motion_correction=1 to apply rigid-body transformation to align DWI volumes. The b-table is also rotated accordingly |     
| make_isotropic | `2.0` for human scans or slice thickness for nonhuman | Resample DWI to an isotropic resolution of the specified resolution (i.e. --make_isotropic=2.0) |
| align_acpc | Set --align_acpc=1.5 to rotate image volume to align ap-pc and resample volume to 1.5 mm. |


## Additional Operation

| Parameters            | Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| mask | Specify the mask file (.nii.gz). To use U-Net, specify --mask=unet |
| rotate_to  | Specify a T1W or T2W for DSI Studio to rotate DWI to its space. (no scaling or shearing) |
| align_to  | Specify a T1W or T2W for DSI Studio to use affine transform to its space. (including scaling or shearing) |
| other_image  | Assign other image volumes (e.g., T1W, T2W image) to be wrapped with QSDR. --other_image=t1w:/path_to_t1w.nii.gz,t2w:/path_to/t2w.nii.gz |
| save_src | Save preprocessed images to a new SRC file |
| save_nii | Save preprocessed images back to 4d NIFTI file |
| cmd  | Specify any of the following commands for preprocessing. Use "+" to combine commands, and use "=" to assign value/parameters (e.g. --cmd="[Step T2][Corrections][EDDY]+[Step T2][Edit][Overwrite Voxel Size]=1.0" |

**available command list**

     [Step T2][File][Save 4D NIFTI]
     [Step T2][File][Save Src File]
     
     [Step T2][Edit][Image flip x]
     [Step T2][Edit][Image flip y]
     [Step T2][Edit][Image flip z]
     [Step T2][Edit][Image swap xy]
     [Step T2][Edit][Image swap yz]
     [Step T2][Edit][Image swap xz]
     [Step T2][Edit][Crop Background]
     [Step T2][Edit][Smooth Signals]
     [Step T2][Edit][Align ACPC]
     
     [Step T2][B-table][flip bx]
     [Step T2][B-table][flip by]
     [Step T2][B-table][flip bz]
     [Step T2][B-table][swap bxby]
     [Step T2][B-table][swap bybz]
     [Step T2][B-table][swap bxbz]
     
     [Step T2][Edit][Overwrite Voxel Size]=1.0
     [Step T2][Edit][Resample]=1.0
     [Step T2][Edit][Overwrite Voxel Size]
     [Step T2][Corrections][TOPUP EDDY]
     [Step T2][Corrections][EDDY]
     
     [Step T2a][Open]     # specify mask
     [Step T2a][Smoothing]
     [Step T2a][Defragment]
     [Step T2a][Dilation]
     [Step T2a][Erosion]
     [Step T2a][Negate]
     [Step T2a][Remove Background]
     [Step T2a][Threshold]=100      

     [Step T2b(2)][Partial FOV]
     
## Accessory Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| thread_count | system thread | number of multi-threads used to conduct reconstruction |
| dti_no_high_b | `1` for human data, `0` for animal data |  specify whether the construction of DTI should ignore high b-value (b>1500) |
| r2_weighted | `0` | specify whether GQI and QSDR uses r2-weighted to calculate SDF |
