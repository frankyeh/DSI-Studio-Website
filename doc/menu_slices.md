# [Slices] Menu

Load external 3D volumes and 2D pictures into the Step T3 tracking window, align them to diffusion space, render 3D isosurfaces and slice overlays, burn tracts or regions into clinical slices, and export volumes or modified DICOMs. Built-in diffusion metric exports live under **[Export]**, and U-Net tissue/lesion models live under **[Slices Segment]**.

> **Command layers:**
> - **GUI path:** Interactive menu selections such as **[Slices][Insert Other Images...]**.
> - **GUI command hint (`run ...`):** Action `toolTip` strings from `tracking_window.ui` (used by internal GUI/AI command dispatch on a tracking window). These are **not** standalone `dsi_studio --action=...` CLI flags.
> - **Console / batch replay (`--action=vis`):** `dsi_studio --action=vis --source=sub.fz --cmd="<command>,<arg1>,..."` executes verified `tracking_window::command()` tokens (chain multiple commands with `+`; see [Command History](/doc/cli_vis.html)).

## Insert Other Images

| Menu item | GUI command hint | `--action=vis --cmd` | Behavior |
|:--|:--|:--|:--|
| Insert Other Images... | `run add_slice` | `add_slice,<path>` | Load NIfTI (`.nii`, `.nii.gz`), Analyze (`.hdr`), uncompressed DICOM series (`.dcm`), Bruker `2dseq`, 2D histology/pictures (`.bmp`, `.jpg`, `.png`, `.tif`, `.tiff`), connectometry databases (`.dz`, `.db.fz`, `.db.fib.gz`; computes a demographic-matched or group-averaged volume warped to subject space), or `http(s)` URLs. Selecting one `.dcm` in the GUI offers to load all `.dcm` files in that folder; pass comma-separated paths in script/AI calls. |
| Insert MNI Images... | `run add_mni_slice` | `add_mni_slice,<path>` | Compute subject-to-MNI mapping if needed and warp a template/MNI-space volume into native diffusion space. |
| Skull Strip... | `run skull_strip_slice` | `skull_strip_slice[,<slice_index>]` | Project the template brain mask into the selected custom slice, smooth the mask edge (two Gaussian passes), and multiply the custom slice volume in place. Built-in diffusion metrics are ignored. |
| Delete Current Slice | `run delete_slice` | `delete_slice[,<slice_index>]` | Remove an inserted custom slice (`CustomSliceModel`) from the slice list and 3D view. Built-in diffusion metrics cannot be deleted. |

Opening a BIDS `sub-*` FIB file also lists sibling `sub-*_*.nii.gz` volumes automatically, and non-label `.nii`/`.nii.gz` files can be dragged directly into the tracking window.

### Slice registration

When **[Slices][Insert Other Images...]** loads a NIfTI or DICOM volume, DSI Studio determines alignment in this order:

1. **Template-space FIB (e.g. QSDR):** aligns directly via the NIfTI header transform (`sform`).
2. **`.mni.` in filename:** treats the volume as MNI-space and warps it to subject space (`mni2sub`).
3. **Saved `<image>.linear_reg.txt` sidecar present:** loads the saved linear transform automatically without re-registering.
4. **`reg` in filename:** treats the volume as pre-aligned in diffusion space and skips registration.
5. **Default (subject-space FIB):** runs background rigid-body mutual-information registration against diffusion `iso`/`fa`. Always inspect anatomical alignment before ROI definition, segmentation, or quantitative sampling.

| Menu item | GUI command hint | `--action=vis --cmd` | Behavior |
|:--|:--|:--|:--|
| Load Slice Registration... | `run open_slice_mapping` | `open_slice_mapping,<mapping.txt>[,<slice_index>]` | Stop background registration on the custom slice and apply a saved `.txt` linear registration (`affine_param` or 4×4 matrix). |
| Save Slice Registration... | `run save_slice_mapping` | `save_slice_mapping,<mapping.txt>[,<slice_index>]` | Export the custom slice's registration parameters and `to_dif` matrix to `.txt` (save as `<image>.linear_reg.txt` for automatic reload). |
| Adjust Slice Registration... | — | — | Stop background registration and open the interactive rigid-body mutual-information alignment dialog against diffusion `iso`/`fa` for manual translation, rotation, scaling, or re-optimization. |

