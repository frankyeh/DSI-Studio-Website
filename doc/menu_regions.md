# [Regions] Menu

Create, import, export, combine, and quantify 3D regions of interest (ROIs) in the Step T3 tracking window. Right-clicking anywhere in the **Assign Regions** table opens the same **[Regions]** menu.

## Overview

The **Assign Regions** dock lists every loaded region with four columns:

| Column | Purpose |
|:--|:--|
| **Name** (with checkbox) | Toggles 2D slice overlay / 3D surface visibility and marks the region as **checked** for fiber tracking, checked-region exports, merging, and statistics. Double-click to rename. |
| **Type** | Tracking role assigned to the region: `ROI` (`0`), `ROA` (`1`), `End` (`2`), `Seed` (`3`), `Terminative` (`4`), `NotEnd` (`5`), `Limiting` (`6`), or `...` (`7`, unassigned; initial state for newly created or loaded regions unless stored in NIfTI `descrip`). See [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html) for role semantics. |
| **Color** | Region overlay and 3D surface RGBA color (`Assigned` color mode). |
| **Dimension × Resolution (mm)** | Voxel grid dimensions and voxel spacing of the slice space in which the region is stored (diffusion grid or a registered structural slice grid). |

Notice the distinction between **selected** and **checked** rows: single-region operations (**Save Current Region As...**, **Copy**, **Undo**, **Redo**, **Region-Atlas Intersection Statistics...**, and default **Delete**) act on the currently highlighted row, whereas multi-region operations (**Save Checked Regions...**, **Merge Checked Regions**, **Statistics...**, **Tract-To-Region Connectome...**, and fiber tracking) act on all rows whose checkbox is checked.

## [Regions] Menu Actions

