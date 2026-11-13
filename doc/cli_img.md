# Image Processing

Use `--action=img` to inspect headers, convert formats, run U-Net models, register volumes, and chain 3D image or matrix-container operations from the command line.

## Basic syntax

```bash
dsi_studio --action=img \
  --source=input.nii.gz \
  --cmd=command1:param+command2+command3:param \
  --output=output.nii.gz
```

Join chained commands in `--cmd` with `+` (executed left to right). Pass a command's argument after `:` (works in all modes) or `,` (standard image mode only when the argument contains no comma). Quote `--cmd="..."` when an argument contains spaces, `<`, or `>`.

### CLI options

| Option | Default | Description |
|:--|:--|:--|
| `--action=img` | required | Selects image processing mode. |
| `--source=<path>` | required | Input image or matrix container. A `*` wildcard automatically runs a batch loop across matching files. |
| `--cmd=<chain>` | `info` (images) | `+`-separated command chain. When omitted on an image file, runs `info`. |
| `--output=<path>` | none | Output file path or existing directory (saves `<output>/<source-filename>`). When omitted, no final file is written unless `save:<path>` appears in `--cmd`. |
| `--overwrite=<0\|1>` | `0` | Set to `1` to overwrite an existing `--output`; when `0`, existing outputs are skipped (`output exist, skipping`). |
| `--model=<model-id>` | none | U-Net `.nz` filename stem for `brain_extraction`, `segmentation`, or `deface` (overrides inline `:<model-id>`). |
| `--loop=<pattern>` | none | Explicit file-glob pattern for batch processing (for example `--loop=*.nii.gz`). |
| `--continue_on_error` | off | Continues processing remaining batch files if one file fails. |
| `--thread_count=<N>` | system threads | Sets maximum CPU thread count. |
| `--interact` | off | Prompts interactively on standard input for omitted options. |

## Common examples

### Show image information

Prints an ASCII preview of the middle axial slice and header metadata without writing a file:

```bash
dsi_studio --action=img --source=t1w.nii.gz
```

### Bias-field correction

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=bias_field_correction \
  --output=t1w_corrected.nii.gz
```

### Regrid to 1-mm isotropic resolution

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=regrid:1 \
  --output=t1w_1mm.nii.gz
```

### Create a binary mask

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=change_type:3+otsu_threshold:1+morphology_defragment+morphology_smoothing+change_type:0 \
  --output=t1w_mask.nii.gz
```

### Apply a mask

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=multiply_image:t1w_mask.nii.gz \
  --output=t1w_masked.nii.gz
```

### Batch processing with wildcards

Wildcards in `--source` automatically substitute matching stems into `*` placeholders in `--output` and `--cmd`:

```bash
dsi_studio --action=img \
  --source=sub-*_T1w.nii.gz \
  --cmd=bias_field_correction+regrid:1 \
  --output=sub-*_T1w_1mm.nii.gz \
  --continue_on_error
```

## U-Net operations

`brain_extraction`, `segmentation`, and `deface` run packaged 3D U-Net models (`unet/README.md`), downloading and caching `<model-id>.nz` automatically in DSI Studio's local application data directory (`unet/`). Pass the model ID via `--model=<model-id>` or inline (`--cmd=brain_extraction:<model-id>`).

Run without `--model` to list every model ID available in your build:

```bash
dsi_studio --action=img --source=t1w.nii.gz --cmd=brain_extraction
```

## Brain extraction

Multiplies voxel intensities by the U-Net foreground probability map:

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=brain_extraction \
  --model=<model-id> \
  --output=t1w_brain.nii.gz
```

## Segmentation

Replaces the volume with `uint8` discrete class labels and marks it as a label image:

```bash
dsi_studio --action=img \
  --source=flair.nii.gz \
  --cmd=segmentation \
  --model=<model-id> \
  --output=segmentation_labels.nii.gz
```

## Defacing

Suppresses facial voxels outside the brain-adjacent protective mask:

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=deface \
  --model=<model-id> \
  --output=t1w_defaced.nii.gz
```

## Image-to-image registration

Registration commands resample the working image into `<target>` space (linear interpolation for scalar images; majority voting for discrete label images) and keep the mapping in memory for later `apply_to_image` calls in the same `--cmd` chain.

