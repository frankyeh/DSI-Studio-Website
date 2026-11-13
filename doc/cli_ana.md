# Region and Tract Analysis

Use `--action=ana` to run quantitative analysis, filtering, atlas overlap, clustering, recognition, and connectome generation on existing tractography or regions using a `.fz` (`.fib.gz`) file.

Mode selection follows a fixed precedence:
1. `--region` or `--atlas` runs **region analysis** (even if `--tract` is also present).
2. `--tract` runs **tract analysis**.
3. `--info` writes the whole-volume **fiber coherence index** (`fiber coherence index\t<value>`) to the specified text file.
4. `--export` alone falls back to whole-volume metric export.

## Tract Analysis Examples

### Tract statistics

Compute bundle shape and diffusion statistics (saved to `tract.tt.gz.stat.txt`):

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --export=stat
```

Sample additional coregistered scalar maps (such as DKI or NODDI outputs) alongside FIB metrics:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --other_slices=DKI.nii.gz,ODI.nii.gz \
  --export=stat
```

Batch-compute tract statistics across subjects using `*` wildcard matching:

```bash
dsi_studio --action=ana \
  --source=*.fz \
  --tract=*.CST.tt.gz \
  --export=stat
```

### Tract overlap with a built-in atlas

Voxelize a single tract bundle and report quantitative statistics for its intersection with every nonempty label of a built-in atlas:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=CST.tt.gz \
  --overlap=HCP-MMP
```

Nonempty atlas intersections are written as columns in standard region-statistics format (zero-overlap labels are omitted) to:

```text
CST.tt.gz.HCP-MMP.overlap.txt
```

Without `--output` (or with `--output=no_file`), the overlap filename uses `<tract_file>.<atlas>.overlap.txt` without re-saving the tract. Passing `--output=<file_or_dir>` saves the tract (or NIfTI mask) to `--output` and uses that path as the base for `.<atlas>.overlap.txt`.

### Merge tract files

Pass `--merge_all` to combine multiple tract files (or clusters within a clustered tract file) into a single unified bundle that supports all post-tracking operations:

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract1.tt.gz,tract2.tt.gz \
  --merge_all \
  --output=merged_tracts.tt.gz
```

Omit `--merge_all` to preserve separate bundle/cluster IDs when saving to `.tt.gz`, `.trk.gz`, or `.trx` (writes `<output>.txt` cluster names for `.tt.gz`/`.trk.gz`) and optionally export a side-by-side multi-bundle table (`tract1.tt.gz.stat.txt`):

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract1.tt.gz,tract2.tt.gz \
  --name=bundle_names.txt \
  --output=clustered_tracts.tt.gz \
  --export=stat
```

### Filter a tract by ROI

Filter streamlines using NIfTI masks, `<atlas>:<label>` regions, morphological modifiers, pruning, or duplicate/length filters:

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tracts.tt.gz \
  --roi=roi.nii.gz \
  --output=filtered_tract.tt.gz
```

Combine built-in atlas regions, morphological dilation, exclusion masks, and topology-informed pruning (TIP):

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=whole_brain.tt.gz \
  --roi=HCP-MMP:Primary_Motor_Cortex_L,dilation \
  --roa=FreeSurferSeg:Brain-Stem_R \
  --tip_iteration=4 \
  --delete_repeat=0.5 \
  --output=CST_L_clean.tt.gz
```

### Export tract density imaging

Export voxel-grid, subvoxel (`tdi2` = 2x resolution), directional RGB (`tdi_color`), or endpoint (`tdi_end`) tract density NIfTI volumes:

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract.tt.gz \
  --export=tdi,tdi2_color,tdi_end
```

### Convert tractography to a NIFTI region/density map

Save a single bundle as a binary voxelized NIfTI mask:

```bash
dsi_studio --action=ana \
  --source=avg.mean.fz \
  --tract=tract.tt.gz \
  --output=tract_region.nii.gz
```

When multiple bundles/clusters are loaded without `--merge_all`, saving `--output=<file>.nii.gz` exports a multi-bundle density map across all loaded clusters.

### Along-tract profiles and per-point metrics

Compute a smoothed 100-step profile with 95% confidence intervals (`tract.tt.gz.qa.3.2.txt`), or export per-point metric values along each streamline:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --profile=qa:3:2 \
  --export=qa,dti_fa.mat
