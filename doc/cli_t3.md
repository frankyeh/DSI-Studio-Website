# Fiber Tracking

Use `--action=trk` for deterministic whole-brain, ROI-based, connectome, or differential fiber tracking. For atlas-guided mapping of named bundles across one or more subjects, use [`--action=atk`](/doc/cli_atk.html).

## Examples

### Whole-brain tracking

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --tract_count=100000 \
  --output=subject_all.tt.gz
```

### ROI-based tracking

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --roi=HCP-MMP:L_V1 \
  --roi2=FreeSurferDKT:Left-Thalamus_Proper,dilation \
  --roa=exclude_region.nii.gz \
  --tract_count=5000 \
  --output=optic_radiation_L.tt.gz
```

Omit `--seed` unless tracking start points must be restricted to a specific region; without `--seed`, DSI Studio seeds across all supra-threshold brain voxels.

### Atlas-overlap statistics after tracking

Use `--overlap=<atlas>` to voxelize the generated tractography and save region statistics for every nonempty label of a built-in atlas:

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --tract_count=10000 \
  --overlap=HCP-MMP
```

Each nonempty tract-atlas intersection is reported as a column (zero-overlap labels are omitted). Without `--output`, the filename is derived from `<source>.tt.gz`:

```text
subject.fz.tt.gz.HCP-MMP.overlap.txt
```

When `--output=<file>` is set (and not `no_file`), the table is saved as `<output>.<atlas>.overlap.txt` (appending `--trk_format` first if `<output>` lacks a recognized tract/image extension). When `--output` names an existing directory, the table is written inside that directory.

### Batch tracking with wildcards

`--action=trk` expands `*` wildcards in `--source` (or `--loop`) and substitutes matching stems into other wildcard arguments:

```bash
dsi_studio --action=trk \
  --source=sub-*.fz \
  --tract_count=50000 \
  --connectivity=HCP-MMP \
  --connectivity_type=end \
  --output=no_file \
  --continue_on_error=1
