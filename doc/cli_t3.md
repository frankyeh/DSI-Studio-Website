# Fiber Tracking

Use `--action=trk` to launch deterministic whole-brain or ROI-based fiber tracking in DSI Studio (Yeh et al. PLoS ONE 8(11):e80713, 2013). You can also compute tract-to-region connectomes, tract statistics, track‐density images, cluster or recognize known bundles, and even perform differential analysis—all from this one command.

For **automatic atlas-based bundle mapping**, see `--action=atk` instead:  
https://dsi-studio.labsolver.org/doc/cli_atk.html

---

## Examples

---

## Quick Examples

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

### Atlas connectivity (HCP-MMP & AAL3)  
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

### Differential tractography

#### Longitudinal differential tractography:
```bash
dsi_studio --action=trk \
           --source=baseline.fz \
           --other_slices=followupnqa.nii.gz \
           --dt_metric1=nqa \
           --dt_metric2=followupnqa \
           --dt_threshold=0.1 \
           --output=longitudinaldiff.tt.gz
```

#### Cross-sectional differential tractography (patient vs. control NQA):
```bash
dsi_studio --action=trk \
           --source=patient.fz \
           --other_slices=controlnqa.nii.gz \
           --dt_metric1=controlnqa \
           --dt_metric2=nqa \
           --dt_threshold=0.1 \
           --output=crosssectional_diff.tt.gz
```

---

## Core Options

| Option                | Default             | Description                                                                                       |
|:----------------------|:--------------------|:--------------------------------------------------------------------------------------------------|
| `--action`            | *(required)*        | Must be `trk`                                                                                     |
| `--source`            | *(required)*        | Path to your `.fib.gz` (or `.fz`) file                                                            |
| `--thread_count`      | Hardware threads    | Number of threads                                                                                 |
| `--output`            | Depends on format   | Output file or directory; extension depends on specified file format                              |
| `--report`            | —                   | Write the raw tracking report (`.txt`)                                                            |
| `--parameter_id`      | —                   | Load all tracking parameters from a saved code                                                     |
| `--refine`            | —                   | Number of “trim + reseed” iterations after initial tracking                                        |
| `--tip_iteration`     | `0` or `16`         | Specify pruning iterations. Default is `16` if `--track_id` or `--dt_threshold_index` is specified |
| `--random_seed`       | `0`                 | Specify the random number for fiber tracking. Use a different integer to vary seed sequences.      |

---

## Tracking Parameters

> These can all be overridden individually, or bundled into a single `--parameter_id`.

| Option              | Default                               | Description                                                                                                                                  |
|:--------------------|:--------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| `--method`          | Algorithm in file (0=streamline…)     | Tracking algorithm. 0=streamline, 1=RK4, …                                                                                                    |
| `--seed_count`      | Auto (from voxels & `track_voxel_ratio`)| Number of seeds to launch; overrides `--tract_count`                                                                                          |
| `--tract_count`     | `0`                                   | Number of tracts to generate (alternative to seeds)                                                                                            |
| `--track_voxel_ratio` | 0.5 (default)                      | Seeds per voxel ratio                                                                                                                         |
| `--turning_angle`   | `0` (random 15–90°)                    | Maximum allowed turning angle (deg); `0` selects a random angle between 15° and 90°.                                                          |
| `--step_size`       | `0` (random)                           | Step size in mm; `0` random between 1–3 voxels.                                                                                                |
| `--smoothing`       | `0`                                   | Fraction of previous direction to mix in (0–1)                                                                                                 |
| `--min_length`      | Handle-min_length()                   | Drops fibers shorter than this length (mm)                                                                                                     |
| `--max_length`      | Max(min_length, handle-max_length())  | Drops fibers longer than this length (mm)                                                                                                      |
| `--otsu_threshold`  | fa_otsu×0.6                           | Default threshold for FA‐based seeding                                                                                                         |
| `--threshold_index` | —                                     | Use a different diffusion index (e.g. QA) for termination instead of FA                                                                         |
| `--check_ending`    | Drop tracks that do not terminate within any ROI |

