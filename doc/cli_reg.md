# Registration

> use --action=`reg` to apply linear and/or nonlinear registration.

Update:
(2025/03/03) The registration has revised.

## Examples

*Apply linear and nonlinear registration to nonlinearly warp subject's QA and ISO maps to template QA and ISO maps (duel modality warping)
```
dsi_studio --action=reg --source=subject_qa.nii.gz,=subject_iso.nii.gz --to=template_qa.nii.gz,template_iso.nii.gz --output_mapping=mapping_field.mz
```

*Warp the subject image to the template space
```
dsi_studio --action=reg --source=subject_qa.nii.gz --mapping=mapping_field.mz
```

*Unwarp the template image to the subject space
```
dsi_studio --action=reg --to=template_qa.nii.gz --mapping=mapping_field.mz
```

*Warp all nifti image and all tract files from the template space to the subject space
```
dsi_studio --action=reg --source=*.nii.gz --mapping=mapping_field.mz
```

 
## Registration Functions

The function registers the subject/source image with the template/target image

| Parameters   |  Description                                                                 |
|:-----------------|:------------------------------------------------------------------------------|
| source | specify the nifti file(s) of the subject/source image. You may specify multiple modalities separated by comma |
| to | specify the nifti file of the template/target image.  You may specify multiple modalities separated by comma |
| output_mapping | (optional) specify the file to store the mapping field (e.g., --output_mapping=mapping.mz) | 
| mapping | specify the previous computed mapping and applied to --source or --to , which can take multiple nii.gz or tt.gz files separated by comma. |


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

