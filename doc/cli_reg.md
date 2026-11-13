# Registration

Use `--action=reg` to compute linear or nonlinear registration between NIfTI volumes, save reusable `.mz` mapping fields, and warp NIfTI images, `.tt.gz` tractography, or compatible `.sz`/`.fz` files.

## Examples

### Register a subject to a template

Multi-modality affine + nonlinear registration, saving the `.mz` mapping field (also warps `--source` to `.wp.nii.gz`):

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz,subject_iso.nii.gz \
  --to=template_qa.nii.gz,template_iso.nii.gz \
  --output_mapping=mapping_field.mz
```

Rigid-body registration (`--reg_type=0`, defaults to mutual-information cost):

```bash
dsi_studio --action=reg \
  --source=subject_t2w.nii.gz \
  --to=subject_t1w.nii.gz \
  --reg_type=0
```

### Apply a saved mapping

Warp a subject-space file to template space (`subject_qa.nii.gz.wp.nii.gz`):

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --mapping=mapping_field.mz
```

Unwarp a template-space file to subject space (`template_qa.nii.gz.uwp.nii.gz`):

```bash
dsi_studio --action=reg \
  --to=template_qa.nii.gz \
  --mapping=mapping_field.mz
```

Batch-warp multiple subject-space files:

```bash
dsi_studio --action=reg \
  --source=*.nii.gz \
  --mapping=mapping_field.mz
```

### Register and transform additional files

Warp specified subject-space files (`--s2t` replaces `--source` as the warp list):

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --to=template_qa.nii.gz \
  --s2t=subject_qa.nii.gz,subject_tract.tt.gz
```

Unwarp template-space files to subject space while registering (`--source` is also warped unless `--s2t` overrides it):

```bash
dsi_studio --action=reg \
  --source=subject_qa.nii.gz \
  --to=template_qa.nii.gz \
  --t2s=template_roi1.nii.gz,template_roi2.nii.gz
```

### Batch registration

Single-modality batch across subjects (`*` in `--source` enables looping automatically):

```bash
dsi_studio --action=reg \
  --source=sub-*_qa.nii.gz \
  --to=template_qa.nii.gz \
  --output_mapping=sub-*.mz
```

Multi-modality batch across subjects (add `--loop` whenever `--source` contains comma-separated wildcards):

```bash
dsi_studio --action=reg \
  --loop=sub-*_qa.nii.gz \
  --source=sub-*_qa.nii.gz,sub-*_iso.nii.gz \
  --to=template_qa.nii.gz,template_iso.nii.gz \
  --output_mapping=sub-*.mz
```

## Registration Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--source` | Required with `--to` (optional with `--mapping` if `--to` is set) | Subject/source NIfTI image(s) (`.nii`, `.nii.gz`), comma-separated for paired multi-modality registration. With `--mapping`, warps listed files source-to-target (`.wp<ext>`). |
| `--to` | Required without `--mapping` (optional with `--mapping` if `--source` is set) | Template/target NIfTI image(s) (`.nii`, `.nii.gz`), comma-separated to pair with `--source`. With `--mapping`, unwarps listed files target-to-source (`.uwp<ext>`). |
| `--output_mapping` | — | Save the displacement mapping field. Appends `.mz` unless the path ends in `.mz` or `.nii.gz`. Ending in `.nii.gz` writes two 4D (`X×Y×Z×3`) coordinate volumes (`<name>` and `<prefix>.inv.nii.gz`) that cannot be reloaded by `--mapping`. Fails with `"no mapping matrix to save"` when displacement fields are empty (such as `--reg_type=0`). |
| `--mapping` | — | Load a saved `.mz` mapping file (`version <= 202408`) to warp `--source` and/or unwarp `--to` without re-running registration. Can be paired with `--output_mapping` to export `.nii.gz` coordinate fields when at least one `--source` or `--to` file is supplied. |
| `--s2t` | `--source` | Comma-separated source-space files to warp to target space (`.wp<ext>`) after computing registration. Replaces `--source` as the warp list when set; ignored with `--mapping`. |
| `--t2s` | — | Comma-separated target-space files to unwarp to source space (`.uwp<ext>`) after computing registration; ignored with `--mapping`. |
| `--output` | Input file's directory | Output directory for warped/unwarped `.wp` and `.uwp` files (created automatically if missing). |

**Output filenames and supported warp formats:**
- Source-to-target outputs append `.wp<ext>` to the full filename (for example, `subject_qa.nii.gz.wp.nii.gz`); target-to-source outputs append `.uwp<ext>` (`template_qa.nii.gz.uwp.nii.gz`).
- Supported warpable extensions: `.nii.gz`, `.nii`, `.tt.gz`, `.sz`, `.src.gz`, `.fz`, `.fib.gz` (`.trx` is not accepted by CLI output naming).
- NIfTI integer label volumes use majority-vote interpolation; scalar NIfTI volumes use cubic spline interpolation. `.tt.gz` files transform streamline coordinates and update target geometry. `.sz`/`.fz` files require exact dimension and transformation-matrix match with the expected space; scalar volumes use cubic interpolation (clamped at `>= 0`), `mask` uses majority assignment, and `index*`/`fixel*` direction codes use nearest-neighbor sampling without vector reorientation.

## Advanced Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--reg_type` | `1` | Linear registration model: `0` = rigid body (linear only); `1` (or non-zero) = affine followed by nonlinear diffeomorphic registration unless `--skip_nonlinear=1`. |
| `--cost_function` | `mi` (`--reg_type=0`) or `corr` (`--reg_type=1`) | Linear-registration cost function: `mi` = mutual information; `corr` = correlation. |
| `--match_vs` | `1` | Match source and target voxel resolution before registration (`1` = enable, `0` = disable). |
| `--large_deform` | `0` | Use expanded linear-registration search bounds when set to `1`. |
| `--skip_linear` | `0` | Skip linear registration when set to `1`. |
| `--skip_nonlinear` | `0` | Skip nonlinear registration when set to `1`. |
| `--resolution` | TIPL default | Relative resolution/downsampling factor for nonlinear registration (`--reg_type=1` only). |
| `--speed` | TIPL default | Nonlinear deformation step speed (`--reg_type=1` only). |
| `--smoothing` | TIPL default | Nonlinear displacement-field smoothing (`--reg_type=1` only). |
| `--min_dimension` | TIPL default | Minimum pyramid dimension for nonlinear registration (`--reg_type=1` only). |
| `--overwrite` | `0` | When `0` (without `--mapping`), skips the run—including `--output_mapping`—if every expected `.wp`/`.uwp` output from `--s2t` and `--t2s` already exists. Set `1` to recompute and overwrite. (`--mapping` always overwrites.) |
| `--export_r` | `0` | When `1` (without `--mapping`), writes an empty correlation marker file `<first_source>.r<int(r*100)>` (for example, `subject_qa.nii.gz.r82`). |
| `--loop` | `*` in `--source` | Explicit file-glob pattern for batch processing, substituting `*` in other wildcard options. Required when `--source` contains comma-separated wildcards or when batching `--to` without a wildcard `--source`. |
| `--continue_on_error` | Off | Presence flag (`--continue_on_error=1`); continue batch loop after a failed item instead of stopping on the first error. |
| `--thread_count` | TIPL default | Maximum CPU thread count (`tipl::max_thread_count`). |

