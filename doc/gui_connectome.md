# Connectome: Region-to-Region (R2R) and Tract-to-Region (T2R)

> **Before building a connectome:** [Import diffusion data](/doc/gui_t1.html) → [reconstruct SRC to FIB](/doc/gui_t2.html) → [inspect whole-brain tracking](/doc/gui_t3_whole_brain.html). For **R2R**, generate/load suitable tracts and use a parcellation aligned with the FIB. For **T2R**, first map/load **named bundles** using [AutoTrack](/doc/gui_t3_atk.html) (or an equivalent tract set) and load an aligned parcellation. Both workflows begin with validated tractography rather than raw DWI.

DSI Studio supports two complementary structural connectome representations. **R2R** relates brain regions to other regions; **T2R** relates named white-matter pathways to parcellation regions. The two matrices answer different questions and should not be interpreted interchangeably.

<div class="ana-two" aria-label="Choose a connectome representation">
  <div><h3>R2R · Region to region</h3><p>A square matrix of region pairs, derived from streamlines that terminate in or traverse the selected regions.</p><p><a href="#region-to-region-r2r-connectome">Generate an R2R matrix →</a></p></div>
  <div><h3>T2R · Tract to region</h3><p>A tract-by-region representation that retains named pathway identities alongside parcellation regions.</p><p><a href="#tract-to-region-t2r-connectome">Generate a T2R matrix →</a></p></div>
</div>

Before creating either matrix, verify diffusion acquisition, b-table orientation, reconstruction, tractography quality, and atlas registration. Keep acquisition, parcellation, streamline selection, and counting or weighting rules consistent across subjects. **Streamline count is an algorithm-dependent measurement, not an axon count or direct biological connection strength.**

## Region-to-Region (R2R) Connectome

DSI Studio can derive region-to-region (R2R) connectivity matrices from tractography. Structural-connectivity matrices are sensitive to acquisition, tracking, parcellation, and counting choices, so keep those choices consistent across subjects and interpret graph measures with appropriate methodological caution.

### Generate an R2R matrix

1. Generate and inspect the [whole-brain tractography](/doc/gui_t3_whole_brain.html) used for the connectome.
2. Open <span class="gui-menu-path"><span class="gui-menu-item">Tracts</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Connectivity Matrix</span></span>.
3. Select a built-in parcellation atlas or use loaded ROIs.
4. Select whether connectivity is defined by streamline **endpoints** or **pass-through** regions.
5. Select the matrix value(s) and recalculate.
6. Export the matrix, connectogram, or network measures as needed.

Do not use a white-matter tract atlas as the node parcellation for a conventional region-to-region connectome.

### Important R2R choices

| Setting | Meaning |
|:--|:--|
| **Parcellation Atlas** | Defines the nodes of the region-to-region network. |
| **End / Pass** | `end` assigns connections according to streamline endpoints; `pass` includes regions traversed by the streamline. |
| **Connectivity Value** | Selects tract count or available tract/diffusion-derived measurements used as matrix entries. Streamline count is a tractography-dependent measure, not a direct axon count or connection-strength measurement. |
| **Threshold** | Filters weak matrix entries. Thresholding changes the resulting graph and should be reported. |

Command-line example:

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --connectivity=HCP-MMP \
  --connectivity_type=end \
  --connectivity_output=matrix,network
```

See [Fiber Tracking CLI](/doc/cli_t3.html) for the current connectivity options.

### Network measures and graph visualization

Network measures can be calculated from a connectivity matrix in the connectivity interface. When comparing subjects or groups, use the same parcellation, tractography strategy, connectivity definition, matrix value, and thresholding rule for every dataset.

Saved connectivity matrices can also be visualized in the tracking window using the graph-visualization functions.


## Tract-to-Region (T2R) Connectome

The **tract-to-region (T2R) connectome** represents connectivity as an *n × m* matrix, where rows are named white-matter tracts and columns are parcellation regions. This keeps the identity of the connecting white-matter pathway, unlike a conventional region-to-region matrix that summarizes connectivity only between region pairs.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7Ns9vHl21J8?si=GxfaKk8cz4FCpGMQ" title="Tract-to-region connectome tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Generate a T2R connectome

#### 1. Map named tract bundles

Use [AutoTrack](/doc/gui_t3_atk.html) to map the tract bundles included in the analysis. T2R is designed around identifiable pathways, so use named bundles rather than an undifferentiated whole-brain tractogram when the goal is an atlas-based T2R representation.

#### 2. Load a brain parcellation

Use <span class="gui-menu-path"><span class="gui-card">Assign Regions</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-action">Atlas...</span></span> to load the desired parcellation, such as HCP-MMP or another atlas appropriate for the study.

#### 3. Create the matrix

Use <span class="gui-menu-path"><span class="gui-menu-item">Regions</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Tract-to-Region Connectome</span></span> to calculate the tract-by-region matrix.

Keep the tract set and parcellation identical across subjects when comparing T2R matrices.


To repeat R2R or T2R processing across datasets, use [Records › Command History](/doc/cli_vis.html) to inspect the corresponding commands after completing a GUI workflow.

## Further resources

- [Whole-Brain Fiber Tracking](/doc/gui_t3_whole_brain.html) — inspect and save whole-brain tractography before constructing R2R.
- [AutoTrack](/doc/gui_t3_atk.html) — generate named tract bundles before constructing T2R.
- [How to Analyze dMRI](/doc/how_to_analyze_dmri.html#structural-connectome-r2r-and-t2r) — overview of the two connectome types.
- [Fiber Tracking / Connectome CLI](/doc/cli_t3.html) and [Region & Tract Analysis CLI](/doc/cli_ana.html) — batch output options.
- [DSI Studio File Formats](/doc/cli_data.html) — data and output formats.