```

### Clustering, atlas recognition, and coordinate export

Cluster streamlines or label them against the built-in tractography atlas, and export MNI-space tracts or endpoint regions:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=whole_brain.tt.gz \
  --recognize=recognized_bundles \
  --output=labeled_whole_brain.tt.gz

dsi_studio --action=ana \
  --source=my.fz \
  --tract=CST.tt.gz \
  --mni_track=CST_mni.tt.gz \
  --end_point1=CST_end1.nii.gz \
  --end_point2=CST_end2.nii.gz
```

## Connectivity Analysis

Calculate region-to-region (`r2r`) and tract-to-region (`t2r`) connectivity matrices using built-in atlases or NIfTI parcellations:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --connectivity=HCP-MMP,Brodmann
```

Use endpoint-based connectivity and export matrix, connectogram, and graph-network tables for selected metrics:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --tract=tract.tt.gz \
  --connectivity=HCP-MMP \
  --connectivity_type=end \
  --connectivity_value=count,qa,ncount \
  --connectivity_output=matrix,connectogram,network
```

| Parameter | Default | Description |
|:--|:--|:--|
| `--connectivity` | none | Comma-separated built-in atlas names (no `.`) or NIfTI parcellation files (contains `.`, such as `aparc+aseg.nii.gz`). |
| `--connectivity_type` | `pass` | Comma-separated `pass` (streamline pass-through) and/or `end` (streamline endpoints). |
| `--connectivity_value` | `all` | `all` or comma-separated metric substrings (e.g. `count`, `ncount`, `mean_length`, `qa`, `dti_fa`) filtering `connectogram` and `network` files. `.connectivity.mat` always stores all `r2r` and `t2r` metrics. |
| `--connectivity_output` | `matrix` | Any combination of `matrix` (`<base>.<atlas>.connectivity.mat`), `connectogram` (`<base>.<atlas>.<metric>.connectogram.txt`), and `network` (`<base>.<atlas>.<metric>.network_measures.txt`). |

