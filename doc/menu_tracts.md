# [Tracts] Menu

The **[Tracts]** menu in the Fiber Tracking window (also opened by right-clicking anywhere inside the **Tracts** bundle list) imports and exports streamline files across native, structural-slice, template, and MNI spaces, organizes loaded bundles, converts streamlines and endpoints into regions, filters pathways against regions of interest, and computes quantitative tract statistics, along-tract profiles, track-density volumes, and structural connectivity matrices.

Each row in the **Tracts** dock displays its **Name** (with a checkbox controlling 2D slice overlay, 3D rendering visibility, and inclusion in multi-bundle operations; double-click a name to rename it in place), **Tracts** (visible streamline count), **Deleted** (streamlines held in the bundle's deleted buffer), and **Seeds** (evaluated seed count during tracking). Single-bundle menu commands—such as **[Tracts][Save Current Tract As...]**, items under **[Tracts][Save Tracts (Misc)...]** and **[Tracts][Save Along Tract Index...]**, **[Tracts][Separate Deleted Tracts]**, **[Tracts][Copy Tracts]**, **[Tracts][Delete]**, **[Tracts][Tracts To Region]**, **[Tracts][Endpoints To Region]**, and **[Tracts][Tract-Atlas Intersection Statistics...]**—operate on the currently highlighted row, whereas multi-bundle commands—such as **[Tracts][Save All Tracts As ...]**, **[Tracts][Save All Tracts As Multiple Files...]**, **[Tracts][Merge All]**, **[Tracts][Filter Tracks by ROI/ROA/End]**, **[Tracts][Tract Profile...]**, **[Tracts][Statistics...]**, and **[Tracts][Connectivity matrix...]**—operate on all checked rows.

## Open and save tractography

| Menu item | Behavior |
|---|---|
| **Open Tracts...** | Opens a multi-file dialog for `.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` streamline files in the host FIB space. When a file's header `srow` affine transform differs from the host FIB grid, DSI Studio applies a linear coordinate transform to align streamlines to the host diffusion grid (matching dimensions and voxel spacing for legacy headers without an `srow` matrix), discards degenerate single-point entries, and splits multi-cluster files into separate bundle rows (reading bundle labels from a companion `<file>.txt` file when present, or naming rows `cluster0`, `cluster1`, and so on). Selecting 1–7 files checks the loaded bundles automatically; selecting more than 7 files loads them unchecked to keep initial 3D rendering responsive. |
| **Open Template-Space Tracts...** | Opens `.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` streamline files stored in template/MNI space inside a native-space FIB session. Computes subject-to-template nonlinear spatial normalization if not already initialized, linearly aligns mismatched template `srow` headers when needed, and nonlinearly warps streamline vertices from template space into subject native diffusion voxel space (applying the same >7-file unchecked rule when opening multiple files at once). |
| **Load Built-In Atlas...** | Loads reference pathways from DSI Studio's built-in population tractography atlas and nonlinearly warps them into subject native voxel space (or displays them directly when the session FIB is already in MNI space), appending atlas provenance to the methods report. Loads every atlas pathway matching the current **Autotrack...** target selector in the **Tracts** dock, or loads all atlas bundles as separate rows when the target selector is set to `All`. |
| **Save Current Tract As...** | Exports the currently highlighted bundle in host FIB space. Supports `.tt.gz` (compact TinyTrack delta encoding; used automatically if an unrecognized extension is entered), `.trk` and `.trk.gz` (TrackVis millimeter coordinates with `vox_to_ras`), `.tck` (MRtrix 32-bit float world millimeter coordinates), `.trx` (preserving loaded per-streamline scalar values when present), `.txt` (one streamline per line as space-separated `x y z ...` voxel coordinates), `.mat` (MATLAB `tracts` `3 × M` coordinate array and per-streamline vertex `length` vector), and `.nii` or `.nii.gz` (binary 3D voxelized tract mask on the host FIB grid). |
| **Save All Tracts As ...** | Combines all **checked** bundles into a single output file. Supports `.tt.gz` and `.trk` or `.trk.gz` (preserving per-streamline cluster/bundle IDs and writing a companion `<file>.txt` file listing the checked bundle names one per line), `.trx` (combining streamlines and preserving per-streamline scalar values when present across all checked bundles), `.txt` (alternating streamline coordinate lines with 0-based bundle-index lines), `.mat` (`tracts`, `length`, and 0-based `cluster` arrays), and `.nii.gz` (exporting a multi-bundle probability density image). |
| **Save All Tracts As Multiple Files...** | Prompts for an output folder and writes each **checked** bundle to `<folder>/<bundle_name><ext>` using the session's active default tract format extension (`.tt.gz`, `.trk.gz`, `.txt`, or `.trx` configured in the **Options** panel). Ensure checked bundles have distinct names so files do not overwrite one another in the destination folder. |

DSI Studio's native compact tract format is `.tt.gz` (1/32-voxel integer delta encoding). Use `.trk`/`.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` primarily when exchanging streamlines with external neuroimaging tools or custom analysis scripts.

### [Tracts][Save Tracts (Misc)...]

The **[Tracts][Save Tracts (Misc)...]** submenu exports spatially transformed streamlines, terminal endpoint coordinates, and track-density volumes for the currently highlighted bundle:

| Menu item | Behavior |
|---|---|
| **Save Current Tract In Current Slice Space...** | Transforms the highlighted bundle from diffusion space onto the currently selected slice's voxel grid using that slice's active registration transform, resamples streamlines at `0.5`-voxel steps, and saves in any format supported by **Tracts › Save Current Tract As...** (default filename `<bundle>_<slice><ext>`). |
| **Save Current Tract in Template Space...** | Computes subject-to-template nonlinear normalization if needed, warps streamline vertices of the highlighted bundle from subject native voxel space into template voxel coordinates, resamples at half the template voxel spacing, and exports in any supported tract format (default filename `<bundle>_template<ext>`). |
| **Save Current Tract in MNI Coordinates...** | Computes subject-to-template nonlinear normalization if needed, warps streamline vertices of the highlighted bundle into MNI millimeter coordinates, resamples at half the template voxel spacing, and saves the transformed coordinates (default filename `<bundle>_mni<ext>`; choose `.txt` or `.mat` when downstream scripts expect raw MNI millimeter coordinates directly). |
| **Save Endpoints As...** | Exports the first and last 3D vertex coordinates (`x1 y1 z1 x2 y2 z2`) of every visible streamline in the highlighted bundle in host FIB voxel space to `.txt` (space-separated floats) or `.mat` (`end_points`, a `3 × 2N` float matrix for `N` streamlines). |
| **Save Endpoints In Current Slice Space...** | Transforms the highlighted bundle onto the currently selected slice's voxel grid, resamples at `0.5`-voxel steps, and writes the first and last voxel coordinates of each streamline to `.txt` (space-separated floats) or `.mat` (`end_points`, `3 × 2N`). |
| **Save Endpoints in MNI Coordinates...** | Computes subject-to-template nonlinear normalization if needed, maps the two terminal vertices of each visible streamline in the highlighted bundle directly into MNI millimeter coordinates, and writes them to `.txt` (space-separated floats) or `.mat` (`end_points`, `3 × 2N`). |
| **Save Tract Density Images...** | Exports a 3D unsigned-integer NIfTI track-density volume (`.nii.gz` or `.nii`, default `<bundle>_tdi.nii.gz`) on the **currently selected slice's voxel grid and `srow` header**, counting how many streamlines from the highlighted bundle pass through each voxel. Select the target diffusion or registered T1w/T2w slice first to control the output grid. |
| **Save Tract Density Images(X2)...** | Exports a 2× super-resolution 3D unsigned-integer NIfTI track-density volume (`.nii.gz` or `.nii`) in **native diffusion space** with doubled matrix dimensions (`dim × 2`), halved voxel spacing (`voxel_size / 2`), and a correspondingly scaled NIfTI `srow` header. |

### [Tracts][Save Along Tract Index...]

The **[Tracts][Save Along Tract Index...]** submenu is populated dynamically with one entry for every diffusion fiber metric and inserted slice volume available in the session (such as `qa...`, `iso...`, `dti_fa...`, `md...`, `ad...`, `rd...`, and added T1w/T2w or multimodal volumes):

| Menu item | Behavior |
|---|---|
| **`<metric>...`** | Samples the chosen metric at every vertex along the highlighted bundle—using fiber-resolved directional lookup for anisotropic fiber metrics and trilinear interpolation for isotropic or inserted scalar slices—and saves per-vertex values to `.txt` (one line per streamline of space-separated values), `.mat` (`data` value vector, per-streamline `length` vector, and `trans` 4×4 matrix), or `.trk` / `.trk.gz` (TrackVis streamlines with attached per-vertex scalars). Save as `.txt`, `.mat`, or `.trk.gz` rather than `.tt.gz`, because `.tt.gz` stores streamline geometry without per-vertex scalar arrays. |

## Manage tract bundles

| Menu item | Behavior |
|---|---|
| **Check/Uncheck All** | Shortcut: `Ctrl+Alt+A`. Toggles visibility checkboxes across every row in the **Tracts** table: checks all rows when the first row is currently unchecked, or unchecks all rows when the first row is checked. |
| **Separate Deleted Tracts** | Shortcut: `Ctrl+Alt+S`. Moves all streamlines currently stored in the highlighted bundle's deleted buffer (from trimming, cutting, ROI filtering, or manual deletion) into a new unchecked bundle row with the same name and clears the original bundle's deleted buffer. |
| **Copy Tracts** | Shortcut: `Ctrl+Alt+C`. Duplicates the highlighted bundle—including its visible streamlines, colors, tags, and coordinate metadata—into a new checked row named `<name>_copy`. |
| **Merge All** | Shortcut: `Ctrl+Alt+M`. Requires at least two checked bundles. Combines the visible and deleted streamlines (plus undo history) of all **checked** bundles into the first checked row and removes the remaining checked rows from the **Tracts** table. |
| **Delete** | Shortcut: `Ctrl+Alt+Backspace`. Permanently removes the currently highlighted bundle row from the **Tracts** table. |
| **Delete All** | Clears every bundle row from the **Tracts** table. |

## Tract statistics

| Menu item | Behavior |
|---|---|
| **Tract Profile...** | Opens the interactive **Tract Profile** dialog plotting Gaussian-weighted (`bandwidth` `0.1`–`10.0`, default `1.0`) along-tract metric curves across all **checked** bundles (auto-assigning distinct colors when bundles share an identical color). Supports spatial coordinate axes (`X`, `Y`, or `Z` at `0.5`-voxel bins) or oriented pathway progression (`left`, `posterior`, `superior`, `right`, `anterior`, or `inferior` first across `100` normalized bins), configurable line width, optional 95% confidence-interval curves, tab-delimited `.txt` profile export, and `.jpg`, `.bmp`, `.pdf`, or `.png` chart export. |
| **Statistics...** | Computes quantitative geometry and scalar microstructure summaries for every **checked** bundle and displays them in a **Tract Statistics** report dialog (default save filename `<stem>_tract_stat.txt`, with copy-to-clipboard and tab-delimited text export). |
| **Tract-Atlas Intersection Statistics...** | Voxelizes the currently highlighted bundle in diffusion space, prompts you to select a built-in parcellation atlas, and opens a **Tract-Atlas Intersection Statistics** dialog (default save filename `<stem>_tract_overlap_stat.txt`) reporting region statistics for every nonempty atlas parcel intersection without adding temporary rows to the **Assign Regions** table. |

**[Tracts][Statistics...]** reports per-bundle geometry and scalar measurements depending on the loaded FIB and slice volumes:

- streamline count (`number of tracts`), mean and median streamline length (mm), span (mm), curl, and elongation;
- 2×-subvoxel voxelized bundle volume (`total`, `LPS quarter`, `2nd and 3rd quarter`, and `RAI quarter` in mm³), trunk volume, surface area, irregularity, and LPS/RAI endpoint areas and radii;
- mean values of diffusion fiber metrics (such as QA, ISO, FA, MD, AD, and RD) and inserted scalar slices across the whole bundle plus `LPS quarter` and `RAI quarter` segments, as well as per-subject `<subject> mean_<metric>` rows when a connectometry database FIB is open.

Streamline count and voxelized bundle volume depend on seeding density, tracking thresholds, image resolution, and registration; do not interpret streamline count as axon count or direct biological connection strength, and treat longitudinal volume changes cautiously unless acquisition, reconstruction, registration, and tracking settings are strictly matched. To examine spatial variation along a pathway, use **[Tracts][Tract Profile...]**; see [AutoTrack and Tractometry](/doc/gui_t3_atk.html#tractometry).

## Convert tracts to regions

| Menu item | Behavior |
|---|---|
| **Tracts To Region** | Voxelizes all visible streamline trajectories of the currently highlighted bundle (using `0.5`-voxel step interpolation) into a new checked ROI row in the **Assign Regions** table on the **currently selected slice's voxel grid**. |
| **Endpoints To Region** | Consistently orients streamlines in the highlighted bundle and creates two new checked ROI rows—`<name> endpoints1` and `<name> endpoints2`—in the **Assign Regions** table on the **currently selected slice's voxel grid**. |
| **Filter Tracks by ROI/ROA/End** | Filters every **checked** bundle against all currently checked regions in the **Assign Regions** table using each region's assigned tracking role (`ROI`, `ROA`, `End`, `Seed`, `Terminative`, `NotEnd`, or `Limiting`), moving excluded streamlines into each bundle's deleted buffer so the operation can be undone or inspected with **Tracts › Separate Deleted Tracts**. |

Because **[Tracts][Tracts To Region]** and **[Tracts][Endpoints To Region]** voxelize streamlines onto the active slice grid, select the intended diffusion or registered structural slice in the slice selector dropdown before converting when voxel resolution or grid alignment matters (and use tract-derived region volumes primarily as within-session denominators rather than absolute cross-session volume measures). See the [Regions menu](/doc/menu_regions.html) for region editing and export tools.

## Editing and pruning

Interactive 3D streamline selection, cutting, painting, undo/redo, and Topology-Informed Pruning (TIP, **[Tract Edit][Trim]**, `Ctrl+T`) are located in **[Tract Edit]**; see the [Tract Edit menu](/doc/menu_edit.html). TIP removes isolated or weakly supported streamline branches on sufficiently dense, visually coherent bundles and should not be applied routinely to whole-brain tractograms. Pruned or cut streamlines remain in the bundle's deleted buffer until cleared or extracted via **[Tracts][Separate Deleted Tracts]**.

Additional automated bundle cleanup and organization tools include **[Tracts Misc][Delete Branches]**, **[Tracts Misc][Delete Repeated Tracks]**, **[Tracts Misc][Delete Short Tracts]**, **[Tracts Misc][Merge Tracts by Name]**, **[Tracts Misc][Reconnect Tracts]**, and **[Tracts Misc][Resample Step Size]** in the [Tracts Misc menu](/doc/menu_tracts_misc.html), alongside **[Tracts Cluster][Sort Tracts by Name]** in the [Tracts Cluster menu](/doc/menu_tracts_cluster.html).

## Clustering and bundle recognition

Unsupervised streamline clustering and bundle-list color/sorting tools reside under **[Tracts Cluster]** (see the [Tracts Cluster menu](/doc/menu_tracts_cluster.html)), and atlas-based bundle recognition, renaming, and cluster splitting (**[Tracts Misc][Recognize...]**, **[Tracts Misc][Recognize and Rename]**, and **[Tracts Misc][Recognize and Cluster]**) reside under **[Tracts Misc]** (see the [Tracts Misc menu](/doc/menu_tracts_misc.html)), whereas **[Tracts][Load Built-In Atlas...]** loads reference population atlas pathways directly. Atlas recognition assigns the **best-matching reference label** as an anatomical classification aid rather than proof of biological identity; always inspect bundles visually against anatomy and report uncertain, cross-midline, or partial pathways conservatively.

For direct atlas-guided mapping of named pathways from diffusion data, [AutoTrack](/doc/gui_t3_atk.html) is generally the preferred workflow.

## Track-density imaging

**[Tracts][Save Tracts (Misc)...][Save Tract Density Images...]** exports a 3D NIfTI density volume aligned to the currently selected slice grid, whereas **[Tracts][Save Tracts (Misc)...][Save Tract Density Images(X2)...]** exports a 2× super-resolution volume in native diffusion space. Each streamline is counted at most once per voxel regardless of step size. Because TDI intensity reflects streamline sampling density and tracking parameters, it should not be interpreted as a direct quantitative measure of axon density.

## Connectivity matrices

| Menu item | Behavior |
|---|---|
| **Connectivity matrix...** | Combines all **checked** bundles and opens the **Connectivity Matrix** dialog to compute region-to-region connectivity across either currently checked `ROIs` in the **Assign Regions** table or a chosen built-in parcellation atlas (defaulting to checked `ROIs` when present, or `HCP-MMP` / the first built-in atlas otherwise). Supports **End** (streamlines ending in both regions) or **Pass** (streamlines passing through regions) criteria and lets you switch matrix values across count, length, and loaded scalar metrics, inspect interactive region-pair hover values and network measures, copy the matrix to the clipboard, or export `.mat` files (`connectivity`, metric matrices, `name`, `atlas`), `.png`/`.tif`/`.bmp` matrix images, Circos connectograms (`*_cg.txt`), and binary/weighted graph network property reports (`*_net.txt`). |
| **Visualize Graph...** | Opens a square connectivity matrix from a `.mat` file (reading the `connectivity` matrix; if a built-in `atlas` name is stored in the `.mat` file, DSI Studio automatically replaces current regions with that atlas parcellation and enables 3D region-graph rendering) or a whitespace-delimited `.txt` matrix file (requiring a matching number of regions already loaded in the **Assign Regions** table) for 3D connectome graph visualization. |

Connectivity values depend strongly on parcellation choice, endpoint versus pass-through criteria, metric weighting, and thresholding. See [Whole-Brain Tractography](/doc/gui_t3_whole_brain.html#connectivity-matrices) for workflow guidance and interpretation cautions.

## Workspaces

Loaded tract bundles can be saved and restored together with regions, devices, inserted slices, slice positions, and 3D camera/rendering settings via **[File][Save Workspace...]** and **[File][Load Workspace...]**; see the [File menu](/doc/menu_file.html) and [Slices menu](/doc/menu_slices.html#workspaces).