```

## Core options

| Option | Default | Description |
|:--|:--|:--|
| `--action` | required | Must be `trk`. |
| `--source` | required | Input `.fz` or `.fib.gz` file, single-digit built-in template index (`0`, `1`, ...), or wildcard pattern (e.g. `*.fz`). |
| `--output` | `<source>.tt.gz` | Output tract path, existing directory, or `no_file` to skip saving streamlines while still running post-tracking analyses (`--overlap`, `--connectivity`, `--export`, `--profile`). Supported extensions: `.tt.gz`, `.trk.gz`, `.trk`, `.tck`, `.trx`, `.txt`, `.mat`, `.nii.gz`, `.nii`. |
| `--other_slices` | — | Comma-separated (or wildcard) NIfTI files loaded as custom slices for `--export`, `--profile`, `--connectivity_value`, differential tracking, or anatomical-space ROI registration. |
| `--subject_demo` | — | Demographics TSV/CSV file or inline text matched against subject ID in `--source` or `--other_slices` when loading database-backed slices. |
| `--bias_field_correction` | `1` | Apply automatic bias-field correction when loading the FIB (unless already recorded in the FIB report). Set `0` to disable. |
| `--template` | `0` | Select built-in template by name or `0`-based index for atlas/MNI warping. |
| `--tractography_atlas` | template default | Custom tractography atlas file path used by `--recognize` or `--track_id`. |
| `--report` | — | Save the generated tracking methods and parameter report text to `<file>`. |
| `--thread_count` | hardware threads | Number of parallel CPU threads used for tracking and atlas mapping. |
| `--loop` | `*` in `--source` | Explicit file wildcard pattern for batch processing across multiple inputs. |
| `--continue_on_error` | unset | Continue batch `--loop` / wildcard execution if an individual file fails (`--continue_on_error=1`). |

## Tracking parameters

| Option | Default | Description |
|:--|:--|:--|
| `--method` | `0` | Tracking algorithm: `0` = streamline (Euler), `1` = 4th-order Runge-Kutta (RK4). |
| `--tract_count` | automatic | Stop after generating `<N>` streamlines (caps seeds at `5000 * N` when `--seed_count` is unset). Ignored if `--seed_count` or `--dt_metric1` is set unless overridden by `--parameter_id`. |
| `--seed_count` | automatic | Stop after placing `<N>` seeds (`max_seed_count = max_tract_count = N`). |
| `--track_voxel_ratio` | `(voxel_size / template_vs)^2` | Density ratio used when neither `--tract_count` nor `--seed_count` is specified. Acts as tract-to-voxel ratio for whole-brain/atlas tracking, or seed-to-voxel ratio when `--roi`, `--end`, or `--dt_metric1` is active. |
| `--threshold_index` | primary FIB index | Metric used for tracking termination (default `fa` for DTI, `qa` for multi-fiber reconstructions). Must match an available index name (e.g. `qa`, `fa`, `nqa`). |
| `--fa_threshold` | `0` | Tracking termination threshold. `0` randomizes per seed uniformly from `(otsu_threshold - 0.1)` to `(otsu_threshold + 0.1)` times the Otsu threshold; nonzero fixes the threshold. |
| `--otsu_threshold` | `0.6` | Center multiplier of the Otsu threshold used when `--fa_threshold=0`. |
| `--turning_angle` | `0` | Maximum turning angle in degrees. `0` randomizes per seed from **45° to 90°**; nonzero sets a fixed angle. |
| `--step_size` | `0` | Step size in mm. `0` = `1.0` voxel spacing; `> 0` = fixed step size in mm (pass a positive `<mm>` value for a fixed sub-voxel step); `< 0` = randomized `0.5`–`1.0` voxel spacing per seed. |
| `--smoothing` | `0` | Propagation direction smoothing fraction. `0` = off; `(0, 1)` = fixed fraction of previous direction; `1` = randomized **0% to 95%** per seed. |
| `--min_length` | `15 * template_vs` | Minimum streamline length in mm (`30` mm for the human 2 mm template). |
| `--max_length` | `100 * template_vs` | Maximum streamline length in mm (`200` mm for the human 2 mm template; clamped `>= min_length`). |
| `--tip_iteration` | `0` (`16` with `--dt_metric1` or `--track_id`) | Topology-informed pruning (TIP) iterations (`0`–`255`). Pass `--tip_iteration=0` explicitly when unpruned differential tractography is needed for initial inspection. |
| `--check_ending` | `0` (`1` with `--track_id`) | Enable atlas endpoint checking (`0` or `1`; forced to `0` when `--dt_metric1` is active). |
| `--search_count` | `16` | Registration search attempts when mapping to template/atlas space (controls spatial normalization, not streamline seeding). |
| `--random_seed` | `0` | Deterministic RNG seed offset (`0`–`255`, `uint8_t`) added to the streamline seed index. |
| `--parameter_id` | — | Hex/run-length DSI Studio parameter string restoring the full `TrackingParam` state. Applied after individual tracking flags in `setup_trk_param` (overrides preceding parameter flags). |

## Region options

| Option | Role |
|:--|:--|
| `--seed` | Seed tracking start points inside the region. |
| `--roi`, `--roi2`..`--roi5` | Keep streamlines passing through every specified ROI (AND logic across `--roi`..`--roi5`). |
| `--roa`, `--roa2`..`--roa5` | Discard streamlines entering any region of avoidance. |
| `--end`, `--end2` | Keep streamlines whose endpoints fall inside the region(s). |
| `--ter`, `--ter2`..`--ter5` | Terminate streamline propagation upon entering the region without discarding the streamline. |
| `--nend` | Exclude streamlines ending inside the region while permitting pass-through. |
| `--lim` | Restrict streamline propagation to remain inside the limiting mask. |
| `--track_id` | Single-bundle atlas pathway name or `0`-based index (legacy in `trk`; prefer `--action=atk` for automated bundle mapping). |
| `--tolerance` | Atlas distance tolerance in mm used with `--track_id` (default `12 * template_vs`, i.e. `24` mm for human). |
| `--use_roi` | Apply companion tractography atlas ROI/ROA constraints with `--track_id` (`1` default, or `0`). |

Each region option accepts comma-separated items combining region sources and in-place modifiers:

- **Region files:** `.nii.gz`, `.nii`, `.txt`, or `.mat`. Include `mni` in a NIfTI filename (e.g. `lesion_mni.nii.gz`) to warp template/MNI masks to native diffusion space; for native T1w/T2w masks with different matrix dimensions, pass the anatomical volume via `--other_slices=t1w.nii.gz` to supply the registration transform.
- **Atlas or multi-label regions:** `<atlas>:<region_name_or_index>` (e.g. `HCP-MMP:L_V1`, `FreeSurferDKT:10`) or `<nifti_file>:<region_name>`. Multiple comma-separated regions within a single flag (e.g. `--roi=A.nii.gz,B.nii.gz`) are merged (OR logic), whereas separate flags (`--roi` + `--roi2`) enforce AND logic.
- **Region modifiers:** `dilation`, `erosion`, `smoothing`, `opening`, `closing`, `defragment`, `negate`, `flipx`, `flipy`, `flipz`, `shiftx`, `shiftnx`, `shifty`, `shiftny`, `shiftz`, `shiftnz` — e.g. `--roi=left_mask.nii.gz,dilation,smoothing`.

See [ROI-Based Fiber Tracking](/doc/gui_t3_roi_tracking.html) for the anatomical behavior of each region role.

## Post-tracking options

| Option | Default | Description |
|:--|:--|:--|
| `--output=<file>` | `<source>.tt.gz` | Save streamlines to `<file>`, write into an existing directory path, or pass `no_file` to suppress tract file output. |
| `--trk_format=<ext>` | `tt.gz` | Extension appended when `--output` has no recognized tract/image suffix (`tt.gz`, `trk.gz`, `trk`, `tck`, `trx`, `txt`, `mat`, `nii.gz`, `nii`). |
| `--ref=<nifti>` | — | Transform saved tractography and `--export=tdi` volumes into the grid of a reference NIfTI image (e.g. T1w/T2w). |
| `--delete_repeat=<dist>` | `0.5` | Remove near-duplicate streamlines within `<dist>` voxel distance units (L1 distance in voxel coordinates; value is optional and defaults to `0.5` voxels, also enabled automatically when `--track_id` is set). |
| `--delete_by_length=<voxels>` | `1` | Remove streamlines shorter than `<voxels>` voxel units (or having `<= 2` points; value is optional and defaults to `1` voxel). |
| `--overlap=<atlas>` | — | Save voxelized intersection statistics for nonempty labels of built-in `<atlas>` to `<tract_file>.<atlas>.overlap.txt`. |
| `--cluster=<method>,<count>,<detail>,<out>` | `<tract_file>_cluster.txt` | Cluster streamlines: `method` `0` = hierarchical tract distance (requires `detail > 0` mm), `1` = k-means, `2` = EM; `count > 0`. The 4th field `<out>` is optional. |
| `--recognize=<prefix>` | — | Classify streamlines against the tractography atlas and save `<prefix>.label.txt` (cluster IDs) and `<prefix>.name.txt` (bundle names). |
| `--template_track=<file>` | `T_<tract_file>` | Save streamlines warped into template voxel space (`<file>` is optional; empty value prefixes `T_`). |
| `--mni_track=<file>` | `mni_<tract_file>` | Save streamlines warped into MNI millimeter space (`<file>` is optional; empty value prefixes `mni_`). |
| `--end_point=<file>` | `<tract_file>.end.txt` | Export coordinates from both streamline ends as `.txt` or `.mat` (`<file>` is optional). |
| `--end_point1=<file>`, `--end_point2=<file>` | `<tract_file>.end1.txt`, `.end2.txt` | Export the two separated endpoint voxel sets as `.txt`, `.mat`, `.nii`, or `.nii.gz` (`<file>` is optional). |
| `--export=<items>` | — | Comma-separated exports:<br>• `stat` — bundle summary statistics (`<tract_file>.stat.txt`)<br>• `tdi`, `tdi<ratio>`, `tdi_color`, `tdi_end`, `tdi<ratio>_color_end` — track density NIfTI (`<tract_file>.<item>.nii.gz`, e.g. `tdi`, `tdi2`, `tdi_color`, `tdi2_color_end`)<br>• `<metric>`, `<metric>.txt`, or `<metric>.mat` — per-point values along each streamline for any FIB or `--other_slices` metric (e.g. `qa`, `fa`, `md.mat`) |
| `--profile=<metric>:<type>:<bw>` | — | Along-tract or axis profile (`<bw> > 0` bandwidth) saved to `<tract_file>.<metric>.<type>.<bw>.txt`. `type`: `0` = x, `1` = y, `2` = z, `3` = left-first, `4` = posterior-first, `5` = superior-first, `6` = right-first, `7` = anterior-first, `8` = inferior-first. (`--export=report` is obsolete; use `--profile`.) |

## Connectivity matrices

Region-to-region (`r2r`) and tract-to-region (`t2r`) structural connectivity can be computed directly from the generated tractography. For the GUI workflow and interpretation, see [Connectome (R2R / T2R)](/doc/gui_connectome.html):

| Option | Default | Description |
|:--|:--|:--|
| `--connectivity=<list>` | — | Comma-separated built-in atlas names (without `.`, e.g. `HCP-MMP`, `AAL3`) or NIfTI parcellation files (containing `.`, e.g. `aparc.nii.gz`, `parc_mni.nii.gz`). |
| `--connectivity_type=<list>` | `pass` | Comma-separated `pass` (regions traversed by streamlines) and/or `end` (regions containing streamline endpoints). |
| `--connectivity_value=<list>` | `all` | Comma-separated metric substrings (e.g. `count`, `ncount`, `mean_length`, `qa`, `fa`) or `all` for connectogram and network outputs. |
| `--connectivity_output=<list>` | `matrix` | Outputs to write (substring-matched, comma-separated):<br>• `matrix` — `<tract_file>.<atlas>.connectivity.mat` (includes `r2r` and `t2r` matrices)<br>• `connectogram` — `<tract_file>.<atlas>.<metric>.connectogram.txt`<br>• `network` — `<tract_file>.<atlas>.<metric>.network_measures.txt` |

Example:

```bash
dsi_studio --action=trk \
  --source=subject.fz \
  --tract_count=100000 \
  --connectivity=HCP-MMP \
  --connectivity_type=end \
  --connectivity_value=count,qa \
  --connectivity_output=matrix,network
