# [Devices] Menu

Create, position, render, convert to contact ROIs, and export 3D stereotactic or surgical devices—such as DBS leads, SEEG electrodes, probes, laser probes, obturators, scales, and coordinate locators—together with slices, regions, and tractography. Right-clicking inside the **Devices** dock opens the same menu.

![Device visualization in DSI Studio](https://user-images.githubusercontent.com/275569/147839140-127aedcb-e626-422a-8a3b-c1f7749d24d5.png)

## Menu actions

| Menu item | Shortcut / GUI command hint | `--action=vis --cmd` | Behavior |
|---|---|---|---|
| New Device | `run new_device` | `new_device` or `new_device,<i j k>` | Appends `Device<N>` (`Length = 100` mm, checked) near the volume center (or at voxel `i j k`), opens the **Devices** dock, and warns if voxels are non-isotropic (`vs[0] != vs[2]`). |
| Open Device... | — | `load_device,<file.dv.csv>` | Appends devices from one or more `.dv.csv` files (`actionOpenDevice` opens a file picker directly; `load_device` is the console/AI command). |
| Save Device As... | — | — | Saves only the currently selected device row to a single-line `.dv.csv` file (`<Name>.dv.csv`). |
| Save All Devices As... | `run save_all_devices` | `save_all_devices,<file.dv.csv>` | Writes all **checked** devices in table order to one `.dv.csv` file; unchecked rows are omitted. |
| Delete | `run delete_device` | `delete_device` or `delete_device,<idx>` | Removes the selected device row (or a 0-based index / `&`-joined index list such as `0&2&5` via command). |
| Delete All | `run delete_all_devices` | `delete_all_devices` | Removes every device from the **Devices** table. |
| Copy Device | `run copy_device` | `copy_device` or `copy_device,<idx>` | Duplicates the selected (or indexed) device with the same type, length, and direction, offset by `+1` voxel along `i` (`pos[0]`) with a new color. |
| Assign Colors | — | — | Reassigns distinct opaque hue colors across all devices in table order. |
| Leads to ROI... | — | — | Prompts for a sphere radius (`1.0`–`10.0` mm, default `2.0` mm) and creates `8×` super-resolution spherical regions (`<Name>_lead_0`, `<Name>_lead_1`, ...) at each cylindrical contact (`seg_type = 1`) or `Locator` (`seg_type = -3`) center of the selected device. |
| Uncheck All | — | — | Unchecks all rows, hiding every device in 3D and excluding them from `[Devices][Save All Devices As...]`. |
| Check All | — | — | Checks all rows so every device renders in 3D and is included when saving all devices. |
| Pull Device | Shortcut: `Ctrl+-` · `run pull_device` | `pull_device` or `pull_device,<idx>` | Shifts the selected (or indexed) device by `+0.5` mm along its shaft vector (`+dir`, toward the proximal shaft/entry), scaled by voxel size. |
| Push Device | Shortcut: `Ctrl+=` · `run push_device` | `push_device` or `push_device,<idx>` | Advances the selected (or indexed) device by `-0.5` mm along its shaft vector (`-dir`, deeper in the distal tip direction), scaled by voxel size. |
| Set AC/PC/Inter Points | `run set_acpc` | `set_acpc` | Maps fixed MNI landmarks—`AC` `(0.05, 2.7, -4.8)`, `PC` `(0.0, -25.0, -2.0)`, and `Inter` `(0.0, -10.0, 30.0)` mm—into subject space as `2` mm `Locator` entries, replacing any existing `AC`/`PC`/`Inter` rows. |
| Statistics... | `run show_device_statistics` | `show_device_statistics` | Reports tab-separated `Name`, `tip location (voxels)`, normalized `direction`, and per-contact (`seg_type = 1` or `-3`) `distal end`, `center`, and `proximal end` voxel coordinates across all devices (opens a saveable dialog interactively; use `save_device_statistics,<file.txt>` to write directly to disk). |

The `run ...` strings above come from `tracking_window.ui` (`toolTip`) and serve as **GUI/agent command hints**, not standalone `dsi_studio --action=...` CLI flags. For command-line replay in `--action=vis`, pass verified command tokens inside `--cmd="..."` (`+` separates commands and `,` separates arguments; see [Command History](/doc/cli_vis.html)).

## Add a device manually

Choose **[Devices][New Device]** (or right-click a visible 3D slice and choose **New Device** to place the tip directly at that slice point), then configure its row in the **Devices** dock:

- **Name / Color / Length:** Check the `Name` cell to show the device in 3D; edit `Color` and total shaft `Length` (mm, range `0.5`–`512.0`). Selecting `Locator` sets `Length` to `1` mm if previously `> 5` mm; selecting `Scale` on a new default row sets `Name` to `50 mm` and `Length` to `50` mm; switching from a short marker (`< 5` mm) to another electrode or probe resets `Length` to `100` mm.
- **Type:** Choose a built-in lead, electrode, probe, obturator, `Scale`, or `Locator` geometry from `device.txt`.
- **Coordinate (`Native Image` vs `AC/PC/MCP`):** Displays tip position either as native diffusion voxel coordinates (`i(voxels)`, `j(voxels)`, `k(voxels)`) or in mid-commissural millimeter coordinates (`X(mm)`, `Y(mm)`, `Z(mm)` relative to `(AC + PC) / 2`, auto-running **[Devices][Set AC/PC/Inter Points]** if `AC`, `PC`, or `Inter` is missing).
- **Orientation (`phi`, `theta`):** Spherical shaft angles in degrees from the distal tip toward the proximal shaft—`phi` (`-180°` to `180°`) is azimuth in the `x–y` plane and `theta` (`0°` to `180°`) is the polar angle from `+z`.

![New device settings](https://user-images.githubusercontent.com/275569/147839144-b4bb48b1-7aed-4774-9d14-4928d81c0cf2.png)

Checked devices render in the 3D view when **Device Rendering** (`show_device`) is enabled in **Rendering Options** (with optional 3D text labels via **Show Device Label** / `show_device_label`).

![Device displayed in 3D view](https://user-images.githubusercontent.com/275569/147839149-88ba5c23-5c45-4481-9f62-22b6a0380af5.png)

To adjust a device interactively in 3D:

- **Select:** Double-click a device in the 3D view to select its row in the **Devices** table.
- **Jump tip to slice point:** Right-click a visible 3D slice and choose **Move Current Device Here**.
- **Translate tip or rotate shaft in 3D:** Activate **[Region Move][Move Region at 3D]** (`Ctrl+A`), then drag the distal 20% of the device (or any device with `Length <= 3` mm) to translate its tip (`pos`), or drag along the proximal 80% of the shaft to rotate its trajectory (`dir`).
- **Fine axial step:** Press `Ctrl+=` (**[Devices][Push Device]**) or `Ctrl+-` (**[Devices][Pull Device]**) to step `-0.5` mm or `+0.5` mm along the shaft axis, inspecting alignment from multiple camera angles.

## CT-guided device placement

1. Load the postoperative CT with **[Slices][Insert Other Images...]**.
2. Verify CT-to-MRI/diffusion registration across views before relying on image-derived coordinates.
3. Place a device with **[Devices][New Device]** (or right-click the target contact on a 3D slice and choose **New Device**), select the matching electrode or lead model under **Type**, and align the tip and trajectory against the CT trajectory artifact (`Ctrl+=` / `Ctrl+-` steps `0.5` mm along the shaft).
4. Optionally run **[Devices][Leads to ROI...]** to generate per-contact spherical ROIs for tractography seeding/filtering, or **[Devices][Statistics...]** to export contact coordinates in voxel space.

The **[Devices]** menu does not include an automatic **Detect Electrodes** action; CT contact localization requires manual verification or a separately validated workflow.

## Save devices

Device files use `.dv.csv` format—one comma-separated line per device with no header row:

```text
name,type,i j k,dx dy dz,length,color
```

`i j k` stores the distal tip in current diffusion voxel space (even when the table displays `AC/PC/MCP`), `dx dy dz` is the unit shaft vector (normalized on load), `length` is in millimeters, and `color` is a 32-bit unsigned ARGB integer.

- **[Devices][Save Device As...]** writes only the currently selected row.
- **[Devices][Save All Devices As...]** writes all **checked** rows in table order.
- Saving a workspace (**[File][Save Workspace...]**) writes checked devices to `<workspace>/devices/device.dv.csv` and restores all `*dv.csv` files from that folder on load; see the [Slices menu](/doc/menu_slices.html#workspaces).

## Automation and command history

Beyond the menu actions above, `tracking_window` supports these parameterized device commands in DSI Studio AI and `--action=vis --cmd="..."` (device indices are 0-based table rows and default to the selected row when omitted):

| Command | Syntax (`--cmd` / AI) | Behavior |
|---|---|---|
| `move_device` | `move_device,<i j k>[,<idx>]` | Moves a device tip to voxel coordinate `i j k` in current diffusion space. |
| `set_device_type` | `set_device_type,<type_name>[,<idx>]` | Sets `Type` by case-insensitive match against `device.txt` and applies default name/length rules. |
| `set_device_direction` | `set_device_direction,<phi theta \| dx dy dz>[,<idx>]` | Sets shaft direction from two spherical angles in degrees (`phi theta`) or a non-zero 3D vector (`dx dy dz`, normalized); alias `set_device_dir`. |
| `list_devices` | `list_devices` | Prints tab-separated `index`, `shown`, `name`, `type`, `color`, `length`, `x`, `y`, `z`, `phi`, `theta`; alias `list_device`. |
| `list_device_types` | `list_device_types` | Prints all available device model names loaded from `device.txt`. |
| `save_device_statistics` | `save_device_statistics,<output.txt>` | Writes the `[Devices][Statistics...]` contact-coordinate table directly to `<output.txt>` without opening a dialog (path required). |

Example scripted placement, export, and contact report with `--action=vis` (requires a display/GUI environment):

```bash
dsi_studio --action=vis --source=sub01.fz --cmd="new_device,80 100 80+set_device_type,DBS Lead:Medtronic 3389,0+set_device_direction,30 45,0+save_all_devices,electrodes.dv.csv+save_device_statistics,device_stat.txt"
```

## Custom device definitions

DSI Studio loads device geometries from `device.txt` in the application package at startup. Back up `device.txt` before editing, keep all lengths and diameters in millimeters, and restart DSI Studio after saving changes.

<details markdown="1">
<summary>Built-in models and <code>device.txt</code> 4-line specification</summary>

`device.txt` ships with 19 built-in models:

- **DBS leads:** `DBS Lead:Medtronic 3387`, `DBS Lead:Medtronic 3389`, `DBS Lead:Medtronic B33005`, `DBS Lead:Medtronic B33015`, `DBS Lead:Abbott Infinity`, `DBS Lead:Boston Scientific`
- **SEEG electrodes:** `SEEG Electrode:8 Contacts`, `SEEG Electrode:10 Contacts`, `SEEG Electrode:12 Contacts`, `SEEG Electrode:14 Contacts`, `SEEG Electrode:16 Contacts`
- **Probes & obturators:** `Probe`, `Laser Probe (1.6mm)`, `Laser Probe (2.2mm)`, `Laser Probe (3.3mm)`, `Obturator:11 mm`, `Obturator:13.5 mm`
- **Markers:** `Scale`, `Locator`

Each entry in `device.txt` consists of four consecutive lines:

1. **Model name** — exact string shown in the `Type` selector and accepted by `set_device_type`.
2. **Segment lengths (mm)** — whitespace-separated segment lengths ordered from the distal tip (`pos`) toward the proximal shaft along `dir`.
3. **Segment type codes** — whitespace-separated integers matching each segment in Line 2:
   - `-1`: hemispherical distal tip cap (`0.0` length entry; consumes half-radius from the next segment).
   - `0`: insulated spacer cylinder rendered in the device's assigned color.
   - `1`: cylindrical electrode contact rendered in dark gray; midpoint/ends are exported by **[Devices][Leads to ROI...]** and **[Devices][Statistics...]**.
   - `2`: 3-part directional segmented contact ring (e.g., `DBS Lead:Abbott Infinity`, `DBS Lead:Boston Scientific`; rendered in 3D, not extracted as `seg_type = 1` contacts).
   - `3`: proximal shaft cylinder whose rendered length adjusts dynamically with the device `Length` setting (minimum `5` mm).
   - `4`: white cylindrical segment.
   - `-2`: flanged obturator cap.
   - `-3`: 3-axis orthogonal crosshair marker (`Locator`; included in **[Devices][Leads to ROI...]** and **[Devices][Statistics...]**).
4. **Outer diameter (mm)** — single float halved internally (`diameter * 0.5`) to set cylinder rendering radius (for `Locator`, radius instead equals the table `Length` value).

</details>
