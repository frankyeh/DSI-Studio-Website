# [View] Menu

Use the **[View]** menu in the Fiber Tracking window to align the 3D OpenGL viewport and the **Draw Regions** 2D slice viewer to standard orthogonal anatomical planes (`Z`, `X`, `C`), save or reload exact 3D camera viewpoints as plain-text matrix files, and store or recall up to four persistent camera preset slots (`Alt+1`–`Alt+4` and `1`–`4`) across sessions.

## Orthogonal views

| Menu item | Behavior |
|---|---|
| **Sagittal View** | Shortcut: `Z`. Switches the **Draw Regions** 2D slice viewer to single-slice sagittal orientation (`X` axis, returning from 3-slice or mosaic layout if active), updates the slice-position slider to the sagittal dimension, re-centers the 3D scene on the volume midpoint while preserving the current 3D zoom level, and aligns the 3D camera to an orthogonal lateral view. Selecting **Sagittal View** or pressing `Z` again flips the 3D camera 180° to alternate between left-lateral and right-lateral viewpoints. |
| **Coronal View** | Shortcut: `X`. Switches the **Draw Regions** 2D slice viewer to single-slice coronal orientation (`Y` axis, returning from 3-slice or mosaic layout if active), updates the slice-position slider to the coronal dimension, re-centers the 3D scene on the volume midpoint while preserving the current 3D zoom level, and aligns the 3D camera to an orthogonal coronal view. Selecting **Coronal View** or pressing `X` again flips the 3D camera 180° to alternate between anterior and posterior viewpoints. |
| **Axial View** | Shortcut: `C`. Switches the **Draw Regions** 2D slice viewer to single-slice axial orientation (`Z` axis, the default startup plane when opening a FIB file), updates the slice-position slider to the axial dimension, re-centers the 3D scene on the volume midpoint while preserving the current 3D zoom level, and aligns the 3D camera to an orthogonal axial view. Selecting **Axial View** or pressing `C` again flips the 3D camera 180° to alternate between superior (top-down) and inferior (bottom-up) viewpoints. |

The three orientation buttons at the bottom-right of the **Draw Regions** dock (**Switch to sagittal view**, **Switch to coronal view**, and **Switch to axial view**) switch to the corresponding plane when clicked; once a plane is already active, use **[View][Sagittal View]** (`Z`), **[View][Coronal View]** (`X`), or **[View][Axial View]** (`C`) to re-center a panned 3D camera or flip 180° to the opposite side. Note that DSI Studio toggles a single shared 180° flip state across all three orthogonal resets, so switching to a different plane uses whichever side is queued next, and pressing the same key a second time always shows the opposite side of that axis.

During interactive 3D inspection between orthogonal resets:

- **Rotate:** Left-drag inside the central region of the 3D viewport to pitch and yaw the scene, or left-drag near the outer border to roll the camera in-plane.
- **Zoom:** Scroll the mouse wheel, right-drag vertically/diagonally in the 3D viewport, or adjust the **Zoom** spinbox (`0.01`–`100.00`) in the 3D toolbar.
- **Pan:** Middle-drag, hold `Shift` while dragging, or hold both left and right mouse buttons together to translate the scene in the screen plane (hold `Ctrl` while panning for `16×` finer movement). When **[Layout][3D Views][Double]** is active, choosing an orthogonal view re-synchronizes the right viewport camera to match the left.

## Camera files

| Menu item | Behavior |
|---|---|
| **Open Camera Position...** | Opens a file dialog (`Text files (*.txt)` or `All files (*)`, defaulting to `<fib_stem>_camera.txt`), reads whitespace-separated floating-point numbers from the selected file, copies the first 16 values into the primary 3D OpenGL `4 × 4` camera transformation matrix, and immediately updates the 3D viewport's rotation, zoom scale, and pan offset. Requires at least 16 numeric values in the file; files containing fewer than 16 numbers are ignored without changing the current view. |
| **Save Camera Position...** | Opens a save dialog (`Text files (*.txt)` or `All files (*)`, defaulting to `<fib_stem>_camera.txt`) and writes the current primary 3D OpenGL `4 × 4` camera transformation matrix as 16 space-separated floating-point values to a plain-text file, recording the exact 3D viewing direction, in-plane roll, zoom scale, and pan translation for reproducible figure framing or sharing across sessions. |

Keep two scope details in mind when working with camera `.txt` files:

- **3D camera pose only:** **[View][Open Camera Position...]** and **[View][Save Camera Position...]** read and write only the primary 3D transformation matrix (and in **[Layout][3D Views][Double]** split mode affect only the left viewport camera; the toolbar **Zoom** spinbox refreshes on the next interactive zoom step). Camera files do not change 2D slice positions, slice visibility checkboxes, rendering parameters, or loaded tracts, regions, and devices. To archive and reload an entire viewing state—including `camera.txt`, `setting.ini`, slice positions, 3D zoom, tracts, regions, and devices—use **[File][Save Workspace...]** and **[File][Load Workspace...]** in the [File menu](/doc/menu_file.html).
- **Grid and voxel-spacing dependence:** Because the 16-number matrix incorporates volume-center offsets (`dim / 2 - 0.5` in voxels) and anisotropic voxel-size scaling, applying the same camera file across multiple subjects reproduces identical framing when those datasets share the same matrix dimensions and voxel spacing (for example, subjects reconstructed on the same QSDR template grid). Loading a camera file created on a volume with different grid dimensions or voxel resolution can shift the brain off-center; press `Z`, `X`, or `C` to re-center on the current volume.

## Camera presets

| Menu item | Behavior |
|---|---|
| **Store Camera Position** | Opens the submenu for saving the current primary 3D camera matrix into one of four persistent local preset slots (**Slot 1**–**Slot 4**, shortcuts `Alt+1`–`Alt+4`). |
| **Recall Camera Position** | Opens the submenu for restoring a stored 3D camera matrix from one of the four persistent local preset slots (**Slot 1**–**Slot 4**, shortcuts `1`–`4`) without opening a file dialog. |

### [Store Camera Position] submenu

| Menu item | Behavior |
|---|---|
| **Slot 1** | Shortcut: `Alt+1`. Saves the current 16-value primary 3D camera transformation matrix (rotation, zoom scale, and pan offset) into persistent local application settings under slot 1, overwriting any previously stored preset in slot 1, and displays a confirmation dialog (`camera location stored at slot 1`). |
| **Slot 2** | Shortcut: `Alt+2`. Saves the current 16-value primary 3D camera transformation matrix into persistent local application settings under slot 2, overwriting any previous preset in slot 2, and displays a confirmation dialog (`camera location stored at slot 2`). |
| **Slot 3** | Shortcut: `Alt+3`. Saves the current 16-value primary 3D camera transformation matrix into persistent local application settings under slot 3, overwriting any previous preset in slot 3, and displays a confirmation dialog (`camera location stored at slot 3`). |
| **Slot 4** | Shortcut: `Alt+4`. Saves the current 16-value primary 3D camera transformation matrix into persistent local application settings under slot 4, overwriting any previous preset in slot 4, and displays a confirmation dialog (`camera location stored at slot 4`). |

### [Recall Camera Position] submenu

| Menu item | Behavior |
|---|---|
| **Slot 1** | Shortcut: `1`. Loads the 16-value 3D camera transformation matrix stored in slot 1 from local application settings and immediately updates the primary 3D viewport's rotation, zoom, and pan. Leaves the view unchanged if no camera has been stored in slot 1. |
| **Slot 2** | Shortcut: `2`. Loads the 16-value 3D camera transformation matrix stored in slot 2 from local application settings and immediately updates the primary 3D viewport. Leaves the view unchanged if slot 2 is empty. |
| **Slot 3** | Shortcut: `3`. Loads the 16-value 3D camera transformation matrix stored in slot 3 from local application settings and immediately updates the primary 3D viewport. Leaves the view unchanged if slot 3 is empty. |
| **Slot 4** | Shortcut: `4`. Loads the 16-value 3D camera transformation matrix stored in slot 4 from local application settings and immediately updates the primary 3D viewport. Leaves the view unchanged if slot 4 is empty. |

Preset slots `1`–`4` are saved in DSI Studio's user-level application preferences on your computer, so stored viewpoints persist across multiple Fiber Tracking windows and across application restarts on the same machine (use **[View][Save Camera Position...]** when archiving a camera alongside study files or transferring it to another machine). As with camera `.txt` files, recalling a slot across different datasets aligns identically when their voxel grids match. Because **[View][Recall Camera Position]** uses single-digit shortcuts `1`–`4`, click the 3D viewport or slice canvas first if keyboard focus is inside a numeric spinbox or text field. For multi-angle composite captures without manually switching cameras, see the [Screen menu](/doc/menu_screen.html); for viewport split modes and dock arrangements, see the [Layout menu](/doc/menu_layout.html).
