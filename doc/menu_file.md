# [File] Menu

The **[File]** menu manages the loaded FIB dataset, saves or restores multi-object workspaces, and imports, exports, or resets tracking and 3D rendering settings. To open a new `.fz` or `.fib.gz` file, start from the main window ([Track: whole-brain tractography](/doc/gui_t3_whole_brain.html)); use [Tracts](/doc/menu_tracts.html), [Regions](/doc/menu_regions.html), and [Slices](/doc/menu_slices.html) for individual overlays.

## FIB files

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Open FIB Directory...</span> | Shortcut: `Ctrl+O`. Opens the folder containing the loaded `.fz` or `.fib.gz` file in your system file manager rather than opening a new FIB file dialog. |
| <span class="gui-menu-item">Save FIB As...</span> | Saves the loaded fiber orientations, metrics, template mapping, and report metadata to a compact `.fz` file (converting `.fib.gz` if needed). Tracked bundles, regions, external slices, and view settings are excluded; use **File › Save Workspace...** to save session overlays. |

## Workspace

A workspace folder bundles checked tracts, checked regions, devices, the active custom slice, camera view, and Options parameters for reopening on the same FIB dataset.

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Load Workspace...</span> | Restores saved tracts (`.tt.gz`), regions (`.nii.gz`), devices (`.dv.csv`), external slices, Options settings (`setting.ini`), slice positions, and camera view from a workspace folder onto the open FIB dataset. Existing tracts, regions, or devices are replaced when matching subfolders are present. |
| <span class="gui-menu-item">Save Workspace...</span> | Exports **checked** tracts, **checked** regions, devices, the active external slice, camera view, slice positions, and all Options settings into a workspace folder. Unchecked tracts/regions and inactive external slices are omitted; keep default `.tt.gz` and `.nii.gz` output formats so **File › Load Workspace...** can reload them. |

## Tracking settings

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Load Tracking Settings...</span> | Loads tracking parameters from an `.ini` file into **Tracking Parameters**, **Differential Tracking**, and **Advanced Options** while leaving rendering settings, camera view, regions, and existing tracts unchanged. |
| <span class="gui-menu-item">Save Tracking Settings...</span> | Saves all current **Tracking Parameters**, **Differential Tracking**, and **Advanced Options** values (thresholds, `mm` step/length/tolerance limits, seed counts, pruning iterations, and algorithm options) to an `.ini` file for reuse across subjects. |
| <span class="gui-menu-item">Load Parameter ID...</span> | Prompts for a `parameter_id` code from DSI Studio's methods report to apply its encoded tracking thresholds (`degrees` and `mm`), seed/tract limits, algorithm, and pruning settings. Tracking index, AutoTrack tolerance, and output format are not encoded; paste the exact report string because malformed IDs are not validated. |
| <span class="gui-menu-item">Restore Tracking Settings</span> | Resets **Tracking Parameters**, **Differential Tracking**, and **Advanced Options** to dataset-scaled defaults (including `mm` length and tolerance limits for the active species and voxel size) without changing rendering settings. |

## Rendering settings

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Load Rendering Settings...</span> | Loads display parameters from an `.ini` file into the Options rendering groups while keeping tracking parameters and camera angle unchanged. Layer visibility checkboxes and nested **Tract Color**, **Region Color**, and **Graph Visualization** settings require **File › Load Workspace...**. |
| <span class="gui-menu-item">Save Rendering Settings</span> | Opens a Save dialog to export direct Options display parameters to an `.ini` file. Layer visibility checkboxes, camera view, and nested **Tract Color**, **Region Color**, and **Graph Visualization** sub-groups are excluded; use **File › Save Workspace...** to preserve full color and graph states. |
| <span class="gui-menu-item">Restore Rendering Settings</span> | Resets all 2D and 3D display options—including nested **Tract Color**, **Region Color**, and **Graph Visualization** settings—to defaults while keeping layer visibility checkboxes, camera angle, and tracking parameters unchanged. |
