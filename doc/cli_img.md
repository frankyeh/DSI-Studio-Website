# Image Processing

Use `--action=img` to inspect, convert, register, and process images from the command line.

## Basic syntax

```bash
dsi_studio --action=img \
  --source=input.nii.gz \
  --cmd=command1:param+command2+command3:param \
  --output=output.nii.gz
```

Commands are executed from left to right. Join multiple commands with `+`; place a command parameter after `:` or, where appropriate, after a comma.

If `--cmd` is omitted, DSI Studio runs `info`. If the output already exists, processing is skipped unless `--overwrite=1` is specified.

## Common examples

### Show image information

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

# U-Net operations

Current DSI Studio can run U-Net-based brain extraction, segmentation, and defacing. Model IDs come from the model catalog packaged with the current DSI Studio release, so discover the current ID rather than copying an old model name.

If a U-Net command is requested without `--model`, DSI Studio lists the currently available model names. Use one of those exact IDs.

## Brain extraction

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=brain_extraction \
  --model=<model-id> \
  --output=t1w_brain.nii.gz
```

## Segmentation

```bash
dsi_studio --action=img \
  --source=flair.nii.gz \
  --cmd=segmentation \
  --model=<model-id> \
  --output=segmentation_labels.nii.gz
```

## Defacing

```bash
dsi_studio --action=img \
  --source=t1w.nii.gz \
  --cmd=deface \
  --model=<model-id> \
  --output=t1w_defaced.nii.gz
```

Supported models are downloaded/cached in the operating system's application-local DSI Studio data directory as needed; they do not need to be manually placed beside the executable.

# Image-to-image registration

Three image commands use a target image or a mapping established earlier in the same command chain.

| Command | Meaning |
|:--|:--|
| `rotate_to_image:<target>` | Rigid-body registration of the source image to the target image. |
| `warp_to_image:<target>` | Affine followed by nonlinear registration of the source image to the target image. |
| `apply_to_image:<image>` | Apply the mapping produced by the preceding registration to another image. Use this within the same command chain after `rotate_to_image` or `warp_to_image`. |

Example nonlinear registration:

```bash
dsi_studio --action=img \
  --source=subject_t1w.nii.gz \
  --cmd=warp_to_image:template_t1w.nii.gz \
  --output=subject_in_template.nii.gz
```

For explicit registration workflows and mapping files, see [Registration CLI](/doc/cli_reg.html).

# Frequently used image commands

## Intensity

| Command | Description |
|:--|:--|
| `normalize` | Normalize image intensity. |
| `normalize_otsu_median` | Normalize intensity using the Otsu-median tissue reference. |
| `bias_field_correction` | Correct smooth intensity inhomogeneity. |
| `add_value:<v>` | Add a constant. |
| `multiply_value:<v>` | Multiply by a constant. |
| `lower_threshold:<v>` | Raise values below the threshold to the threshold value. |
| `upper_threshold:<v>` | Lower values above the threshold to the threshold value. |
| `threshold:<v>` | Create a binary thresholded image. |
| `otsu_threshold:<scale>` | Threshold using the Otsu value times a scale factor. |
| `select_value:<v>` | Select voxels equal to a value. |
| `equation:<expression>` | Apply an expression using `x` as the image value. |

## Datatype

| Command | Result |
|:--|:--|
| `change_type:0` | 8-bit integer. |
| `change_type:1` | 16-bit integer. |
| `change_type:2` | 32-bit integer. |
| `change_type:3` | 32-bit floating point. |

Convert to floating point before operations that require continuous intensity values.

## Filtering and morphology

Common commands include:

```text
mean_filter
gaussian_filter
sobel_filter
smoothing_filter
morphology_defragment
morphology_defragment_by_size
morphology_dilation
morphology_erosion
morphology_edge
morphology_smoothing
```

## Geometry and header

Common commands include:

```text
flip_x  flip_y  flip_z
swap_xy swap_xz swap_yz
header_flip_x header_flip_y header_flip_z
header_swap_xy header_swap_xz header_swap_yz
set_mni
set_translocation
set_transformation
regrid
upsampling
downsampling
resize
resize_at_center
reshape
crop_to_fit
translocate
transform
```

For example:

```bash
dsi_studio --action=img \
  --source=image.nii.gz \
  --cmd="crop_to_fit:5 5 5" \
  --output=image_cropped.nii.gz
```

## Image arithmetic

Common commands include:

```text
multiply_image
add_image
minus_image
max_image
min_image
concatenate_image
```

Example:

```bash
dsi_studio --action=img \
  --source=image1.nii.gz \
  --cmd=add_image:image2.nii.gz \
  --output=sum.nii.gz
```

# Input formats

Image mode supports NIfTI, NRRD, DICOM, Bruker 2dseq, and other image formats recognized by DSI Studio.

When the source is a DSI Studio matrix container such as `.fz`, `.fib.gz`, `.sz`, `.src.gz`, `.dz`, `.mz`, or `.nz`, the `img` action enters matrix-file inspection/editing mode instead of the ordinary 3D-image processing path. For normal conversion/export of FIB/SRC/database data, use the corresponding documented DSI Studio actions rather than modifying internal matrices directly.

See [DSI Studio File Formats](/doc/cli_data.html) for current file-format guidance.
