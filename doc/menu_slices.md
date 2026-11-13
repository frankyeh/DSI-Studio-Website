# [Slices] Menu

Use the **[Slices]** menu in the Fiber Tracking window to load external 3D structural, clinical, or statistical volumes and 2D histology pictures, register them to diffusion space, render 3D brain isosurfaces with cutaway views, blend multimodal slice overlays with custom colormaps, and burn fiber tracts or regions into anatomical slices for NIfTI or surgical-navigation DICOM export. Built-in diffusion metric volumes are exported from the **[Export]** menu (see the [Export menu](/doc/menu_export.html)), and automated deep-learning tissue or lesion parcellations live under **[Slices Segment]** (see the [Slices Segment menu](/doc/menu_slices_segment.html)).

## Insert Other Images

| Menu item | Behavior |
|---|---|
| **Insert Other Images...** | Opens a file dialog (labeled *Insert T1W/T2W...* in earlier DSI Studio releases; also triggered by the `+` button beside the slice selector) to load external 3D volumes or 2D images into the slice list. Supports NIfTI (`.nii`, `.nii.gz`), Analyze (`.hdr`), uncompressed DICOM series (`.dcm`), Bruker `2dseq` volumes (naming the slice from the parent `method` file when available), 2D pictures or histology (`.bmp`, `.jpg`, `.png`, `.tif`, `.tiff`), connectometry databases (`.dz`, `.db.fz`, `.db.fib.gz`, computing a demographic-matched volume when subject demographics are present or a group-averaged volume otherwise and warping it to diffusion space), and `http`/`https` image URLs (downloaded next to the `.fz` file). Selecting a single `.dcm` file prompts whether to load all `.dcm` files in that folder as one 3D volume; selecting multiple 2D picture files stacks them into a 3D volume (`1 × 1 × 1` mm nominal voxel spacing). |
| **Insert MNI Images...** | Opens a file dialog to load a template- or MNI-space volume (such as an atlas map, lesion probability map, or group statistical volume), computes the subject-to-template mapping if not yet initialized, and nonlinearly warps the volume into native subject diffusion space. When a template-space FIB dataset (such as QSDR) is open, alignment uses the NIfTI header transformation directly. |
| **Skull Strip...** | Projects the template brain mask into the currently selected custom slice's voxel grid, smooths the mask boundary with two 3D Gaussian passes to prevent jagged step edges, and multiplies the custom slice intensities in place. Applies only to inserted custom slices; built-in diffusion metrics are ignored. |
| **Delete Current Slice** | Removes the currently selected custom slice from the slice selector and 3D viewport (also triggered by the delete button beside the slice selector). Built-in diffusion metrics such as `qa`, `fa`, and `iso` cannot be deleted. |

DSI Studio also populates convenience slices automatically when opening a dataset:

- **Template-space datasets (QSDR):** Opening a template-space `.fz` file automatically lists `t1w_template`, `t2w_template`, and `wm_template` in the slice dropdown.
- **BIDS sibling volumes:** Opening a BIDS `sub-*` `.fz` file automatically lists sibling `sub-*_*.nii.gz` anatomical images found in the same directory and loads/registers each volume on demand when first selected.
- **Drag and drop:** Dragging non-label `.nii` or `.nii.gz` volumes into the tracking window loads them directly as custom slices, whereas integer-valued label NIfTI files are loaded into the **Assign Regions** dock.

### Slice registration

When **[Slices][Insert Other Images...]** loads a NIfTI or DICOM volume, DSI Studio determines slice-to-diffusion alignment in the following order:

