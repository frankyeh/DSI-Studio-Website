# Automatic Fiber Tracking

> Use `--action=atk` to map named white-matter pathways across one or more FIB files.

AutoTrack combines deterministic fiber tracking with tractography-atlas recognition to map pathways consistently across subjects (Yeh, *NeuroImage* 2020;223:117329; Yeh, *Nat Commun* 2022;13:4933).

## Examples

### 1. Batch-track standard pathways across a cohort

```bash
dsi_studio --action=atk --source="*.fz"
```

`--action=atk` expands `--source` wildcards internally rather than running isolated per-file loops. When multiple FIB files match, DSI Studio writes per-subject bundles and statistics under `<tract_name>/` and aggregates cohort tables (`<tract_name>.stat.txt` and `all_results_subject_wise.txt`) in the working folder.

### 2. Track specific bundles into an existing output directory

```bash
mkdir -p out_atk
dsi_studio --action=atk \
  --source=sub-01.fz \
  --track_id=Association_ArcuateFasciculusL,ProjectionBrainstem_CorticospinalTractL \
  --output=out_atk
```

`--output` must point to an existing directory; when set, all bundle files are written directly inside that directory without `<tract_name>/` subfolders.

### 3. Customize tolerance, TIP pruning, and template/MNI tract outputs

```bash
dsi_studio --action=atk \
  --source="*.fz" \
  --track_id=Corticos,Optic \
  --tolerance=22,26,30 \
  --tip_iteration=4 \
  --template_track \
  --mni_track
```

### 4. Calculate bundle overlap with a built-in parcellation atlas

```bash
dsi_studio --action=atk \
  --source=subject.fz \
  --track_id=Corticos \
  --overlap=HCP-MMP
```

Each mapped bundle is voxelized and intersected with every nonempty label of the specified built-in atlas, writing `<bundle-tract-file>.<atlas-name>.overlap.txt` beside the tract file (for example, `ProjectionBrainstem_CorticospinalTractL/subject.ProjectionBrainstem_CorticospinalTractL.tt.gz.HCP-MMP.overlap.txt`).

### 5. Export diagnostic registration and per-attempt region files

```bash
dsi_studio --action=atk \
  --source=subject.fz \
  --track_id=Arcuate \
  --debug=1 \
  --overwrite=1
```

---

## Core Functions

| Option | Default | Description |
|:--|:--|:--|
| `--action` | *(required)* | Must be `atk`. |
| `--source` | *(required)* | Input `.fz` or legacy `.fib.gz` file, wildcard pattern (e.g., `"*.fz"`), or comma-separated file list. |
| `--track_id` | `Association,Tract,Thalamic,Optic,Fornix,Medial,Corpus` | Comma-separated case-insensitive substrings matched against all hierarchy levels of the tractography atlas (note spelling: `--track_id`, not `--tract_id`). Use full atlas identifiers for exact bundle selection. |
| `--output` | `<source_dir>/<tract_name>/` | Existing output directory. Must already exist on disk (`expecting a directory at --output` otherwise); writes files directly in `<output>/` without per-bundle subdirectories. |
| `--template` | Auto-matched per FIB (`0` for initial `track_id` list) | Template name from `atlas/` (e.g., `human`, `chimpanzee`, `rhesus`, `marmoset`, `rat`, `mouse`) or zero-based template index. Specify explicitly for non-human datasets so candidate `--track_id` lookup uses that species atlas. |
| `--tractography_atlas` | Template tract atlas | Path to a custom `.tt.gz` tractography atlas (requires a matching `<atlas_base>.txt` label file). |
| `--tolerance` | `d, d+vs, d+2*vs` | Single float or comma-separated tolerance schedule in template mm. Default base `d = 12 * default_template_vs` (`24` mm for `human`, so `24,26,28` mm on 2 mm data), capped at `0.5 * median_length` for short pathways. AutoTrack stops advancing once an attempt produces tracts. |
| `--track_voxel_ratio` | `(vs[0] / default_template_vs)^2` | Target tract-to-seed-voxel ratio (`1.0` on 2 mm human data). Sets `max_tract_count = track_voxel_ratio * seed_voxels` and `max_seed_count = 5000 * max_tract_count`. |
| `--tip_iteration` | `16` | Topology-informed pruning iterations. Automatically undone if pruning removes every streamline (`post_tip == 0`). |
| `--check_ending` | `1` | Set `1` to place a high-anisotropy white-matter `not_end` constraint preventing premature termination inside deep white matter; `0` disables. |
| `--use_roi` | `1` | Set `1` to apply built-in `<atlas>_roi.nii.gz` and `<atlas>_roa.nii.gz` masks matching the target bundle; `0` disables built-in ROI/ROA masks. |
| `--search_count` | `16` | Linear registration search attempts when aligning the subject FIB to the template (independent of streamline seed count). |
| `--yield_rate` | `0.00001` | Minimum streamline-per-seed yield checked after `10 / yield_rate` seeds (`1,000,000` seeds at default). Below this rate, AutoTrack aborts the attempt and advances to the next `--tolerance` value. |
| `--overwrite` | `0` | When `0`, skips bundles that already have `.no_result.txt` or both `.<trk_format>` and `.stat.txt`, and reuses an existing `.<trk_format>` file if only post-tracking exports are missing. Set `1` to re-track and overwrite. |
| `--trk_format` | `tt.gz` | Output tractography extension: `tt.gz`, `trx`, `trk.gz`, `trk`, `tck`, `txt`, `mat`, `nii.gz`, or `nii`. |
| `--export` | `stat` | Comma-separated post-tracking exports: `stat` (`<tract_file>.stat.txt`), `tdi` / `tdi<ratio>` / `tdi_color` / `tdi_end` (`<tract_file>.<cmd>.nii.gz`), or any FIB metric name such as `qa` or `dti_fa` (`<tract_file>.<metric>.txt`). Keep `stat` enabled when running multi-subject cohorts so group tables populate. |
| `--report` | *(none)* | Write the methods and tracking report text to the specified file path. |

