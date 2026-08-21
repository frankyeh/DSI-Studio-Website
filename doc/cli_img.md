# Image Processing

> Use `--action=img` to inspect, convert, and process image files.

The `img` action supports image information display, datatype conversion, intensity normalization, bias field correction, thresholding, filtering, morphology, resampling, header editing, image arithmetic, and UNet-based brain extraction or segmentation.

---

## Examples

**1. Show image information and a quick terminal preview of the middle slice:**

```bash
dsi_studio --action=img --source=t1w.nii.gz
```

**2. Reduce anatomical image size by normalizing intensity and saving as 8-bit NIFTI:**

```bash
mkdir -p ../reduced_anat

for f in *.nii; do
    dsi_studio --action=img --source=$f 
        --cmd=change_type:3+normalize_otsu_median+upper_threshold:1+normalize:1+change_type:0 
        --output=../reduced_anat/${f/%.nii/.nii.gz}
done
```

**3. Reduce anatomical image size after bias field correction:**

```bash
mkdir -p ../reduced_anat

for f in *.nii; do
    dsi_studio --action=img --source=$f 
        --cmd=change_type:3+bias_field_correction+normalize_otsu_median+bias_field_correction+upper_threshold:1+normalize:1+change_type:0 
        --output=../reduced_anat/${f/%.nii/.nii.gz}
done
```

**4. Resample an image to 1-mm isotropic resolution:**

```bash
dsi_studio --action=img --source=t1w.nii.gz --cmd=regrid:1 --output=t1w_1mm.nii.gz
```

**5. Resample an image to anisotropic voxel size:**

```bash
dsi_studio --action=img --source=t1w.nii.gz --cmd="regrid:1 1 2" --output=t1w_1x1x2mm.nii.gz
```

**6. Create a binary mask using Otsu thresholding and morphology:**

```bash
dsi_studio --action=img --source=t1w.nii.gz 
    --cmd=change_type:3+otsu_threshold:1.0+morphology_defragment+morphology_smoothing+change_type:0 
    --output=t1w_mask.nii.gz
```

**7. Apply a mask to an image:**

```bash
dsi_studio --action=img --source=t1w.nii.gz 
    --cmd=multiply_image:t1w_mask.nii.gz 
    --output=t1w_masked.nii.gz
```

**8. Crop an image to the non-zero region with a 5-voxel margin:**

```bash
dsi_studio --action=img --source=t1w_masked.nii.gz 
    --cmd="crop_to_fit:5 5 5" 
    --output=t1w_cropped.nii.gz
```

**9. Run UNet-based brain extraction:**

```bash
dsi_studio --action=img --source=t1w.nii.gz 
    --cmd=brain_extraction --model=human_tissue_T1w.nz 
    --output=t1w_brain.nii.gz
```

**10. Run UNet-based segmentation:**

```bash
dsi_studio --action=img --source=flair.nii.gz 
    --cmd=segmentation --model=human_tumor_FLAIR.nz 
    --output=tumor_label.nii.gz
```

**11. Batch process all NIFTI files using wildcard matching:**

```bash
dsi_studio --action=img --source=*.nii.gz 
    --cmd=change_type:3+normalize_otsu_median+change_type:0 
    --output=normalized_*.nii.gz
```

---

## Basic Syntax

```bash
dsi_studio --action=img --source=<input> --cmd=<command1:param+command2+command3:param> --output=<output>
```

Commands are executed from left to right. Multiple commands are joined using `+`. Command parameters are specified using `:`.

For example:

```bash
--cmd=change_type:3+normalize_otsu_median+upper_threshold:1+change_type:0
```

Use quotation marks if the parameter contains spaces:

```bash
--cmd="crop_to_fit:5 5 5"
```

---

## Core Functions

|**Parameter**|**Description**|
|-|-|
|`source`|Specify the input image or DSI Studio data file.|
|`cmd`|Specify one command or a chain of commands. If not specified, `info` is used.|
|`output`|Specify the output image file. If not specified, DSI Studio prints image information or runs commands without saving.|

---

## Optional Functions

|**Parameter**|**Description**|
|-|-|
|`model`|Specify the UNet model used by `brain_extraction` or `segmentation`. The model should be in DSI Studio's `unet` folder.|
|`overwrite`|Specify whether to overwrite an existing output file. If not specified, DSI Studio skips processing when the output file already exists.|
|`thread_count`|Specify the number of CPU threads used by DSI Studio.|
|`loop`|Specify a wildcard loop pattern. In most cases, `--source=*.nii.gz` is sufficient for batch processing.|

