# Registration

Use `--action=reg` for linear/nonlinear registration, saving reusable mapping fields, and applying those mappings to images or tractography.

## Examples

### Register a subject to a template

Use two modalities for registration and save the mapping field:

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz,subject_iso.nii.gz \
  --to=template_qa.nii.gz,template_iso.nii.gz \
  --output_mapping=mapping_field.mz
```

### Apply a saved mapping

Warp a subject-space image to template space:

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --mapping=mapping_field.mz
```

Unwarp a template-space image to subject space:

```bash
dsi_studio --action=reg \
  --to=template_qa.nii.gz \
  --mapping=mapping_field.mz
```

Apply a saved mapping to multiple subject-space files:

```bash
dsi_studio --action=reg \
  --source=*.nii.gz \
  --mapping=mapping_field.mz
```

### Register and transform additional files

Transform additional files from subject to template space:

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --to=template_qa.nii.gz \
  --s2t=additional_image1.nii.gz,additional_image2.nii.gz
```

Transform additional files from template to subject space:

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --to=template_qa.nii.gz \
  --t2s=additional_image1.nii.gz,additional_image2.nii.gz
```

## Registration Parameters

| Parameter | Description |
|:--|:--|
| `--source` | Subject/source image or images. Multiple modalities can be comma-separated. With `--mapping`, source files are transformed source-to-target. |
| `--to` | Template/target image or images. Multiple modalities can be comma-separated. With `--mapping`, target files are transformed target-to-source. |
| `--output_mapping` | Save the computed mapping field. Mapping files use `.mz` unless a supported mapping-image output is explicitly requested. |
| `--mapping` | Reuse a previously computed mapping field. |
| `--s2t` | Additional comma-separated files to transform from subject to template space while computing the registration. |
| `--t2s` | Additional comma-separated files to transform from template to subject space while computing the registration. |
| `--output` | Optional output directory for transformed files. |

Current registration mapping can be applied to NIFTI images, TT tractography, and compatible DSI Studio SRC/FIB files (`.sz`, `.src.gz`, `.fz`, `.fib.gz`) when their source space matches the mapping.

## Advanced Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--reg_type` | `1` | Linear registration: `0` = rigid body; `1` = affine. The affine workflow proceeds to nonlinear registration unless `--skip_nonlinear=1` is used. |
| `--cost_function` | depends on `reg_type` | Linear-registration cost. Mutual information is used for the rigid-body default and correlation for the affine default. |
| `--match_vs` | `1` | Match source and target resolution before registration. |
| `--resolution` | registration default | Relative resolution/downsampling used for nonlinear registration. |
| `--speed` | registration default | Nonlinear deformation speed. |
| `--smoothing` | registration default | Mapping-field smoothing. |
| `--min_dimension` | registration default | Minimum dimension used by nonlinear registration. |
| `--large_deform` | `0` | Use larger linear-registration search bounds when set to `1`. |
| `--skip_linear` | `0` | Skip linear registration. |
| `--skip_nonlinear` | `0` | Skip nonlinear registration. |
| `--overwrite` | `0` | Overwrite existing outputs when set to `1`. |

Label images are resampled using label-preserving interpolation, while scalar images use continuous interpolation. Always confirm that the files being transformed belong to the source or target space expected by the mapping.
