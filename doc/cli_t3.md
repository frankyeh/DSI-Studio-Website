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

| **Option**         | **Description**                                                                                      |
|---------------------|------------------------------------------------------------------------------------------------------|
| `--output=<file name for saving tractography>`  | save tractography (e.g. `--output=result.tt.gz`)                                      |
| `--trk_format=<format> | specify default tractography format (e.g. --trk_format=tt.gz if --output is not specified  |
| `--delete_repeat=<length>`  | Remove duplicate streamlines within a specified voxel distance. (e.g. `--delete_repeat=8`)  |
| `--delete_by_length=<length>`  | Remove streamlines shorter than a specified voxel distance.                                       |
| `--cluster=<method_id>,<cluster count>,<detail>,<output txt file name>`  | Apply clustering, (e.g. `--cluster=0,50,8,cluster.txt`. The method_id includes 0: single linkage, 1: k-means 2:EM. <detail> is only used in single linkage method.|
| `--recognize=<recognize output>`      | Apply atlas-based bundle recognition.                                                                |
| `--template_track=<file name>`      | Save tracts in the template's voxel space.                                                                            |
| `--mni_track=<file name>`      | Save tracts in MNI space.                                                                            |
| `--end_point=<file name>`      | Save a `.txt` or `.mat` file of all endpoint coordinates.                                            |
| `--end_point1=<file name>`      | Save a `.txt` or `.mat` file of endpoint coordinates of tract's one end.                                            |
| `--end_point2=<file name>`      | Save a `.txt` or `.mat` file of endpoint coordinates of tract's another end.                                            |
| `--export=stat,tdi,report:<index_name>:<0:x 1:y 2:z 3: along tract 4:mean value>:<bandwidth>`         | Export options for statistics and visualizations: --export=stat outputs the statistics <br> --export=tdi outputs the track density images <br> --export=report:dti_fa:0:1 outputs the tract x-direction profile at bandwidth=1 |
| `--connectivity`          | Comma-separated list of atlases or ROI files (e.g., `AAL2,HCP-MMP`).                                |
| `--connectivity_value`    | Metrics to compute for each matrix entry: `count`, `length`, `qa`, etc.                            |
| `--connectivity_threshold`| Threshold to binarize connection weights before graph measures.                                    |

Here's a revised and cleaner version of your markdown table, improving clarity, grammar, and formatting:

---

## Post-Tracking Routine

| **Option**                                     | **Description**                                                                                                                                                                       |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--output=<file>`                              | Save tractography result (e.g., `--output=result.tt.gz`).                                                                                                                             |
| `--trk_format=<format>`                        | Set default tract format if `--output` is not specified (e.g., `--trk_format=tt.gz`).                                                                                                 |
| `--delete_repeat=<length>`                     | Remove duplicate streamlines within the specified voxel distance (e.g., `--delete_repeat=8`).                                                                                         |
| `--delete_by_length=<length>`                  | Remove streamlines shorter than the specified voxel length.                                                                                                                           |
| `--cluster=<method>,<count>,<detail>,<output>` | Perform clustering (e.g., `--cluster=0,50,8,cluster.txt`).<br>`method`: 0=single linkage, 1=k-means, 2=EM.<br>`detail` is used only in method 0.                                      |
| `--recognize=<file>`                           | Apply atlas-based bundle recognition and save recognized tracts.                                                                                                                      |
| `--template_track=<file>`                      | Save tracts in the template voxel space.                                                                                                                                              |
| `--mni_track=<file>`                           | Save tracts in MNI space.                                                                                                                                                             |
| `--end_point=<file>`                           | Save endpoint coordinates (both ends) to `.txt` or `.mat`.                                                                                                                            |
| `--end_point1=<file>`                          | Save coordinates of tract starting points to `.txt` or `.mat`.                                                                                                                        |
| `--end_point2=<file>`                          | Save coordinates of tract endpoints to `.txt` or `.mat`.                                                                                                                              |
| `--export=<type>,<type>,<type>`                              | Export tract data (e.g. `--export=stat,tdi,report:dti_fa:0:1`) :<br>• `stat` – statistics<br>• `tdi` – track density image<br>• `report:<index>:<dim>:<bandwidth>` – tract profile (e.g., `--export=report:dti_fa:0:1`) |
| `--connectivity=<atlases>`                     | Specify comma-separated atlas or ROI names (e.g., `AAL2,HCP-MMP`).                                                                                                                    |
| `--connectivity_value=<metric>`                | Set matrix value metric: `count`, `length`, `qa`, etc.                                                                                                                                |
| `--connectivity_threshold=<value>`             | Threshold to binarize connection weights before computing graph measures.                                                                                                             |

---

Let me know if you'd like to group related options, add examples, or create a collapsible version for a website.


---

## Differential Tracking

| **Option**            | **Description**                                                                                     |
|------------------------|-----------------------------------------------------------------------------------------------------|
| `--dt_metric1`        | Metric for baseline tracking (e.g., `qa`).                                                          |
| `--dt_metric2`        | Metric for comparison tracking (e.g., `nqa`).                                                       |
| `--dt_threshold`      | Percent change threshold for differential analysis.                                                 |
| `--dt_threshold_type` | Type of threshold: `0=(m1−m2)/m1`, `1=(m1−m2)/m2`, `2=abs(m1−m2)`.                                  |

