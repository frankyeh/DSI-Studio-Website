# [Regions Misc] Menu

The **[Regions Misc]** menu creates threshold, MNI-coordinate, and whole-brain seed masks, manages region colors and list sorting, exports voxel-level metrics, and refines subject-to-atlas alignment. For loading and saving masks, editing shapes, or constraining tractography, see the [Regions menu](/doc/menu_regions.html), [Region Edit menu](/doc/menu_region_edit.html), [Region Move menu](/doc/menu_region_move.html), and [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html).

## Create regions

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">New Region From Thresholding...</span> | Creates a checked visualization-only (**Type** `...`) region on the active slice from an intensity threshold pre-filled with Otsu's estimate (use a negative value to select lower intensities instead). Set **Type** to constrain tracking; canceling leaves an empty `New Region` row. |
| <span class="gui-menu-item">New Region From MNI Coordinate...</span> | Creates a checked spherical region (**Type** `...`) on the active slice from `x y z radius`, computing subject-to-MNI registration automatically if needed. Note the mixed units: center `(x, y, z)` is in MNI **mm**, whereas `radius` is in active-slice **voxels**. |

## Region colors

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Load Region Color...</span> | Loads display colors (`0`–`255`) for all listed regions from a `.txt` file using positional `R G B` / `R G B A` lines or tab-separated `Name R G B A` rows. Mismatched counts, unknown or duplicate names, or invalid values abort the load unchanged. |
| <span class="gui-menu-item">Save Region Color...</span> | Saves the assigned `R G B A` display color (`0`–`255`) of every listed region in row order to a `.txt` file, ready to reload with **Regions Misc › Load Region Color...**. |

## Sort regions

Sorting reorders **checked** regions within their checked row slots while leaving unchecked rows in place; selecting the same sort again reverses ascending and descending order.

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Sort By Name</span> | Sorts checked regions alphabetically by **Name**, reversing between ascending and descending order when selected again. |
| <span class="gui-menu-item">Sort By Size</span> | Sorts checked regions by physical volume in **mm³** from smallest to largest, reversing to largest-first when selected again. |
| <span class="gui-menu-item">Sort By X</span> | Sorts checked regions by center-of-mass **X** position in diffusion voxel space (reversing on repeat selection). Avoid sorting empty (`0`-voxel) checked regions, which lack a center of mass. |
| <span class="gui-menu-item">Sort By Y</span> | Sorts checked regions by center-of-mass **Y** position in diffusion voxel space, reversing order when selected again. |
| <span class="gui-menu-item">Sort By Z</span> | Sorts checked regions by center-of-mass **Z** position in diffusion voxel space, reversing order when selected again. |

## Seeding and voxel data

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Whole Brain Seeding</span> | Creates a checked `whole brain` region (**Type** `Seed`) on the active slice from primary anisotropy using the current **Otsu Threshold** in Tracking Parameters, letting you inspect, edit, or save the seed mask before tracking. |
| <span class="gui-menu-item">Save Voxel Data As...</span> | Exports per-voxel data for the **highlighted** region row to a tab-delimited `.txt` table containing diffusion voxel coordinates, resolved fiber unit vectors, and values from all loaded diffusion metrics and slices. Large masks can produce very large text files. |

## Mapping and alignment

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Move Slices To Current Region</span> | Centers the 2D and 3D slice crosshairs on the **highlighted** region's median voxel position across X, Y, and Z so outlier fragments do not pull the view off target. Has no effect if no non-empty region is highlighted. |
| <span class="gui-menu-item">Load MNI mapping...</span> | Loads a precomputed nonlinear subject-to-template mapping file (`.mz`) matching the open FIB dataset and updates subsequent atlas parcellations and MNI region operations to use the loaded warp. |
| <span class="gui-menu-item">Manual Atlas Alignment...</span> | Opens an interactive dialog to align the subject's maps to the active template (unavailable for QSDR datasets already in template space). Accepting recomputes subject-to-MNI registration, replaces any cached `.mz` mapping on disk, and opens the atlas region dialog. |
