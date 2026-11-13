# [Tracts] Menu

The **[Tracts]** menu in the Step T3 tracking window (also opened by right-clicking the **Tracts** bundle list) manages streamline files, bundle-list organization, region conversion, ROI filtering, tract statistics, along-tract profiles, track-density exports, and structural connectivity.

DSI Studio exposes tract actions across three distinct layers:

1. **GUI menu path** (for example, **[Tracts][Open Tracts...]**): interactive menu selection in the Step T3 window.
2. **GUI / agent hint (`run ...`)**: the Qt `toolTip` command dispatched inside an open tracking window or DSI Studio AI session. A `run ...` string is an interactive GUI/agent hint, **not** a standalone `dsi_studio --action=...` CLI flag.
3. **Post-visualization `--action=vis --cmd=...`**: batch command-history tokens executed sequentially (`+` separates commands; `,` separates arguments). Dialog-only items without command-history handlers are marked `—`.

## Open and save tractography

| Menu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **Open Tracts...** | `run open_tract` | Opens `.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` tract files (`--cmd=open_tract,<file>[,0]`). Linearly aligns mismatched header `srow` transforms to the host FIB grid, splits multi-cluster files into separate bundle rows (reading `<file>.txt` bundle names if present), and loads unchecked when `,0` is appended or more than 7 files are selected in the GUI. |
| **Open Template-Space Tracts...** | `run open_mni_tract` | Loads template/MNI-space streamlines into a native-space FIB session (`--cmd=open_mni_tract,<file>[,0]`), running nonlinear normalization if needed and warping coordinates (`temp2sub`) into subject native voxel space. |
| **Load Built-In Atlas...** | `run load_tract_atlas` | Loads bundles from the built-in population tractography atlas and warps them into subject space (`--cmd=load_tract_atlas[,<tract_name>]`). In the GUI, loads the bundle chosen in the AutoTrack target selector (`All` loads every atlas bundle); in `--cmd`, loads case-insensitive substring matches for `<tract_name>` (or all bundles when omitted). |
| **Save Current Tract As...** | `run save_tract` | Exports the selected (or 0-based `<tract_index>`) bundle in host FIB space (`--cmd=save_tract,<file>[,<tract_index>]`). Supports `.tt.gz`, `.trk`, `.trk.gz`, `.tck` (world mm), `.trx`, `.txt` (`x y z ...` per line), `.mat` (`tracts`, `length`), and `.nii`/`.nii.gz` (binary voxelized mask). |
| **Save All Tracts As ...** | `run save_all_tracts` | Combines all **checked** bundles into one file (`--cmd=save_all_tracts,<file>`). Supports `.tt.gz` and `.trk`/`.trk.gz` (preserving cluster IDs and writing `<file>.txt` bundle labels), `.trx`, `.txt`, `.mat`, or `.nii.gz` (multi-bundle probability density volume). |
| **Save All Tracts As Multiple Files...** | `run save_all_tracts_to_folder` | Saves each **checked** bundle to `<folder>/<bundle_name><ext>` (`--cmd=save_all_tracts_to_folder,<folder>`) using the current `track_format` extension (`.tt.gz`, `.trk.gz`, `.txt`, or `.trx`). |

DSI Studio's native compact tract format is `.tt.gz` (1/32-voxel integer delta encoding). Use `.trk`/`.trk.gz`, `.tck`, `.trx`, `.txt`, or `.mat` only when interoperability with external software requires it.

### [Tracts][Save Tracts (Misc)...]

The **[Tracts][Save Tracts (Misc)...]** submenu exports transformed coordinates, streamline endpoints, and track-density volumes for the currently selected (or 0-based `<tract_index>`) bundle:

