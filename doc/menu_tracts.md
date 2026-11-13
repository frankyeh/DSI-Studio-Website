# [Tracts] Menu

The **[Tracts]** menu (also opened by right-clicking the **Tracts** dock) imports and exports streamline files, manages loaded bundles, converts pathways into regions, and computes tract profiles, summary statistics, and connectivity matrices. Single-bundle actions apply to the highlighted row in the **Tracts** list, whereas multi-bundle actions operate on all checked rows (double-click a bundle name to rename it).

## Open and save tractography

| Menu item | Behavior |
|---|---|
| **Open Tracts...** | Loads native-space streamline files (`.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat`), aligning mismatched grid headers automatically and splitting multi-cluster files into separate bundle rows. Opening more than 7 files loads them unchecked to keep 3D rendering responsive. |
| **Open Template-Space Tracts...** | Loads template- or MNI-space streamline files (`.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat`) into a native-space session by computing nonlinear template registration and warping streamlines into subject space. Opening more than 7 files loads them unchecked. |
| **Load Built-In Atlas...** | Loads reference pathways from the built-in population tractography atlas and warps them into subject space (or displays them directly in MNI sessions). Imports pathways matching the **Autotrack...** target selector in the **Tracts** dock, or all atlas bundles when set to `All`. |
| **Save Current Tract As...** | Saves the **highlighted** bundle in host FIB space as `.tt.gz`, `.trk`/`.trk.gz`, `.tck`, `.trx`, `.txt`, `.mat`, or a binary `.nii`/`.nii.gz` tract mask. Unrecognized file extensions default to `.tt.gz`. |
| **Save All Tracts As ...** | Combines all **checked** bundles into one `.tt.gz`, `.trk`/`.trk.gz`, `.trx`, `.txt`, or `.mat` file (preserving bundle cluster labels and writing a companion `.txt` name list for `.tt.gz`/`.trk`), or exports a multi-bundle probability map as `.nii.gz`. |
| **Save All Tracts As Multiple Files...** | Saves each **checked** bundle to a chosen folder as `<bundle_name><ext>` using the default tract format (`.tt.gz`, `.trk.gz`, `.txt`, or `.trx`) set in the **Options** dock. Give checked bundles distinct names to avoid overwriting files. |

DSI Studio uses compact `.tt.gz` files natively; use `.trk`/`.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` when exchanging streamlines with external tools.

### [Tracts][Save Tracts (Misc)...]

Exports transformed streamlines, endpoint coordinates, or track-density volumes for the highlighted bundle.

| Menu item | Behavior |
|---|---|
| **Save Current Tract In Current Slice Space...** | Transforms the **highlighted** bundle onto the active slice's voxel grid using its current registration, resamples streamlines at `0.5`-voxel steps, and saves in any supported tract format. |
| **Save Current Tract in Template Space...** | Warps the **highlighted** bundle into template voxel space (computing nonlinear subject-to-template registration if needed) and saves it in any supported tract format. |
| **Save Current Tract in MNI Coordinates...** | Warps the **highlighted** bundle into MNI millimeter coordinates (computing nonlinear registration if needed) and saves it; choose `.txt` or `.mat` when scripts require raw MNI `mm` coordinates. |
| **Save Endpoints As...** | Exports the start and end voxel coordinates of each visible streamline in the **highlighted** bundle to a space-separated `.txt` file or MATLAB `.mat` file in host FIB space. |
| **Save Endpoints In Current Slice Space...** | Transforms the **highlighted** bundle onto the active slice's voxel grid and exports each streamline's start and end voxel coordinates to `.txt` or `.mat`. |
| **Save Endpoints in MNI Coordinates...** | Warps the start and end points of each visible streamline in the **highlighted** bundle into MNI millimeter coordinates (computing nonlinear registration if needed) and saves them to `.txt` or `.mat`. |
| **Save Tract Density Images...** | Exports a 3D NIfTI track-density volume (`.nii.gz` or `.nii`) on the active slice's voxel grid counting streamlines per voxel for the **highlighted** bundle. Select the target diffusion or anatomical slice first to set the output grid. |
| **Save Tract Density Images(X2)...** | Exports a 2× super-resolution 3D NIfTI track-density volume (`.nii.gz` or `.nii`) for the **highlighted** bundle in native diffusion space at half the native voxel spacing. |

### [Tracts][Save Along Tract Index...]

Lists one dynamic entry for each available fiber metric and loaded slice volume (such as `qa...`, `dti_fa...`, `md...`, or inserted T1w/T2w volumes).

| Menu item | Behavior |
|---|---|
| **`<metric>...`** | Samples the chosen metric at every vertex along the **highlighted** bundle and exports per-vertex values to `.txt`, `.mat`, or `.trk`/`.trk.gz`. Do not save as `.tt.gz`, which omits per-vertex scalar values. |

## Manage tract bundles

