# [Tracts] Menu

The **Tracts** menus manage tractography after tracks have been generated or loaded in Step T3. They provide saving, quantitative analysis, editing, clustering, recognition, and connectivity functions.

## Open and save tractography

Use the Tracts menu to:

- open tractography files;
- load tracts defined in template/MNI space when appropriate;
- save the selected tract group;
- save all loaded tract groups together or as separate files;
- export tract coordinates/endpoints in supported formats.

DSI Studio's current compact tract format is `.tt.gz`. Use another format only when interoperability with other software requires it.

## Tract statistics

**[Tracts][Statistics]** reports quantitative information for the selected tract group. Depending on the loaded FIB/images, this can include:

- tract count and length;
- tract shape/volume measurements;
- QA and other GQI measurements;
- DTI measurements such as FA, MD, AD, and RD;
- values sampled from additional images inserted into the tracking window.

Tract count is a property of the tractography sampling and tracking settings. It should not be interpreted as an axon count or direct biological connection strength.

For along-tract variation rather than one summary value, use **Tract Profile**. See [AutoTrack and Tractometry](/doc/gui_t3_atk.html#tractometry).

## Convert tracts to regions

Tractography can be converted to spatial regions, including tract coverage and endpoint regions. These regions can then be used for visualization, quantitative analysis, or as anatomically justified tracking constraints.

See the [[Regions] menu](/doc/menu_regions.html) for region operations.

## Editing and pruning

Interactive selection, deletion, cutting, painting, and trimming are documented under the [[Edit] menu](/doc/menu_edit.html).

Topology-informed pruning (TIP) removes isolated or poorly supported streamline segments. Use it primarily on sufficiently dense, coherent tract bundles; it is not a mandatory step for every whole-brain tractogram.

## Clustering and bundle recognition

Tractography can be grouped by clustering or recognized against a tractography atlas. Recognition assigns the **best-matching atlas label** to the tractography; it is a classification aid rather than definitive proof of anatomical identity. Inspect the bundle manually and report uncertain, cross-midline, or extra components conservatively.

For direct atlas-guided mapping of named bundles, [AutoTrack](/doc/gui_t3_atk.html) is usually the clearer workflow.

## Track-density imaging

Track-density images (TDI) can be exported from a tractogram in diffusion space or other supported mappings/resolutions. TDI reflects streamline sampling density and therefore depends on the tractography strategy and number of generated streamlines. It is not a direct measurement of axon density.

## Connectivity matrices

**[Tracts][Connectivity Matrix]** calculates region-to-region connectivity from the current tractography. The result depends on the parcellation, streamline definition, endpoint/pass-through rule, connectivity value, and threshold.

See [Whole-Brain Tractography](/doc/gui_t3_whole_brain.html#connectivity-matrices) for recommended workflow and interpretation cautions.

## Workspaces

Tracts can be saved together with regions, devices, slices, and rendering settings using **[View][Save Workspace]**. See the [[Slices] menu](/doc/menu_slices.html#workspaces).

## Command-line equivalents

For scripted workflows use:

- [Fiber Tracking CLI](/doc/cli_t3.html) for tracking, tract exports, profiles, and connectivity;
- [Automatic Fiber Tracking CLI](/doc/cli_atk.html) for named atlas-based bundles;
- [Region and Tract Analysis CLI](/doc/cli_ana.html) for analysis of existing tractography.

The GUI **Command History** can be used when you need the exact current command name for a particular GUI operation; see [Command History](/doc/cli_vis.html).