> **GUI command hints vs. CLI flags:** The `run ...` strings shown below come from `tracking_window.ui` action tooltips (`QAction::toolTip`) and are internal GUI/AI window command hints — **not** standalone `dsi_studio --action=...` command-line flags. See [Command History and CLI](#command-history-and-cli) for batch execution.

| Menu item | Shortcut / GUI command hint | Target & behavior |
|:--|:--|:--|
| New Region | `Ctrl+Shift+F1` · `run new_region` | Appends an empty checked region (`new region`, role `...`) matching the active slice's dimensions, voxel size, and slice-to-diffusion transform for manual 2D slice drawing. |
| Open Region... | `Ctrl+Shift+F2` · `run open_region` | Loads one or more `.nii`, `.nii.gz`, `.hdr`, `.mat`, or `.txt` region files in native/registered subject space (or MNI space when the filename contains `mni`); splits 3D multi-label parcellations and 4D NIfTI volumes into separate region rows. |
| Open MNI Region... | `Ctrl+Shift+F3` · `run open_mni_region` | Computes subject-to-template registration if needed and nonlinearly warps template/MNI-space NIfTI masks or multi-label parcellations into subject space using majority-vote interpolation. |
| Save Current Region As... | `Ctrl+Shift+F5` · `run save_region` | Saves the currently selected region as `.nii.gz` / `.nii` (`uint8` mask storing `color` and `roi` role in NIfTI `descrip`), `.mat` (`0`/`255` `uint8` 3D matrix), or `.txt` (`x y z` voxel coordinates per line); appends `.nii.gz` when no recognized extension is given. |
| Save Checked Regions to one 3D NIFTI... | `Ctrl+Shift+F6` · `run save_all_regions` | Writes all checked regions into one 3D label NIfTI on the first checked region's grid (`uint8` for ≤ 255 regions, `uint16` for > 255) with **1-based** integer IDs (`1..N`) and a companion `<stem>.txt` label table. Where regions overlap, later checked rows overwrite earlier ones. |
| Save Checked Regions to multiple 3D NIFTIs... | `Ctrl+Shift+F7` · `run save_all_regions_to_folder` | Exports every checked region into a chosen directory as `<region_name>.nii.gz` (or `.mat` / `.txt` when the active ROI output format is set accordingly). |
| Save Checked Regions to one 4D NIFTI... | `Ctrl+Shift+S` · `run save_4d_region` | Writes all checked regions into a single `X × Y × Z × N` `uint8` (`0`/`1`) 4D NIfTI on the first checked region's grid — preserving overlapping voxels across volumes — alongside a **0-based** `<stem>.txt` label table (`0..N-1`). |
| Check All | `Ctrl+Shift+A` · `run check_all_regions` | Checks every row in the **Assign Regions** table. |
| Uncheck All | `Ctrl+Shift+U` · `run uncheck_all_regions` | Unchecks (hides) every row in the **Assign Regions** table. |
| Merge Checked Regions | `Ctrl+Shift+M` · `run merge_regions` | Requires at least two checked regions; resamples later checked regions into the first checked region's grid, replaces that first region with the voxel union, and removes the remaining merged rows. |
| Delete | `Ctrl+Shift+Backspace` · `run delete_region` | Removes the currently selected region row from the table. |
| Delete All | `run delete_all_regions` | Removes all regions from the table and resets automatic region color cycling. |
| Copy | `Ctrl+Shift+V` · `run copy_region` | Duplicates the currently selected region (voxels, coordinate space, role, color, and edit history) and inserts the checked copy immediately below the selected row (not to the system clipboard). |
| Undo | `Ctrl+Shift+Z` · — | Reverts the most recent voxel drawing or mask-modification step on the currently selected region; does not undo row deletion or **Merge Checked Regions**. |
| Redo | `Ctrl+Shift+Y` · — | Re-applies the most recently undone voxel edit on the currently selected region. |
| Tract-To-Region Connectome... | `run show_t2r` | Requires ≥ 1 checked tract and ≥ 1 checked region; identifies streamlines intersecting each checked region and reports per-region tract counts, volumes, lengths, and scalar means in a dialog (default save name `<stem>_<tract>_t2r.txt`). |
| Statistics... | `run show_region_statistics` | Computes a tab-separated summary across all checked regions: voxel count, volume (`mm^3`), diffusion-space and MNI center of mass and bounding box, min/max/mean across loaded scalar slices, and per-subject connectometry DB metrics when present. |
| Region-Atlas Intersection Statistics... | `run show_region_overlap_statistics` | Prompts for a built-in atlas, intersects the currently selected region with every parcel of that atlas in the region's space **without adding rows to the region table**, omits zero-overlap parcels, and reports full region statistics for each overlapping parcel. |

### File Formats and Coordinate Alignment

- **Single-ROI vs. multi-label NIfTI (`Open Region...` / `Open MNI Region...`):** Binary or continuous scalar NIfTI volumes load as a single region (`voxel > 0`), restoring saved display color and tracking role when NIfTI `descrip` contains `color=...;roi=...`. Integer label volumes (or 3D NIfTIs paired with a `<stem>.txt` label file) and 4D NIfTI volumes split into one region per nonzero label or 4D volume. Parcel names resolve from `<stem>.txt` (`<id> <name>`), `<stem>.json` (`arealabel` / `labelIndex`), or built-in `FreeSurferColorLUT.txt` when the filename contains `aparc` or `aseg`.
- **Subject-space vs. structural-slice vs. MNI alignment:**
  - In **[Regions][Open Region...]**, if the filename contains `mni`, DSI Studio warps it from MNI space automatically. Otherwise, if image dimensions match the FIB grid, voxels load directly in diffusion space; if dimensions differ on a native-space FIB, DSI Studio reuses the registration transform of an already loaded structural slice with matching dimensions (load the structural T1w/T2w volume via **[Slices][Insert Other Images...]** first, or use **[Regions][Open MNI Region...]** for template masks). On an MNI-space FIB, header `srow` affine matrices align mismatched grids.
  - `.txt` coordinate files store whitespace-separated `x y z` voxel coordinates per line; an optional trailing `<scale> -1 -1` line defines a subvoxel resolution multiplier (`1 / scale`).

### Built-In Atlas Parcels

Click **Atlas...** at the top of the **Assign Regions** dock (`ui->addRegionFromAtlas`) to warp parcels from DSI Studio's built-in templates (`ICBM152_adult`, FreeSurfer/HCP-MMP, AAL3, Brainnetome, subcortical atlases, etc.) into the active slice's space:

- **Add** (`add_region_from_atlas "<template_id> <atlas_id> <label_id&label_id...>"`): Appends checked rows for the highlighted parcels (or all parcels when `<label_id>` is omitted in command history). Notice that `template_id` updates the active template before `atlas_id` is indexed.
- **Merge and Add**: Combines all highlighted parcels into a single region row named after the selected parcels.

## Command History and CLI

DSI Studio separates interactive GUI navigation, GUI/agent command hints, Command History replay, and standalone CLI actions:

1. **GUI menu paths** such as **[Regions][Open Region...]** trigger interactive Qt dialogs in the tracking window.
2. **`run ...` strings** (such as `run open_region`) are internal tracking-window / DSI Studio AI command hints, not shell flags.
3. **`--action=vis --cmd=...` ([Command History CLI](/doc/cli_vis.html))** replays recorded tracking-window commands (`+` separates chained commands, `,` separates arguments, and `&` joins multiple 0-based region row indices). **[Regions][Undo]** and **[Regions][Redo]** are direct GUI slots and have no `--cmd` token.
4. **Headless CLI actions** run region statistics, overlap tables, and ROI fiber tracking directly without opening the GUI via [`--action=ana`](/doc/cli_ana.html) and [`--action=trk`](/doc/cli_t3.html).

| `--action=vis` `--cmd` syntax | Notes & unattended batch behavior |
|:--|:--|
| `new_region` | Appends empty `"new region"` at the end of the region list. |
| `open_region,<path>` | Loads `<path>` (or `&`-joined paths) in subject/slice space. |
| `open_mni_region,<path>` | Warps MNI/template mask or parcellation `<path>` into subject space. |
| `add_region_from_atlas,<template_id> <atlas_id>[ <l1&l2...>]` | Space-separated `template_id`, `atlas_id`, and optional `&`-joined 0-based parcel indices (adds all parcels if omitted). |
| `save_region,<path>[,<index>]` | Saves 0-based `<index>` (default: selected row) to `.nii.gz`, `.nii`, `.mat`, or `.txt`. |
| `save_all_regions,<path.nii.gz>` | Saves checked regions as one 1-based 3D label NIfTI + `<stem>.txt`. |
| `save_all_regions_to_folder,<dir>` | Saves each checked region into `<dir>/<name>.nii.gz`. |
| `save_4d_region,<path.nii.gz>` | Saves checked regions as one `0`/`1` 4D NIfTI + 0-based `<stem>.txt`. |
| `check_all_regions` / `uncheck_all_regions` | Checks or unchecks all region rows (`check_region,<i1&i2>,1\|0` toggles specific rows). |
| `merge_regions[,<i1&i2...>]` | Merges checked regions (or explicit `&`-joined 0-based indices) into the first listed row. |
| `copy_region[,<index>]` | Duplicates row `<index>` (default: selected row) into `<index> + 1`. |
| `delete_region[,<i1&i2...>]` / `delete_all_regions` | Deletes selected/indexed rows or clears all regions. |
| `save_region_statistics,<out.txt>` | Unattended file export for `[Regions][Statistics...]` (`show_region_statistics` opens a modal dialog in `--action=vis`). |
| `save_t2r,<out.txt>` | Unattended file export for `[Regions][Tract-To-Region Connectome...]` (`show_t2r` opens a modal dialog in `--action=vis`). |
| `show_region_overlap_statistics,<index>,<atlas_name>` | Opens the overlap dialog in `--action=vis`; for unattended file output use `--action=ana --overlap=<atlas_name>` below. |

Example batch workflow (`--action=vis` vs. headless `--action=ana`):

```bash
# Load an MNI mask via --action=vis, export checked-region stats, and save the warped mask
dsi_studio --action=vis --source=sub01.fz --cmd="open_mni_region,lesion_mni.nii.gz+save_region_statistics,lesion_stat.txt+save_region,lesion_sub.nii.gz,0"

# Headless CLI region-atlas intersection statistics without GUI dialogs
dsi_studio --action=ana --source=sub01.fz --region=lesion_sub.nii.gz --overlap=HCP-MMP --output=lesion_hcpmmp_overlap.txt
```

## [Regions Misc] Additional Features

Additional region-generation commands live under the sibling **[Regions Misc]** top-level menu (`menuRegions_Misc`):

### New Region From Thresholding

**[Regions Misc][New Region From Thresholding...]** (`run new_region_from_threshold` / `--cmd="new_region_from_threshold,<thr>"`) appends `"New Region"` and masks voxels on the active slice satisfying `I > thr` (when prompting interactively, the default threshold is Otsu's threshold on the active slice; supplying a negative `<thr>` selects low-intensity voxels `I <= |thr|`).

### New Region From MNI Coordinate

**[Regions Misc][New Region From MNI Coordinate...]** (`run new_region_from_mni` / `--cmd="new_region_from_mni,<mni_x> <mni_y> <mni_z> <radius_vox>"`) maps the millimeter MNI coordinate `(mni_x, mni_y, mni_z)` into the active slice's voxel space and populates a spherical region with radius `<radius_vox>` in voxels.

### Whole Brain Seeding

**[Regions Misc][Whole Brain Seeding]** (`run new_region_whole_brain_seed` / `--cmd="new_region_whole_brain_seed[,<otsu_ratio>]"`) creates a checked `Seed` region named `"whole brain"` on the active slice grid where diffusion `fa0 > otsu_threshold * fa_otsu` (using the tracking window's `otsu_threshold` parameter unless an explicit `<otsu_ratio>` argument is passed).

## [Regions][Edit Region]

Morphological, voxel-mask, and spatial transform operations are organized under the top-level **[Region Edit]** (`menuRegion_Edit`) and **[Region Move]** (`menuRegion_Move`) menus:

- **[Region Edit]**: **Dilation** (`Ctrl+Shift+D`), **Dilation By Voxel...**, **Dilation By Threshold...**, **Erosion** (`Ctrl+Shift+E`), **Erosioin By Threshold...**, **Opening** (`Ctrl+Shift+O`), **Closing** (`Ctrl+Shift+C`), **Smoothing** (`Ctrl+Shift+S`), **Defragment** (`Ctrl+Shift+F`), **Separate** (`Ctrl+Shift+P`), **Negate** (`Ctrl+Shift+N`), **Threshold** (`Ctrl+Shift+T`), and multi-region set operations (**All Intersect First** `Ctrl+Shift+1`, **All Exclude First** `Ctrl+Shift+2`, **First Excludes All** `Ctrl+Shift+3`, **All Fit into First** `Ctrl+Shift+4`, **Refine All** `Ctrl+Shift+5`). Toggle **[Region Edit][Modify All Checked]** (`actionModify_All`) to apply edits across all checked regions instead of only the selected row.
- **[Region Move]**: **Flip X** (`Ctrl+7`), **Flip Y** (`Ctrl+8`), **Flip Z** (`Ctrl+9`), **Shift +X** / **Shift -X** (`Ctrl+1` / `Ctrl+2`), **Shift +Y** / **Shift -Y** (`Ctrl+3` / `Ctrl+4`), **Shift +Z** / **Shift -Z** (`Ctrl+5` / `Ctrl+6`), and **Move Region at 3D** (`Ctrl+A`).

## [Regions Misc][Advanced]

Advanced region utilities reside in **[Region Edit]** and **[Regions Misc]**:

- **Separate & Negate:** **[Region Edit][Separate]** (`Ctrl+Shift+P`, `region_action_separate`) runs connected-component labeling on the target region mask and appends up to `256` `<name>_1`, `<name>_2`, ... component rows, and **[Region Edit][Negate]** (`Ctrl+Shift+N`, `region_action_negate`) inverts the binary mask across the region's slice volume.
- **Sorting, Colors, Voxel Export, Slice Centering, and Atlas Mapping:** **[Regions Misc]** provides **Save Voxel Data As...** (`run save_region_info`), **Load Region Color...** / **Save Region Color...** (`run load_region_color` / `run save_region_color`), **Sort By Name**, **Sort By Size**, **Sort By X**, **Sort By Y**, **Sort By Z**, **Move Slices To Current Region** (`run move_slice_to_region`, centering slices on the selected region's per-axis median voxel), **Load MNI mapping...** (`run open_mapping`), and **Manual Atlas Alignment...**.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui) · [regiontablewidget.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/region/regiontablewidget.cpp).