### Output Files and Batch Aggregation

For each subject `<fib_base>` and matched bundle `<tract_name>`, DSI Studio writes:

- `<out_dir>/<fib_base>.<tract_name>.<trk_format>` and `<out_dir>/<fib_base>.<tract_name>.<trk_format>.stat.txt` when streamlines are found (`<out_dir>` is `<work_dir>/<tract_name>` by default, or `<output>` when `--output` is set).
- `<out_dir>/<fib_base>.<tract_name>.no_result.txt` when all tolerance attempts return zero streamlines.
- When `> 1` source FIB file is processed in one `--action=atk` call, DSI Studio also writes cohort summaries `<work_dir>/<tract_name>.stat.txt` (metrics × subjects) and `<work_dir>/all_results_subject_wise.txt` (subjects × metrics × bundles). Avoid wrapping `--action=atk` in `--loop="*.fz"` if cohort summary tables are needed, because `--loop` invokes single-subject runs (`--continue_on_error` applies only to `--loop`).

---

## Optional Functions

### Tracking and Execution Options

| Option | Default | Description |
|:--|:--|:--|
| `--bias_field_correction` | `1` | Run bias-field correction on load when `"bias field"` is not already recorded in the FIB report (`0` disables). |
| `--threshold_index` | Primary FIB index | Diffusion/anisotropy metric name used for seeding and termination thresholding (e.g., `qa`, `dti_fa`). |
| `--fa_threshold` | `0` | Anisotropy threshold. `0` randomizes per seed across `[otsu_threshold - 0.1, otsu_threshold + 0.1] * fa_otsu`; nonzero fixes the threshold. |
| `--otsu_threshold` | `0.6` | Center multiplier for Otsu-based threshold randomization when `--fa_threshold=0`. |
| `--turning_angle` | `0` | Maximum turning angle in degrees. `0` randomizes per seed from `45` to `90` degrees; nonzero fixes the angle. |
| `--step_size` | `0` | Propagation step in mm. `0` uses voxel spacing; nonzero step sizes resample streamlines to `1.0` voxel spacing prior to TIP. |
| `--smoothing` | `0` | Directional smoothing fraction (`0` = off, `1` = randomized `0%`–`95%`, `0 < x < 1` = fixed fraction). |
| `--parameter_id` | *(none)* | Hexadecimal `TrackingParam` code from DSI Studio; overwrites prior tracking settings on `thread.param`. |
| `--thread_count` | Hardware threads | Maximum worker threads used for parallel tracking and registration. |
| `--debug` | *(off)* | Save registration and per-attempt region diagnostics without changing tracking results (see details below). |

### Post-Tracking and Connectivity Options

Every mapped bundle passes through the shared post-tracking pipeline:

