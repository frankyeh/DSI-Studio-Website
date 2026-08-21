# Registration

> Use `--action=reg` to apply linear and nonlinear registration. This command provides the same core functions used by the R1 linear and R2 nonlinear registration tools.

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

**4. Warp all matching NIfTI images from the subject/source space to the template/target space:**
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
| `source`            | Specify the NIfTI file(s) of the subject/source image. Multiple modalities can be specified, separated by commas. With `--mapping`, these files are warped source-to-target. |
| `to`                | Specify the NIfTI file(s) of the template/target image. Multiple modalities can be specified, separated by commas. With `--mapping`, these files are unwarped target-to-source. |
| `output_mapping`    | (Optional) Specify the file to store the mapping field (e.g., `--output_mapping=mapping.mz`). |
| `mapping`           | Specify a previously computed mapping file to apply to `--source` or `--to`. Multiple NIfTI or tractography files (e.g., `.nii.gz`, `.tt.gz`) can be separated by commas. |
| `s2t`               | Additional files to transform from subject-to-template space. Accepts multiple files separated by commas. Requires `--source` and `--to` to define the mapping. Results are stored in the template space. |
| `t2s`               | Additional files to transform from template-to-subject space. Accepts multiple files separated by commas. Requires `--source` and `--to` to define the mapping. Results are stored in the subject space. |
| `output`            | Optional output directory for warped/unwarped files. |

---

## Advanced Parameters

| **Parameter**         | **Default** | **Description**                                                                 |
|------------------------|-------------|---------------------------------------------------------------------------------|
| `reg_type`            | `1` | Linear registration type: `0`=rigid body; `1`=affine. The affine workflow proceeds to nonlinear registration unless `--skip_nonlinear=1` is used. |
| `cost_function`       | Depends on `reg_type` | Linear-registration cost: `mi` (mutual information) is the rigid-body default; `corr` (correlation) is the affine default. |
| `match_vs`            | `1` | Match source and target resolution before registration. |
| `resolution`          | Registration default | Relative resolution/downsampling used for nonlinear registration. |
| `speed`               | Registration default | Nonlinear deformation speed. |
| `smoothing`           | Registration default | Mapping-field smoothing for nonlinear registration. |
| `min_dimension`       | Registration default | Minimum dimension used by nonlinear registration. |
| `large_deform`        | `0` | Use larger linear-registration bounds when set to `1`. |
| `skip_linear`         | `0` | Skip the linear-registration stage when set to `1`. |
| `skip_nonlinear`      | `0` | Skip the nonlinear-registration stage when set to `1`. |
| `overwrite`           | `0` | Overwrite existing output files when set to `1`. |

---

## Notes from the Source Code
- **Dual Modality Support**: The registration function supports multiple modalities for both `--source` and `--to` parameters, ensuring alignment across modalities.
- **Mapping Files**: Mapping fields can be saved using the `--output_mapping` parameter and reused for warping/unwarping.
- **`s2t` and `t2s` Parameters**: These accept multiple files separated by commas and rely on `--source` and `--to` for the mapping definition.
- **Error Handling**: Errors are reported for unsupported file formats, mismatched dimensions, or invalid mapping files.
- **Output Formats**: Results can be saved in `.nii.gz` for images or `.tt.gz` for tractography.
