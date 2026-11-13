# [Devices] Menu

Use the **[Devices]** menu in the Fiber Tracking window to place, align, render, and export 3D surgical trajectories—such as DBS leads, SEEG electrodes, probes, scales, and coordinate locators—alongside slices, regions, and tracts. Right-clicking inside the **Devices** dock opens this same menu, while the dock toolbar provides quick actions and switches coordinate display between `Native Image` voxels and `AC/PC/MCP` millimeters.

![Device visualization in DSI Studio](https://user-images.githubusercontent.com/275569/147839140-127aedcb-e626-422a-8a3b-c1f7749d24d5.png)

## Menu actions

| Menu item | Behavior |
|---|---|
| **New Device** | Adds a new checked device (`100` mm default length) near the center of the volume and opens the **Devices** dock if hidden. Warns if the loaded dataset has non-isotropic voxel spacing. |
| **Open Device...** | Loads one or more `.dv.csv` device trajectory files into the **Devices** table as checked rows. Does not load 3D polygon meshes such as `.obj`, and shows an error if a file contains no valid devices. |
| **Save Device As...** | Saves only the highlighted device row to a single-line `.dv.csv` trajectory file (`<Name>.dv.csv`). Does nothing if no device row is selected. |
| **Save All Devices As...** | Saves all checked devices in table order to a single `.dv.csv` trajectory file (`<CurrentName>.dv.csv`), omitting unchecked rows. |
| **Delete** | Removes the highlighted device row from the **Devices** table and clears it from the 3D view. |
| **Delete All** | Clears every row from the **Devices** table and removes all devices from the 3D view. |
| **Copy Device** | Duplicates the highlighted device with the same type, length, and orientation, offsetting its tip by `1` voxel with a new color so both remain visible. |
| **Assign Colors** | Assigns distinct opaque colors across all rows in the **Devices** table in order and refreshes the 3D view. |
| **Leads to ROI...** | Creates spherical regions (`1.0`–`10.0` mm radius, default `2.0` mm) in the **Regions** dock at each cylindrical electrode contact or `Locator` point of the highlighted device, ordered from distal tip to shaft. Directional segmented rings render in 3D but are not extracted. |
| **Uncheck All** | Unchecks every row in the **Devices** table, hiding all devices in 3D and excluding them from **Devices › Save All Devices As...** and **File › Save Workspace...**. |
| **Check All** | Checks every row in the **Devices** table so all devices render in 3D and are included when saving all devices or workspaces. |
| **Pull Device** | Shortcut: `Ctrl+-`. Retracts the highlighted device by `0.5` mm along its shaft toward the entry point. |
| **Push Device** | Shortcut: `Ctrl+=`. Advances the highlighted device by `0.5` mm deeper along its shaft toward the distal tip. |
| **Set AC/PC/Inter Points** | Places three `2` mm `Locator` markers (`AC`, `PC`, and midline `Inter`) in subject space from standard MNI landmarks, replacing any existing rows with those names. Requires valid MNI registration on the loaded FIB file. |
| **Statistics...** | Opens a **Device Statistics** report listing tip coordinates, shaft direction, and per-contact distal, center, and proximal voxel positions across all devices, with an option to save `<stem>_device_stat.txt`. |

## Add a device manually

Choose **[Devices][New Device]** (or right-click a visible 3D slice and choose **New Device** to place the distal tip at that point), then configure **Name**, **Color**, **Length** (`0.5`–`512.0` mm), **Type**, **Coordinate** (`Native Image` voxels vs `AC/PC/MCP` millimeters), and spherical shaft angles (**phi**, **theta**) in the **Devices** dock. Visibility, lighting, and proximal 3D labels for checked devices are controlled under **Device Rendering** and **Label Rendering** in **Rendering Options**.

![New device settings](https://user-images.githubusercontent.com/275569/147839144-b4bb48b1-7aed-4774-9d14-4928d81c0cf2.png)

In the 3D view, double-click a device to select its row, or right-click a slice and choose **Move Current Device Here** to jump its distal tip to that point. Enable **[Region Move][Move Region at 3D]** (`Ctrl+A`) to drag near the distal tip to translate the device or drag along the shaft to rotate its trajectory, then step `0.5` mm axially with `Ctrl+=` and `Ctrl+-`.

![Device displayed in 3D view](https://user-images.githubusercontent.com/275569/147839149-88ba5c23-5c45-4481-9f62-22b6a0380af5.png)

### Coordinate and voxel cautions

Reconstruct or resample datasets to isotropic voxel spacing before evaluating surgical trajectories, because device scaling converts millimeters to voxels using in-plane voxel width. Before relying on `AC/PC/MCP` stereotactic coordinates, verify the `AC`, `PC`, and `Inter` locators on anatomical slices and keep those three rows intact while working in `AC/PC/MCP` mode.

## CT-guided device placement

Load a co-registered postoperative CT or anatomical MRI via **[Slices][Insert Other Images...]**, verify alignment across axial, coronal, and sagittal views, and match the device tip and shaft to the trajectory artifact using 3D slice placement, `Ctrl+A` dragging, and `Ctrl+=` / `Ctrl+-` stepping. Because DSI Studio does not automatically detect electrodes on CT, confirm contact positions visually before running **Leads to ROI...** or **Statistics...**.

## Save devices

Device files use plain-text `.dv.csv` format (`name,type,i j k,dx dy dz,length,color`, one line per device storing native tip voxel coordinates, shaft direction, length in mm, and color), and **[File][Save Workspace...]** saves checked devices to `<workspace>/devices/device.dv.csv`; see the [Slices menu](/doc/menu_slices.html#workspaces).

**[Screen][Save 3D Model...]** exports visible slices, tracts, regions, and isosurfaces to `.obj` or `.glb` without rendered devices; run **[Devices][Leads to ROI...]** first to include electrode contact spheres as region meshes in exported 3D models.

## Custom device definitions

DSI Studio loads built-in DBS lead, SEEG electrode, probe, obturator, scale, and locator geometries at startup from `device.txt` in the application folder, where each model is defined by four lines (model name, distal-to-proximal segment lengths in mm, segment type codes, and outer diameter in mm). Back up `device.txt` before editing custom entries, and restart DSI Studio to load your changes.
