# [Region Edit] Menu

The **[Region Edit]** menu refines binary region masks in place through 3D morphological cleanup, slice-intensity filtering, connected-component tools, and multi-region Boolean or parcellation operations. Single-region edits modify the highlighted row by default and can be reverted with **[Regions][Undo]** (`Ctrl+Shift+Z`), while the [Regions menu](/doc/menu_regions.html), [Region Move menu](/doc/menu_region_move.html), and [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html) cover file management, spatial shifts, and tracking roles.

## Checked regions

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Modify All Checked</span> | Checkable toggle (off by default) that applies single-region edits to all checked regions instead of only the highlighted row. **Separate** still splits only the first checked region, and **Region set operations** always process all checked regions. |

## Dilation and erosion

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Dilation</span> | Shortcut: `Ctrl+Shift+D`. Expands the region mask outward by `1` voxel in 3D, thickening thin tracking barriers or bridging single-voxel boundary gaps. |
| <span class="gui-menu-item">Dilation By Voxel...</span> | Expands the region mask outward in 3D by a chosen radius (`1–100` voxels, default `10`). Distance is measured in voxels rather than mm, so physical expansion scales with the region's voxel size. |
| <span class="gui-menu-item">Dilation By Threshold...</span> | Expands the region outward by `1` voxel only where boundary neighbors meet an intensity cutoff on the active slice (pre-filled with Otsu's threshold). Positive values grow into brighter voxels; negative values grow into darker voxels. |
| <span class="gui-menu-item">Erosioin By Threshold...</span> | Shrinks the region inward by `1` voxel only along borders that meet an intensity cutoff on the active slice (pre-filled with Otsu's threshold; positive for brighter voxels, negative for darker voxels). *(Spelled `Erosioin By Threshold...` in the menu.)* |
| <span class="gui-menu-item">Erosion</span> | Shortcut: `Ctrl+Shift+E`. Shrinks the region mask inward by `1` voxel in 3D. Thin `1–2` voxel sheets or small islands can vanish completely; use **Regions › Undo** if over-eroded. |

## Opening and closing

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Opening</span> | Shortcut: `Ctrl+Shift+O`. Removes thin exterior protrusions, narrow bridges, and stray boundary spurs while preserving the main mask shape without uniform shrinkage. |
| <span class="gui-menu-item">Closing</span> | Shortcut: `Ctrl+Shift+C`. Fills small internal holes, narrow gaps, and boundary clefts without uniformly expanding the outer mask surface. |

## Smoothing and mask structure

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Smoothing</span> | Shortcut: `Ctrl+Shift+S`. Smooths jagged, stair-stepped 3D boundaries on the region mask. Because `Ctrl+Shift+S` is shared with **Regions › Save Checked Regions to one 4D NIFTI...**, click this menu item directly if the shortcut conflicts. |
| <span class="gui-menu-item">Defragment</span> | Shortcut: `Ctrl+Shift+F`. Keeps only the single largest connected 3D piece of the region and deletes all smaller disconnected islands. Avoid using on bilateral or multi-part masks unless smaller pieces should be discarded. |
| <span class="gui-menu-item">Separate</span> | Shortcut: `Ctrl+Shift+P`. Splits disconnected 3D pieces into up to `256` new checked regions (`<Name>_1`, `<Name>_2`, ...) while leaving the original region intact. Even with **Modify All Checked** enabled, only the first checked region is split. |
| <span class="gui-menu-item">Negate</span> | Shortcut: `Ctrl+Shift+N`. Inverts the region mask across the full 3D volume, turning background voxels into foreground and vice versa. Intersect the result with a brain mask (via **Region Edit › All Intersect First**) to exclude outer background. |

## Threshold

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Threshold</span> | Shortcut: `Ctrl+Shift+T`. Filters existing voxels inside the region by active-slice intensity (pre-filled with Otsu's threshold; positive values keep brighter voxels, negative values keep darker voxels). Never adds exterior voxels; use **Regions Misc › New Region From Thresholding...** for whole-slice thresholding. |

## Region set operations

All five operations require at least two checked regions and process them in top-to-bottom table order on the topmost checked region's voxel grid (resampling any lower region on a different grid and clearing its undo history).

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">All Intersect First</span> | Shortcut: `Ctrl+Shift+1`. Replaces each lower checked region in place with its overlap against the topmost checked region, leaving the topmost region unchanged. Requires at least 2 checked regions. |
| <span class="gui-menu-item">All Exclude First</span> | Shortcut: `Ctrl+Shift+2`. Subtracts the topmost checked region from each lower checked region in place, leaving the topmost region unchanged. Requires at least 2 checked regions. |
| <span class="gui-menu-item">First Excludes All</span> | Shortcut: `Ctrl+Shift+3`. Subtracts all lower checked regions from the topmost checked region in place (for example, to carve out ventricles or lesions), leaving lower regions unchanged. Requires at least 2 checked regions. |
| <span class="gui-menu-item">All Fit into First</span> | Shortcut: `Ctrl+Shift+4`. Clips, expands, and smooths all lower checked regions into a gap-free, non-overlapping parcellation filling the topmost checked region (lower rows take precedence where masks overlap; topmost stays unchanged). Requires at least 2 checked regions. |
| <span class="gui-menu-item">Refine All</span> | Shortcut: `Ctrl+Shift+5`. Adjusts shared borders across all checked regions (including the topmost) to follow local tissue intensities on the active slice, producing non-overlapping masks. Requires at least 2 checked regions and an active slice. |