| Option | Default in `atk` | Description |
|:--|:--|:--|
| `--delete_repeat` | `0.5` | Near-duplicate streamline removal distance in voxels (`--action=atk` enables `0.5` by default; pass `--delete_repeat=0` to keep all streamlines). |
| `--delete_by_length` | *(off; `1` if value omitted)* | Delete streamlines shorter than the given length in voxel units. |
| `--overlap` | *(off)* | Exact built-in atlas name (e.g., `HCP-MMP`, `Brodmann`); writes `<tract_file>.<atlas>.overlap.txt` with nonempty label intersections. |
| `--template_track` | *(off)* | Save bundle in template voxel space (`T_<bundle_filename>` per bundle when passed with no value, or explicit path for a single bundle). |
| `--mni_track` | *(off)* | Save bundle in MNI mm space (`mni_<bundle_filename>` per bundle when passed with no value, or explicit path for a single bundle). |
| `--ref` | *(off)* | Reference NIfTI volume; transforms saved bundle tracts and `tdi` exports into that image space. |
| `--end_point` / `--end_point1` / `--end_point2` | *(off)* | Export both endpoint sets (`.end.txt`) or separated ends (`.end1.txt`, `.end2.txt`) beside each bundle when passed with no value, or to an explicit path. |
| `--profile` | *(off)* | `<metric>:<direction>:<bandwidth>` (`direction` `0`–`2` = `x`/`y`/`z`, `3`–`8` = fiber orientation modes; `bandwidth > 0`); writes `<tract_file>.<metric>.<direction>.<bandwidth>.txt`. |
| `--cluster` | *(off)* | `<method>,<count>,<detail>[,<output>]` (`0` = tract distance with `detail > 0` mm, `1` = k-means, `2` = EM); writes `<tract_file>_cluster.txt` when `<output>` is omitted. |
| `--recognize` | *(off)* | `<prefix>`; writes `<prefix>.label.txt` and `<prefix>.name.txt` (single-bundle use; fixed `<prefix>` overwrites across multiple bundles). |
| `--connectivity` | *(off)* | Comma-separated built-in atlas names or NIfTI parcellation files; computes per-bundle connectivity matrices. |
| `--connectivity_type` | `pass` | `pass` or `end` (comma-separated). |
| `--connectivity_value` | `all` | Connectivity metric filter (`all` or comma-separated metric names). |
| `--connectivity_output` | `matrix` | Comma-separated outputs: `matrix` (`.connectivity.mat`), `connectogram` (`.connectogram.txt`), `network` (`.network_measures.txt`). |

**`--action=trk` options ignored by `--action=atk`:** `--min_length` and `--max_length` are derived automatically from the tractography atlas and `--tolerance`; `--tract_count`, `--seed_count`, `--method`, and `--random_seed` are not read unless packed in `--parameter_id`; external region and differential flags (`--roi`, `--roa`, `--seed`, `--end`, `--ter`, `--nend`, `--lim`, `--other_slices`, `--dt_metric1`, `--dt_metric2`) are not used. Use [`--action=trk`](/doc/cli_t3.html) for manual ROI or differential tractography.

<details markdown="1">
<summary>Diagnostic files written by <code>--debug</code></summary>

Passing `--debug=1` (add `--overwrite=1` when rerunning completed bundles) writes diagnostic files without altering registration, tracking, or statistics:

- **Once per FIB in `<work_dir>`:** `<fib_base>.debug.template_qa.nii.gz`, `<fib_base>.debug.template_iso.nii.gz` (when the template includes an ISO volume), `<fib_base>.debug.atlas.tt.gz`, and `<fib_base>.debug.atlas.tt.gz.txt` (warped into subject space). Misalignment here indicates a template-registration problem.
- **Per bundle:** `<trk_base>.debug.tsv` recording one row per tolerance attempt (`attempt`, `tolerance`, `effective_tolerance`, `min_length`, `max_length`, `tip_iteration`, `seeds`, `generated`, `pre_tip`, `post_tip`, `tip_undone`, `selected`, and `outcome`: `low_yield`, `no_tracks`, `tip_undone`, or `selected`).
- **Per bundle and attempt:** `<trk_base>.debug<attempt>.<seed|limiting|not_end|roi|roa>.nii.gz` containing the nonempty atlas-derived masks used for that attempt.

</details>

---

## AutoTrack guidance

- **Hierarchy and constraints:** Named AutoTrack entries already carry built-in anatomical regions (`--use_roi=1`, `--check_ending=1`). Select the parent atlas identifier (for example, `Association_CingulumL`) when mapping an entire tract family and a child identifier only when isolating a specific branch. Note that CLI `--track_id` performs substring matching, so a parent prefix also selects any child subdivisions sharing that prefix.
- **Topology-informed pruning (TIP):** TIP is designed for coherent bundles rather than whole-brain tractograms. Although the CLI defaults to `--tip_iteration=16` (with automatic undo if pruning empties the bundle), `--tip_iteration=4` (or `3`–`4`) is a practical setting for well-populated bundles (~5,000–10,000+ streamlines before pruning); set `--tip_iteration=0` when inspecting unpruned trajectories or sparse pathways.
- **Interpretation and QC:** Streamline counts reflect sampling settings (`--track_voxel_ratio`), not axon count or biological connection strength. Always verify whole-brain fiber orientations before trusting automated bundle outputs; see [Automatic Fiber Tracking (AutoTrack)](/doc/gui_t3_atk.html) for visual quality control and troubleshooting.
