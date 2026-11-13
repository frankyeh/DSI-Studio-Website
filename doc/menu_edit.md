# [Tract Edit] Menu

Prune, select, delete, recolor, and cut streamlines in the Step T3 tracking window.

<iframe width="560" height="315" src="https://www.youtube.com/embed/1xfhaFQhCtY" title="Interactive tract editing tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Undo, Redo, and Trim

| Menu item | Shortcut / GUI command hint | Target & behavior |
|:--|:--|:--|
| Undo | Shortcut: `Ctrl+Z` · `run undo_tract` | Restores streamlines removed by the latest selection, deletion, trim, or cut across all **checked** bundles (and removes cut sub-segments); does not revert **Paint** color changes. |
| Redo | Shortcut: `Ctrl+Y` · `run redo_tract` | Re-applies the most recently undone streamline removal in **checked** bundles. Cleared as soon as a new deletion or cut occurs. |
| Trim | Shortcut: `Ctrl+T` · `run trim_tract` | Applies one Topology-Informed Pruning (TIP) iteration to the **currently selected bundle row** (checked or unchecked), deleting streamlines that pass through 2×2×2 voxel neighborhoods occupied by 3 or fewer streamlines. Intended for populated coherent bundles rather than whole-brain tractography. |

## Select

Choose **[Tract Edit][Select]** (`Ctrl+S`), then drag across the 3D view to isolate streamlines in all **checked** bundles: streamlines crossing the selection stroke are retained, and all non-crossing streamlines are moved to the bundle's deleted buffer (`Ctrl+Z` to undo).

All four 3D gesture tools (**Select**, **Delete**, **Paint**, and **Cut**) switch the 3D cursor to a one-shot crosshair and share the same stroke modifiers:

| 3D mouse gesture | Behavior |
|:--|:--|
| **Left-drag** | Defines a straight viewing-ray cutting plane between press and release, matching crossing streamlines at any angle. |
| **`Shift` + drag** | Draws a multi-segment freehand polyline across the 3D view (sampled every ≥ 20 pixels). |
| **Right-drag** | Constrains the stroke by **Tract Selection Angle** (`tract_sel_angle` under **Tract** rendering options, `0`–`90`°, initial `45`°), matching only streamlines whose local tangent satisfies `|tangent · plane_normal| ≥ cos(tract_sel_angle)`. |

## Delete

Choose **[Tract Edit][Delete]** (`Ctrl+D`), then drag across the 3D view (`Left-drag`, `Shift` + drag, or angle-constrained `Right-drag`) to remove crossing streamlines from all **checked** bundles while keeping all remaining streamlines (`Ctrl+Z` to undo). Unlike **[Tracts][Delete]** (`delete_tract`), which removes entire bundle rows from the tract table, **[Tract Edit][Delete]** deletes individual streamlines inside checked bundles.

## Paint

Choose **[Tract Edit][Paint]** (`Ctrl+P`), drag across the 3D view (`Left-drag`, `Shift` + drag, or `Right-drag`), and pick an RGBA color in the color dialog. Crossing streamlines in all **checked** bundles receive the chosen color, and **Tract Color → Style** (`tract_color_style`) switches automatically to `Assigned` (manual per-streamline coloring). Because **Paint** updates streamline colors in place without removing streamlines, it is not reverted by **Undo**.

## Cut

The cutting actions split streamlines either interactively in 3D or deterministically along the bundle's dominant LPS/RAI orientation (`+X` = Left, `+Y` = Posterior, `+Z` = Superior in DSI Studio's internal LPS voxel space). All four operations can be undone with **[Tract Edit][Undo]** (`Ctrl+Z`).

| Menu item | Shortcut / GUI command hint | Target & behavior |
|:--|:--|:--|
| Cut | Shortcut: `Ctrl+X` · — | Interactive 3D gesture (`Left-drag`, `Shift` + drag, or `Right-drag`) that splits each crossing streamline (> 2 points) in **checked** bundles at the crossing point into two segments within the same bundle, so a subsequent **Select** or **Delete** stroke can isolate one side. |
| Cut End Portion | `run cut_tract_end_portion` | Aligns streamlines along the bundle's dominant axis and trims 25% off **both** ends (`0.25`–`0.75` central trunk retained) on the **selected bundle row** (must be checked; skips ≤ 2-point fragments). |
| Cut L/P/S End Portion | `run cut_tract_lps_end` | Removes the 25% terminal portion on the **Left / Posterior / Superior (LPS)** side (`0.25`–`1.0` retained) on the **selected bundle row** (must be checked). |
| Cut R/A/I End Portion | `run cut_tract_rai_end` | Removes the 25% terminal portion on the **Right / Anterior / Inferior (RAI)** side (`0.0`–`0.75` retained) on the **selected bundle row** (must be checked). |

## Cut by Slice

**[Tract Edit][Cut By Slice]** cuts streamlines in all **checked** bundles at the active slice's current `X`, `Y`, or `Z` slider position (`slice_pos`), discarding points on the chosen LPS side and retaining remaining sub-segments with at least 2 points (`Ctrl+Z` to undo). When a registered non-diffusion slice (such as T1w) is active, streamline coordinates are transformed into that slice's voxel space before cutting.

