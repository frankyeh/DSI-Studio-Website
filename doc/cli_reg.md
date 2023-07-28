# Registration

> use --action=`reg` to apply linear and/or nonlinear registration.

## Examples

*Apply linear and nonlinear registration to register subject's QA/ISO to template QA/ISO
```
dsi_studio --action=reg --from=subject_qa.nii.gz --from2=subject_iso.nii.gz --to=template_qa.nii.gz --to2=template_iso.nii.gz --output=subject_qa_in_template_space.nii.gz --output_warp=warp_field.map.gz
```

*Warp the subject image to the template space
```
dsi_studio --action=reg --apply_warp=subject_qa.nii.gz --warp=warp_field.nii.gz
```

*Warp all nifti image and all tract files from the template space to the subject space
```
dsi_studio --action=reg --apply_warp=*.nii.gz,*.tt.gz --inv_warp=warp_field.nii.gz
```

 
## Registration Functions

The function registers the subject/source image with the template/target image

| Parameters   |  Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| from | specify the nifti file of the subject/source image. <br> the image can be smoothed by specifying --from=t1w.nii.gz+gaussian or --from=t1w.nii.gz+mean |
| from2 | (optional) specify the nifti file of the second image modality of the subject/source image. |
| to | specify the nifti file of the template/target image.  |
| to2 | (optional) specify the nifti file of the second image modality of the template/target image. |
| output | (optional) specify the nifti file of the warped subject/source image. |
| output_warp | (optional) specify the file to store the warping field (e.g., --output_warp=reg.map.gz) | 
| apply_warp | specify the image (*.nii.gz) or tracts (*.tt.gz) to be warped or unwarped from the subject space to the template space. Use comma to specify multiple files.|


| Parameters |Default |  Description                                                                 |
|:--------|:----------|:----------|
| cost_function | mi | specify the cost function to linear registration. <br> mi: mutual information <br> cc: correlation coefficient |
| reg_type | 1 | specify whether nonlinear registration is needed. set it to 0 if only linear registration is needed |
| normalize_signal | 1 | specify whether pre-registration signal normalization is needed. The normalization scales the image value between 0 and 1 | 
| resolution | 2 | Specify the final resolution. The value can be 1,2,4,8. 2 means the nonlinear registration will be conducted at x2 down sampling. | 
| speed | 1 | specify the convergence speed for nonlinear registration |
| smoothing | 0.2 | specify the mapping field smoothing for nonlinear registration. The value can be 0 (no smoothing) or 0.95 (more smoothing applied) | 
| iterations | 200 | specify number of iterations applied to compute nonlinear registration |
| min_dimension | 8 | specify the minimum dimension to start the nonlinear registration. |

## Warping Functions

The function applies warping to the subject/source image, or applies inversed warping to the template/target image. 

| Parameters            |  Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| warp or inv_warp  | specify the mapping data (.map.gz) to warp (use --warp=) or unwarp (--inv_warp=) the image |
| apply_warp | specify the image (*.nii.gz) or tracts (*.tt.gz) to be warped or unwarped. |