| Command | Behavior |
|:--|:--|
| `rotate_to_image:<target.nii.gz>` | Rigid-body registration using mutual information, resampled into `<target>` space. |
| `warp_to_image:<target.nii.gz>` | Affine registration (correlation) followed by nonlinear registration, resampled into `<target>` space. |
| `apply_to_image:<other.nii.gz>` | Loads `<other.nii.gz>` and transforms it into `<target>` space using the mapping from a preceding `rotate_to_image` or `warp_to_image` in the same chain. |

Register a T1w volume to a template, save it mid-chain, and warp a companion mask with the same mapping:

```bash
dsi_studio --action=img \
  --source=subject_t1w.nii.gz \
  --cmd=warp_to_image:template_t1w.nii.gz+save:subject_in_template.nii.gz+apply_to_image:lesion_mask.nii.gz \
  --output=lesion_in_template.nii.gz
```

For multi-modality registration and reusable `.mz` mapping files, see [Registration CLI](/doc/cli_reg.html).

## Frequently used image commands

On load, DSI Studio automatically detects discrete label volumes so resampling commands use label-preserving majority voting instead of linear interpolation. Mid-chain utility commands:

| Command | Behavior |
|:--|:--|
| `info` | Prints an ASCII middle-slice preview and header metadata. |
| `save:<file.nii.gz>` | Saves the current 3D image state (`T`, `vs`, MNI flag, voxels) mid-chain. |
| `open:<file.nii.gz>` | Replaces the current 3D image and header from `<file.nii.gz>` mid-chain. |

## Intensity

| Command | Description |
|:--|:--|
| `normalize` | Scales `float32` intensities to `[0, 1]`, or integer types to `[0, 255]` (clamped at `255`). Precede with `change_type:3` when `[0, 1]` float output is needed from an integer image. |
| `normalize_otsu_median` | Scales intensities so the median above the Otsu threshold equals `0.5` (convert integer images with `change_type:3` first). |
| `bias_field_correction` | Iteratively estimates (up to 10 passes on positive voxels) and removes smooth bias-field inhomogeneity. |
| `add_value:<v>` | Adds scalar `<v>` to every voxel (negative values subtract). |
| `multiply_value:<v>` | Multiplies every voxel by scalar `<v>`. |
| `lower_threshold:<v>` | Clamps values below `<v>` up to `<v>`. |
| `upper_threshold:<v>` | Clamps values above `<v>` down to `<v>`. |
| `threshold:<v>` | Binarizes voxels strictly greater than `<v>` to `1` and all others to `0`. |
| `otsu_threshold:<scale>` | Binarizes voxels strictly greater than `otsu * <scale>` to `1` and all others to `0`. |
| `select_value:<v>` | Binarizes voxels equal to `<v>` to `1` and all others to `0`. |
| `equation:<expr>` | Evaluates voxelwise expression `<expr>` with `x` as image intensity (`-`, `*`, `/`, `>`, `<`, `=`, parentheses, and parameterless filter/morphology functions such as `gaussian_filter(x)`; `*` and `/` evaluate first). Avoid `+` and `,` inside `<expr>` because `--cmd` splits chains on `+` and parameters on `,`; chain `add_value:<v>` instead. |

## Datatype

| Command | Result |
|:--|:--|
| `change_type:0` | `uint8` (8-bit unsigned integer). Converting from `float32` when `0 < max <= 1` scales values by `255.99 / max` and clamps negatives to `0`. |
| `change_type:1` | `uint16` (16-bit unsigned integer). |
| `change_type:2` | `uint32` (32-bit unsigned integer). |
| `change_type:3` | `float32` (32-bit floating point). |

Convert integer images with `change_type:3` before operations that produce fractional intensities (`normalize`, `normalize_otsu_median`, fractional `multiply_value`, or `equation`).

## Filtering and morphology

On `float32` images, `morphology_*` commands operate on the positive-value mask (`I > 0`) and preserve original float intensities inside the surviving mask.

