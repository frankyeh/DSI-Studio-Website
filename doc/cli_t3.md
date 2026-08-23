# Fiber Tracking

Use `--action=trk` for deterministic whole-brain, ROI-based, or differential fiber tracking. For atlas-guided mapping of named bundles, use [`--action=atk`](/doc/cli_atk.html).

## Examples

### Whole-brain tracking

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --output=subject_all.tt.gz
```

### ROI-based tracking

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --roi=regionA.nii.gz \
  --roi2=regionB.nii.gz \
  --roa=exclude_region.nii.gz \
  --output=bundle.tt.gz
```

Do not add a Seed region unless the tracking start locations intentionally need to be restricted. Without `--seed`, normal whole-brain seeding is used.

## Core options

| Option | Default | Description |
|:--|:--|:--|
| `--action` | required | Must be `trk`. |
| `--source` | required | Input `.fz` file (legacy `.fib.gz` is also supported). |
| `--output` | generated name | Output tractography file, normally `.tt.gz`. |
| `--tip_iteration` | workflow dependent | Topology-informed pruning iterations. Normal whole-brain tracking does not require TIP; atlas/differential workflows may apply it. |

## Tracking parameters

| Option | Default | Description |
|:--|:--|:--|
| `--method` | current tracking setting | Tracking algorithm. |
| `--tract_count` | automatic | Stop after the requested number of tracts. When no tract/seed count is fixed, the track-to-voxel ratio determines the amount of tractography. |
| `--seed_count` | automatic | Stop after the requested number of seeds; used when a fixed seeding count is required. |
| `--track_voxel_ratio` | FIB/template dependent | Track-to-voxel ratio used when tract/seed counts are not explicitly fixed. The default scales with FIB voxel size and template scale. |
| `--fa_threshold` | `0` | Tracking/anisotropy threshold. `0` randomizes the threshold around the Otsu-derived default. |
| `--otsu_threshold` | `0.6` | Center of the Otsu-based default threshold. With `--fa_threshold=0`, the threshold is randomized from `default_otsu-0.1` to `default_otsu+0.1` times the Otsu threshold. |
| `--threshold_index` | current tracking index | Diffusion index used for tracking/termination. |
| `--turning_angle` | `0` | Maximum turning angle in degrees. `0` randomizes the angular threshold from **45° to 90°**. |
| `--step_size` | `0` | Step size in millimeters. `0` uses the **voxel spacing**. |
| `--smoothing` | `0` | Direction-smoothing fraction. `0` disables smoothing; `1` uses a randomized smoothing percentage. |
| `--min_length` | template/image dependent | Minimum tract length in mm. |
| `--max_length` | template/image dependent | Maximum tract length in mm. |
| `--check_ending` | `0` for ordinary tracking | Enable ending checks when required by the workflow. |
| `--parameter_id` | — | Restore tracking settings from a DSI Studio parameter code. |
| `--random_seed` | `0` | Random-number seed when deterministic random-sequence control is needed. |

The current default length and track-density settings are derived from the loaded FIB/template rather than one universal human value.

## Region options

The region roles correspond to the GUI tracking roles:

| Option | Role |
|:--|:--|
| `--seed` | Start tracking inside the region. |
| `--roi`, `--roi2`, ... | Keep tracks that pass through the region. Multiple ROIs are AND constraints. |
| `--roa`, `--roa2`, ... | Exclude tracks that pass through the region. |
| `--end`, `--end2` | Keep tracks ending in the region. |
| `--ter`, `--ter2`, ... | Terminate tracking when the streamline enters the region. |
| `--nend` | Exclude tracks that end in the region while allowing pass-through. |
| `--lim` | Restrict tracking to a limiting spatial region. |

Region specifications can be files, supported atlas-region expressions, and region operations. Multiple items can be separated by commas, for example:

```bash
--roi=left.nii.gz,dilation
```

See [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html) for the anatomical meaning of each role.

## Post-tracking options

| Option | Description |
|:--|:--|
| `--output=<file>` | Save tractography, e.g. `--output=result.tt.gz`. |
| `--trk_format=<format>` | Select the tract format when a default output name is used. |
| `--delete_repeat=<distance>` | Remove near-duplicate streamlines using the specified distance criterion. |
| `--delete_by_length=<length>` | Remove short streamlines using the specified length criterion. |
| `--cluster=<method>,<count>,<detail>,<output>` | Cluster the resulting streamlines. |
| `--recognize=<file>` | Recognize bundles and save the recognized result. |
| `--template_track=<file>` | Save tract coordinates in template voxel space. |
| `--mni_track=<file>` | Save tract coordinates in MNI space. |
| `--end_point=<file>` | Export endpoints from both ends. |
| `--end_point1=<file>` / `--end_point2=<file>` | Export the two endpoint sets separately. |
| `--export=<type>,...` | Export tract statistics or track-density products. |
| `--profile=<metrics>:<type>:<bandwidth>` | Export along-tract profiles. |

## Connectivity matrices

Connectivity can be calculated from the tractography produced by the same command.

| Option | Description |
|:--|:--|
| `--connectivity=<atlases>` | Comma-separated atlas names or NIfTI parcellations. |
| `--connectivity_type=<types>` | `end` for endpoint connectivity or `pass` for regions traversed by a streamline. Multiple types can be requested. |
| `--connectivity_value=<metrics>` | Select connectivity matrix value(s); `all` requests the supported set. |
| `--connectivity_output=<outputs>` | Select outputs such as `matrix`, `connectogram`, and `network`. |

Example:

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --connectivity=HCP-MMP \
  --connectivity_type=end \
  --connectivity_output=matrix,network
```

For atlas-based **tract-to-region** workflows using named tracts, use [Automatic Fiber Tracking CLI](/doc/cli_atk.html).

## Differential tracking

The differential **Type 1–4** terminology describes study design and analysis space. It is separate from the numeric `--dt_threshold_type` formula index below.

| Option | Description |
|:--|:--|
| `--dt_metric1` | First differential metric (`m1`). |
| `--dt_metric2` | Second differential metric (`m2`). Both metric options are required together. |
| `--dt_threshold` | Required difference threshold; `0.2` represents a 20% change when using a fractional threshold type such as type `0`. |
| `--dt_threshold_type` | Differential formula: `0=(m1-m2)/m1`, `1=(m1-m2)/m2`, `2=m1-m2`, `3=(m2-m1)/m1`, `4=(m2-m1)/m2`, `5=m2-m1`, `6=m1/max(m1)`, `7=m2/max(m2)`. |

For study-design guidance, see [Differential Tractography](/doc/gui_t3_dt.html).
