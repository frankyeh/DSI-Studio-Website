# [Tract Edit] Menu

The **[Tract Edit]** menu provides interactive 3D stroke tools, automated pruning, and geometric cropping to refine fiber bundles in the Fiber Tracking window. Removed or split streamlines are stored in a per-bundle deleted buffer so edits can be undone or separated into a new bundle, while file management and region-based filtering live in the [Tracts](/doc/menu_tracts.html), [Tracts Misc](/doc/menu_tracts_misc.html), and [Tracts Cluster](/doc/menu_tracts_cluster.html) menus.

<iframe width="560" height="315" src="https://www.youtube.com/embed/1xfhaFQhCtY" title="Interactive tract editing tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Undo, Redo, and Trim

| Menu item | Behavior |
|---|---|
| **Undo** | Shortcut: `Ctrl+Z`. Restores the most recently removed or split streamlines across all **checked** bundles from their deleted buffers. Unchecked rows are skipped, and **Undo** does not revert **Paint** colors, whole-bundle deletion (**Tracts › Delete**), or clustering. |
| **Redo** | Shortcut: `Ctrl+Y`. Re-applies the most recently undone streamline removal across all **checked** bundles. Performing any new edit clears the redo history, and redoing an undone cut moves the uncut streamlines to the deleted buffer without re-creating split segments. |
| **Trim** | Shortcut: `Ctrl+T`. Applies one pass of Topology-Informed Pruning (TIP) to the **highlighted** bundle row (checked or unchecked), moving low-density stray streamlines to the deleted buffer. Repeat as needed on dense single pathways; avoid trimming sparse bundles or whole-brain tractograms. |

### Trim vs. Filter by ROI

Use **[Tracts][Filter Tracks by ROI/ROA/End]** first to isolate or exclude pathways across all **checked** bundles using anatomical regions ([ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html)), regardless of fiber density. Then apply **[Tract Edit][Trim]** (`Ctrl+T`) to the **highlighted** bundle to prune residual low-density fringe branches from an already isolated, well-populated pathway.

## Select

| Menu item | Behavior |
|---|---|
| **Select** | Shortcut: `Ctrl+S`. Arms a one-shot 3D crosshair stroke that keeps intersected streamlines across all **checked** bundles and moves non-intersected streamlines to the deleted buffer. Note that `Ctrl+S` selects fibers rather than saving files; use **Tracts › Save Current Tract As...** to save. |

All four 3D stroke tools (**Select**, **Delete**, **Paint**, and **Cut**) use **Left-drag** for a straight line, **`Shift` + drag** for a freehand curve, and **Right-drag** to restrict the stroke to fibers crossing near perpendicular (adjusted by **Tract Rendering › Tract Selection Angle** in **Options**).

## Delete

| Menu item | Behavior |
|---|---|
| **Delete** | Shortcut: `Ctrl+D`. Arms a one-shot 3D crosshair stroke that moves intersected streamlines across all **checked** bundles to the deleted buffer while keeping the rest. Removed fibers can be restored with **Undo** (`Ctrl+Z`) or extracted into a new row via **Tracts › Separate Deleted Tracts** (`Ctrl+Alt+S`). |

## Paint

| Menu item | Behavior |
|---|---|
| **Paint** | Shortcut: `Ctrl+P`. Arms a one-shot 3D crosshair stroke and opens a color picker to recolor intersected streamlines across all **checked** bundles, switching **Tract Color › Style** to `Assigned`. Cannot be undone with **Undo** (`Ctrl+Z`); canceling the picker paints stroked fibers black, and **Tract Color › Opacity** controls transparency. |

## Cut

Cut operations split or crop streamlines and store the uncut originals in the deleted buffer (`Ctrl+Z` to undo).

| Menu item | Behavior |
|---|---|
| **Cut** | Shortcut: `Ctrl+X`. Arms a one-shot 3D crosshair stroke that splits intersected streamlines across all **checked** bundles into two separate segments at the crossing point. Follow with **Select** (`Ctrl+S`) or **Delete** (`Ctrl+D`) to isolate or remove a severed branch. |
| **Cut End Portion** | Crops the outer `25%` from both ends of each streamline along the bundle's main axis, keeping the central `50%` trunk. Acts only on the **highlighted** bundle row, which must also be **checked**. |
| **Cut L/P/S End Portion** | Crops the terminal `25%` on the **Left, Posterior, or Superior** end of each streamline along the bundle's main axis, keeping the remaining `75%`. Acts only on the **highlighted** bundle row, which must also be **checked**. |
| **Cut R/A/I End Portion** | Crops the terminal `25%` on the **Right, Anterior, or Inferior** end of each streamline along the bundle's main axis, keeping the remaining `75%`. Acts only on the **highlighted** bundle row, which must also be **checked**. |

## Cut by Slice

The **[Tract Edit][Cut By Slice]** submenu crops all **checked** bundles at the currently selected slice's crosshair position in the **Slices** panel (`Ctrl+Z` to undo), splitting curved fibers that cross the kept side more than once.

| Menu item | Behavior |
|---|---|
| **Cut X+** | Crops all **checked** bundles at the current sagittal (`X`) slice plane, removing the **Left (`+X`)** side and keeping the **Right (`-X`)** side. |
| **Cut X-** | Crops all **checked** bundles at the current sagittal (`X`) slice plane, removing the **Right (`-X`)** side and keeping the **Left (`+X`)** side. |
| **Cut Y+** | Crops all **checked** bundles at the current coronal (`Y`) slice plane, removing the **Posterior (`+Y`)** side and keeping the **Anterior (`-Y`)** side. |
| **Cut Y-** | Crops all **checked** bundles at the current coronal (`Y`) slice plane, removing the **Anterior (`-Y`)** side and keeping the **Posterior (`+Y`)** side. |
| **Cut Z+** | Crops all **checked** bundles at the current axial (`Z`) slice plane, removing the **Superior (`+Z`)** side and keeping the **Inferior (`-Z`)** side. |
| **Cut Z-** | Crops all **checked** bundles at the current axial (`Z`) slice plane, removing the **Inferior (`-Z`)** side and keeping the **Superior (`+Z`)** side. |

## Shortcuts

Streamline edits in **[Tract Edit]** use plain `Ctrl` shortcuts, whereas whole-bundle actions in [Tracts](/doc/menu_tracts.html) use `Ctrl+Alt` and region-mask operations in [Regions](/doc/menu_regions.html) and [Region Edit](/doc/menu_region_edit.html) use `Ctrl+Shift`. Deterministic edits (**Trim**, endpoint cuts, and **Cut By Slice**) are also logged in **[Records][Command History...]**; see the [Records menu](/doc/menu_records.html).
