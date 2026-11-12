# [Tract Edit] Menu

<iframe width="560" height="315" src="https://www.youtube.com/embed/1xfhaFQhCtY" title="Interactive tract editing tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The **Tract Edit** menu provides interactive tract-selection and editing tools in the Step T3 tracking window.

## Select

Use **[Tract Edit][Select]** or `Ctrl+S`, then drag across the 3D view to define a selection plane. Tracks crossing the plane are selected. The right mouse button can additionally constrain the selection by the incoming angle.

## Delete

Use **[Tract Edit][Delete]** or `Ctrl+D` to delete tracks selected by the editing gesture. Selection/deletion actions can be undone with `Ctrl+Z` and redone with `Ctrl+Y`.

## Cut

Use **[Tract Edit][Cut]** or `Ctrl+X` to cut tracks at the selection plane. This is useful when only one portion of a bundle should be retained or analyzed.

## Paint

Use **[Tract Edit][Paint]** or `Ctrl+P` to assign the selected tracks a color.

## Cut by Slice

**[Tract Edit][Cut by Slice]** cuts tracks according to the current slice position.

## Shortcuts

| Shortcut | Action |
|:--|:--|
| `Ctrl+S` | Select tracts in the 3D window. |
| `Ctrl+D` | Delete tracts in the 3D window. |
| `Ctrl+P` | **Paint tracts** in the 3D window. |
| `Ctrl+X` | Cut tracts in the 3D window. |
| `Ctrl+T` | Trim tracts using topology-informed pruning. |
| `Ctrl+Z` | Undo selection/deletion. |
| `Ctrl+Y` | Redo selection/deletion. |

## Command History

The interactive **Select**, **Delete**, **Cut**, and **Paint** 3D gestures do not each generate a correspondingly named Command History command. In particular, `select_tract`, `cut_tract`, `paint_tract`, `cut_by_slice`, and `trim_tracts` are not valid commands. The **[Tracts][Delete]** operation uses `delete_tract`, which should not be confused with the **[Tract Edit]** mouse gesture.

Recorded tract-editing commands include `trim_tract`, `undo_tract`, `redo_tract`, `cut_tract_by_x`, `cut_tract_by_x2`, `cut_tract_by_y`, `cut_tract_by_y2`, `cut_tract_by_z`, `cut_tract_by_z2`, `cut_tract_end_portion`, `cut_tract_lps_end`, and `cut_tract_rai_end`. See [Command History](/doc/cli_vis.html) for automation guidance.
