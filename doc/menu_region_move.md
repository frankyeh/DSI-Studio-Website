# [Region Move] Menu

Use the **[Region Move]** menu in the Fiber Tracking window to mirror regions across primary voxel axes, nudge regions in single-voxel increments along `X`, `Y`, or `Z`, or drag regions interactively inside the 3D viewport. By default, axis flips and single-voxel shifts modify only the currently highlighted row in the **Assign Regions** dock; when **[Region Edit][Modify All Checked]** is enabled, those nine operations apply simultaneously to every checked region in the table.

## Axis flips

| Menu item | Behavior |
|---|---|
| **Flip X** | Shortcut: `Ctrl+7`. Mirrors the target region across the `X` axis midpoint of its 3D voxel grid (`x' = dim_x - 1 - x`, left–right sagittal mirror in DSI Studio's internal LPS orientation) and records an undo snapshot for **Regions › Undo**. When **Region Edit › Modify All Checked** is active, flips all checked regions; otherwise flips the currently selected region. |
| **Flip Y** | Shortcut: `Ctrl+8`. Mirrors the target region across the `Y` axis midpoint of its 3D voxel grid (`y' = dim_y - 1 - y`, anterior–posterior coronal mirror in standard LPS orientation) and records an undo snapshot for **Regions › Undo**. Respects **Region Edit › Modify All Checked**. |
| **Flip Z** | Shortcut: `Ctrl+9`. Mirrors the target region across the `Z` axis midpoint of its 3D voxel grid (`z' = dim_z - 1 - z`, superior–inferior axial mirror in standard LPS orientation) and records an undo snapshot for **Regions › Undo**. Respects **Region Edit › Modify All Checked**. |

## Axis shifts

| Menu item | Behavior |
|---|---|
| **Shift +X** | Shortcut: `Ctrl+1`. Translates every voxel of the target region by `+1` voxel along the `+X` axis of the region's own grid (toward subject Left in standard LPS diffusion space), updating both the 2D slice overlay and 3D surface mesh immediately. Applies to the selected region, or to all checked regions when **Region Edit › Modify All Checked** is enabled. |
| **Shift -X** | Shortcut: `Ctrl+2`. Translates every voxel of the target region by `-1` voxel along the `X` axis of the region's own grid (toward subject Right in standard LPS diffusion space), updating 2D and 3D views immediately. Respects **Region Edit › Modify All Checked**. |
| **Shift +Y** | Shortcut: `Ctrl+3`. Translates every voxel of the target region by `+1` voxel along the `+Y` axis of the region's own grid (toward subject Posterior in standard LPS diffusion space), updating 2D and 3D views immediately. Respects **Region Edit › Modify All Checked**. |
| **Shift -Y** | Shortcut: `Ctrl+4`. Translates every voxel of the target region by `-1` voxel along the `Y` axis of the region's own grid (toward subject Anterior in standard LPS diffusion space), updating 2D and 3D views immediately. Respects **Region Edit › Modify All Checked**. |
| **Shift +Z** | Shortcut: `Ctrl+5`. Translates every voxel of the target region by `+1` voxel along the `+Z` axis of the region's own grid (toward subject Superior in standard LPS diffusion space), updating 2D and 3D views immediately. Respects **Region Edit › Modify All Checked**. |
| **Shift -Z** | Shortcut: `Ctrl+6`. Translates every voxel of the target region by `-1` voxel along the `Z` axis of the region's own grid (toward subject Inferior in standard LPS diffusion space), updating 2D and 3D views immediately. Respects **Region Edit › Modify All Checked**. |

## Interactive placement

| Menu item | Behavior |
|---|---|
| **Move Region at 3D** | Shortcut: `Ctrl+A`. Switches the 3D viewport cursor to a crosshair for a single click-and-drag move operation, then automatically restores normal camera navigation when the mouse button is released. Clicking and dragging a visible checked region highlights its row in **Assign Regions** and translates it in whole-voxel steps parallel to whichever orthogonal slice plane (sagittal, coronal, or axial) most directly faces the camera. Requires region rendering to be enabled in the 3D view and the target region's checkbox to be checked. |

### Coordinate, undo, and 3D picking cautions

- **Voxel units vs. physical millimeters:** All six shift commands (`Ctrl+1` through `Ctrl+6`) step by exactly `1` voxel in the region's stored grid rather than `1` mm. If a region was created or loaded on a high-resolution structural slice inserted via **[Slices][Insert Other Images...]**, each step equals one structural-grid voxel while DSI Studio rotates and scales the 3D surface displacement into diffusion space automatically.
- **Grid-center mirroring vs. anatomical midline:** **[Region Move][Flip X]** mirrors voxel indices across the center of the image matrix (`dim_x - 1 - x`), not an anatomical interhemispheric plane. On native-space scans where the head is yawed, tilted, or off-center in the field of view, inspect the mirrored region across orthogonal slices and refine its position with `Ctrl+1`–`Ctrl+6` or **[Region Move][Move Region at 3D]** (`Ctrl+A`).
- **Undo and grid-boundary behavior:** **[Region Move][Flip X]**, **[Region Move][Flip Y]**, and **[Region Move][Flip Z]** save undo history for **[Regions][Undo]** (`Ctrl+Shift+Z`), whereas single-voxel shifts and 3D drags shift coordinates directly without pushing undo snapshots—use the opposite shift shortcut (for example, `Ctrl+2` after `Ctrl+1`) to step back. Voxels shifted past `[0, dim - 1]` remain in memory during back-and-forth nudging so temporary boundary crossings do not clip the shape, and are clipped to grid bounds only when converted to a volume mask or saved via **[Regions][Save Current Region As...]**.
- **3D slice occlusion and shared 3D object dragging:** When **[Region Move][Move Region at 3D]** (`Ctrl+A`) is active, DSI Studio ray-casts the 3D scene in depth order. If a visible 3D orthogonal slice (`Sag`, `Cor`, or `Axi`) sits closer to the camera than the region at the click location, dragging moves that slice plane along its normal instead of moving the region; rotate the camera so the region lies unobstructed in front of the slices (or temporarily uncheck the occluding slice plane) before pressing `Ctrl+A`. Because `Ctrl+A` is the shared 3D object-move tool, clicking a visible 3D color bar, a checked stereotactic trajectory from **[Devices]**, or an inserted 2D picture slice from **[Slices]** moves or scales that object instead.
- **Direct 3D slice jump:** To recenter the currently selected region at a specific anatomical landmark without dragging, right-click that point on any visible slice in the 3D viewport and choose **Move Current Region Here**, which shifts the region so its center of mass lands on the clicked voxel.