## Tract Analysis Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--tract` | Required (tract mode) | Input tract files (`.tt.gz`, `.trk.gz`, `.trk`, `.trx`, `.tck`, `.mat`, `.txt`), comma-separated or `*` wildcard. Filenames containing `mni` warp from template/MNI space to native space on native FIBs. Clustered files read cluster names from `<tract_file>.txt` if present. |
| `--name` | none | Text file with one bundle name per line assigned sequentially to loaded bundles/clusters. |
| `--merge_all` | disabled | Merge multiple `--tract` files or clusters into one bundle. Without `--merge_all`, multi-cluster inputs support only `--name`, `--tip_iteration`, ROI filtering, `--output` (`.tt.gz`, `.trk.gz`, `.trx`, `.nii.gz`), and `--export=stat`. |
| `--tip_iteration` | `0` (if value omitted) | Topology-informed pruning (TIP) iterations applied to each loaded bundle before ROI filtering. |
| `--roi`, `--roi2`–`--roi5` | none | Keep streamlines passing through every specified ROI (AND logic; comma-separated entries within one `--roi` flag are unioned). Accepts `.nii.gz`/`.nii`/`.txt`/`.mat`, `<file.nii.gz>:<label>`, `<atlas>:<label>`, and comma-chained modifiers: `dilation`, `erosion`, `smoothing`, `opening`, `closing`, `defragment`, `negate`, `flipx`, `flipy`, `flipz`, `shiftx`, `shiftnx`, `shifty`, `shiftny`, `shiftz`, `shiftnz`. |
| `--roa`, `--roa2`–`--roa5` | none | Exclude streamlines entering any specified region of avoidance (supports the same region syntax and modifiers as `--roi`). |
| `--end`, `--end2` | none | Keep streamlines with at least one endpoint in `--end` (single region) or connecting `--end` and `--end2` (two regions). |
| `--nend` | none | Exclude streamlines terminating in the specified region while allowing pass-through streamlines. |
| `--lim` | none | Exclude streamlines stepping outside the specified limiting mask. |
| `--delete_repeat` | `0.5` (if value omitted) | Delete near-duplicate streamlines within the given voxel distance threshold (single-bundle mode). |
| `--delete_by_length` | `1` (if value omitted) | Delete streamlines shorter than the given length in **voxel units** (single-bundle mode). |
| `--overlap` | none | Exact built-in atlas name for tract-atlas overlap statistics (`<base>.<atlas>.overlap.txt`; zero-overlap labels omitted; single-bundle mode). |
| `--cluster` | none | `<method>,<count>,<detail>` or `<method>,<count>,<detail>,<output_txt>`. `method`: `0` = spatial tract distance (`detail > 0` in mm; keeps top `count` clusters), `1` = feature k-means (`count > 0`, `detail >= 0`), `2` = feature EM (`count > 0`, `detail >= 0`). Writes per-streamline cluster IDs to `<output_txt>` (default `<tract_base>_cluster.txt`) and updates in-memory cluster IDs for `--output`. |
| `--recognize` | none | `<output_prefix>`. Matches streamlines against the tractography atlas, writes `<output_prefix>.label.txt` and `<output_prefix>.name.txt`, and sets cluster IDs for `--output`. |
| `--output` | none (`no_file`) | Output file or existing directory. Single-bundle mode saves `.tt.gz`, `.trk.gz`, `.trk`, `.tck`, `.trx`, `.txt`, `.mat`, or binary mask `.nii.gz`/`.nii`, and sets `<base>` for downstream exports. Multi-cluster mode without `--merge_all` accepts `.tt.gz`, `.trk.gz`, `.trx`, or `.nii.gz`. |
| `--trk_format` | `tt.gz` | Default extension (without leading dot) appended when `--output` has no recognized tract/image suffix in single-bundle mode. |
| `--overwrite` | `1` | Set `0` to skip overwriting an existing multi-cluster `.nii.gz` output file. |
| `--ref` | none | Reference NIfTI image (e.g. T1w/T2w) defining the output grid/space for single-bundle `--output` and `--export=tdi*`. |
| `--template_track` | `T_<tract_file>` (if empty) | Warp and save native streamlines in template voxel space (single-bundle mode). |
| `--mni_track` | `mni_<tract_file>` (if empty) | Warp and save native streamlines in MNI coordinates (single-bundle mode). |
| `--end_point` | `<base>.end.txt` (if empty) | Save both endpoints of each streamline in voxel coordinates as `.txt` or `.mat`. |
| `--end_point1`, `--end_point2` | `<base>.end1.txt`, `<base>.end2.txt` | Save separated first and second streamline endpoint voxel sets as `.txt`, `.mat`, `.nii`, or `.nii.gz` region files. |
| `--export` | none | Comma-separated tract exports: `stat` (`<base>.stat.txt`; includes per-subject rows when `--source` is a `.dz` database), `tdi` variants (`tdi`, `tdi<ratio>`, `tdi_color`, `tdi_end` -> `<base>.<token>.nii.gz`), or any loaded scalar metric name `<metric>` / `<metric>.<ext>` (`<base>.<metric>.txt` default, or `.mat`, `.trk.gz`, `.trk` per-point scalar values; `.tt.gz` writes streamline geometry only without per-point scalars). |
| `--profile` | none | `<metric>:<direction>:<bandwidth>` (`bandwidth > 0`). Writes smoothed profile + 95% CI to `<base>.<metric>.<direction>.<bandwidth>.txt`. `direction`: `0`=X, `1`=Y, `2`=Z (voxel axis at 0.5-voxel steps), `3`=left-first, `4`=posterior-first, `5`=superior-first, `6`=right-first, `7`=anterior-first, `8`=inferior-first (100 steps). |

## Region Analysis

Analyze quantitative diffusion and slice metrics across one or more regions:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --region=roi1.nii.gz,roi2.nii.gz
```

Analyze a specific parcel from a labeled NIfTI file, a single built-in atlas parcel, or all parcels in one or more built-in atlases:

```bash
dsi_studio --action=ana --source=my.fz --region=labels.nii.gz:Hippocampus

dsi_studio --action=ana --source=my.fz --region=AAL2:Hippocampus_L

dsi_studio --action=ana --source=my.fz --atlas=HCP-MMP,FreeSurferSeg
```

### Region overlap with a built-in atlas

Intersect a single source region with every label of a built-in atlas without loading atlas parcels individually:

```bash
dsi_studio --action=ana \
  --source=my.fz \
  --region=tumor.nii.gz \
  --overlap=CHA \
  --output=tumor_CHA.txt