1. **Template-space FIB (e.g., QSDR):** Aligns directly using the NIfTI header `sform` transformation without rigid-body optimization. If the NIfTI header has `sform_code < 4`, DSI Studio logs a warning because a non-MNI header is being placed into QSDR template space.
2. **`.mni.` in the filename** (or loaded via **[Slices][Insert MNI Images...]**): Treats a NIfTI volume whose grid dimensions differ from the diffusion volume as MNI/template-space data and warps it into native subject diffusion space. Conversely, if a subject-loaded NIfTI has `sform_code >= 4` without `.mni.` in its filename, DSI Studio logs a warning that an MNI-header image is being treated as native subject space.
3. **Matching dimensions with `reg` in the filename, or a saved `<image>.linear_reg.txt` sidecar:** If a NIfTI has the exact same voxel dimensions as the diffusion data and includes `reg` in its filename, it is treated as pre-aligned in diffusion space immediately. Otherwise, if `<image>.linear_reg.txt` exists alongside the image file, DSI Studio loads that saved linear mapping automatically; if `reg` appears in the filename of another volume, registration is skipped (`2D` datasets with fewer than `10` slices align by matching field-of-view scaling).
4. **Default (native subject-space FIB):** Starts background mutual-information rigid-body registration against the diffusion `iso` and anisotropy volumes (inserted 2D pictures use narrow-bound affine registration). Always inspect anatomical alignment across axial, coronal, and sagittal views before drawing ROIs, segmenting tissue, or sampling quantitative metrics.

