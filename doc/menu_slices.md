# [Slices] Menu

Use the **[Slices]** menu to load external 3D volumes or 2D histology images, align them to diffusion space, build 3D brain isosurfaces, blend multimodal overlays, and burn tracts or regions onto anatomical slices. Export built-in diffusion metrics from the [Export menu](/doc/menu_export.html), and access automated deep-learning parcellations in the [Slices Segment menu](/doc/menu_slices_segment.html).

## Insert Other Images

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Insert Other Images...</span> | Loads external 3D volumes or 2D images into the slice list (previously labeled *Insert T1W/T2W...*). Supports NIfTI (`.nii`, `.nii.gz`), Analyze (`.hdr`), uncompressed DICOM (`.dcm`), Bruker `2dseq`, 2D pictures (`.bmp`, `.jpg`, `.png`, `.tif`, `.tiff`), connectometry databases (`.dz`, `.db.fz`, `.db.fib.gz`), and image URLs. |
| <span class="gui-menu-item">Insert MNI Images...</span> | Loads an MNI- or template-space volume (such as an atlas or group statistical map) and nonlinearly warps it into native subject diffusion space. In template-space datasets (such as QSDR), alignment uses the NIfTI header directly. |
| <span class="gui-menu-item">Skull Strip...</span> | Masks out non-brain tissue in the selected custom slice in place using a smoothed brain mask projected from template space. Applies only to inserted custom slices; built-in diffusion metrics are ignored. |
| <span class="gui-menu-item">Delete Current Slice</span> | Removes the currently selected custom slice from the slice selector and 3D viewport. Built-in diffusion metrics such as `qa`, `fa`, and `iso` cannot be deleted. |

Template-space (QSDR) datasets automatically list anatomical templates in the slice selector, and BIDS datasets list sibling anatomical volumes in the same folder for on-demand loading. Dragging non-label `.nii` or `.nii.gz` files into the window loads them as custom slices, whereas integer label volumes open in **Assign Regions**.

### Slice registration

Inserted NIfTI and DICOM volumes align automatically—using header coordinates in template space, nonlinear warping when `.mni.` is in the filename, a saved `<image>.linear_reg.txt` sidecar when present, or background rigid-body registration in native subject space.

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Load Slice Registration...</span> | Applies a saved `.txt` linear alignment file to the selected custom slice and stops any running background registration. Cannot be used on built-in diffusion metrics. |
| <span class="gui-menu-item">Save Slice Registration...</span> | Saves the selected custom slice's linear alignment to diffusion space as a `.txt` file. Naming it `<image_filename>.linear_reg.txt` next to the source volume reloads that alignment automatically when re-inserted. |
| <span class="gui-menu-item">Adjust Slice Registration...</span> | Opens the manual alignment dialog for the selected custom slice to translate, rotate, scale, or re-optimize registration against the diffusion volume. Requires an inserted custom slice to be selected. |

Always confirm anatomical alignment across sagittal, coronal, and axial views before drawing regions or exporting clinical slices.