```

`--overlap` requires exactly one loaded region (`regions.size() == 1`). Each nonempty intersection becomes a result column using the standard region-statistics rows (voxel count, volume, center, and mean/SD across FIB and `--other_slices` metrics), and zero-overlap labels are omitted.

Default output filenames when `--output` is omitted:
- Standard region statistics: `<source>.statistics.txt`
- Region atlas overlap (`--overlap=<atlas>`): `<source>.<atlas>.overlap.txt`

If `--output` points to an existing directory, DSI Studio writes that default filename inside the directory; otherwise `--output` sets the output filename (appending `.txt` if missing).

Supported `--region` forms:

```text
--region=mask.nii.gz
--region=parcellation.nii.gz
--region=parcellation.nii.gz:Hippocampus
--region=AAL2:Hippocampus_L
--region=roi1.nii.gz,roi2.nii.gz,roi3.txt,roi4.mat
```

| Parameter | Default | Description |
|:--|:--|:--|
| `--region` | Conditional | Comma-separated NIfTI files (`.nii.gz`, `.nii`: binary mask, 3D integer label volume `<= 65535`, or 4D multi-ROI volume), `<file.nii.gz>:<label>`, `<atlas>:<label>`, or `.txt`/`.mat` region files. |
| `--atlas` | Conditional | Comma-separated built-in atlas names; loads every parcel of each listed atlas for region statistics. |
| `--overlap` | none | Exact built-in atlas name for region-atlas overlap statistics. Requires exactly one loaded region; defaults output to `<source>.<atlas>.overlap.txt`. |
| `--output` | `<source>.statistics.txt` or `<source>.<atlas>.overlap.txt` | Output `.txt` file path or destination directory. |

NIfTI region alignment and labeling rules:
- **MNI-space regions:** If a NIfTI filename contains `mni` and its grid dimensions differ from a native-space FIB, DSI Studio warps it from template/MNI space to native space (`mni` is ignored if dimensions already match the native FIB).
- **Native anatomical-space regions:** If a native-space NIfTI region has different dimensions from the FIB, pass its anatomical reference via `--other_slices=<T1w.nii.gz>` so DSI Studio applies the matching registration.
- **Label lookup:** Multi-label 3D NIfTI files load parcel names from a matching `<basename>.txt` (`<index> <name>`) or `<basename>.json` (`arealabel` / `labelIndex`) file when present; filenames containing `aparc` or `aseg` automatically use the built-in FreeSurfer lookup table.

## Exporting Whole-Volume Metrics

Whole-volume diffusion maps are exported with [`--action=exp`](/doc/cli_exp.html):

```bash
dsi_studio --action=exp --source=subject.fz --export=qa,iso,dti_fa,rd,ad
```

Calling `--action=ana --source=subject.fz --export=...` without `--region`, `--atlas`, `--tract`, or `--info` delegates to the same whole-volume exporter.

### Common Source, Slice, and Batch Options

| Parameter | Default | Description |
|:--|:--|:--|
| `--source` | Required | Input `.fz` / `.fib.gz` file (or `.dz` database for tract statistics), single-digit template index, or `*` wildcard pattern. |
| `--other_slices` | none | Comma-separated NIfTI volumes (or `*` wildcard) coregistered to the FIB. Sampled in region statistics, tract `--export=stat`, `--export=<metric>`, and `--profile`, and used to align native-space NIfTI regions/ROIs with non-DWI dimensions. |
| `--subject_demo` | none | Demographic `.csv`/`.tsv` file (matched by subject ID substring in `--source` or `--other_slices`) or literal demographic string attached when `--other_slices` is loaded on a single-subject FIB. |
| `--bias_field_correction` | `1` | Set `1` to run bias-field correction when loading a FIB that has not yet been bias-corrected; `0` disables. |
| `--info` | none | Output text file for the whole-volume fiber coherence index (`fiber coherence index\t<value>`), evaluated when `--region`, `--atlas`, and `--tract` are omitted. |
| `--loop` | `--source` when `--source` contains `*` | Wildcard file pattern for batch execution; substitutes `*` across companion options (`--tract`, `--region`, `--output`, `--other_slices`) and clears `--other_slices` between iterations. |
| `--continue_on_error` | disabled | Continue remaining wildcard batch iterations if one file fails. |
| `--thread_count` | hardware threads | Maximum worker threads (`tipl::max_thread_count`). |
