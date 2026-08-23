# [Edit] Menu

<iframe width="560" height="315" src="https://www.youtube.com/embed/1xfhaFQhCtY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The **Edit** menu provides interactive tract-selection and editing tools in the Step T3 tracking window.

## Select

Use **[Edit][Select]** or `Ctrl+S`, then drag across the 3D view to define a selection plane. Tracks crossing the plane are selected. The right mouse button can additionally constrain the selection by the incoming angle.

## Delete

Use **[Edit][Delete]** or `Ctrl+D` to delete tracks selected by the editing gesture. Selection/deletion actions can be undone with `Ctrl+Z` and redone with `Ctrl+Y`.

## Cut

Use **[Edit][Cut]** or `Ctrl+X` to cut tracks at the selection plane. This is useful when only one portion of a bundle should be retained or analyzed.

## Paint

Use **[Edit][Paint]** or `Ctrl+P` to assign the selected tracks a color.

## Cut by Slice

**[Edit][Cut by Slice]** cuts tracks according to the current slice position.

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

## Command History names

| Action | Command |
|:--|:--|
| Select | `select_tract` |
| Delete | `delete_tract` |
| Cut | `cut_tract` |
| Paint | `paint_tract` |
| Cut by Slice | `cut_by_slice` |
| Trim Tracts | `trim_tracts` |