```

For bundle-specific **tract-to-region** connectomes across named white-matter pathways, use `--action=atk`.

## Differential tracking

Differential **Type 1–4** study designs describe longitudinal vs reference comparison and native vs template space; they are distinct from the numeric `--dt_threshold_type` formula index below.

| Option | Default | Description |
|:--|:--|:--|
| `--dt_metric1` | required with `--dt_metric2` | First metric (`m1`): exact metric name in the FIB or `--other_slices` (or literal `zero` / `one`). |
| `--dt_metric2` | required with `--dt_metric1` | Second metric (`m2`): exact metric name in the FIB or `--other_slices` (or literal `zero` / `one`). |
| `--dt_threshold` | `0.2` | Minimum differential value required during tracking (e.g. `0.2` = 20% decrease under formula `0`). |
| `--dt_threshold_type` | `0` | Differential formula: `0` = `(m1-m2)/m1`, `1` = `(m1-m2)/m2`, `2` = `m1-m2`, `3` = `(m2-m1)/m1`, `4` = `(m2-m1)/m2`, `5` = `m2-m1`, `6` = `m1/max(m1)`, `7` = `m2/max(m2)`. |

When `--dt_metric1` and `--dt_metric2` are active, `--tip_iteration` defaults to `16` (pass `--tip_iteration=0` explicitly for unpruned initial review), `--check_ending` is forced to `0`, and `--tract_count` is ignored in favor of `--seed_count` (or `--track_voxel_ratio` as a seed-to-voxel ratio).

```bash
dsi_studio --action=trk \
  --source=baseline.fz \
  --other_slices=followup_qa.nii.gz \
  --dt_metric1=qa \
  --dt_metric2=followup_qa \
  --dt_threshold_type=0 \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --tip_iteration=0 \
  --output=dt_decrease_20pct.tt.gz
```

For study design, custom slice registration, sham/FDR evaluation, and incremental pruning, see [Differential Tractography](/doc/gui_t3_dt.html).
