# Fiber Tracking

Use `--action=trk` to launch deterministic whole-brain or ROI-based fiber tracking in DSI Studio (Yeh et al. PLoS ONE 8(11):e80713, 2013).  You can also compute tract-to-region connectomes, tract statistics, track‐density images, cluster or recognize known bundles, and even perform differential analysis—all from this one command.

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

### Differential tractography (connectometry)  
```bash
dsi_studio --action=trk \
           --source=patient.fib.gz \
           --other_slices=HCP1065_nqa_mni.nii.gz \
           --dt_metric1=HCP1065_nqa_mni \
           --dt_metric2=qa \
           --dt_threshold=0.2 \
           --seed_count=200000 \
           --min_length=30 \
           --output=diff_tracts.tt.gz
```

## Core Options

| Option                | Default             | Description                                                                                       |
|:----------------------|:--------------------|:--------------------------------------------------------------------------------------------------|
| `--action`            | *(required)*        | Must be `trk`                                                                                     |
| `--source`            | *(required)*        | path to your `.fib.gz` (or `.fz`) file                                                            |
| `--thread_count`      | hardware threads    | number of threads                                                                                 |
| `--output`            | `source + .tt.gz`   | output file or directory; use `no_file` to skip saving tracts                                      |
| `--report`            | —                   | write the raw tracking report (`.txt`)                                                            |
| `--parameter_id`      | —                   | load all tracking parameters from a saved code                                                     |
| `--refine`            | —                   | number of “trim + reseed” iterations after initial tracking                                        |

---

## Tracking Parameters

> These can all be overridden individually, or bundled into a single `--parameter_id`.

| Option              | Default                               | Description                                                                                                                                  |
|:--------------------|:--------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| `--method`          | algorithm in file (0=streamline…)     | Tracking algorithm. 0=streamline, 1=RK4, …                                                                                                    |
| `--seed_count`      | auto (from voxels & `track_voxel_ratio`)| number of seeds to launch; overrides `--tract_count`                                                                                          |
| `--tract_count`     | 0                                     | number of tracts to generate (alternative to seeds)                                                                                            |
| `--track_voxel_ratio` | 0.5 (default)                      | seeds per voxel ratio                                                                                                                         |
| `--turning_angle`   | `0` (random 15–90°)                    | maximum allowed turning angle (deg); `0` selects a random angle between 15° and 90°.                                                          |
| `--step_size`       | `0` (random)                           | step size in mm; `0` random between 1–3 voxels.                                                                                                |
| `--smoothing`       | 0                                     | fraction of previous direction to mix in (0–1)                                                                                                 |
| `--min_length`      | handle-min_length()                   | drops fibers shorter than this length (mm)                                                                                                     |
| `--max_length`      | max(min_length, handle-max_length())  | drops fibers longer than this length (mm)                                                                                                      |
| `--otsu_threshold`  | fa_otsu×0.6                           | default threshold for FA‐based seeding                                                                                                         |
| `--threshold_index` | —                                     | use a different diffusion index (e.g. QA) for termination instead of FA                                                                         |
| `--random_seed`     | 0                                     | seed for random number generator                                                                                                               |
| `--check_ending`    | `po.has("track_id")?1:0`              | drop tracks that do not terminate inside any ROI                                                                                               |

---

The following settings are also included in `--parameter_id` but  usually the default values are used.

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| method | `0` |  tracking methods 0:streamline , 1:rk4 |
| otsu_threshold | `0.6` | The default Otsu's threshold can be adjusted to any ratio. |
| tip_iteration | `0` | specify pruning iterations. If --track_id or --dt_threshold_index is specified, the default value is `16` |
| random_seed | `0` | specify the random number for fiber tracking. Specify a different interger to get different seed sequences. |

> Please note that parameter ID does not override ROI settings, dt_threshold_index, or threshold_index settings. You may still need to assign ROI/ROA...etc. in the command line.

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
- **Actions** (e.g. `dilation`, `erosion`, `defragment`, `negate`, `flipx`, `shiftnx`, …)

You can **merge** multiple volumes with `+`:

```bash
--roi=left.nii.gz+right.nii.gz,dilation,smoothing
```

---

## Differential Settings

| Option            | Description                                                                                                         |
|:------------------|:--------------------------------------------------------------------------------------------------------------------|
| `--other_slices`  | MNI‐space QA map or connectometry DB (e.g. `study.db.fz`); wildcard & comma‐list supported.                         |
| `--dt_metric1`    | first metric name (e.g. `qa`, or MNI‐template metric)                                                               |
| `--dt_metric2`    | second metric name (e.g. `qa` for subject)                                                                          |
| `--dt_threshold`  | percent change to capture (e.g. `0.1` = 10%)                                                                         |
| `--dt_threshold_type` | `0`=(m1−m2)/m1 `1`=(m1−m2)/m2 `2`=(m1−m2)                                                                      |

---

## Connectivity Analysis

| Option                      | Default       | Description                                                                                       |
|:----------------------------|:--------------|:--------------------------------------------------------------------------------------------------|
| `--connectivity`            | —             | comma‐list of atlases or NIfTI ROI files (e.g. `HCP-MMP,AAL3` or `/path/atlas.nii.gz+…`)           |
| `--connectivity_value`      | `count`       | `count,ncount,mean_length,trk,dti_fa,qa,…` (comma‐list of metrics to fill each matrix entry)      |
| `--connectivity_type`       | `pass`        | count tracks that “pass through” vs “end in” regions                                              |
| `--connectivity_threshold`  | `0.001`       | relative threshold (fraction of max) to binarize before graph measures                            |
| `--connectivity_output`     | `matrix,connectogram,measure` | which files to write: raw matrix, connectogram text, network measures                           |

---

## Post-Tracking & Export

| Option             | Description                                                                                                                  |
|:-------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| `--delete_repeat`  | `<distance>` in voxels—remove duplicate streamlines within this tolerance                                                    |
| `--recognize`      | `<basename>`—run atlas recognition; writes `<basename>.label.txt` & `.name.txt`                                            |
| `--template_track` | save tracts warped into the template (MNI) space                                                                            |
| `--mni_track`      | save tracts in MNI space (alias of `template_track` with forced MNI flag)                                                   |
| `--end_point`      | `.txt` or `.mat` of all endpoint coordinates                                                                                 |
| `--end_point1/2`   | NIfTI volumes of the first/second endpoint regions                                                                            |
| `--export`         | comma‐list of:  - `stat` → per‐track statistics text  - `tdi`/`tdi2` → track‐density images (diffusion/subvoxel)  - `tdi_t1t2` → TDI in T1/T2 space (requires `--ref`)  - `tdi_color`/`tdi2_color` → color TDIs  - `tdi_end`/`tdi2_end` → endpoint TDIs  - `report:<index>:<dir>:<bandwidth>` → along‐tract profiles    - *any* slice name loaded via `--other_slices`                                             |
| `--ref`            | `<T1w|T2w>.nii.gz`—save tracks or TDIs in that space                                                                         |

---

## Clustering

```bash
--cluster=<METHOD_ID>,<CLUSTER_COUNT>,<RESOLUTION>,<OUTPUT_FILE>
```
- **METHOD_ID**: `0`=single-linkage, `1`=k-means, `2`=EM  
- **CLUSTER_COUNT**: number of clusters (or max clusters for linkage)  
- **RESOLUTION**: mm merge distance (linkage only)  
- **OUTPUT_FILE**: text file for cluster labels  

---

_All options above now exactly match every `po.has("…")` and `po.get("…")` in **trk.cpp**._
