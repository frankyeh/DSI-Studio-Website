# Fiber Tracking

Use `--action=trk` to perform deterministic whole-brain or ROI-based fiber tracking in DSI Studio (Yeh et al. PLoS ONE 8(11):e80713, 2013). Additionally, you can compute tract-to-region connectomes, tract statistics, track-density images, cluster or recognize known bundles, and even perform differential analysis—all from this single command.

For **automatic atlas-based bundle mapping**, see `--action=atk` instead:  
[Atlas-Based Tracking Documentation](https://dsi-studio.labsolver.org/doc/cli_atk.html)

---

## Examples

### Whole-brain deterministic tracking
```bash
dsi_studio --action=trk \
           --source=subject.fib.gz \
           --output=subject_all.tt.gz
```

### Seed-and-target ROI tracking
```bash
dsi_studio --action=trk \
           --source=subject.fib.gz \
           --seed=wholeBrain.nii.gz \
           --roi=regionA.nii.gz \
           --roi2=regionB.nii.gz \
           --output=streamlines.tt.gz
```

### Connectivity analysis (HCP-MMP & AAL3 atlases)
```bash
dsi_studio --action=trk \
           --source=subject.fib.gz \
           --output=no_file \
           --connectivity=HCP-MMP,AAL3 \
           --connectivity_value=count,qa
```

### Bundle-specific tracking by parameter ID
```bash
dsi_studio --action=trk \
           --source=subject.fib.gz \
           --parameter_id=c9A99193Fba3F2EFF013Fcb2041b96438813dcb \
           --output=subject_bundle.tt.gz
```

---

## Core Options

| **Option**         | **Default**          | **Description**                                                                                       |
|---------------------|----------------------|-------------------------------------------------------------------------------------------------------|
| `--action`         | *(required)*         | Must be `trk`.                                                                                       |
| `--source`         | *(required)*         | Path to your `.fib.gz` (or `.fz`) file.                                                              |
| `--output`         | Depends on format    | Output file or directory. Extensions depend on the specified format.                                  |
| `--thread_count`   | Hardware threads     | Number of threads to use.                                                                             |
| `--report`         | —                    | Write the raw tracking report (`.txt`).                                                              |
| `--parameter_id`   | —                    | Load all tracking parameters from a saved parameter code.                                             |
| `--refine`         | —                    | Number of "trim + reseed" iterations after initial tracking.                                          |
| `--tip_iteration`  | `0` or `16`          | Specify pruning iterations. Default is `16` for `track_id` or `dt_threshold_index`.                   |
| `--random_seed`    | `0`                  | Seed for random number generation in fiber tracking. Set to an integer to vary seed sequences.        |

---

## Tracking Parameters

| **Option**           | **Default**                    | **Description**                                                                                      |
|-----------------------|--------------------------------|------------------------------------------------------------------------------------------------------|
| `--method`           | Algorithm in file             | Tracking algorithm. `0=streamline`, `1=RK4`, etc.                                                   |
| `--tract_count`      | `0`                           | Number of tracts to generate (alternative to seed count).                                            |
| `--seed_count`       | Auto                          | Number of seeds to launch. Overrides `tract_count`.                                                 |
| `--track_voxel_ratio`| `0.5`                         | Seeds-per-voxel ratio.                                                                               |
| `--turning_angle`    | `0` (random)                  | Maximum allowable turning angle (deg). Default is random between 15° and 90°.                       |
| `--step_size`        | `0` (random)                  | Step size in mm. Default is random between 1–3 voxels.                                              |
| `--smoothing`        | `0`                           | Fraction of previous direction to mix in (0-1).                                                     |
| `--min_length`       | Dataset-specific              | Minimum fiber length (mm).                                                                          |
| `--max_length`       | Dataset-specific              | Maximum fiber length (mm).                                                                          |
| `--otsu_threshold`   | FA threshold × 0.6            | Default threshold for FA-based seeding.                                                             |
| `--threshold_index`  | —                             | Use a different diffusion index (e.g., QA) for termination instead of FA.                           |
| `--check_ending`     | Off for whole-brain tracking  | Drop tracks that do not terminate within any ROI.                                                   |

---

## ROI / Region Options

The following region types can be specified (comma-separated actions are applied in order):

```text
--seed=<file>
--roi=<file_or_atlas:region>[,dilation,erosion,smoothing,…]
--roi2=…
--roa=…
--end=…
--ter=…
--nend=…
--lim=…
```

### Actions
- **Actions:** `dilation`, `erosion`, `defragment`, `negate`, `flipx`, `flipy`, `flipz`, etc.
- **Merge Volumes:** You can merge multiple volumes using `+`:
```bash
--roi=left.nii.gz+right.nii.gz,dilation,smoothing
```

---

## Post-Tracking & Export Options

| **Option**         | **Description**                                                                                      |
|---------------------|------------------------------------------------------------------------------------------------------|
| `--delete_repeat`  | Remove duplicate streamlines within a specified voxel distance.                                       |
| `--recognize`      | Apply atlas-based bundle recognition.                                                                |
| `--mni_track`      | Save tracts in MNI space.                                                                            |
| `--end_point`      | Save a `.txt` or `.mat` file of all endpoint coordinates.                                            |
| `--export`         | Export options for statistics and visualizations: `stat`, `tdi`, `tdi_color`, `report:index`, etc.   |

---

## Differential Tracking

| **Option**            | **Description**                                                                                     |
|------------------------|-----------------------------------------------------------------------------------------------------|
| `--dt_metric1`        | Metric for baseline tracking (e.g., `qa`).                                                          |
| `--dt_metric2`        | Metric for comparison tracking (e.g., `nqa`).                                                       |
| `--dt_threshold`      | Percent change threshold for differential analysis.                                                 |
| `--dt_threshold_type` | Type of threshold: `0=(m1−m2)/m1`, `1=(m1−m2)/m2`, `2=abs(m1−m2)`.                                  |

---

## Connectivity Options

| **Option**                | **Default**       | **Description**                                                                                     |
|----------------------------|-------------------|-----------------------------------------------------------------------------------------------------|
| `--connectivity`          | —                 | Comma-separated list of atlases or ROI files (e.g., `AAL2,HCP-MMP`).                                |
| `--connectivity_value`    | `count`           | Metrics to compute for each matrix entry: `count`, `length`, `qa`, etc.                            |
| `--connectivity_threshold`| `0.001`           | Threshold to binarize connection weights before graph measures.                                    |