| Menu item | Behavior |
|---|---|
| **Check/Uncheck All** | Shortcut: `Ctrl+Alt+A`. Checks all rows in the **Tracts** list when the first row is unchecked, or unchecks all rows when the first row is checked. |
| **Separate Deleted Tracts** | Shortcut: `Ctrl+Alt+S`. Moves streamlines from the **highlighted** bundle's deleted buffer (from trimming, cutting, or ROI filtering) into a new unchecked bundle row and clears the deleted buffer. |
| **Copy Tracts** | Shortcut: `Ctrl+Alt+C`. Duplicates the **highlighted** bundle—including visible streamlines, colors, and metadata—into a new checked row named `<name>_copy`. |
| **Merge All** | Shortcut: `Ctrl+Alt+M`. Combines all **checked** bundles (and their undo history) into the first checked row and removes the remaining checked rows; requires at least 2 checked bundles. |
| **Delete** | Shortcut: `Ctrl+Alt+Backspace`. Permanently removes the **highlighted** bundle row from the **Tracts** list (cannot be undone). |
| **Delete All** | Permanently removes every bundle row from the **Tracts** list (cannot be undone). |

## Tract statistics

| Menu item | Behavior |
|---|---|
| **Tract Profile...** | Plots smoothed along-tract metric curves (with optional 95% confidence intervals) across all **checked** bundles by coordinate axis (`X`/`Y`/`Z`) or normalized pathway position, with `.txt` data and `.png`/`.jpg`/`.bmp`/`.pdf` chart export. |
| **Statistics...** | Computes per-bundle geometry (count, length and span in `mm`, curl, volume in `mm³`, surface area) and mean fiber or slice metrics across all **checked** bundles, with clipboard copy and `.txt` export. |
| **Tract-Atlas Intersection Statistics...** | Voxelizes the **highlighted** bundle in diffusion space and reports overlap statistics across intersected parcels of a chosen built-in atlas (exportable as `.txt`) without adding regions to **Assign Regions**. |

Streamline count and voxelized bundle volume depend on seeding density, tracking thresholds, and resolution, so do not interpret them as axon count or direct biological connection strength. For spatial variation along a pathway, see [AutoTrack and Tractometry](/doc/gui_t3_atk.html#tractometry).

## Convert tracts to regions

| Menu item | Behavior |
|---|---|
| **Tracts To Region** | Voxelizes all visible streamlines of the **highlighted** bundle onto the active slice's voxel grid as a new checked ROI in **Assign Regions**. |
| **Endpoints To Region** | Orients streamlines in the **highlighted** bundle and creates two checked endpoint ROIs (`<name> endpoints1` and `<name> endpoints2`) on the active slice's voxel grid in **Assign Regions**. |
| **Filter Tracks by ROI/ROA/End** | Filters all **checked** bundles against checked regions in **Assign Regions** using each region's tracking role, moving excluded streamlines to the deleted buffer (undoable, or recoverable via **Tracts › Separate Deleted Tracts**). |

Select the intended diffusion or registered anatomical volume in the slice selector dropdown before converting tracts or endpoints to regions, and see the [Regions menu](/doc/menu_regions.html) for region editing and export.

## Editing and pruning

Interactive 3D selection, cutting, painting, undo/redo, and Topology-Informed Pruning (**[Tract Edit][Trim]**) are in the [Tract Edit menu](/doc/menu_edit.html); apply pruning to coherent individual bundles rather than whole-brain tractograms. Automated cleanup tools—including removing branches or short/duplicate streamlines, reconnecting, and resampling—are in the [Tracts Misc menu](/doc/menu_tracts_misc.html).

## Clustering and bundle recognition

Unsupervised streamline clustering and bundle sorting tools are in the [Tracts Cluster menu](/doc/menu_tracts_cluster.html), while atlas-based bundle recognition and cluster splitting are in the [Tracts Misc menu](/doc/menu_tracts_misc.html). Treat atlas recognition labels as anatomical classification aids rather than proof of biological identity, and prefer [AutoTrack](/doc/gui_t3_atk.html) for direct atlas-guided pathway mapping.

## Track-density imaging

Track-density exports under **[Tracts][Save Tracts (Misc)...]** count each streamline at most once per voxel on either the active slice grid or a 2× native diffusion grid. Because track-density intensity depends on seeding density and tracking parameters, do not interpret it as a direct quantitative measure of axon density.

## Connectivity matrices

| Menu item | Behavior |
|---|---|
| **Connectivity matrix...** | Computes a region-to-region connectivity matrix from all **checked** bundles across checked ROIs or a built-in atlas using endpoint or pass-through criteria, with count, length, or scalar weighting and `.mat`, `.png`/`.tif`/`.bmp`, and `.txt` network or Circos exports. |
| **Visualize Graph...** | Opens a square connectivity matrix from a `.mat` file (automatically loading its stored atlas parcellation when named) or a whitespace-delimited `.txt` file (matching regions currently in **Assign Regions**) for 3D connectome graph rendering. |

Connectivity values depend strongly on parcellation choice, endpoint versus pass-through criteria, metric weighting, and thresholding; see [Connectome (R2R / T2R)](/doc/gui_connectome.html) for guidance.

## Workspaces

Checked tract bundles can be saved and restored together with regions, devices, slices, and 3D view settings via **[File][Save Workspace...]** and **[File][Load Workspace...]**; see the [File menu](/doc/menu_file.html) and [Slices menu](/doc/menu_slices.html#workspaces).
