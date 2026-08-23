# Step T3: Whole-Brain Tractography

Open an `.fz` file in **Step T3: Fiber Tracking** and click **[Step T3d: Tracts][Fiber Tracking]** to generate a whole-brain tractogram.

Whole-brain tracking is the first quality check before ROI tracking, AutoTrack, differential tractography, or tract-based quantitative analysis. Major pathways should look anatomically plausible before more restrictive analyses are attempted.

![image](https://user-images.githubusercontent.com/275569/147802869-07d8b7e9-aea9-4fc3-8af5-29286787841f.png)

## Troubleshooting

| Problem | First things to check |
|:--|:--|
| **No or very few tracts** | Reduce **Min Length** and check whether the tracking threshold is too restrictive. Verify the mask, reconstruction, and b-table orientation. |
| **Many short fragments** | Increase **Min Length** and check data quality/noise. |
| **Tracts stop prematurely** | Check the tracking threshold and whether the diffusion signal supports continuation toward the expected anatomy. |
| **Odd or noisy anatomy** | Increase the tracking threshold only if appropriate, then re-check acquisition quality, motion/distortion, b-table orientation, and reconstruction. |

If major pathways remain anatomically incorrect, return to [Step T2: Reconstruction](/doc/gui_t2.html) rather than adding ROI/ROA constraints to force a desired result.

## Tracking parameters

The current defaults are designed to scale with the loaded FIB/template where appropriate. For reproducible research, report any values that you explicitly change.

| Parameter | Current behavior / guidance |
|:--|:--|
| **Tracking Index** | Selects the diffusion index used for the tracking threshold. QA is commonly used for GQI/QSDR data; tensor-based datasets may use FA. |
| **Tracking Threshold** | `0` uses a randomized threshold around the current Otsu-derived default. Specify a nonzero threshold only when a fixed value is needed. Lower values increase sensitivity but can admit spurious fibers. |
| **Angular Threshold** | `0` randomizes the angular threshold between **45° and 90°**. Specify a degree value to use a fixed angular threshold. |
| **Step Size** | `0` uses the **voxel spacing**. Specify a positive value in millimeters to use a fixed step size. |
| **Min Length** | Derived from the selected template/image scale. Increase it to remove short fragments; reduce it only when anatomically short pathways are expected. |
| **Max Length** | Derived from the selected template/image scale. |
| **Seed / Tract Count** | If neither is explicitly fixed, DSI Studio uses the track-to-voxel ratio to determine the amount of tractography. Fix the relevant count when the study requires the same stopping rule across subjects. |
| **Track/Voxel Ratio** | Derived from FIB voxel size and template scale. A higher ratio generates more streamlines and requires more computation. |
| **Smoothing** | `0` disables trajectory smoothing. Use only when the analysis specifically requires it. |
| **Check Ending** | Removes tracks that appear to terminate within high-anisotropy white matter when enabled. It is not required for ordinary whole-brain tracking. |
| **Topology-Informed Pruning (TIP)** | Use primarily on sufficiently dense, coherent tract bundles. It is not required for routine whole-brain quality-control tracking. |

The exact settings used for a tractogram are recorded in the tractography report/parameter code.

For command-line equivalents, see [Fiber Tracking CLI](/doc/cli_t3.html).

## Saving tractography

Use **[Tracts][Save Tracts As]** to save the selected tract group. Use **[Tracts][Save All Tracts As]** when multiple loaded/segmented tract groups should be saved together.

DSI Studio's current compact tractography format is `.tt.gz`. Other formats can be exported when interoperability with other software is needed.

# Connectivity matrices

DSI Studio can derive region-to-region (R2R) connectivity matrices from tractography. Structural-connectivity matrices are sensitive to acquisition, tracking, parcellation, and counting choices, so keep those choices consistent across subjects and interpret graph measures with appropriate methodological caution.

## GUI workflow

1. Generate and inspect the tractography used for the connectome.
2. Open **[Tracts][Connectivity Matrix]**.
3. Select a built-in parcellation atlas or use loaded ROIs.
4. Select whether connectivity is defined by streamline **endpoints** or **pass-through** regions.
5. Select the matrix value(s) and recalculate.
6. Export the matrix, connectogram, or network measures as needed.

Do not use a white-matter tract atlas as the node parcellation for a conventional region-to-region connectome.

## Important connectivity choices

| Setting | Meaning |
|:--|:--|
| **Parcellation Atlas** | Defines the nodes of the region-to-region network. |
| **End / Pass** | `end` assigns connections according to streamline endpoints; `pass` includes regions traversed by the streamline. |
| **Connectivity Value** | Selects tract count or available tract/diffusion-derived measurements used as matrix entries. |
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

# Network measures and graph visualization

Network measures can be calculated from a connectivity matrix in the connectivity interface. When comparing subjects or groups, use the same parcellation, tractography strategy, connectivity definition, matrix value, and thresholding rule for every dataset.

Saved connectivity matrices can also be visualized in the tracking window using the graph-visualization functions.

# Related workflows

- [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html)
- [Automatic Fiber Tracking (AutoTrack)](/doc/gui_t3_atk.html)
- [Differential Tractography](/doc/gui_t3_dt.html)
- [Correlational Tractography](/doc/gui_cx.html)