| Submenu item | GUI command hint | Points removed | Points retained |
|:--|:--|:--|:--|
| Cut X+ | `run cut_tract_by_x` | `X ≥ slice_pos` (Left / `+X`) | `X < slice_pos` (Right / `-X`) |
| Cut X- | `run cut_tract_by_x2` | `X < slice_pos` (Right / `-X`) | `X ≥ slice_pos` (Left / `+X`) |
| Cut Y+ | `run cut_tract_by_y` | `Y ≥ slice_pos` (Posterior / `+Y`) | `Y < slice_pos` (Anterior / `-Y`) |
| Cut Y- | `run cut_tract_by_y2` | `Y < slice_pos` (Anterior / `-Y`) | `Y ≥ slice_pos` (Posterior / `+Y`) |
| Cut Z+ | `run cut_tract_by_z` | `Z ≥ slice_pos` (Superior / `+Z`) | `Z < slice_pos` (Inferior / `-Z`) |
| Cut Z- | `run cut_tract_by_z2` | `Z < slice_pos` (Inferior / `-Z`) | `Z ≥ slice_pos` (Superior / `+Z`) |

## Shortcuts

| Shortcut | Menu path | Action |
|:--|:--|:--|
| `Ctrl+Z` | **[Tract Edit][Undo]** | Undo the latest streamline selection, deletion, trim, or cut in checked bundles. |
| `Ctrl+Y` | **[Tract Edit][Redo]** | Redo the most recently undone streamline removal in checked bundles. |
| `Ctrl+T` | **[Tract Edit][Trim]** | Run one TIP pruning iteration on the selected bundle row. |
| `Ctrl+S` | **[Tract Edit][Select]** | Arm 3D stroke to keep crossing streamlines in checked bundles. |
| `Ctrl+D` | **[Tract Edit][Delete]** | Arm 3D stroke to delete crossing streamlines in checked bundles. |
| `Ctrl+P` | **[Tract Edit][Paint]** | Arm 3D stroke to assign an RGBA color to crossing streamlines. |
| `Ctrl+X` | **[Tract Edit][Cut]** | Arm 3D stroke to split crossing streamlines at the selection plane. |

## Command History

DSI Studio separates interactive menu gestures, UI command hints, and CLI / Command History execution into distinct layers:

1. **Interactive 3D gestures have no command-history token:** **[Tract Edit][Select]**, **[Tract Edit][Delete]**, **[Tract Edit][Paint]**, and **[Tract Edit][Cut]** require live 3D mouse strokes and do not record a Command History entry. Names such as `select_tract`, `cut_tract`, `paint_tract`, `cut_by_slice`, and `trim_tracts` are **not** valid commands, and `delete_tract` belongs to **[Tracts][Delete]** (deleting whole bundle rows rather than 3D-selected streamlines).
2. **`run ...` strings are GUI/agent command hints:** The `run ...` tooltips defined in `tracking_window.ui` (and used by DSI Studio AI window dispatch) are internal tracking-window command hints, **not** standalone `dsi_studio --action=...` CLI flags.
3. **Verified `--action=vis --cmd=...` commands:** Deterministic **[Tract Edit]** actions record to [Command History](/doc/cli_vis.html) and can be replayed via CSV or executed with `dsi_studio --action=vis --source=<fib.fz> --cmd="..."` (`+` separates chained steps, `,` separates arguments, and `&` joins multiple 0-based bundle indices):

| `--action=vis` `--cmd` syntax | Default target when index omitted | Operation |
|:--|:--|:--|
| `undo_tract[,<indices>]` | All checked bundles | Undo latest streamline deletion/cut |
| `redo_tract[,<indices>]` | All checked bundles | Redo latest undone streamline removal |
| `trim_tract[,<indices>]` | Selected bundle row (checked or unchecked) | One TIP pruning iteration |
| `cut_tract_end_portion[,<index>]` | Selected bundle row (must be checked) | Retain central `0.25`–`0.75` trunk |
| `cut_tract_lps_end[,<index>]` | Selected bundle row (must be checked) | Remove LPS `0.0`–`0.25` end; retain `0.25`–`1.0` |
| `cut_tract_rai_end[,<index>]` | Selected bundle row (must be checked) | Remove RAI `0.75`–`1.0` end; retain `0.0`–`0.75` |
| `cut_tract_by_x[,<slice_pos>[,<indices>]]` | Active slice `X` pos; all checked bundles | Cut `X ≥ slice_pos`; retain `X < slice_pos` |
| `cut_tract_by_x2[,<slice_pos>[,<indices>]]` | Active slice `X` pos; all checked bundles | Cut `X < slice_pos`; retain `X ≥ slice_pos` |
| `cut_tract_by_y[,<slice_pos>[,<indices>]]` | Active slice `Y` pos; all checked bundles | Cut `Y ≥ slice_pos`; retain `Y < slice_pos` |
| `cut_tract_by_y2[,<slice_pos>[,<indices>]]` | Active slice `Y` pos; all checked bundles | Cut `Y < slice_pos`; retain `Y ≥ slice_pos` |
| `cut_tract_by_z[,<slice_pos>[,<indices>]]` | Active slice `Z` pos; all checked bundles | Cut `Z ≥ slice_pos`; retain `Z < slice_pos` |
| `cut_tract_by_z2[,<slice_pos>[,<indices>]]` | Active slice `Z` pos; all checked bundles | Cut `Z < slice_pos`; retain `Z ≥ slice_pos` |

Example batch trim and inferior-end slice cut on bundle `0`:

```bash
dsi_studio --action=vis --source=sub01.fz --cmd="open_tract,cst.tt.gz+trim_tract,0+cut_tract_by_z2,30,0+save_tract,cst_edited.tt.gz,0"
```

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
