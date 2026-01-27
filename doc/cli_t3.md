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

### Mapping the left arcuate fasciculus and derive is tract-to-region connectome at HCP-MMP parcellation
```bash
dsi_studio --action=trk \
           --source=subject.fib.gz \
           --track_id=ArcuateFasciculusL \
           --connectivity=HCP-MMP
```

---

## Core Options

| **Option**         | **Default**          | **Description**                                                                                       |
|---------------------|----------------------|-------------------------------------------------------------------------------------------------------|
| `--action`         | *(required)*         | Must be `trk`.                                                                                       |
| `--source`         | *(required)*         | Path to your `.fib.gz` (or `.fz`) file.                                                              |
| `--output`         |                      | Output tractography file (e.g. --output=tract.tt.gz) or directory (e.g. --output=/path).                                  |
| `--track_id`       | (optional)           | Specify the name of the bundle to be mapped (e.g. --track_id=ArcuateFasciculusL ). The complete list can be found [here](https://github.com/frankyeh/data-atlas/blob/main/human/human.tt.gz.txt)                                        |
| `--tip_iteration`  | `0` or `16`          | Specify pruning iterations. Default is `16` for `track_id` or `dt_threshold_index`.                   |
| `--thread_count`   | Hardware threads     | Number of threads to use.                                                                             |

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
| `--parameter_id`     | —                    | Load all tracking parameters from a parameter code.                                             |
| `--random_seed`    | `0`                  | Seed for random number generation in fiber tracking. Set to an integer to vary seed sequences.        |

---

## ROI / Region Options

The following region types can be specified (comma-separated actions are applied in order):

```text
--seed=<file>
--roi=<file_or_atlas:region>,<other region file>,<region actions>,... 
--roi2=…
--roa=…
--end=…
--ter=…
--nend=…
--lim=…
```

### Region Actions
- **Region Actions:** `dilation`, `erosion`, `defragment`, `negate`, `flipx`, `flipy`, `flipz`, etc.
- **Merge Volumes:** You can also add more volumes using `,`:
```bash
--roi=left.nii.gz,right.nii.gz,dilation,smoothing
```


---

## Post-Tracking Routine

| **Option**                                     | **Description**                                                                                                                                                                                                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--output=<file>`                              | Save tractography result (e.g., `--output=result.tt.gz`).                                                                                                                                                                                            |
| `--trk_format=<format>`                        | Set default tract format if `--output` is not specified (e.g., `--trk_format=tt.gz`).                                                                                                                                                                |
| `--delete_repeat=<length>`                     | Remove duplicate streamlines within the specified voxel distance (e.g., `--delete_repeat=8`).                                                                                                                                                        |
| `--delete_by_length=<length>`                  | Remove streamlines shorter than the specified voxel length.                                                                                                                                                                                          |
| `--cluster=<method>,<count>,<detail>,<output>` | Perform clustering (e.g., `--cluster=0,50,8,cluster.txt`).<br>• `method`: 0 = single linkage, 1 = k-means, 2 = EM<br>• `detail`: only used in method 0                                                                                               |
| `--recognize=<file>`                           | Apply atlas-based bundle recognition and save recognized tracts.                                                                                                                                                                                     |
| `--template_track=<file>`                      | Save tracts in template voxel space.                                                                                                                                                                                                                 |
| `--mni_track=<file>`                           | Save tracts in MNI space.                                                                                                                                                                                                                            |
| `--end_point=<file>`                           | Save endpoint coordinates (both ends) to `.txt` or `.mat`.                                                                                                                                                                                           |
| `--end_point1=<file>`                          | Save starting point coordinates to `.txt` or `.mat`.                                                                                                                                                                                                 |
| `--end_point2=<file>`                          | Save endpoint coordinates to `.txt` or `.mat`.                                                                                                                                                                                                       |
| `--export=<type>,<type>,...`                   | Export tract data (e.g., `--export=stat,tdi,`)<br>• `<type>`:  stat = tract statistics, tdi = track density image, tdi2= track density image with x2 resolution                                                                                      |
| `--profile=<metrics>,<type>,<bandwidth>`       | Get tract profile, for example: `--profile=dti_fa:3:1` exports `FA` profile along `fiber direction (left first)` using `bandwidth = 1`.                                                                                                                        |
|                                                | `<type>`: 0=x direction, 1=y direction, 2=z direction,<br> 3=fiber orientation (left first), 4=fiber orientation (posterior first), 5=fiber orientation (superior first),<br> 6=fiber orientation (right first), 7=fiber orientation (anterior first), 8=fiber orientation (inferior first)|



## **Connectivity Matrix (Connectome)**

| **Option**                        | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--connectivity=<atlases>`        | A comma-separated list of brain parcellation atlases (NIfTI files or atlas names recognized by DSI Studio, e.g., `AAL2,HCP-MMP`). The specified parcellations are used to compute region-to-region (R2R) and tract-to-region (T2R) connectomes.                                                                                                                                                                                                                                                                                                                            |
| `--connectivity_type=<types>`     | Defines how streamlines contribute to the structural connectivity. Use `pass` to include streamlines that pass through ROIs (pass-through), or `end` to include streamline by checking the endpoints. Multiple types can be specified using commas to export multiple connectomes in a single run, e.g., `--connectivity_type=pass,end`.                                                                                                                                                                                                                                                            |
| `--connectivity_value=<metrics>`  | Selects which connectivity metric(s) to export. The default value `all` exports all supported metrics. To export a subset, provide a comma-separated list of metric keywords (matched by name), e.g., `--connectivity_value=volume,fa`.                                                                                                                                                                                                                                                                                                                                                |
| `--connectivity_output=<outputs>` | Specifies which outputs to save. The default value `all` saves all available outputs. Otherwise, provide a comma-separated list: `matrix` saves the connectivity matrix as `*.connectivity.mat` (MATLAB v4 square matrix); `connectogram` saves `*.connectogram.txt` for visualization with **CIRCUS**; `network` saves graph-theoretic network measures as `*.network_measures.txt`; `t2r` saves the tract-to-region connectome as `.tract2region.txt` (one row per metric and one column per ROI). Example: `--connectivity_output=matrix,network`. |


---

## Differential Tracking

| **Option**            | **Description**                                                                                     |
|------------------------|-----------------------------------------------------------------------------------------------------|
| `--dt_metric1`        | Metric for baseline tracking (e.g., `qa`).                                                          |
| `--dt_metric2`        | Metric for comparison tracking (e.g., `nqa`).                                                       |
| `--dt_threshold`      | Percent change threshold for differential analysis.                                                 |
| `--dt_threshold_type` | Type of threshold: `0=(m1−m2)/m1`, `1=(m1−m2)/m2`, `2=abs(m1−m2)`.                                  |

