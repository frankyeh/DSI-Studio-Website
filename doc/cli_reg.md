# Registration

> Use `--action=reg` to apply linear and/or nonlinear registration. This command line provides the same functions used at [R1: Linear Registration] and [R2" Nonlinear Registration] registration tools.

## Update
**(2025/03/03)** The registration syntax has been revised.

---

## Examples

**1. Apply linear and nonlinear registration to warp the subject's QA and ISO maps to template QA and ISO maps (dual modality warping):**
```bash
dsi_studio --action=reg --source=subject_qa.nii.gz,subject_iso.nii.gz --to=template_qa.nii.gz,template_iso.nii.gz --output_mapping=mapping_field.mz
```

**2. Warp the subject image to the template space using a previously computed mapping field:**
```bash
dsi_studio --action=reg --source=subject_qa.nii.gz --mapping=mapping_field.mz
```

**3. Unwarp the template image to the subject space using a previously computed mapping field:**
```bash
dsi_studio --action=reg --to=template_qa.nii.gz --mapping=mapping_field.mz
```

**4. Warp all NIfTI images and tract files from the template space to the subject space:**
```bash
dsi_studio --action=reg --source=*.nii.gz --mapping=mapping_field.mz
```

**5. Transform specific files from subject-to-template space (requires `--source` and `--to` to define the mapping):**
```bash
dsi_studio --action=reg --source=subject_qa.nii.gz --to=template_qa.nii.gz --s2t=additional_image1.nii.gz,additional_image2.nii.gz
```

**6. Transform specific files from template-to-subject space (requires `--source` and `--to` to define the mapping):**
```bash
dsi_studio --action=reg --source=subject_qa.nii.gz --to=template_qa.nii.gz --t2s=additional_image1.nii.gz,additional_image2.nii.gz
```

---

## Registration Functions

The function warps the subject/source image to the template/target image.

| **Parameter**       | **Description**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| `source`            | Specify the NIfTI file(s) of the subject/source image. Multiple modalities can be specified, separated by commas. |
| `to`                | Specify the NIfTI file(s) of the template/target image. Multiple modalities can be specified, separated by commas. |
| `output_mapping`    | (Optional) Specify the file to store the mapping field (e.g., `--output_mapping=mapping.mz`). |
| `mapping`           | Specify a previously computed mapping file to apply to `--source` or `--to`. Multiple NIfTI or tractography files (e.g., `.nii.gz`, `.tt.gz`) can be separated by commas. |
| `s2t`               | Additional files to transform from subject-to-template space. Accepts multiple files separated by commas. Requires `--source` and `--to` to define the mapping. Results are stored in the template space. |
| `t2s`               | Additional files to transform from template-to-subject space. Accepts multiple files separated by commas. Requires `--source` and `--to` to define the mapping. Results are stored in the subject space. |

---

## Advanced Parameters

| **Parameter**         | **Default** | **Description**                                                                 |
|------------------------|-------------|---------------------------------------------------------------------------------|
| `cost_function`       | `mi`        | Specify the cost function for linear registration:<br> `mi`: mutual information<br> `corr`: correlation coefficient. |
| `reg_type`            | `1`         | Specify whether nonlinear registration is needed. Set to `0` for linear registration only. |
| `normalize_signal`    | `1`         | Perform pre-registration signal normalization (scales image values between 0 and 1). |
| `resolution`          | `2`         | Specify the final resolution. Options are: `1`, `2`, `4`, `8` (e.g., `2` means nonlinear registration is conducted at x2 downsampling). |
| `speed`               | `1`         | Set the convergence speed for nonlinear registration.                           |
| `smoothing`           | `0.2`       | Adjust mapping field smoothing for nonlinear registration. Range: `0` (no smoothing) to `0.95` (maximum smoothing). |
| `iterations`          | `200`       | Number of iterations for nonlinear registration.                                |
| `min_dimension`       | `8`         | Minimum dimension to start nonlinear registration.                              |
| `large_deform`        | `0`         | Use large deformation registration bounds if set to `1`.                       |
| `skip_linear`         | `0`         | Skip linear registration stage if set to `1`.                                   |
| `skip_nonlinear`      | `0`         | Skip nonlinear registration stage if set to `1`.                                |
| `overwrite`           | `0`         | Overwrite existing output files if set to `1`.                                  |

---

## Notes from the Source Code
- **Dual Modality Support**: The registration function supports multiple modalities for both `--source` and `--to` parameters, ensuring alignment across modalities.
- **Mapping Files**: Mapping fields can be saved using the `--output_mapping` parameter and reused for warping/unwarping.
- **`s2t` and `t2s` Parameters**: These accept multiple files separated by commas and rely on `--source` and `--to` for the mapping definition.
- **Error Handling**: Errors are reported for unsupported file formats, mismatched dimensions, or invalid mapping files.
- **Output Formats**: Results can be saved in `.nii.gz` for images or `.tt.gz` for tractography.