| Command | Description |
|:--|:--|
| `mean_filter` | 3D mean (box) smoothing filter. |
| `gaussian_filter` | 3D Gaussian smoothing filter. |
| `sobel_filter` | 3D Sobel gradient filter. |
| `smoothing_filter` | Edge-preserving 3D anisotropic diffusion smoothing. |
| `morphology_defragment` | Retains only the largest connected component. |
| `morphology_defragment_by_size` or `morphology_defragment_by_size:<ratio>` | Removes connected components smaller than `<ratio>` of total positive voxels (defaults to `0.05`). |
| `morphology_fill_holes` | Fills enclosed 3D holes. |
| `morphology_fill_holes_by_slice` | Fills enclosed 2D holes independently per slice. |
| `morphology_dilation` | Dilates each nonzero label independently. |
| `morphology_erosion` | Erodes each nonzero label independently. |
| `morphology_opening` | Applies morphological opening per nonzero label. |
| `morphology_closing` | Applies morphological closing per nonzero label. |
| `morphology_edge` | Extracts 3D boundary voxels per nonzero label. |
| `morphology_edge_xy` | Extracts 2D boundary voxels in XY planes per nonzero label. |
| `morphology_edge_xz` | Extracts 2D boundary voxels in XZ planes per nonzero label. |
| `morphology_smoothing` | Smooths a binary mask (`0/1`), or applies multi-region smoothing when multiple labels exist. |
| `refine_label:<ref.nii.gz>` | Resamples `<ref.nii.gz>` into current space and refines current discrete label boundaries. |

## Geometry and header

`flip_*` and `swap_*` reorder the voxel array without updating the 4×4 NIfTI transformation `T`, whereas `header_flip_*`, `header_swap_*`, `set_transformation`, and `set_translocation` modify header metadata only.

| Command | Description |
|:--|:--|
| `regrid:<iso>` or `regrid:<vx vy vz>` | Resamples to isotropic `<iso>` mm or 3-axis `<vx vy vz>` mm voxel size, updating dimensions, `vs`, and `T`. |
| `upsampling` | Upsamples dimensions by 2×, halves `vs`, and updates `T`. |
| `downsampling` | Downsamples dimensions by 2×, doubles `vs`, and updates `T`. |
| `crop_to_fit:<m>` or `crop_to_fit:<mx my mz>` | Crops around the largest positive component with `<m>` or `<mx my mz>` voxel margin and updates `T` translation. |
| `resize:<w h d>` | Resizes the 3D canvas from voxel `(0, 0, 0)` (zero-padding or cropping) without changing `T`. |
| `resize_at_center:<w h d>` | Resizes the 3D canvas around the volume center without changing `T`. |
| `reshape:<w h d>` | Reinterprets 3D voxel buffer dimensions as `<w h d>` without resampling or changing `T`. |
| `translocate:<dx dy dz>` | Shifts voxel data by `<dx dy dz>` voxels (interpolated when fractional) and updates `T` translation. |
| `transform:<12 floats>` | Resamples/reorients the volume to match the first 12 row-major entries of a target 4×4 matrix. |
| `flip_x`, `flip_y`, `flip_z` | Reverses voxel order along X, Y, or Z without changing `T`. |
| `swap_xy`, `swap_xz`, `swap_yz` | Swaps voxel axes and `vs` entries without changing `T`. |
| `header_flip_x`, `header_flip_y`, `header_flip_z` | Flips axis sign and origin in `T` without modifying voxel data. |
| `header_swap_xy`, `header_swap_xz`, `header_swap_yz` | Swaps rows of `T` and `vs` entries without modifying voxel data. |
| `set_transformation:<16 floats>` | Replaces the 4×4 row-major matrix `T` and recalculates `vs` without resampling voxels. |
| `set_translocation:<tx ty tz>` | Sets header translation entries `T[3]`, `T[7]`, `T[11]` without shifting voxels. |
| `set_mni:<0\|1>` | Sets the MNI-space header flag (`1` = MNI, `0` = native). |

Crop around foreground tissue with a 5-voxel margin on each axis:

```bash
dsi_studio --action=img \
  --source=image.nii.gz \
  --cmd="crop_to_fit:5 5 5" \
  --output=image_cropped.nii.gz
```

## Image arithmetic

Except for `concatenate_image`, multi-image commands resample `<other.nii.gz>` into the current image's grid and transformation `T` before applying the voxelwise operation.

