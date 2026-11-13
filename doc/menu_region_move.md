# [Region Move] Menu

Use **[Region Move]** to mirror regions across voxel axes, nudge them in 1-voxel steps, or drag them in the 3D view. Axis flips and single-voxel shifts modify the highlighted row in **Assign Regions**, or all checked regions when **[Region Edit][Modify All Checked]** is enabled.

## Axis flips

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Flip X</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>7</kbd></kbd>. Mirrors the highlighted region (or all checked regions when **[Region Edit][Modify All Checked]** is enabled) left–right across the `X`-axis center of the voxel grid. Undoable via **[Regions][Undo]**. |
| <span class="gui-menu-item">Flip Y</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>8</kbd></kbd>. Mirrors the highlighted region (or all checked regions) anterior–posterior across the `Y`-axis center of the voxel grid. Undoable via **[Regions][Undo]**. |
| <span class="gui-menu-item">Flip Z</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>9</kbd></kbd>. Mirrors the highlighted region (or all checked regions) superior–inferior across the `Z`-axis center of the voxel grid. Undoable via **[Regions][Undo]**. |

## Axis shifts

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Shift +X</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>1</kbd></kbd>. Shifts the highlighted region (or all checked regions when **[Region Edit][Modify All Checked]** is enabled) by `1` voxel along `+X` (subject Left). Cannot be undone with **[Regions][Undo]**. |
| <span class="gui-menu-item">Shift -X</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>2</kbd></kbd>. Shifts the highlighted region (or all checked regions) by `1` voxel along `-X` (subject Right). Cannot be undone with **[Regions][Undo]**. |
| <span class="gui-menu-item">Shift +Y</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>3</kbd></kbd>. Shifts the highlighted region (or all checked regions) by `1` voxel along `+Y` (subject Posterior). Cannot be undone with **[Regions][Undo]**. |
| <span class="gui-menu-item">Shift -Y</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>4</kbd></kbd>. Shifts the highlighted region (or all checked regions) by `1` voxel along `-Y` (subject Anterior). Cannot be undone with **[Regions][Undo]**. |
| <span class="gui-menu-item">Shift +Z</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>5</kbd></kbd>. Shifts the highlighted region (or all checked regions) by `1` voxel along `+Z` (subject Superior). Cannot be undone with **[Regions][Undo]**. |
| <span class="gui-menu-item">Shift -Z</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>6</kbd></kbd>. Shifts the highlighted region (or all checked regions) by `1` voxel along `-Z` (subject Inferior). Cannot be undone with **[Regions][Undo]**. |

## Interactive placement

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Move Region at 3D</span> | Shortcut: <kbd><kbd>Ctrl</kbd>+<kbd>A</kbd></kbd>. Enables a one-shot crosshair drag to move any visible checked region in whole-voxel steps parallel to the camera-facing slice plane, then restores normal camera control. Requires 3D region display and cannot be undone with **[Regions][Undo]**. |

### Coordinate, undo, and 3D picking cautions

Flips mirror across the center of the voxel grid rather than the anatomical midline, and shifts step by `1` voxel in the region's own image grid (not `mm`) without clipping out-of-bounds voxels until saved. Before pressing <kbd><kbd>Ctrl</kbd>+<kbd>A</kbd></kbd>, rotate the view or hide foreground slices so closer slices or 3D objects are not dragged instead, or right-click any 3D slice and choose **Move Current Region Here** to recenter the highlighted region.
