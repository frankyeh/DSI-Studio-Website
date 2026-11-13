# [Regions Misc] Menu

The **[Regions Misc]** menu in the Fiber Tracking window provides specialized tools for creating regions from slice intensity thresholds, MNI coordinates, or whole-brain anisotropy masks, batch-managing region display palettes, sorting the Region list by name, volume, or spatial position, exporting per-voxel fiber orientations and quantitative metrics, centering slice views on a region, and loading or manually refining subject-to-atlas spatial registration. For opening and saving region files, assigning tracking constraint roles, and computing region summary statistics, see the [Regions menu](/doc/menu_regions.html); for morphological operations and spatial shifts, see the [Region Edit menu](/doc/menu_region_edit.html) and [Region Move menu](/doc/menu_region_move.html).

## Create regions

| Menu item | Behavior |
|---|---|
| **New Region From Thresholding...** | Appends a checked region named `New Region` on the currently active slice grid (with **Type** initially set to `...`, visualization-only) and opens an input dialog pre-filled with Otsu's automatic intensity threshold for the active slice. Entering a positive cutoff `T` masks voxels strictly above that intensity (`I > T`); entering a negative value `-T` selects the low-intensity complement (`I <= T`). Because the `New Region` row is appended before the prompt appears, canceling the dialog leaves an empty region row in the Region list that should be deleted if unneeded. Change **Type** (for example to `ROI`, `ROA`, or `Seed`) before using the mask to constrain fiber tracking. |
| **New Region From MNI Coordinate...** | Prompts for four space-separated numbers — the center MNI coordinate `(x, y, z)` in **millimeters (mm)** and the sphere radius in **voxels** (pre-filled with `0 0 0 10`; if only three coordinates are entered, a second dialog asks for a voxel radius from `1` to `100`, default `5`). Automatically computes or loads subject-to-MNI registration if needed, transforms the MNI millimeter center into subject diffusion space and into the active slice's voxel grid, appends a checked `New Region` row (**Type** `...`) on the active slice grid, and fills all voxels within that Euclidean voxel radius (`distance <= radius`). Note the mixed units: the center is specified in MNI millimeters, whereas the radius is in voxels of the active slice, so physical sphere diameter scales with the active slice resolution. |

## Region colors

| Menu item | Behavior |
|---|---|
| **Load Region Color...** | Opens a file dialog (`*.txt`) to batch-assign custom display colors to every region currently in the Region list. Accepts either **positional** whitespace-separated rows (`R G B` with alpha defaulting to `255`, or `R G B A`, one non-blank line per region in Region-list order) or **name-keyed tab-separated** rows (`<Region Name><TAB>R<TAB>G<TAB>B<TAB>A`, naming each unique region in the list exactly once), with integer channel values from `0` to `255`. The entire file is validated before any color changes: mixed formats, mismatched region counts, out-of-range channel numbers, or unknown, duplicate, or missing region names abort the load without altering existing colors. Assigned region colors are displayed when region coloring is set to assigned-color mode rather than metric or tract colormaps. |
| **Save Region Color...** | Opens a save file dialog (`*.txt`) and writes the current assigned color of every region in Region-list row order as one space-separated `R G B A` integer line (`0–255` per channel) per region, ready to reload later with **Regions Misc › Load Region Color...**. |

## Sort regions

Sorting items reorder **checked** regions within their checked row slots in the Region list while leaving any unchecked rows at their existing positions. Selecting the same sort item a second time consecutively reverses the sort direction between ascending and descending.

| Menu item | Behavior |
|---|---|
| **Sort By Name** | Sorts checked regions lexicographically by region **Name** (ascending on first selection; selecting **Sort By Name** again immediately reverses to descending order). |
| **Sort By Size** | Sorts checked regions by physical region volume in **cubic millimeters (`mm³`)** (`voxel count × voxel_size_x × voxel_size_y × voxel_size_z`), smallest to largest on first selection and reversing to largest-first when selected again consecutively. |
| **Sort By X** | Sorts checked regions by the **X** coordinate of each region's center of mass mapped into FIB diffusion voxel space, ascending on first selection and reversing order when selected again consecutively. Avoid sorting empty (`0`-voxel) checked regions, which have no defined center of mass. |
| **Sort By Y** | Sorts checked regions by the **Y** coordinate of each region's center of mass mapped into FIB diffusion voxel space, ascending on first selection and reversing order when selected again consecutively. |
| **Sort By Z** | Sorts checked regions by the **Z** coordinate of each region's center of mass mapped into FIB diffusion voxel space, ascending on first selection and reversing order when selected again consecutively. |

## Seeding and voxel data

| Menu item | Behavior |
|---|---|
| **Whole Brain Seeding** | Appends a checked region named `whole brain` on the active slice grid with **Type** automatically set to `Seed`, masking all voxels where primary fiber anisotropy (`fa0` / QA, sampled into the active slice space when an anatomical overlay slice is active) exceeds `otsu_threshold × fa_otsu` using the current **Otsu Threshold** ratio in the Tracking Parameters panel. Use this when you want to inspect, manually edit, or save the explicit whole-brain seed mask before tracking; see [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html) for how explicit `Seed` regions interact with tracking constraints. |
| **Save Voxel Data As...** | Prompts for a text file path (`*.txt`) and exports voxel-level data for the **currently selected** (highlighted) row in the Region list. Maps the selected region's voxels into the FIB diffusion grid and writes a tab-delimited table with one row per voxel containing integer diffusion voxel coordinates (`x`, `y`, `z`), unit fiber orientation vector components (`dx0`, `dy0`, `dz0`, `dx1`, `dy1`, `dz1`, ... for each resolved fiber population, or `0` where unresolved), and quantitative values from every loaded diffusion metric and inserted slice volume. Operates only on the single highlighted region row; exporting large whole-brain masks can produce very large text files. |

## Mapping and alignment

| Menu item | Behavior |
|---|---|
| **Move Slices To Current Region** | Centers the orthogonal 2D and 3D slice crosshairs on the **currently selected** (highlighted) region in the Region list using the region's **per-axis median voxel** across X, Y, and Z (mapped into the active slice's voxel space if needed). Using the per-axis median rather than the arithmetic mean prevents small disconnected outlier fragments from pulling the slice crosshairs off the main structure or lesion; has no effect if no region is selected or if the selected region is empty. |
| **Load MNI mapping...** | Opens a file dialog (`*.mz`) to load a precomputed nonlinear subject-to-template spatial mapping file for the open FIB dataset and resets any cached tractography atlas so subsequent atlas parcellations and MNI region operations use the loaded warp. Requires a `.mz` file whose source grid dimensions match the open FIB file and whose modification timestamp is **not older** than the FIB file itself (when template dimensions or MNI transforms differ from the active template, the template side of the warp is adjusted automatically). |
| **Manual Atlas Alignment...** | Opens the interactive **Manual Atlas Alignment** dialog overlaying the subject's anisotropy and isotropic maps against the active template across sagittal, coronal, and axial views, with interactive controls for translation, rotation, scaling, shearing, cost-function selection, local refinement, and full affine registration re-estimation. If the open FIB file is already in template space (QSDR), an informational dialog reports that manual atlas alignment is unnecessary. Accepting the dialog (`OK`) stores the manual affine alignment, removes any existing `.mz` mapping cache file on disk for the subject, recomputes the full subject-to-MNI registration, and immediately opens the **Add Regions** atlas selection dialog; canceling leaves existing mappings unchanged. |