---

## Supported Input Files

|**Format**|**Description**|
|-|-|
|`.nii`, `.nii.gz`, `.hdr`|NIFTI image files.|
|`.nrrd`, `.nhdr`|NRRD image files.|
|DICOM|Single DICOM image input.|
|`2dseq`|Bruker 2dseq image.|
|`.fz`, `.fib.gz`|DSI Studio FIB files.|
|`.sz`, `.src.gz`|DSI Studio SRC files.|
|`.mz`|DSI Studio mapping/data files.|

---

## Image Information and Saving Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`info`|none|Print image dimension, voxel size, header information, and a terminal preview of the middle slice.|
|`save`|output file|Save the current image. This is normally handled by `--output`.|
|`open`|input file|Load another image into the current image buffer.|

---

## Datatype Commands

|**Command**|**Parameter**|**Output Datatype**|
|-|-|-|
|`change_type`|`0`|8-bit integer.|
|`change_type`|`1`|16-bit integer.|
|`change_type`|`2`|32-bit integer.|
|`change_type`|`3`|32-bit floating point.|

A common workflow is to convert to floating point before intensity processing and then convert back to 8-bit integer for compact storage:

```bash
--cmd=change_type:3+normalize_otsu_median+upper_threshold:1+normalize:1+change_type:0
```

---

## Intensity Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`normalize`|optional|Normalize image intensity. For integer images, values are scaled to 0-255.|
|`normalize_otsu_median`|none|Normalize intensity using Otsu-median tissue normalization.|
|`bias_field_correction`|none|Correct smooth intensity inhomogeneity.|
|`add_value`|value|Add a constant value to all voxels.|
|`multiply_value`|value|Multiply all voxels by a constant value.|
|`lower_threshold`|value|Set values below the threshold to the threshold value.|
|`upper_threshold`|value|Set values above the threshold to the threshold value.|
|`threshold`|value|Convert to a binary image using `value` as the threshold.|
|`otsu_threshold`|scale|Convert to a binary image using Otsu threshold multiplied by `scale`.|
|`select_value`|value|Convert voxels equal to `value` to 1 and all others to 0.|
|`equation`|expression|Apply an equation using `x` as the image variable.|

---

## Filter Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`mean_filter`|none|Apply mean filtering.|
|`gaussian_filter`|none|Apply Gaussian filtering.|
|`sobel_filter`|none|Apply Sobel edge filtering.|
|`smoothing_filter`|none|Apply anisotropic diffusion smoothing.|

---

## Morphology Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`morphology_defragment`|none|Keep the main connected component and remove small fragments.|
|`morphology_defragment_by_size`|optional ratio|Remove fragments based on size ratio. Default is `0.05`.|
|`morphology_dilation`|none|Dilate the mask.|
|`morphology_erosion`|none|Erode the mask.|
|`morphology_edge`|none|Extract mask edge.|
|`morphology_edge_xy`|none|Extract edge in the xy plane.|
|`morphology_edge_xz`|none|Extract edge in the xz plane.|
|`morphology_smoothing`|none|Smooth a binary or multi-label mask.|

---

## Geometry and Header Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`flip_x`|none|Flip voxel data along x.|
|`flip_y`|none|Flip voxel data along y.|
|`flip_z`|none|Flip voxel data along z.|
|`swap_xy`|none|Swap x and y axes in voxel data and voxel size.|
|`swap_xz`|none|Swap x and z axes in voxel data and voxel size.|
|`swap_yz`|none|Swap y and z axes in voxel data and voxel size.|
|`header_flip_x`|none|Flip x direction in the image header without changing voxel data.|
|`header_flip_y`|none|Flip y direction in the image header without changing voxel data.|
|`header_flip_z`|none|Flip z direction in the image header without changing voxel data.|
|`header_swap_xy`|none|Swap x and y axes in the image header.|
|`header_swap_xz`|none|Swap x and z axes in the image header.|
|`header_swap_yz`|none|Swap y and z axes in the image header.|
|`set_mni`|`0` or `1`|Set whether the image is treated as an MNI-space image.|
|`set_translocation`|`x y z`|Set the x/y/z translation terms in the image transformation matrix.|
|`set_transformation`|16 numbers|Set the full 4-by-4 image transformation matrix.|