| Submenu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **Save Current Tract In Current Slice Space...** | `run save_slice_tract` | Transforms the bundle into the currently selected slice's voxel grid, resamples at `0.5`-voxel steps, and exports any supported tract format (`--cmd=save_slice_tract,<file>[,<tract_index>]`). |
| **Save Current Tract in Template Space...** | `run save_template_tract` | Nonlinearly maps streamlines (`sub2temp`) to template voxel space, resamples at half template voxel size, and exports (`--cmd=save_template_tract,<file>[,<tract_index>]`). |
| **Save Current Tract in MNI Coordinates...** | `run save_mni_tract` | Nonlinearly maps streamlines (`sub2mni`) to MNI millimeter coordinates, resamples, and exports—typically as `.txt` or `.mat` (`--cmd=save_mni_tract,<file>[,<tract_index>]`). |
| **Save Endpoints As...** | `run save_tract_endpoint` | Exports first and last 3D coordinates of each streamline in FIB voxel space to `.txt` (6 floats per line) or `.mat` (`end_points`, `3 × 2N` matrix) (`--cmd=save_tract_endpoint,<file>[,<tract_index>]`). |
| **Save Endpoints In Current Slice Space...** | `run save_slice_tract_endpoint` | Transforms streamlines into the active slice's voxel space and saves terminal coordinates to `.txt` or `.mat` (`--cmd=save_slice_tract_endpoint,<file>[,<tract_index>]`). |
| **Save Endpoints in MNI Coordinates...** | `run save_mni_tract_endpoint` | Nonlinearly maps streamline terminal coordinates (`sub2mni`) and saves MNI millimeter endpoints to `.txt` or `.mat` (`--cmd=save_mni_tract_endpoint,<file>[,<tract_index>]`). |
| **Save Tract Density Images...** | `run save_tdi` | Exports a 3D NIfTI (`.nii`/`.nii.gz`) track-density image in the **currently selected slice space** (`--cmd=save_tdi,<file>[,<tract_index>]`). See [Track-density imaging](#track-density-imaging). |
| **Save Tract Density Images(X2)...** | `run save_tdi2` | Exports a 2× subvoxel-resolution NIfTI TDI (`dim × 2`, `voxel_size / 2`) in **native diffusion space** (`--cmd=save_tdi2,<file>[,<tract_index>]`). |

### [Tracts][Save Along Tract Index...]

The **[Tracts][Save Along Tract Index...]** submenu is populated dynamically from every fiber metric and slice volume loaded in the session (such as `qa...`, `iso...`, `dti_fa...`, `md...`, `ad...`, `rd...`, and inserted T1w/T2w/modality slices):

| Dynamic submenu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **`<metric>...`** | `run save_tract_values` | Samples `<metric>` at each vertex of the selected/indexed bundle (using directional fiber-resolved interpolation for anisotropy/fiber metrics and trilinear interpolation for scalar slices) and writes `.txt` (one line per streamline), `.mat` (`data`, `length`, `trans`), or `.trk`/`.trk.gz` (TrackVis per-vertex scalar) (`--cmd=save_tract_values,<file>,<tract_index>,<metric>`). Use `.txt`, `.mat`, or `.trk.gz` rather than `.tt.gz` when saving per-vertex scalar values. |

## Manage tract bundles

| Menu item | Shortcut | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- | --- |
| **Check/Uncheck All** | `Ctrl+Alt+A` | `run check_uncheck_all_tract` | Checks all rows if row `0` is unchecked, or unchecks all rows otherwise (`--cmd=check_uncheck_all_tract`, `,1` to check all, `,0` to uncheck all). |
| **Separate Deleted Tracts** | `Ctrl+Alt+S` | `run separate_deleted_tract` | Moves streamlines from the selected/indexed bundle's deleted buffer into a new unchecked bundle row and clears the original deleted buffer (`--cmd=separate_deleted_tract[,<tract_index>]`). |
| **Copy Tracts** | `Ctrl+Alt+C` | `run copy_tract` | Duplicates the selected/indexed bundle as `<name>_copy` (`--cmd=copy_tract[,<tract_index>]`). |
| **Merge All** | `Ctrl+Alt+M` | `run merge_all_tracts` | Merges all **checked** bundles into the first checked row and deletes the remaining checked rows (`--cmd=merge_all_tracts`). |
| **Delete** | `Ctrl+Alt+Backspace` | `run delete_tract` | Removes the selected bundle row, or `&`-separated 0-based row indices in batch mode (`--cmd=delete_tract[,<index_or_&_list>]`). |
| **Delete All** | `—` | `run delete_all_tracts` | Removes all tract bundles from the list (`--cmd=delete_all_tracts`). |

## Tract statistics

| Menu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **Statistics...** | `run show_tract_statistics` | Computes quantitative shape and diffusion/slice metrics for every **checked** bundle (`--cmd=show_tract_statistics` opens the GUI table; `--cmd=save_tract_statistics,<file.txt>` writes a tab-delimited report directly). |
| **Tract-Atlas Intersection Statistics...** | `run show_tract_overlap_statistics` | Voxelizes the selected/indexed bundle and reports region statistics for all nonempty intersections with a chosen built-in parcellation atlas without adding temporary regions (`--cmd=show_tract_overlap_statistics,<tract_index>,<atlas_name>`). |
| **Tract Profile...** | `—` | GUI dialog only. Plots Gaussian-weighted (`bandwidth` `0.1`–`10.0`, default `1.0`) along-tract metric profiles across all **checked** bundles along `X`/`Y`/`Z` coordinates (`0.5`-voxel bins) or oriented progression (`left`, `posterior`, `superior`, `right`, `anterior`, or `inferior` first; `100` bins) with optional 95% confidence bounds; exports `.txt` data or `.png`/`.jpg`/`.bmp`/`.pdf` plots. |

**[Tracts][Statistics...]** reports per-bundle geometry and microstructure depending on loaded FIB/slice data:

- streamline count, mean/median length (mm), span (mm), curl, and elongation;
- 2×-subvoxel bundle volume (`total`, `LPS quarter`, `2nd and 3rd quarter`, `RAI quarter`), surface area, irregularity, and LPS/RAI endpoint areas and radii;
- mean fiber metrics (such as QA, ISO, FA, MD, AD, RD) and mean values from inserted slices across the whole bundle plus `LPS quarter` and `RAI quarter` segments (and per-subject means if a connectometry database is open).

Tract count and voxelized volume depend on seeding density and tracking parameters; do not interpret streamline count as axon count or direct biological connection strength. For spatial variation along a pathway, use **[Tracts][Tract Profile...]**; see [AutoTrack and Tractometry](/doc/gui_t3_atk.html#tractometry).

## Convert tracts to regions

| Menu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **Tracts To Region** | `run tract_to_region` | Voxelizes streamline trajectories of the selected/indexed bundle (`0.5`-voxel step interpolation) into a new ROI row in the **currently selected slice space** (`--cmd=tract_to_region[,<tract_index>]`). |
| **Endpoints To Region** | `run endpoint_to_region` | Consistently orients streamlines and creates `<name> endpoints1` and `<name> endpoints2` ROI rows in the **currently selected slice space** (`--cmd=endpoint_to_region[,<tract_index>]`). |
| **Filter Tracks by ROI/ROA/End** | `run filter_tract` | Filters **checked** bundles against currently checked regions in the Region list (`--cmd=filter_tract[,<roi_settings>[,<tract_indices>]]`), moving excluded streamlines to the deleted buffer. In `--cmd`, optional `<roi_settings>` overrides region roles using `&`-separated `region_index:type` pairs (`0`=ROI, `1`=ROA, `2`=End, `3`=Seed, `4`=Terminative, `5`=NotEnd, `6`=Limiting). |

Because **[Tracts][Tracts To Region]** and **[Tracts][Endpoints To Region]** voxelize onto the active slice grid, select the target anatomical or diffusion slice first when grid resolution matters. See the [Regions menu](/doc/menu_regions.html) for region editing and export operations.

## Editing and pruning

Interactive 3D streamline selection, cutting, painting, undo/redo, and Topology-Informed Pruning (TIP, **[Tract Edit][Trim]**, `Ctrl+T`, `run trim_tract`) are located in **[Tract Edit]**; see the [Tract Edit menu](/doc/menu_edit.html). TIP removes isolated or poorly supported streamline segments on sufficiently dense, coherent bundles and is not required for whole-brain tractograms. Removed streamlines remain in the bundle's deleted buffer until cleared or extracted with **[Tracts][Separate Deleted Tracts]**.

Additional automated bundle cleanup operations—including **[Tracts Misc][Delete Branches]** (`run delete_branch`), **[Tracts Misc][Delete Repeated Tracks]** (`run delete_repeated_tract`), **[Tracts Misc][Delete Short Tracts]** (`run delete_tract_by_length`), **[Tracts Misc][Merge Tracts by Name]**, **[Tracts Misc][Reconnect Tracts]**, and **[Tracts Misc][Resample Step Size]**—are located under **[Tracts Misc]**.

## Clustering and bundle recognition

Streamline clustering methods reside under **[Tracts Cluster]**, and atlas-based bundle recognition/labeling resides under **[Tracts Misc]**, whereas **[Tracts][Load Built-In Atlas...]** loads reference atlas bundles directly. Recognition assigns the **best-matching atlas label** as a classification aid rather than proof of anatomical identity; inspect bundles manually and report uncertain, cross-midline, or spurious components conservatively.

For direct atlas-guided mapping of named pathways, [AutoTrack](/doc/gui_t3_atk.html) is generally the preferred workflow.

## Track-density imaging

**[Tracts][Save Tracts (Misc)...][Save Tract Density Images...]** (`run save_tdi`) exports a 3D NIfTI density volume aligned to the active slice grid, while **[Tracts][Save Tracts (Misc)...][Save Tract Density Images(X2)...]** (`run save_tdi2`) exports a 2× super-resolution volume in native diffusion space. Each streamline is counted at most once per voxel. Because TDI reflects streamline sampling density and tracking settings, it should not be interpreted as a direct measure of axon density.

## Connectivity matrices

| Menu item | GUI hint (`toolTip`) | Behavior / `--cmd` |
| --- | --- | --- |
| **Connectivity matrix...** | `—` | GUI dialog only. Combines all **checked** bundles across checked `ROIs` or a built-in parcellation atlas using **End** (`end in region`) or **Pass** (`pass region`) criteria. Exports `.mat` files (`connectivity`, `<metric> r2r`, `<metric> t2r`, `name`, `atlas`), normalized matrix images, Circos connectograms (`*_cg.txt`), and binary/weighted graph network properties (`*_net.txt`). |
| **Visualize Graph...** | `run open_connectivity_matrix` | Opens a square connectivity matrix from `.mat` (`connectivity` array; auto-loads atlas regions if `atlas` is stored) or `.txt` (requires matching regions already loaded in the Region list) and enables 3D connectome graph rendering (`--cmd=open_connectivity_matrix,<file>`). |

Connectivity values depend on parcellation choice, endpoint vs pass-through rule, metric selection, and thresholding. See [Whole-Brain Tractography](/doc/gui_t3_whole_brain.html#connectivity-matrices) for workflow guidance and interpretation cautions.

## Workspaces

Loaded tracts can be saved and restored together with regions, devices, slices, and 3D camera/rendering settings via **[File][Save Workspace...]** (`run save_workspace`) and **[File][Load Workspace...]** (`run load_workspace`). See the [Slices menu](/doc/menu_slices.html#workspaces).

## Command-line equivalents

For standalone headless batch pipelines, use:

- [Fiber Tracking CLI](/doc/cli_t3.html) (`--action=trk`) for streamline generation, tract/TDI exports, profiles, and connectivity matrices;
- [Automatic Fiber Tracking CLI](/doc/cli_atk.html) (`--action=atk`) for atlas-guided tractography;
- [Region and Tract Analysis CLI](/doc/cli_ana.html) (`--action=ana`) for ROI filtering, tract statistics, and post-tracking exports on existing tract files;
- [Command History](/doc/cli_vis.html) (`--action=vis`) to replay tracking-window `--cmd` tokens (remembering that GUI `run ...` hints are interactive agent/UI hints, not standalone `--action=...` CLI flags):

```bash
dsi_studio --action=vis --source=sub01.fz \
  --cmd="open_tract,whole_brain.tt.gz+filter_tract,0:0&1:2+save_tract_statistics,bundle_stats.txt+save_tdi,bundle_tdi.nii.gz,0"
```