![Slices menu and image registration interface](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

## Add Isosurface

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="Slices menu tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Isosurfaces extract a 3D surface mesh from the active custom slice (or from a warped white-matter template when a built-in diffusion metric is selected), with visibility, color, opacity, and mesh smoothing controlled in **Options** under **Surface Rendering**. Directional presets cut away part of the volume relative to the current 3D slice crosshair to expose internal fiber tracts.

### Add Isosurface submenu

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Full</span> | Prompts for an intensity threshold and extracts a complete 3D isosurface mesh across the entire volume without a cutaway. |
| <span class="gui-menu-item">Right</span> | Prompts for an intensity threshold and builds an isosurface keeping only the right side of the current sagittal slice plane. |
| <span class="gui-menu-item">Left</span> | Prompts for an intensity threshold and builds an isosurface keeping only the left side of the current sagittal slice plane. |
| <span class="gui-menu-item">Lower</span> | Prompts for an intensity threshold and builds an isosurface keeping only the region inferior to the current axial slice plane. |
| <span class="gui-menu-item">Upper</span> | Prompts for an intensity threshold and builds an isosurface keeping only the region superior to the current axial slice plane. |
| <span class="gui-menu-item">Anterior</span> | Prompts for an intensity threshold and builds an isosurface keeping only the region anterior to the current coronal slice plane. |
| <span class="gui-menu-item">Posterior</span> | Prompts for an intensity threshold and builds an isosurface keeping only the region posterior to the current coronal slice plane. |
| <span class="gui-menu-item">Right Lower</span> | Prompts for an intensity threshold and cuts away the left-superior wedge relative to the sagittal and axial slice planes. |
| <span class="gui-menu-item">Left Lower</span> | Prompts for an intensity threshold and cuts away the right-superior wedge relative to the sagittal and axial slice planes. |
| <span class="gui-menu-item">Posterior Lower</span> | Prompts for an intensity threshold and cuts away the anterior-superior wedge relative to the coronal and axial slice planes. |
| <span class="gui-menu-item">Anterior Lower</span> | Prompts for an intensity threshold and cuts away the posterior-superior wedge relative to the coronal and axial slice planes. |
| <span class="gui-menu-item">Right Anterior Lower</span> | Prompts for an intensity threshold and cuts away the left-posterior-superior octant relative to the three slice crosshair planes. |
| <span class="gui-menu-item">Left Anterior Lower</span> | Prompts for an intensity threshold and cuts away the right-posterior-superior octant relative to the three slice crosshair planes. |
| <span class="gui-menu-item">Right Posterior Lower</span> | Intended to cut away the left-anterior-superior octant relative to the three slice crosshair planes, but is currently inactive in the desktop menu. |
| <span class="gui-menu-item">Left Posterior Lower</span> | Prompts for an intensity threshold and cuts away the right-anterior-superior octant relative to the three slice crosshair planes. |

## Slice overlays

To composite statistical maps, lesions, PET, or multimodal scans onto a base slice, select the inserted image in the slice toolbar, set its **Min** / **Max** window or colormap, check **Overlay** (blends supra-threshold voxels onto other slices) or **Stay** (keeps its 3D planes visible), and switch back to the base slice. The slice toolbar above the 3D viewport also controls orthogonal plane visibility, crosshair position, two-color contrast endpoints, and **Directional Color** modulation.

### Color maps, slice editing, and 2D pictures

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Edit Slice Image...</span> | Opens the selected custom slice in the standalone Volume Viewer/Editor to crop, pad, resample, reorient, or filter intensities; choosing **File › Save** updates the slice in memory. Ignored for built-in diffusion metrics. |
| <span class="gui-menu-item">Load Color Map...</span> | Loads a plain-text RGB lookup table (`.txt`, defaulting to DSI Studio's `color_map/` folder) and applies it immediately to the currently selected slice. |

### [Insert Picture]

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Insert Sagittal Picture...</span> | Loads a 2D image (`.jpg`, `.tif`, `.bmp`, `.png`) onto the sagittal plane at the current sagittal offset (in mm), shows only the sagittal 3D plane, and offers automatic affine registration. |
| <span class="gui-menu-item">Insert Coronal Picture...</span> | Loads a 2D image (`.jpg`, `.tif`, `.bmp`, `.png`) onto the coronal plane at the current coronal offset (in mm), shows only the coronal 3D plane, and offers automatic affine registration. |
| <span class="gui-menu-item">Insert Axial Picture...</span> | Loads a 2D image (`.jpg`, `.tif`, `.bmp`, `.png`) onto the axial plane at the current axial offset (in mm), shows only the axial 3D plane, and offers automatic affine registration. |

If you skip automatic registration when inserting a 2D picture, press <kbd><kbd>Ctrl</kbd>+<kbd>A</kbd></kbd> (**[Region Move][Move Region at 3D]**) to drag (left-click) or resize (right-click) the picture in 3D before refining alignment.

## Mark tracts or regions on DICOM slices

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Save Slices as...</span> | Exports the selected custom slice—including its alignment header and any skull-stripping, edits, or burned tracts/regions—as NIfTI (`.nii.gz`, `.nii`) or MATLAB (`.mat`). Built-in diffusion metrics are exported from **Export** instead. |
| <span class="gui-menu-item">Save Slices to DICOM...</span> | Writes modified uncompressed DICOM files (`mod_<stem>.dcm`) to a chosen folder while preserving original clinical DICOM headers and leaving source files untouched. Requires a custom slice loaded directly from an uncompressed DICOM series; NIfTI and compressed DICOM inputs are rejected. |
| <span class="gui-menu-item">Mark Tracts on Slices</span> | Prompts for an intensity multiplier (`0.0`–`10.0`, default `1.0`) and burns all **checked** tract bundles into the active custom slice in memory (not undoable). Requires an active custom slice and at least one checked tract. |
| <span class="gui-menu-item">Mark Region on Slices</span> | Prompts for an intensity multiplier (`0.0`–`10.0`, default `1.0`) and burns the **highlighted** region mask into the active custom slice in memory (not undoable). Requires an active custom slice and a highlighted region row. |

For surgical navigation export, load an uncompressed original DICOM series, confirm alignment across all three planes, burn the checked tracts or highlighted region (for example at ratio `1.2` so trajectories stand out), and write `mod_*.dcm` files to a separate output folder. Marking overwrites custom-slice voxels cumulatively in memory with no undo; reload the original slice to clear marks, or review logged GUI steps in the [Records menu](/doc/menu_records.html).

## Workspaces

Save and reload visualization sessions (checked tracts, checked regions, devices, camera view, display settings, and the active custom slice with its registration) via **[File][Save Workspace...]** and **[File][Load Workspace...]** (see the [File menu](/doc/menu_file.html)). Workspaces store an 8-bit normalized (`0–255`) copy of only the currently selected custom slice and omit unselected custom slices and the `.fz` diffusion file.

## Shortcuts and graphic control

## Slice/region view

- **`Q` / `A`, `W` / `S`, `E` / `D`:** Step the sagittal, coronal, or axial slice `+1` / `-1` voxel.
- **`Z` / `X` / `C`:** Switch the 2D slice view to sagittal, coronal, or axial orientation.
- **<kbd><kbd>F1</kbd></kbd>–<kbd><kbd>F6</kbd></kbd>:** Toggle fiber orientations (<kbd><kbd>F1</kbd></kbd>), tract intersections (<kbd><kbd>F2</kbd></kbd>), ruler/grid (<kbd><kbd>F3</kbd></kbd>), crosshair lines (<kbd><kbd>F4</kbd></kbd>), `R` label (<kbd><kbd>F5</kbd></kbd>), and region outlines (<kbd><kbd>F6</kbd></kbd>).
- **Double-click / `Ctrl` + wheel:** Jump the 3D crosshair to the clicked voxel, or zoom the 2D slice view.

## 3D view

- **Left / Right / Middle drag:** Rotate (`Shift` locks horizontal; `Ctrl` slows motion), zoom (or right-click a 3D slice plane for quick actions), and pan (or use arrow keys).
- **<kbd><kbd>Ctrl</kbd>+<kbd>A</kbd></kbd>:** Interactively drag 3D slice planes, regions, devices, or 2D pictures.

### Tract editing shortcuts

- **<kbd><kbd>Ctrl</kbd>+<kbd>S</kbd></kbd> / <kbd><kbd>Ctrl</kbd>+<kbd>D</kbd></kbd> / <kbd><kbd>Ctrl</kbd>+<kbd>T</kbd></kbd> / <kbd><kbd>Ctrl</kbd>+<kbd>X</kbd></kbd> / <kbd><kbd>Ctrl</kbd>+<kbd>P</kbd></kbd>:** Select, delete, trim, cut, or paint tracts (<kbd><kbd>Ctrl</kbd>+<kbd>Z</kbd></kbd> / <kbd><kbd>Ctrl</kbd>+<kbd>Y</kbd></kbd> to undo or redo selection and deletion; see the [Tract Edit menu](/doc/menu_edit.html)).