---

## Resampling and Size Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`upsampling`|none|Upsample the image by 2.|
|`downsampling`|none|Downsample the image by 2.|
|`regrid`|`resolution` or `x y z`|Resample to a new voxel size.|
|`resize`|`x y z`|Resize image volume and place the old image at the origin.|
|`resize_at_center`|`x y z`|Resize image volume and place the old image at the center.|
|`reshape`|`x y z`|Reshape the voxel buffer to a new image dimension.|
|`crop_to_fit`|`x y z`|Crop to the non-background region with margin.|
|`translocate`|`x y z`|Shift image content by voxel units.|
|`transform`|12 numbers|Resample image using an affine transform.|

---

## Image Combination Commands

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`load_image`|image file|Replace the current image with another image.|
|`multiply_image`|image file|Multiply by another image.|
|`add_image`|image file|Add another image.|
|`minus_image`|image file|Subtract another image.|
|`max_image`|image file|Take the voxelwise maximum with another image.|
|`min_image`|image file|Take the voxelwise minimum with another image.|
|`concatenate_image`|image file|Append another image along the z dimension.|

---

## UNet Brain Extraction and Segmentation

|**Command**|**Parameter**|**Description**|
|-|-|-|
|`brain_extraction`|`--model=[model name]`|Run the selected UNet model and apply the resulting soft mask to the input image.|
|`segmentation`|`--model=[model name]`|Run the selected UNet model and save the label image.|

If `--model` is not specified, DSI Studio lists available model files in the `unet` folder.

|**Example Model**|**Likely Use**|
|-|-|
|`human_stroke_T1w.nz`|Human stroke segmentation from T1w.|
|`human_synthseg2.nz`|Human SynthSeg-style segmentation.|
|`human_tissue_FLAIR.nz`|Human tissue segmentation from FLAIR.|
|`human_tissue_T1w.nz`|Human tissue segmentation or brain extraction from T1w.|
|`human_tissue_T2w.nz`|Human tissue segmentation or brain extraction from T2w.|
|`human_tumor_FLAIR.nz`|Human tumor segmentation from FLAIR.|
|`human_tumor_T1w.nz`|Human tumor segmentation from T1w.|
|`human_tumor_gad_T1w.nz`|Human tumor segmentation from gadolinium-enhanced T1w.|
|`human_tumorsynth.nz`|Human TumorSynth model.|
|`marmoset_tissue_T1w.nz`|Marmoset tissue segmentation from T1w.|
|`marmoset_tissue_T2w.nz`|Marmoset tissue segmentation from T2w.|
|`rat_tissue_T2w.nz`|Rat tissue segmentation from T2w.|
|`mouse_tissue_T2w.nz`|Mouse tissue segmentation from T2w.|

---

## Notes from the Source Code

1. **Command Chaining**:

   * Commands in `--cmd` are separated by `+`.
   * Command parameters are specified after `:`.
   * Commands are executed sequentially from left to right.
2. **Default Behavior**:

   * If `--cmd` is not specified, the default command is `info`.
   * If `--output` is specified and the file already exists, DSI Studio skips processing unless `--overwrite=1` is used.
3. **Datatype Handling**:

   * `change_type:3` converts the image to 32-bit floating point.
   * `change_type:0` converts the image to 8-bit integer.
   * Converting a floating-point image in the range 0 to 1 to 8-bit scales it to 0-255.
4. **Label Image Handling**:

   * DSI Studio detects label images and uses nearest-neighbor or majority-style handling when possible.
   * Morphology commands treat non-zero voxels as foreground.
5. **UNet Models**:

   * `brain_extraction` applies the model-derived soft mask to the input image.
   * `segmentation` replaces the image with the model-derived label image.
   * Model files should be placed in the `unet` folder next to the DSI Studio executable.
6. **FIB/SRC/MZ Files**:

   * If the input file is `.fib.gz`, `.fz`, `.src.gz`, `.sz`, or `.mz`, DSI Studio opens it as a matrix file and prints the internal matrix information.
   * This mode is mainly for inspection or advanced internal file editing.
7. **Batch Processing**:

   * Wildcard input such as `--source=*.nii.gz` can be used for batch processing.
   * Wildcards can also be used in `--output`, for example `--output=normalized_*.nii.gz`.

