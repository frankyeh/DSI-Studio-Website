# [Regions] Menu

The **[Regions]** menu creates, loads, saves, combines, and measures 3D regions of interest (ROIs) for slice overlays, 3D rendering, fiber tracking, and statistics. Right-clicking anywhere in the **Assign Regions** table opens the same menu.

## Overview

The **Assign Regions** dock lists loaded regions with checkboxes (controlling 2D/3D visibility and inclusion in tracking or batch operations), tracking roles (`ROI`, `ROA`, `End`, `Seed`, `Terminative`, `NotEnd`, `Limiting`, or unassigned `...`; see [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html)), color swatches, voxel grid dimensions, and toolbar shortcuts such as **Atlas...** and **Segment...**. Single-region commands act on the **highlighted** row, whereas multi-region commands and fiber tracking use all **checked** rows.

## [Regions] Menu Actions

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">New Region</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F1</kbd></kbd>. Creates an empty checked region on the active slice's voxel grid for manual 2D drawing. Select the target diffusion or registered structural slice beforehand so the region inherits that grid. |
| <span class="gui-menu-item">Open Region...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F2</kbd></kbd>. Loads `.nii`, `.nii.gz`, `.hdr`, `.mat`, or `.txt` regions in diffusion or registered slice space, auto-warping mismatched NIfTIs with lowercase `mni` in the filename. Single masks load as one row; multi-label, `.txt`-labeled, and 4D NIfTIs split into separate checked rows. |
| <span class="gui-menu-item">Open MNI Region...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F3</kbd></kbd>. Loads MNI/template-space `.nii`, `.nii.gz`, `.hdr`, `.mat`, or `.txt` regions and nonlinearly warps them into subject diffusion space, running template registration first if needed. Caution: NIfTIs whose voxel dimensions already match a native-space FIB load directly without warping. |
| <span class="gui-menu-item">Save Current Region As...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F5</kbd></kbd>. Saves the highlighted region on its voxel grid as `.nii.gz` or `.nii` (binary mask preserving color and tracking role), `.mat` (3D mask), or `.txt` (0-based `x y z` voxel coordinates). Unrecognized file extensions default to `.nii.gz`. |
| <span class="gui-menu-item">Save Checked Regions to one 3D NIFTI...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F6</kbd></kbd>. Combines checked regions onto the first checked region's voxel grid as a 3D NIfTI parcellation with **1-based** label IDs (`1..N`) and a `<stem>.txt` label file. Caution: later rows overwrite overlapping voxels; save a 4D NIfTI to preserve overlaps. |
| <span class="gui-menu-item">Save Checked Regions to multiple 3D NIFTIs...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F7</kbd></kbd>. Exports each checked region to a chosen folder on its own voxel grid as `<region_name>.nii.gz` (or `.mat` / `.txt` if set as the default ROI format). Give checked regions unique names beforehand to avoid overwriting duplicate filenames. |
| <span class="gui-menu-item">Save Checked Regions to one 4D NIFTI...</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>S</kbd></kbd>. Stacks all checked regions onto the first checked region's grid as a 4D NIfTI volume with a **0-based** `<stem>.txt` label file, preserving overlapping voxels across volumes. Choose the menu item directly if <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>S</kbd></kbd> conflicts with **[Region Edit][Smoothing]**. |
| <span class="gui-menu-item">Check All</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>A</kbd></kbd>. Checks every region in the **Assign Regions** table, showing all regions in 2D/3D views and including them in fiber tracking and checked-region actions. |
| <span class="gui-menu-item">Uncheck All</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>U</kbd></kbd>. Unchecks every region in the **Assign Regions** table, hiding all regions from 2D/3D views and excluding them from fiber tracking and checked-region actions. |
| <span class="gui-menu-item">Merge Checked Regions</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>M</kbd></kbd>. Combines two or more checked regions onto the first checked region's voxel grid, replacing that first row with their union and deleting the other merged rows. Caution: **Undo** reverts the first row's mask, but cannot restore deleted rows. |
| <span class="gui-menu-item">Delete</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Backspace</kbd></kbd>. Permanently removes the highlighted region row from the **Assign Regions** table; row deletion cannot be undone with **Undo**. |
| <span class="gui-menu-item">Delete All</span> | Removes every region row from the **Assign Regions** table and resets automatic region color cycling. |
| <span class="gui-menu-item">Copy</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd></kbd>. Duplicates the highlighted region—including its mask, voxel grid, tracking role, and color—and inserts the checked copy directly below it. |
| <span class="gui-menu-item">Undo</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Z</kbd></kbd>. Reverts the last voxel-mask edit (drawing, morphological or boolean edits, flips, or the merged first row's mask) on the highlighted region. Does not undo row deletion, renaming, or role and color changes. |
| <span class="gui-menu-item">Redo</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Y</kbd></kbd>. Reapplies the most recently undone voxel-mask edit on the currently highlighted region row. |
| <span class="gui-menu-item">Tract-To-Region Connectome...</span> | Reports passing streamline counts, tract metrics, scalar means, and overlap volume (`<stem>_<tract>_t2r.txt`) between checked tract bundles and checked regions. Caution: compute streamline fraction from passing streamline counts rather than `intersect ratio` (voxel coverage); streamlines crossing multiple regions count in each. |
| <span class="gui-menu-item">Statistics...</span> | Reports voxel count, volume (`mm^3`), diffusion-grid voxel bounds, MNI `mm` coordinates (once template mapping is active), and nonzero scalar metrics (`<stem>_region_stat.txt`) across all checked regions, including per-subject means for connectometry databases. |
| <span class="gui-menu-item">Region-Atlas Intersection Statistics...</span> | Intersects the highlighted region with a chosen built-in atlas and reports full statistics and MNI coordinates (`<stem>_region_overlap_stat.txt`) for every overlapping parcel without adding rows to **Assign Regions**. |

For the complete tract-to-region workflow, see [Connectome (R2R / T2R)](/doc/gui_connectome.html#tract-to-region-t2r-connectome).

### File Formats and Coordinate Alignment

Multi-label and 4D NIfTI volumes resolve parcel names from companion `<stem>.txt` or `<stem>.json` files (or built-in FreeSurfer `aparc`/`aseg` tables), while `.txt` files store 0-based `x y z` voxel coordinates and `.mat` files store 3D masks. On native-space datasets, mismatched non-MNI NIfTIs require a matching structural slice loaded first via **[Slices][Insert Other Images...]**; on template-space datasets, NIfTI header transforms must match MNI space.

### Built-In Atlas Parcels

Choose a template at the top of the **Assign Regions** dock and click **Atlas...** to search built-in atlases and warp selected parcels onto the active slice grid individually (**Add**) or as one combined region (**Merge&Add**). Inspect warped parcels on native-space slices before tracking, and refine inaccurate registrations with **[Regions Misc][Manual Atlas Alignment...]**.

## [Regions Misc] Additional Features

Additional region-creation tools live under the [Regions Misc menu](/doc/menu_regions_misc.html).

### New Region From Thresholding

Creates a checked region on the active slice grid from an intensity threshold (positive values keep brighter voxels; negative values keep darker voxels).

### New Region From MNI Coordinate

Creates a spherical region on the active slice grid from millimeter MNI coordinates and a radius in voxels (`x y z radius`).

### Whole Brain Seeding

Creates a checked `whole brain` **Seed** region covering suprathreshold anisotropic tissue across the active slice grid.

## Editing and moving regions

For morphological filtering, thresholding, boolean set operations, flips, voxel shifts, and 3D placement, see the [Region Edit menu](/doc/menu_region_edit.html) and [Region Move menu](/doc/menu_region_move.html).

## Sorting, colors, and alignment

Region sorting, color-table import/export, per-voxel data export, crosshair centering, and atlas alignment tools are documented in the [Regions Misc menu](/doc/menu_regions_misc.html), while session step replay is covered in the [Records menu](/doc/menu_records.html).