| Menu item | Behavior |
|---|---|
| **Load Slice Registration...** | Stops any active background registration on the selected custom slice and applies a saved `.txt` linear registration file (accepting either DSI Studio's translocation/rotation/scaling parameter lines or a 4×4 transformation matrix). Cannot be applied to built-in diffusion slices. |
| **Save Slice Registration...** | Exports the selected custom slice's linear registration parameters and 4×4 slice-to-diffusion matrix to a `.txt` file. Saving the file as `<image_filename>.linear_reg.txt` next to the source volume allows DSI Studio to reload that alignment automatically whenever the image is inserted again. |
| **Adjust Slice Registration...** | Stops background registration on the selected custom slice and opens the interactive manual alignment dialog against the diffusion `iso` and anisotropy volumes (rigid-body mutual information) for manual translation, rotation, scaling, or re-optimization. Requires an inserted custom slice to be selected in the slice dropdown. |

![Slices menu and image registration interface](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

## Add Isosurface

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="Slices menu tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**[Slices][Add Isosurface]** extracts a 3D surface mesh from slice intensities and transforms the mesh into diffusion space:

- **Source volume and threshold:** Selecting any item prompts for an intensity threshold. When a built-in diffusion metric is active, DSI Studio loads the ICBM152 white-matter template warped to subject space (default threshold `25.0`, falling back to the active slice at `1.25 × Otsu` if template mapping is unavailable). When an inserted custom slice (such as a skull-stripped T1w MRI) is active, DSI Studio extracts the surface directly from that custom volume (default threshold `1.25 × Otsu`).
- **Surface styling and visibility:** In the **Options** panel under **Surface Rendering**, uncheck **Surface Rendering** to hide the mesh or adjust **Color**, **Opacity** (default `0.20`), lighting, and **Mesh Rendering** (**Original**, **Smoothed**, or **Smoothed2** morphological/Gaussian pre-smoothing; smoothing mode applies the next time an isosurface is generated).
- **Cutaway geometry:** Directional presets crop the source volume relative to the current 3D slice crosshair `(pos_x, pos_y, pos_z)` in active-slice voxel coordinates. Multi-axis presets keep the **union** of the named half-spaces, cutting away only the complementary wedge or octant to expose internal fiber tracts.

### Add Isosurface submenu

| Menu item | Behavior |
|---|---|
| **Full** | Prompts for an intensity threshold and extracts a complete 3D isosurface mesh across the entire volume with no cutaway. |
| **Right** | Prompts for a threshold and keeps only the right hemisphere half-space (`x < pos_x` relative to the sagittal slice crosshair). |
| **Left** | Prompts for a threshold and keeps only the left hemisphere half-space (`x >= pos_x` relative to the sagittal slice crosshair). |
| **Lower** | Prompts for a threshold and keeps only voxels inferior to the current axial slice plane (`z < pos_z`). |
| **Upper** | Prompts for a threshold and keeps only voxels superior to the current axial slice plane (`z >= pos_z`). |
| **Anterior** | Prompts for a threshold and keeps only voxels anterior to the current coronal slice plane (`y < pos_y`). |
| **Posterior** | Prompts for a threshold and keeps only voxels posterior to the current coronal slice plane (`y >= pos_y`). |
| **Right Lower** | Prompts for a threshold and keeps voxels that are right (`x < pos_x`) or inferior (`z < pos_z`), cutting away the left-superior wedge. |
| **Left Lower** | Prompts for a threshold and keeps voxels that are left (`x >= pos_x`) or inferior (`z < pos_z`), cutting away the right-superior wedge. |
| **Posterior Lower** | Prompts for a threshold and keeps voxels that are posterior (`y >= pos_y`) or inferior (`z < pos_z`), cutting away the anterior-superior wedge. |
| **Anterior Lower** | Prompts for a threshold and keeps voxels that are anterior (`y < pos_y`) or inferior (`z < pos_z`), cutting away the posterior-superior wedge. |
| **Right Anterior Lower** | Prompts for a threshold and keeps voxels that are right (`x < pos_x`), anterior (`y < pos_y`), or inferior (`z < pos_z`), cutting away the left-posterior-superior octant. |
| **Left Anterior Lower** | Prompts for a threshold and keeps voxels that are left (`x >= pos_x`), anterior (`y < pos_y`), or inferior (`z < pos_z`), cutting away the right-posterior-superior octant. |
| **Right Posterior Lower** | Intended to keep voxels that are right (`x < pos_x`), posterior (`y >= pos_y`), or inferior (`z < pos_z`), cutting away the left-anterior-superior octant; currently inactive when clicked in the desktop menu. |
| **Left Posterior Lower** | Prompts for a threshold and keeps voxels that are left (`x >= pos_x`), posterior (`y >= pos_y`), or inferior (`z < pos_z`), cutting away the right-anterior-superior octant. |

## Slice overlays

To composite statistical maps, lesions, PET, or multimodal MRI onto an anatomical or diffusion base slice:

1. Select the inserted image in the slice dropdown at the top of the slice toolbar.
2. Adjust its display intensity window (**Min** / **Max** spinboxes and sliders) and endpoint color buttons, or apply a lookup table via **[Slices][Load Color Map...]**.
3. Check **Overlay** to blend supra-threshold voxels (`50%` RGB blend for positive values strictly above **Min** and negative values strictly below **Max**) onto whichever base slice you view next, or check **Stay** to keep this slice's 3D planes visible in the 3D viewport after switching slices.
4. Switch the slice dropdown back to the anatomical or diffusion base slice. Multiple overlays can remain active at the same time; always verify spatial alignment before interpreting anatomical overlap.

The slice toolbar above the 3D viewport provides per-slice display controls:

- **Slice selector dropdown:** Switches the active volume shown in the 2D slice view and primary 3D planes (loading or registering listed BIDS or URL slices on demand).
- **Sagittal / Coronal / Axial checkboxes, spinboxes, and sliders:** Toggle visibility of each orthogonal plane in the 3D viewport and step the shared crosshair in active-slice voxel coordinates.
- **Min / Max values, sliders, and color buttons:** Set the display contrast window and two-color gradient endpoints for the active slice.
- **Overlay:** Blends supra-threshold positive (`> Min`) and negative (`< Max`) voxels from this slice onto other slices in both 2D and 3D views.
- **Stay:** Keeps this slice's enabled 3D planes rendered in the 3D scene when another slice is selected.
- **Directional Color:** Modulates slice RGB brightness by the local primary fiber orientation (`|dx|, |dy|, |dz|` mapped to red, green, and blue).

### Color maps, slice editing, and 2D pictures

| Menu item | Behavior |
|---|---|
| **Edit Slice Image...** | Opens the currently selected custom slice in DSI Studio's standalone Volume Viewer/Editor (`float32` intensities, voxel size, and header transformation) to crop, pad, resample, reorient axes, normalize intensities, filter, or inspect header matrices. Choosing **File › Save** inside that editor writes the modified volume and header back into the active custom slice in memory (`Image Updated`). Ignored when a built-in diffusion metric is selected. |
| **Load Color Map...** | Opens a file dialog (defaulting to DSI Studio's `color_map/` folder) to load a plain-text RGB lookup table (`.txt`) and applies it immediately to the currently selected slice. |

### [Insert Picture]

| Menu item | Behavior |
|---|---|
| **Insert Sagittal Picture...** | Opens a file dialog for 2D images (`.jpg`, `.tif`, `.bmp`, `.png`), loads the image as a thin 10-slice slab oriented to the sagittal plane at the current sagittal slider offset (in mm), turns on only the sagittal 3D plane (hiding coronal and axial planes), and prompts whether to apply automatic narrow-bound affine registration. Pictures wider than `2 ×` the maximum diffusion dimension are downsampled for interactive registration while retaining full resolution for high-resolution rendering. |
| **Insert Coronal Picture...** | Opens a 2D picture (`.jpg`, `.tif`, `.bmp`, `.png`) oriented to the coronal plane at the current coronal slider offset (in mm), turns on only the coronal 3D plane, and prompts whether to apply automatic narrow-bound affine registration. |
| **Insert Axial Picture...** | Opens a 2D picture (`.jpg`, `.tif`, `.bmp`, `.png`) oriented to the axial plane at the current axial slider offset (in mm), turns on only the axial 3D plane, and prompts whether to apply automatic narrow-bound affine registration. |

If you decline automatic registration when inserting a 2D picture, press `Ctrl+A` (**[Region Move][Move Region at 3D]**) and hold the **left** mouse button on the picture in the 3D viewport to translate it—or hold the **right** mouse button to resize it—close to the target anatomy before refining alignment with **[Slices][Adjust Slice Registration...]**. In the 2D slice view, you can also apply local landmark point-warping to an inserted 2D picture with the seventh drawing tool.

## Mark tracts or regions on DICOM slices

| Menu item | Behavior |
|---|---|
| **Save Slices as...** | Opens a save dialog to export the currently selected custom slice volume—including its header matrix and any skull-stripping, volume edits, or burned tracts/regions—as NIfTI (`.nii.gz`, `.nii`) or MATLAB (`.mat`). Rejects built-in diffusion metrics (export those from **Export** instead). |
| **Save Slices to DICOM...** | Prompts for an output folder and writes modified uncompressed DICOM files named `mod_<original_stem>.dcm`, replacing each file's 16-bit image buffer with the modified custom slice intensities while preserving original clinical DICOM headers and leaving source files untouched. Prompts for confirmation if `mod_*.dcm` files already exist in the destination folder. Requires the active custom slice to have been loaded directly from an uncompressed original DICOM series via **Slices › Insert Other Images...**; NIfTI volumes and compressed DICOM files are rejected. |
| **Mark Tracts on Slices** | Prompts for an intensity ratio (`0.0`–`10.0`, default `1.0`) and burns all **checked** tract bundles from the tract list into the active custom slice at `ratio × max_intensity` (sampling streamline segments at `0.2`-step sub-voxel intervals in custom-slice voxel space). Fails if no custom slice is active or if no tract bundle is checked. |
| **Mark Region on Slices** | Prompts for an intensity ratio (`0.0`–`10.0`, default `1.0`), resamples the currently selected region mask into the active custom slice's voxel grid using majority interpolation when grids differ, and sets masked voxels to `ratio × max_intensity`. Requires an active custom slice and a selected row in the region list. |

For surgical-navigation DICOM export workflows:

1. Load an **uncompressed original DICOM series** with **[Slices][Insert Other Images...]** and wait until registration finishes, then visually confirm anatomical alignment across all three orthogonal planes (compressed DICOMs must be converted to uncompressed DICOM before loading, and NIfTI-loaded volumes cannot be exported via **[Slices][Save Slices to DICOM...]**).
2. Check **only** the target fiber bundles in the tract list, or highlight the target region row in the region list.
3. Choose **[Slices][Mark Tracts on Slices]** or **[Slices][Mark Region on Slices]** and enter an intensity multiplier (for example, `1.2` writes `1.2 ×` the slice maximum so burned trajectories stand out clearly on surgical navigation stations). Caution: marking overwrites custom-slice voxels **cumulatively in memory** with no undo; reload the original slice if you need to clear prior marks.
4. Choose **[Slices][Save Slices to DICOM...]** and select a **separate output folder** to write `mod_*.dcm` files without modifying clinical source data, or choose **[Slices][Save Slices as...]** for NIfTI export. Slice loading, marking, and saving steps performed in the GUI are also logged in **[Records][Command History...]** for session review and batch replay (see the [Records menu](/doc/menu_records.html)).

## Workspaces

A workspace saves the active visualization session to a directory via **[File][Save Workspace...]** and restores it later on top of the same FIB dataset via **[File][Load Workspace...]** (see the [File menu](/doc/menu_file.html)):

- **`tracts/`:** Checked tractography bundles saved as `.tt.gz`.
- **`regions/`:** Checked region masks saved as `.nii.gz`.
- **`devices/`:** Checked stereotactic/surgical devices saved as `device.dv.csv`.
- **`slices/`:** An 8-bit normalized NIfTI copy (`<Name>.nii.gz`) and matching `<Name>.linear_reg.txt` alignment matrix when the currently selected slice is an inserted custom volume.
- **`camera.txt`:** 3D camera transformation matrix.
- **`commands.csv`:** Saved 3D slice crosshair voxel coordinates, orthogonal 3D plane visibility checkboxes, and 3D zoom level.
- **`setting.ini`:** Tracking and rendering parameters from the **Options** panel.

Caution: **[File][Save Workspace...]** stores only the currently active custom slice (normalized to 8-bit `0–255`), not unselected custom slices or the underlying `.fz` diffusion dataset. Include the `.fz` file when archiving or sharing a workspace folder.

## Shortcuts and graphic control

## Slice/region view

- **`Q` / `A`:** Step the sagittal slice `+1` / `-1` voxel.
- **`W` / `S`:** Step the coronal slice `+1` / `-1` voxel.
- **`E` / `D`:** Step the axial slice `+1` / `-1` voxel.
- **`Z` / `X` / `C`:** Switch the 2D slice view to sagittal, coronal, or axial orientation.
- **`F1`–`F6`:** Toggle fiber orientations (`F1`), tract intersections (`F2`), ruler/grid (`F3`), crosshair position lines (`F4`), the `R` orientation label (`F5`), and region outlines (`F6`).
- **Double-click:** Jump the 3D slice crosshair to the clicked voxel (in Single Slice or 3 Slices layout).
- **`Ctrl` + Mouse wheel:** Zoom the 2D slice view (plain mouse wheel also zooms when scrollbars are at their bounds).

## 3D view

- **Left drag:** Rotate the 3D camera (`Shift` + left drag locks horizontal rotation; holding `Ctrl` slows mouse motion `16×` for fine adjustment).
- **Right drag / Mouse wheel:** Zoom the 3D viewport. Right-clicking directly on a visible 3D slice plane opens quick actions including **Move Slices Here**, **Move Current Region Here**, **Move Current Device Here**, **New Region**, and **New Device**.
- **Middle drag / Arrow keys:** Pan the 3D scene.
- **Double-click a region:** Select that region in the **Assign Regions** dock.
- **`Ctrl+A`** (**[Region Move][Move Region at 3D]**): Drag a 3D slice plane along its normal axis, interactively move regions or devices in 3D, or hold the **left** / **right** mouse button on an inserted 2D picture slice to translate / resize it.

### Tract editing shortcuts

- **`Ctrl+S`:** Select tracts.
- **`Ctrl+D`:** Delete tracts.
- **`Ctrl+P`:** Paint tracts.
- **`Ctrl+X`:** Cut tracts.
- **`Ctrl+T`:** Trim tracts.
- **`Ctrl+Z`:** Undo tract selection or deletion.
- **`Ctrl+Y`:** Redo tract selection or deletion.

See the [Tract Edit menu](/doc/menu_edit.html) for tract-editing details.