![Slices menu and image registration interface](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

## Add Isosurface

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="Slices menu tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**[Slices][Add Isosurface]** extracts a 3D surface mesh and transforms it into diffusion space:

- **Source & threshold:** When a built-in diffusion metric is selected, DSI Studio loads the ICBM152 white-matter template warped to subject space (default threshold `25.0`, falling back to the active slice if template mapping is unavailable). When an inserted custom slice is selected, it extracts the surface directly from that volume (default threshold `1.25 × Otsu`). GUI selection prompts for the threshold; `--cmd` requires `<slice_index>,<threshold>` (use `clear_surface` to remove the mesh).
- **Cutaway presets:** Directional presets keep voxels on the named side(s) of the current 3D crosshair (`slice_pos`). Multi-axis presets keep the union of the named half-spaces, cutting away the complementary wedge to expose internal tracts. Adjust mesh smoothing (`surface_mesh_smoothed`), color, and opacity (`surface_alpha`) in the rendering options panel.

### Add Isosurface submenu

| Menu item | GUI command hint | `--action=vis --cmd` | Kept region (relative to crosshair) |
|:--|:--|:--|:--|
| Full | `run add_surface` | `add_surface,<slice_index>,<threshold>` | Entire volume (no cutaway). |
| Right | `run add_surface_right` | `add_surface_right,<slice_index>,<threshold>` | Right of sagittal plane (`x < pos_x`). |
| Left | `run add_surface_left` | `add_surface_left,<slice_index>,<threshold>` | Left of sagittal plane (`x >= pos_x`). |
| Lower | `run add_surface_lower` | `add_surface_lower,<slice_index>,<threshold>` | Inferior to axial plane (`z < pos_z`). |
| Upper | `run add_surface_upper` | `add_surface_upper,<slice_index>,<threshold>` | Superior to axial plane (`z >= pos_z`). |
| Anterior | `run add_surface_anterior` | `add_surface_anterior,<slice_index>,<threshold>` | Anterior to coronal plane (`y < pos_y`). |
| Posterior | `run add_surface_posterior` | `add_surface_posterior,<slice_index>,<threshold>` | Posterior to coronal plane (`y >= pos_y`). |
| Right Lower | `run add_surface_right_lower` | `add_surface_right_lower,<slice_index>,<threshold>` | Right or inferior (cuts away left-upper wedge). |
| Left Lower | `run add_surface_left_lower` | `add_surface_left_lower,<slice_index>,<threshold>` | Left or inferior (cuts away right-upper wedge). |
| Posterior Lower | `run add_surface_posterior_lower` | `add_surface_posterior_lower,<slice_index>,<threshold>` | Posterior or inferior (cuts away anterior-upper wedge). |
| Anterior Lower | `run add_surface_anterior_lower` | `add_surface_anterior_lower,<slice_index>,<threshold>` | Anterior or inferior (cuts away posterior-upper wedge). |
| Right Anterior Lower | `run add_surface_right_anterior_lower` | `add_surface_right_anterior_lower,<slice_index>,<threshold>` | Right, anterior, or inferior (cuts away left-posterior-upper octant). |
| Left Anterior Lower | `run add_surface_left_anterior_lower` | `add_surface_left_anterior_lower,<slice_index>,<threshold>` | Left, anterior, or inferior (cuts away right-posterior-upper octant). |
| Right Posterior Lower | — | `add_surface_right_posterior_lower,<slice_index>,<threshold>` | Right, posterior, or inferior (`toolTip` omitted in `.ui`; supported via `--cmd` / AI). |
| Left Posterior Lower | `run add_surface_left_posterior_lower` | `add_surface_left_posterior_lower,<slice_index>,<threshold>` | Left, posterior, or inferior (cuts away right-anterior-upper octant). |

## Slice overlays

To composite statistical maps, lesions, or multimodal volumes onto another slice:

1. Select the inserted image in the slice selector (`SliceModality`).
2. Set its display range/colors or apply **[Slices][Load Color Map...]**.
3. Enable **Overlay** (`is_overlay`) to blend supra-threshold voxels (positive values `> min` and negative values `< max`) onto whichever base slice you view next, or enable **Stay** (`stay`) to keep this slice's 3D planes visible in the 3D viewport after switching slices.
4. Switch back to the anatomical or diffusion base slice. Multiple overlays can be active simultaneously; verify registration before interpreting spatial overlap.

| Slice panel / agent control | Widget hint | `--action=vis --cmd` / AI command | Behavior |
|:--|:--|:--|:--|
| Slice selector (`SliceModality`) | — | `set_slice,<index>` · `set_slice_by_name,<name>` | Switch active slice (loads/registers URL or custom slices on demand; inspect readiness via AI `list_slice`). |
| Sagittal / Coronal / Axial checks | `run enable_slice` | `enable_slice,<sag> <cor> <axi>` | Toggle 3D slice plane visibility (e.g. `enable_slice,1 1 0`). |
| Slice position sliders | `run move_slice` | `move_slice,<x> <y> <z>` | Move the 3D crosshair in active slice voxel coordinates. |
| Min / Max value & color | — | `set_slice_contrast,<min> <max>[,<cmin> <cmax>]` | Set display intensity window (and optional packed RGB min/max colors) for the active slice. |
| Overlay (`is_overlay`) | `run set_slice_overlay` | `set_slice_overlay,<slice_index>,<0\|1>` | Composite this slice onto other slices in 2D and 3D views. |
| Stay (`stay`) | `run set_slice_stay` | `set_slice_stay,<slice_index>,<0\|1>` | Keep this slice's planes rendered in 3D when another slice is selected. |
| Directional Color | `run set_slice_dir_color` | `set_slice_dir_color,<slice_index>,<0\|1>` | Modulate slice RGB intensity by primary fiber orientation (`\|dx\|, \|dy\|, \|dz\|`). |

### Color maps, slice editing, and 2D pictures

| Menu item | GUI command hint | `--action=vis --cmd` | Behavior |
|:--|:--|:--|:--|
| Load Color Map... | `run load_color_map` | `load_color_map,<colormap.txt>` | Apply an RGB lookup table `.txt` file (default folder `color_map/`) to the currently selected slice. |
| Edit Slice Image... | — | — | Open the current custom slice in DSI Studio's volume editor (`view_image`) to crop, resample, normalize, or edit header transforms; saving updates the slice in memory. |
| **Insert Picture** → Insert Sagittal Picture... | — | — | Load a 2D picture (`.jpg`, `.tif`, `.bmp`, `.png`) as a thin sagittal slab at the current sagittal slider position, show only the sagittal 3D plane, and offer affine registration. |
| **Insert Picture** → Insert Coronal Picture... | — | — | Load a 2D picture oriented to the coronal plane at the current coronal slider position and offer affine registration. |
| **Insert Picture** → Insert Axial Picture... | — | — | Load a 2D picture oriented to the axial plane at the current axial slider position and offer affine registration. |

If automatic registration is declined when inserting a 2D picture, press `Ctrl+A` (**[Region Move][Move Region at 3D]**) and hold the **left** or **right** mouse button on the picture in the 3D view to translate or scale it close to the target anatomy before refining with **[Slices][Adjust Slice Registration...]**.

## Mark tracts or regions on DICOM slices

| Menu item | GUI command hint | `--action=vis --cmd` | Behavior |
|:--|:--|:--|:--|
| Save Slices as... | `run save_slice_volume` | `save_slice_volume,<output.nii.gz>[,<slice_index>]` | Save the custom slice volume (including skull-stripping, edits, or burned tracts/regions) as `.nii.gz`/`.nii` or `.mat`. Rejects built-in diffusion metrics (use `[Export]` instead). |
| Mark Tracts on Slices | — | `mark_tracts_on_slices,<ratio>` | Burn all **checked** tract bundles into the active custom slice at `ratio × max_intensity` (GUI prompts for `ratio`, default `1.0`, range `0.0–10.0`; fails if no tract is checked). |
| Mark Region on Slices | — | `mark_region_on_slices,<region_index>,<ratio>` | Resample one region mask (majority interpolation) into the active custom slice and burn its voxels at `ratio × max_intensity`. |
| Save Slices to DICOM... | — | `save_slices_to_dicom,<output_dir>` | Write modified uncompressed DICOM files (`mod_<original_stem>.dcm`) to `<output_dir>`, preserving original clinical DICOM headers while leaving source files untouched. |

For surgical-navigation DICOM export:

1. Load an **uncompressed original DICOM series** via **[Slices][Insert Other Images...]** and wait until registration finishes (NIfTI inputs and compressed DICOMs cannot be exported by **[Slices][Save Slices to DICOM...]**).
2. Check only the target bundles in the tract list (or select the target region).
3. Run **[Slices][Mark Tracts on Slices]** or **[Slices][Mark Region on Slices]**. Marking modifies custom-slice voxels cumulatively in memory; reload the slice to clear prior marks.
4. Run **[Slices][Save Slices to DICOM...]** to write `mod_*.dcm` into a separate folder (or **[Slices][Save Slices as...]** for NIfTI output).

```bash
# Example: burn checked tract 0 into an inserted NIfTI T1w slice and save as NIfTI via --action=vis
dsi_studio --action=vis --source=sub-01.fz \
  --cmd="open_tract,cst.tt.gz+add_slice,T1w_reg.nii.gz+show_only_tracts,0+mark_tracts_on_slices,1.2+save_slice_volume,T1w_marked.nii.gz"
```

## Workspaces

A workspace saves the active visualization state to a directory via **[File][Save Workspace...]** (`run save_workspace`) and restores it via **[File][Load Workspace...]** (`run load_workspace`).

| Item | Purpose |
|:--|:--|
| `tracts/` | Saved tractography bundles (`.tt.gz`). |
| `regions/` | Saved region masks (`.nii.gz`). |
| `devices/` | Saved device table (`device.dv.csv`). |
| `slices/` | Active custom slice volume (`<Name>.nii.gz`, 8-bit normalized) and its `<Name>.linear_reg.txt` transform. |
| `camera.txt` | 3D camera matrix. |
| `commands.csv` | Slice crosshair (`move_slice`), 3D plane visibility (`enable_slice`), and 3D zoom (`set_zoom`). |
| `setting.ini` | Rendering and tracking parameters. |

Include the underlying `.fz` file when sharing a workspace folder.

## Shortcuts and graphic control

## Slice/region view

| Shortcut | Action |
|:--|:--|
| `Q` / `A` | Move sagittal slice (`+1` / `-1`). |
| `W` / `S` | Move coronal slice (`+1` / `-1`). |
| `E` / `D` | Move axial slice (`+1` / `-1`). |
| `Z` / `X` / `C` | Switch to sagittal / coronal / axial view. |
| `F1`–`F6` | Toggle fiber directions (`F1`), tracts (`F2`), ruler/grid (`F3`), crosshair (`F4`), `R` label (`F5`), and region edges (`F6`). |
| Double-click | Jump 3D slice crosshair to clicked voxel (single/3-slice view). |
| `Ctrl` + Mouse wheel | Zoom 2D slice view (mouse wheel also zooms when scrollbars are at bounds). |

## 3D view

- **Left drag:** rotate (`Shift` + left drag locks horizontal rotation; `Ctrl` slows mouse motion 16×).
- **Right drag / Mouse wheel:** zoom (right-clicking a visible 3D slice opens quick actions including **Move Slices Here**, **New Region**, and **New Device**).
- **Middle drag / Arrow keys:** pan the scene.
- **Double-click a region:** select it in the region list.
- **`Ctrl+A`** (**[Region Move][Move Region at 3D]**): drag a 3D slice plane along its normal, drag regions/devices, or left-drag / right-drag an inserted 2D picture slice to translate / scale it.

### Tract editing shortcuts

| Shortcut | Action |
|:--|:--|
| `Ctrl+S` | Select tracts. |
| `Ctrl+D` | Delete tracts. |
| `Ctrl+P` | Paint tracts. |
| `Ctrl+X` | Cut tracts. |
| `Ctrl+T` | Trim tracts. |
| `Ctrl+Z` | Undo selection/deletion. |
| `Ctrl+Y` | Redo selection/deletion. |

See the [Edit menu](/doc/menu_edit.html) for tract-editing details.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui), [tracking_window.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.cpp), [tracking_window_action.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window_action.cpp), and [SliceModel.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/SliceModel.cpp).