Additional Parameters:
- `--seed_position`
- `--seed_orientation`
- `--swap_xy`
- `--flip_x`
- `--flip_y`
- `--flip_z`

---

## ROI / Region Options

Any of the following region types can be specified (comma-separated actions are applied in order):

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

- **`<file_or_atlas:region>`**  
  NIfTI file → volume or atlas name  
- **Actions** (e.g., `dilation`, `erosion`, `defragment`, `negate`, `flipx`, `flipy`, `flipz`, `shiftnx`, `shiftx`, `smoothing`, `fit`)

You can **merge** multiple volumes with `+`:

```bash
--roi=left.nii.gz+right.nii.gz,dilation,smoothing
```

---

## Differential Settings

| Option            | Description                                                                                                         |
|:------------------|:--------------------------------------------------------------------------------------------------------------------|
| `--other_slices`  | MNI‐space QA map or connectometry DB (e.g. `study.db.fz`); wildcard & comma‐list supported.                         |
| `--dt_metric1`    | First metric name (e.g., `qa`, or MNI‐template metric)                                                               |
| `--dt_metric2`    | Second metric name (e.g., `qa` for subject)                                                                          |
| `--dt_threshold`  | Percent change to capture (e.g., `0.1` = 10%)                                                                         |
| `--dt_threshold_type` | `0=(m1−m2)/m1`, `1=(m1−m2)/m2`, `2=abs(m1−m2)`                                                                  |

---

## Connectivity Analysis

| Option                      | Default       | Description                                                                                       |
|:----------------------------|:--------------|:--------------------------------------------------------------------------------------------------|
| `--connectivity`            | —             | Comma‐list of atlases or NIfTI ROI files (e.g., `HCP-MMP,AAL3` or `/path/atlas.nii.gz+…`)           |
| `--connectivity_value`      | `count`       | `count,ncount,mean_length,trk,dti_fa,qa,…` (comma‐list of metrics to fill each matrix entry)      |
| `--connectivity_type`       | `pass`        | Count tracks that “pass through” vs. “end in” regions                                              |
| `--connectivity_threshold`  | `0.001`       | Relative threshold (fraction of max) to binarize before graph measures                            |
| `--connectivity_output`     | `matrix,connectogram,measure,t2r` | Which files to write: raw matrix, connectogram text, network measures, track-to-region (t2r) |

---

## Post-Tracking & Export

| Option             | Description                                                                                                                  |
|:-------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| `--delete_repeat`  | `<distance>` in voxels—remove duplicate streamlines within this tolerance                                                    |
| `--recognize`      | `<basename>`—run atlas recognition; writes `<basename>.label.txt` & `.name.txt`                                            |
| `--mni_track`      | Save tracts in MNI space (alias of `template_track` with forced MNI flag)                                                   |
| `--end_point`      | `.txt` or `.mat` of all endpoint coordinates                                                                                 |
| `--end_point1/2`   | NIfTI volumes of the first/second endpoint regions                                                                            |
| `--export`         | Comma‐list of:  - `stat` → per‐track statistics text  - `tdi`/`tdi2` → track‐density images (diffusion/subvoxel)  - `tdi_t1t2` → TDI in T1/T2 space (requires `--ref`)  - `tdi_color`/`tdi_color2` → color TDIs  - `tdi_end`/`tdi2_end` → endpoint TDIs  - `report:<index>:<dir>:<bandwidth>` → along‐tract profiles    - *any* slice name loaded via `--other_slices`                                             |
| `--ref`            | `<T1w|T2w>.nii.gz`—save tracks or TDIs in that space                                                                         |

---

## Clustering

```bash
--cluster=<METHOD_ID>,<CLUSTER_COUNT>,<RESOLUTION>,<OUTPUT_FILE>
```
- **METHOD_ID**: `0=single-linkage`, `1=k-means`, `2=EM`  
- **CLUSTER_COUNT**: Number of clusters (or max clusters for linkage)  
- **RESOLUTION**: mm merge distance (linkage only)  
- **OUTPUT_FILE**: Text file for cluster labels  

---

_All options above now exactly match every `po.has("…")` and `po.get("…")` in **trk.cpp**._
