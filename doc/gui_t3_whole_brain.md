# Whole-Brain Tractography

> **Starting here?** First [import diffusion data](/doc/gui_t1.html) to create `.sz`, run [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control), then [reconstruct SRC to FIB](/doc/gui_t2.html) to obtain `.fz`. This page begins with that FIB file; inspect whole-brain tracking before moving to [ROI tracking](/doc/gui_t3_roi_tracking.html), [AutoTrack](/doc/gui_t3_atk.html), or [Connectome](/doc/gui_connectome.html).

From the main window, select **Tractography → 3 Track → FIB File (*.fz)** to open an `.fz` file in the fiber tracking window, then click **Tracts → Fiber Tracking** to generate a whole-brain tractogram.

Whole-brain tracking is the first quality check before ROI tracking, AutoTrack, differential tractography, or tract-based quantitative analysis. Major pathways should look anatomically plausible before more restrictive analyses are attempted.

A streamline is a computational trajectory reconstructed from diffusion orientations, not an observed axon. Streamline/tract count should not be interpreted directly as axon count or biological connection strength.

![Whole-brain streamline tractography illustration](https://user-images.githubusercontent.com/275569/147802869-07d8b7e9-aea9-4fc3-8af5-29286787841f.png)

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

Use **Tracts → Save Current Tract As...** to save the selected tract group. Use **Tracts → Save All Tracts As** when multiple loaded/segmented tract groups should be saved together.

DSI Studio's current compact tractography format is `.tt.gz`. Other formats can be exported when interoperability with other software is needed.

<span id="connectivity-matrices"></span>

## Connectome analysis

To generate **region-to-region (R2R)** or **tract-to-region (T2R)** connectivity matrices from tractography, follow the dedicated [Connectome guide](/doc/gui_connectome.html). Whole-brain tracking is a useful quality check before constructing a connectivity matrix.

## Related workflows

- [Connectome: R2R and T2R](/doc/gui_connectome.html)

- [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html)
- [Automatic Fiber Tracking (AutoTrack)](/doc/gui_t3_atk.html)
- [Differential Tractography](/doc/gui_t3_dt.html)
- [Correlational Tractography](/doc/gui_cx.html)
