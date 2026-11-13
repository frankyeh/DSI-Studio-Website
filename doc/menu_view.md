# [View] Menu

Use the **[View]** menu to snap the 3D viewport and **Draw Regions** slice viewer to standard anatomical planes, save or load 3D camera viewpoints as `.txt` files, and store up to four camera presets across sessions.

## Orthogonal views

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Sagittal View</span> | Shortcut: <kbd>Z</kbd>. Switches the **Draw Regions** viewer to single-slice sagittal view and centers the 3D camera on a lateral view at the current zoom. Repeat to flip 180° between left and right sides. |
| <span class="gui-menu-item">Coronal View</span> | Shortcut: <kbd>X</kbd>. Switches the **Draw Regions** viewer to single-slice coronal view and centers the 3D camera on a coronal view at the current zoom. Repeat to flip 180° between anterior and posterior sides. |
| <span class="gui-menu-item">Axial View</span> | Shortcut: <kbd>C</kbd>. Switches the **Draw Regions** viewer to single-slice axial view (the default startup plane) and centers the 3D camera on an axial view at the current zoom. Repeat to flip 180° between superior and inferior sides. |

## Camera files

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Open Camera Position...</span> | Loads a saved 3D camera viewpoint from a `.txt` file and immediately updates the primary 3D viewport's rotation, zoom, and pan, overwriting the current view. Incomplete or non-numeric files leave the view unchanged. |
| <span class="gui-menu-item">Save Camera Position...</span> | Saves the primary 3D viewport's current rotation, zoom, and pan to a `.txt` camera file for reproducible figure framing or sharing across sessions. |

Camera `.txt` files store only the primary 3D viewpoint—use **[File][Save Workspace...]** in the [File menu](/doc/menu_file.html) to save slices, tracts, regions, devices, and rendering settings together. Saved viewpoints and presets align identically across datasets only when they share the same voxel grid and spacing; press <kbd>Z</kbd>, <kbd>X</kbd>, or <kbd>C</kbd> to re-center if a volume appears shifted.

## Camera presets

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Store Camera Position</span> | Opens the submenu to save the current primary 3D camera viewpoint into one of four persistent local preset slots (**Slot 1**–**Slot 4**, <kbd><kbd>Alt</kbd>+<kbd>1</kbd></kbd>–<kbd><kbd>Alt</kbd>+<kbd>4</kbd></kbd>). |
| <span class="gui-menu-item">Recall Camera Position</span> | Opens the submenu to restore a saved 3D camera viewpoint from one of four persistent local preset slots (**Slot 1**–**Slot 4**, <kbd>1</kbd>–<kbd>4</kbd>). |

### [Store Camera Position] submenu

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Slot 1</span> | Shortcut: <kbd><kbd>Alt</kbd>+<kbd>1</kbd></kbd>. Saves the current primary 3D camera rotation, zoom, and pan to local preset slot 1 across sessions, overwriting any previous preset in that slot with a confirmation dialog. |
| <span class="gui-menu-item">Slot 2</span> | Shortcut: <kbd><kbd>Alt</kbd>+<kbd>2</kbd></kbd>. Saves the current primary 3D camera rotation, zoom, and pan to local preset slot 2 across sessions, overwriting any previous preset in that slot with a confirmation dialog. |
| <span class="gui-menu-item">Slot 3</span> | Shortcut: <kbd><kbd>Alt</kbd>+<kbd>3</kbd></kbd>. Saves the current primary 3D camera rotation, zoom, and pan to local preset slot 3 across sessions, overwriting any previous preset in that slot with a confirmation dialog. |
| <span class="gui-menu-item">Slot 4</span> | Shortcut: <kbd><kbd>Alt</kbd>+<kbd>4</kbd></kbd>. Saves the current primary 3D camera rotation, zoom, and pan to local preset slot 4 across sessions, overwriting any previous preset in that slot with a confirmation dialog. |

### [Recall Camera Position] submenu

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Slot 1</span> | Shortcut: <kbd>1</kbd>. Restores the 3D camera rotation, zoom, and pan saved in local preset slot 1 to the primary 3D viewport, or leaves the view unchanged if slot 1 is empty. |
| <span class="gui-menu-item">Slot 2</span> | Shortcut: <kbd>2</kbd>. Restores the 3D camera rotation, zoom, and pan saved in local preset slot 2 to the primary 3D viewport, or leaves the view unchanged if slot 2 is empty. |
| <span class="gui-menu-item">Slot 3</span> | Shortcut: <kbd>3</kbd>. Restores the 3D camera rotation, zoom, and pan saved in local preset slot 3 to the primary 3D viewport, or leaves the view unchanged if slot 3 is empty. |
| <span class="gui-menu-item">Slot 4</span> | Shortcut: <kbd>4</kbd>. Restores the 3D camera rotation, zoom, and pan saved in local preset slot 4 to the primary 3D viewport, or leaves the view unchanged if slot 4 is empty. |

Presets persist across sessions on the current computer; click the 3D viewport before pressing <kbd>1</kbd>–<kbd>4</kbd> if focus is inside an input field. See the [Screen menu](/doc/menu_screen.html) for multi-angle captures and the [Layout menu](/doc/menu_layout.html) for split 3D views.