| Command | Description |
|:--|:--|
| `multiply_image:<other.nii.gz>` | Multiplies current voxels by resampled `<other.nii.gz>`. |
| `add_image:<other.nii.gz>` | Adds resampled `<other.nii.gz>` voxelwise. |
| `minus_image:<other.nii.gz>` | Subtracts resampled `<other.nii.gz>` voxelwise. |
| `max_image:<other.nii.gz>` | Keeps the voxelwise maximum with resampled `<other.nii.gz>`. |
| `min_image:<other.nii.gz>` | Keeps the voxelwise minimum with resampled `<other.nii.gz>`. |
| `load_image:<other.nii.gz>` | Replaces current voxel values with `<other.nii.gz>` resampled into current geometry. |
| `concatenate_image:<other.nii.gz>` | Appends `<other.nii.gz>` raw along Z (`width` and `height` must match). |

```bash
dsi_studio --action=img \
  --source=image1.nii.gz \
  --cmd=add_image:image2.nii.gz \
  --output=sum.nii.gz
```

## Input formats

Standard image mode reads NIfTI/Analyze (`.nii.gz`, `.nii`, `.hdr`, 3D or 4D; `NaN` values in float NIfTI are set to `0`), NRRD (`.nrrd`, `.nhdr`), single-file DICOM, and Bruker `2dseq`. Saved image outputs use NIfTI (`.nii.gz` or `.nii`). On 4D NIfTI inputs, volumes are stacked along Z during `--cmd` execution and written back with the original 4D dimensions, so only voxel-count-preserving commands (`change_type`, intensity scaling, filtering) should be saved directly as 4D.

When `--source` ends in `.fz`, `.fib.gz`, `.sz`, `.src.gz`, `.dz`, `.mz`, or `.nz`, `--action=img` enters matrix-container inspection and editing mode instead of 3D image mode. Prefer dedicated DSI Studio actions (`--action=exp`, `--action=rec`, `--action=reg`) for normal data conversion.

<details markdown="1">
<summary>Matrix container commands (<code>.fz</code>, <code>.sz</code>, <code>.dz</code>, <code>.mz</code>, <code>.nz</code>)</summary>

Matrix mode always prints an ASCII slice preview (`fa0` / `image0`) and lists every stored matrix. In matrix mode, `--cmd` separates arguments strictly with `:` (not `,`).

| Command | Description |
|:--|:--|
| `mat_remove:<name>` | Deletes matrix `<name>`. |
| `mat_resize:<name> <rows> <cols>` | Resizes matrix `<name>` to `<rows> × <cols>`. |
| `mat_set_name:<name> <new_name>` | Renames matrix `<name>` to `<new_name>`. |
| `mat_set_value:<name> <values...>` | Overwrites `<name>` with space-separated numeric values or text. |
| `mat_add_string:<after> <new_name>` | Inserts a char/string entry after `<after>`. |
| `mat_add_float:<after> <new_name>` | Inserts a `float` entry after `<after>`. |
| `mat_add_int:<after> <new_name>` | Inserts an `uint32` entry after `<after>`. |
| `mat_add_short:<after> <new_name>` | Inserts an `uint16` entry after `<after>`. |
| `mat_add_int64:<after> <new_name>` | Inserts an `uint64` entry after `<after>`. |
| `save:<output.fz>` | Saves the container (also run by `--output`). Legacy `.fib.gz` and `.src.gz` extensions are rejected on save; write `.fz`, `.sz`, `.dz`, `.mz`, or `.nz`. |
| `save_mini:<output.fz>` | Saves a compact `.fz` omitting ODF vertices/faces, `z0`, `mapping`, DTI/RDI/NRDI maps, `fa3`/`fa4`, `index3`/`index4`, `fixel3`/`fixel4`, and `subject*` fields. |
| Spatial/volume commands (`regrid`, `flip_*`, `crop_to_fit`, etc.) | Applied across matching `dim.size()` and `3 * dim.size()` volume matrices, updating `dimension`, `voxel_size`, and `trans`. On `.fz` files, nearest-neighbor sampling is used for every matrix so fiber anisotropy, indices, and directions come from the same source voxel. |

</details>

See [DSI Studio File Formats](/doc/cli_data.html) for container structure details.
