# [Devices] Menu

Use the **[Devices]** menu in the Fiber Tracking window to create, load, position, render, convert into contact regions, and export 3D stereotactic or surgical devices—including DBS leads, SEEG electrodes, probes, laser probes, obturators, millimeter scales, and coordinate locators—together with image slices, regions, and tractography. Right-clicking inside the **Devices** dock opens this same menu, and the **Devices** dock toolbar provides quick buttons for creating, copying, opening, saving, saving all, and deleting devices alongside the **Coordinate** display selector (`Native Image` or `AC/PC/MCP`).

![Device visualization in DSI Studio](https://user-images.githubusercontent.com/275569/147839140-127aedcb-e626-422a-8a3b-c1f7749d24d5.png)

## Menu actions

| Menu item | Behavior |
|---|---|
| **New Device** | Appends a new checked device row (`Device1`, `Device2`, …) with default `Length = 100` mm near the center of the diffusion volume, opens the **Devices** dock if hidden, and warns if the loaded volume has non-isotropic voxel spacing (when in-plane voxel width differs from slice thickness). |
| **Open Device...** | Opens a file dialog (`*dv.csv` or all files) and appends every device entry from one or more `.dv.csv` trajectory files into the **Devices** table as checked rows. Does not load 3D polygon meshes such as Wavefront `.obj` files; device 3D geometry is generated from parametric definitions in `device.txt`. Displays an error dialog if a selected file yields no valid device rows. |
| **Save Device As...** | Opens a save dialog defaulting to `<Name>.dv.csv` and writes only the currently selected device row to a single-line `.dv.csv` file (distal tip in native diffusion voxel coordinates, unit shaft vector, length in mm, and 32-bit ARGB color). Does nothing if no device is selected. |
| **Save All Devices As...** | Opens a save dialog defaulting to `<CurrentName>.dv.csv` and writes all **checked** devices in table row order to a single `.dv.csv` file; unchecked rows are omitted. |
| **Delete** | Removes the currently selected row from the **Devices** table and updates the 3D viewport. |
| **Delete All** | Clears every device row from the **Devices** table and removes all devices from the 3D viewport. |
| **Copy Device** | Duplicates the currently selected device with the same **Type**, **Length**, and shaft direction, offsets the copy's distal tip by `+1` voxel along `i` so both trajectories remain distinguishable, and assigns a new opaque color. |
| **Assign Colors** | Reassigns distinct opaque hue colors across all rows in the **Devices** table in row order and refreshes 3D rendering. |
| **Leads to ROI...** | Prompts for a sphere radius (`1.0`–`10.0` mm, default `2.0` mm) and creates `8×` super-resolution spherical regions (`<Name>_lead_0`, `<Name>_lead_1`, …) in the **Regions** dock centered at each standard cylindrical electrode contact or `Locator` point of the currently selected device, ordered from the distal tip toward the proximal shaft. Directional 3-segment ring contacts (such as the middle rings on `DBS Lead:Abbott Infinity` and `DBS Lead:Boston Scientific`) render in 3D but are not extracted as spherical ROIs. |
| **Uncheck All** | Unchecks every row in the **Devices** table, hiding all devices in the 3D viewport and excluding them from **Devices › Save All Devices As...** and **File › Save Workspace...**. |
| **Check All** | Checks every row in the **Devices** table so all loaded devices render in 3D and are included when saving all devices or saving a workspace. |
| **Pull Device** | Shortcut: `Ctrl+-`. Retracts the currently selected device by `+0.5` mm along its shaft vector (toward the proximal shaft and entry point), converting the physical `0.5` mm step into native voxel units using the volume's voxel size. |
| **Push Device** | Shortcut: `Ctrl+=`. Advances the currently selected device by `-0.5` mm along its shaft vector (deeper in the distal tip direction), converting the physical `0.5` mm step into native voxel units using the volume's voxel size. |
| **Set AC/PC/Inter Points** | Maps three standard MNI reference coordinates—anterior commissure `AC` `(0.05, 2.7, -4.8)` mm, posterior commissure `PC` `(0.0, -25.0, -2.0)` mm, and interhemispheric midline point `Inter` `(0.0, -10.0, 30.0)` mm—into current subject diffusion space as three checked `2` mm `Locator` entries, replacing any existing `AC`, `PC`, or `Inter` rows. Requires valid subject-to-MNI mapping on the loaded FIB volume. |
| **Statistics...** | Opens a **Device Statistics** dialog across all devices in the table listing tab-separated `Name`, `tip location (voxels)`, normalized shaft `direction`, and per-contact (`contact0`, `contact1`, …) `distal end position (voxels)`, `center position (voxels)`, and `proximal end position (voxels)` for cylindrical contacts and `Locator` markers, with a save prompt defaulting to `<stem>_device_stat.txt`. |

## Add a device manually

Choose **[Devices][New Device]**—or right-click a visible slice in the 3D viewport and choose **New Device** to place the distal tip directly at that slice point—then configure its row in the **Devices** dock:

- **Name / Color / Length:** Check the **Name** box to display the device in 3D, click **Color** to pick an opaque color, and adjust total rendered shaft **Length** in millimeters (`0.5`–`512.0` mm). Selecting `Locator` automatically sets **Length** to `1` mm if it was previously above `5` mm; selecting `Scale` on a new default row sets **Name** to `50 mm` and **Length** to `50` mm; switching from a short marker (`< 5` mm) to a lead, electrode, or probe resets **Length** to `100` mm.
- **Type:** Select one of the built-in DBS leads, SEEG electrodes, probes, laser probes, obturators, `Scale`, or `Locator` definitions loaded from `device.txt`. When a default row name (`Device1`, `Lead1`, …) has not been customized, changing **Type** updates the prefix automatically.
- **Coordinate (`Native Image` vs `AC/PC/MCP`):** Switches the table's three position columns between native diffusion voxel coordinates (`i(voxels)`, `j(voxels)`, `k(voxels)`) and right-handed mid-commissural millimeter coordinates (`X(mm)`, `Y(mm)`, `Z(mm)` centered at `(AC + PC) / 2`). If `AC`, `PC`, or `Inter` is not yet present in the table, selecting `AC/PC/MCP` automatically runs **[Devices][Set AC/PC/Inter Points]**.
- **Orientation (`phi`, `theta`):** Spherical shaft angles in degrees pointing from the distal tip toward the proximal shaft, where `phi` (`-180°` to `180°`) is the azimuth in the `x–y` plane and `theta` (`0°` to `180°`) is the polar angle from `+z`.

![New device settings](https://user-images.githubusercontent.com/275569/147839144-b4bb48b1-7aed-4774-9d14-4928d81c0cf2.png)

Checked devices render in the 3D viewport whenever **Device Rendering** is enabled in the **Rendering Options** dock, where **Device** lighting sources, light direction, shading, diffuse/ambient/specular intensity, material properties, and blend functions can be adjusted. To display or style 3D device names at the proximal shaft end, toggle **Show Device Label** and adjust **Device Color**, **Device Bold**, and **Device Size** under **Label Rendering**.

![Device displayed in 3D view](https://user-images.githubusercontent.com/275569/147839149-88ba5c23-5c45-4481-9f62-22b6a0380af5.png)

To select and move a device interactively in the 3D viewport:

- **Select in 3D:** Double-click a rendered device in the 3D view to highlight its row in the **Devices** dock.
- **Jump tip to a slice point:** Right-click any visible slice in the 3D view and choose **Move Current Device Here** to place the selected device's distal tip at that 3D slice coordinate.
- **Drag tip or rotate trajectory in 3D:** Enable **[Region Move][Move Region at 3D]** (`Ctrl+A`), then drag near the distal 20% of the device (or anywhere on a short marker with `Length <= 3` mm) to translate its distal tip, or drag along the proximal 80% of the shaft to rotate its trajectory direction.
- **Fine axial stepping:** Press `Ctrl+=` (**[Devices][Push Device]**) to advance `-0.5` mm deeper toward the distal tip or `Ctrl+-` (**[Devices][Pull Device]**) to retract `+0.5` mm toward the proximal shaft, inspecting alignment from multiple camera angles.

### Coordinate and voxel cautions

- **Use isotropic voxel spacing:** Device 3D cylinder scaling and contact coordinate offsets convert millimeters to voxels using the primary in-plane voxel size. Reconstruct or resample datasets with isotropic voxels (equal voxel dimensions across all three axes) before evaluating surgical trajectories; **[Devices][New Device]** displays a warning whenever in-plane voxel width differs from slice thickness.
- **Verify `AC`, `PC`, and `Inter` landmarks before reading `AC/PC/MCP` coordinates:** Automatically placed `AC`, `PC`, and `Inter` locators rely on nonlinear MNI-to-subject registration. Always inspect and refine those three `Locator` positions on high-resolution anatomical slices before relying on `X(mm)`, `Y(mm)`, `Z(mm)` stereotactic readouts, and do not delete or rename `AC`, `PC`, or `Inter` while editing in `AC/PC/MCP` mode.
- **Prefer 3D slice placement, 3D dragging, or `AC/PC/MCP` entry over typing `i(voxels)`:** When positioning a tip in `Native Image` mode, place or move the device via 3D slice right-click (**Move Current Device Here**), **[Region Move][Move Region at 3D]**, **[Devices][Push Device]** / **[Devices][Pull Device]**, or `.dv.csv` import rather than typing directly into the `i(voxels)` spinbox, and confirm tip coordinates visually across orthogonal slices.

## CT-guided device placement

1. Load the postoperative CT (or co-registered anatomical MRI) with **[Slices][Insert Other Images...]**.
2. Verify CT-to-diffusion registration across axial, coronal, and sagittal views before relying on image-derived coordinates.
3. Add a device with **[Devices][New Device]** (or right-click the distal tip artifact on a visible 3D slice and choose **New Device**), select the matching electrode or lead model under **Type**, and align the tip and shaft against the CT trajectory artifact using **[Region Move][Move Region at 3D]** (`Ctrl+A`) and `0.5` mm axial steps (`Ctrl+=` / `Ctrl+-`).
4. Choose **[Devices][Leads to ROI...]** to create per-contact spherical regions for tractography seeding or filtering, or **[Devices][Statistics...]** to inspect and save per-contact voxel coordinates.

The **[Devices]** menu does not include an automatic **Detect Electrodes** tool; contact localization from postoperative CT requires manual visual alignment or an externally validated localization workflow.

## Save devices

Device files use plain-text `.dv.csv` format—one comma-separated line per device with no header row:

```text
name,type,i j k,dx dy dz,length,color
```

`i j k` always stores the distal tip in current native diffusion voxel coordinates (even when the **Devices** dock displays `AC/PC/MCP`), `dx dy dz` is the unit shaft vector pointing from the distal tip toward the proximal shaft (normalized on load), `length` is in millimeters, and `color` is an unsigned 32-bit ARGB integer.

- **[Devices][Save Device As...]** saves only the currently selected row to `.dv.csv`.
- **[Devices][Save All Devices As...]** saves all **checked** rows in table order to one `.dv.csv` file.
- **[File][Save Workspace...]** writes checked devices to `<workspace>/devices/device.dv.csv`, and **[File][Load Workspace...]** restores all `*dv.csv` files from that folder alongside tracts, regions, slices, and camera settings; see the [Slices menu](/doc/menu_slices.html#workspaces).
- **CSV trajectories vs `.obj` / `.glb` 3D meshes:** **[Devices][Open Device...]** and **[Devices][Save Device As...]** exchange parametric `.dv.csv` trajectory lines rather than Wavefront `.obj` polygon meshes. Likewise, **[Screen][Save 3D Model...]** exports visible slices, tracts, regions, and isosurfaces to `.obj` or `.glb` without procedural OpenGL device cylinders—use **[Devices][Leads to ROI...]** first if you want electrode contact spheres included as region meshes in an exported 3D model.
- Interactive device creation and placement steps performed in the GUI are also logged in **[Records][Command History...]** for session review and replay; see the [Records menu](/doc/menu_records.html).

## Custom device definitions

DSI Studio loads device geometries at startup from `device.txt` in the application folder. Back up `device.txt` before making edits, keep all segment lengths and outer diameters in millimeters, and restart DSI Studio after saving changes.

<details markdown="1">
<summary>Built-in models and <code>device.txt</code> 4-line specification</summary>

`device.txt` includes 19 built-in models:

- **DBS leads:** `DBS Lead:Medtronic 3387`, `DBS Lead:Medtronic 3389`, `DBS Lead:Medtronic B33005`, `DBS Lead:Medtronic B33015`, `DBS Lead:Abbott Infinity`, `DBS Lead:Boston Scientific`
- **SEEG electrodes:** `SEEG Electrode:8 Contacts`, `SEEG Electrode:10 Contacts`, `SEEG Electrode:12 Contacts`, `SEEG Electrode:14 Contacts`, `SEEG Electrode:16 Contacts`
- **Probes & obturators:** `Probe`, `Laser Probe (1.6mm)`, `Laser Probe (2.2mm)`, `Laser Probe (3.3mm)`, `Obturator:11 mm`, `Obturator:13.5 mm`
- **Markers:** `Scale`, `Locator`

Each model entry in `device.txt` consists of four consecutive lines:

1. **Model name** — exact label shown in the **Type** dropdown and saved in `.dv.csv`.
2. **Segment lengths (mm)** — whitespace-separated segment lengths ordered from the distal tip toward the proximal shaft.
3. **Segment type codes** — whitespace-separated integers matching each segment in Line 2:
   - `-1`: distal tip sphere cap (`0.0` length entry; shifts rendering forward by half-radius, shortens the next segment by half-radius, and matches the next segment's color).
   - `0`: insulated spacer cylinder rendered in the device's assigned color.
   - `1`: cylindrical electrode contact rendered in dark gray; distal end, center, and proximal end coordinates are exported by **[Devices][Statistics...]** and contact centers are converted to spheres by **[Devices][Leads to ROI...]**.
   - `2`: 3-part directional segmented contact ring (used on `DBS Lead:Abbott Infinity` and `DBS Lead:Boston Scientific`; rendered in 3D, not extracted as standard cylindrical contacts by **[Devices][Leads to ROI...]** or **[Devices][Statistics...]**).
   - `3`: proximal shaft cylinder rendered in the device's assigned color whose length adjusts dynamically to match the table **Length** setting (minimum `5` mm).
   - `4`: white cylindrical segment.
   - `-2`: flanged obturator cap (`radius` inner cylinder and `radius + 5` mm outer flange).
   - `-3`: 3-axis orthogonal crosshair marker (`Locator`; arm half-length equals the table **Length** value in mm, and its center is included in **[Devices][Leads to ROI...]** and **[Devices][Statistics...]**).
4. **Outer diameter (mm)** — single floating-point diameter halved internally (`diameter * 0.5`) to set cylinder rendering radius (except `Locator`, whose crosshair half-length uses the **Length** column directly).

</details>
