# Connectometry Analysis

Run group connectometry (correlational tractography) from a `.dz` connectometry database using `--action=cnt` (GUI-backed) or `--action=cnt_cl` (headless).

## Examples

### Multiple regression

Fit a regression model with three demographic columns (`0:SEX`, `1:BMI`, `2:AGE`) and test `BMI` (`--voi=1`):

```bash
dsi_studio --action=cnt \
  --source=CMU60.dz \
  --index_name=qa \
  --effect_size=0.3 \
  --demo=CMU60.txt \
  --variable_list=0,1,2 \
  --voi=1
```

### Cohort selection and anatomical constraints

```bash
dsi_studio --action=cnt \
  --source=study.dz \
  --demo=demo.csv \
  --effect_size=0.2 \
  --roa=excluded_mni_region.nii.gz \
  --select="scanner=1,group/3" \
  --variable_list=2,4,5 \
  --voi=5
```

### Longitudinal change

Test overall longitudinal change on a pairwise difference database (`--voi=longitudinal`) with FDR length selection:

```bash
dsi_studio --action=cnt \
  --source=study.dif.dz \
  --demo=demo.tsv \
  --index_name=dti_fa \
  --voi=longitudinal \
  --variable_list=0 \
  --effect_size=0.3 \
  --fdr_threshold=0.05 \
  --output=longitudinal_fa
```

### Headless and batch execution

Use `--action=cnt_cl` on headless servers without a display, and `*` wildcards to batch-process multiple databases with distinct output prefixes:

```bash
dsi_studio --action=cnt_cl \
  --source=*.dz \
  --demo=participants.tsv \
  --index_name=qa \
  --variable_list=0,1,2 \
  --voi=0 \
  --fdr_threshold=0.05 \
  --thread_count=8 \
  --output=*.cnt
```

## Core Parameters

| Parameter | Requirement | Description |
|:--|:--|:--|
| `--source` | Required | Connectometry database (`.dz`; legacy `.db.fz` and `.db.fib.gz` remain supported). |
| `--index_name` | Optional | Diffusion metric in the database, such as `qa`, `dti_fa`, or `rdi`. Defaults to the first stored index (`db.index_list.front()`). |
| `--demo` | Conditional | Demographic `.csv` or tab-separated (`.tsv`/`.txt`) table. Required when demographics are not embedded in the database; overrides embedded demographics when supplied. |
| `--variable_list` | Required (diffusion) | Comma- or space-separated **zero-based feature indices** (or exact feature titles) in the regression model, e.g. `0,1,2`. Must include at least one valid feature column even with `--voi=longitudinal`; `--voi` is added automatically if omitted. |
| `--voi` | Required (diffusion) | **Zero-based feature index** (or exact feature title) of the variable of interest. Use `--voi=longitudinal` or `--voi=Intercept` on a longitudinal difference database. |
| `--output` | Optional | Prefix for output files. When omitted, writes to the current working directory using `<index_name>.<voi>.r<int(rho*10)>.l<length>` (or `.fdr<fdr>` when `--fdr_threshold` is nonzero). |

At startup, DSI Studio prints `available index name:` and `selectable variables include` with zero-based feature indices and titles. Two-level text columns are encoded as `0` and `1` and renamed `<column>(0=<value0> 1=<value1>)`; columns with more than two non-numeric text values are skipped. Empty, `na`, or `n/a` entries in any selected column exclude those subjects. Derived or longitudinal databases do not automatically inherit external demographics—verify available columns and supply `--demo` when needed.

## Analysis Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--effect_size` | `0.3` | Correlation (`rho`) propagation threshold used when `--t_threshold` is omitted (`t = rho * sqrt(n - 2) / sqrt(1 - rho^2)`). |
| `--t_threshold` | none (`2.5` for task fMRI) | Local T-statistic propagation threshold; overrides `--effect_size` when specified (`rho = t / sqrt(t^2 + n - 2)`). |
| `--otsu` | `0.6` (`0.3` for task fMRI) | Otsu multiplier for fiber tracking and whole-brain seeding, randomized per seed across `otsu ± 0.1` (`fiber_threshold = (otsu - 0.1) * fa_otsu`). |
| `--length_threshold` | `(dim[0]/4)/5*5` | Minimum track length in **voxel distance**. Selects reported tracks directly when `--fdr_threshold=0`, or sets the minimum candidate length evaluated when `--fdr_threshold` is nonzero. |
| `--fdr_threshold` | `0` | Nonzero value (e.g. `0.05`) truncates reported tracts to the shortest voxel length `>= length_threshold` with `FDR <= fdr_threshold`; `0` disables FDR length selection. |
| `--permutation` | `2000` | Number of randomized permutations used to estimate the null track-length distribution and FDR. |
| `--normalize_iso` | `1` | Set `1` to normalize `qa`, `rdi`, or `nrdi*` by `iso` on non-longitudinal databases containing `iso`; `0` disables. |
| `--tip_iteration` | `16` | Topology-informed pruning (TIP) iterations applied to observed and null tractograms before FDR calculation (`0` disables). |
| `--region_pruning` | `1` | Set `1` to remove fragmented findings (`region_pruning(0.2)`), `0` to disable. |
| `--no_tractogram` | `1` | Set `0` under `--action=cnt` to render 3D tractogram and ROI `.jpg` figures (`1` skips rendering and is enforced in `--action=cnt_cl`). |

## Cohort and Region Options

| Parameter | Default | Description |
|:--|:--|:--|
| `--select` | none | Comma-separated subject filters using `=`, `/` (not equal), `>`, or `<` (3-decimal precision), e.g. `--select="Gender=1,Age>20"`. Match exact printed feature titles (including `(0=... 1=...)` on encoded text columns); `value<op><num>` filters across all selected features, and `subject<op><text>` filters by subject name (`<` keeps substring matches, `>` excludes substring matches, `/` keeps exact match, `=` excludes exact match). Remaining cohort size `n` must exceed `2` and the model column count. |
| `--exclude_cb` | `0` (`1` for task fMRI) | Set `1` to exclude `BrainSeg:Cerebellum` as a terminative region on human databases. |
| `--seed`, `--roi`, `--roi2`–`--roi5`, `--roa`, `--roa2`–`--roa5`, `--end`, `--end2`, `--ter`, `--ter2`–`--ter5`, `--nend`, `--lim` | whole-brain seed | Anatomical region constraints using the same file, `<atlas>:<label>`, and region-operation syntax as [`--action=trk`](/doc/cli_t3.html) (`--other_slices` supported when NIfTI regions require slice transforms). Whole-brain seeding is used when `--seed` is omitted. |
| `--track_id`, `--tolerance`, `--use_roi` | none (`--use_roi=1`) | Atlas bundle region constraints (`--track_id` accepts bundle name or index; `--tolerance` defaults to the database default tolerance). |

## Task fMRI Parameters

When `--source` contains a `bold` time-series matrix, `--action=cnt` runs first-level task fMRI correlational tractography (SPM canonical HRF, 128-s DCT high-pass, and synchronized Rademacher wild-bootstrap resamples) instead of group demographic regression:

| Parameter | Default | Description |
|:--|:--|:--|
| `--tr` | database `TR` | Repetition time in seconds (`> 0`); required when `TR` is not stored in the database. |
| `--events` | database `events` | BIDS TSV file with `onset`, `duration`, and optional `trial_type` columns; required when `events` is not stored in the database. |
| `--condition` | first `trial_type` (`task` if untyped) | Task condition name to test. |
| `--ar1` | `1` | Set `1` for global AR(1) Cochrane-Orcutt GLS whitening, `0` for uncorrected OLS. |

## Execution and Batch Options

| Option | Description |
|:--|:--|
| `--action=cnt` / `--action=cnt_cl` | `--action=cnt` starts the Qt GUI application (`--no_tractogram=0` and `--stay_open` supported). `--action=cnt_cl` runs headlessly via `QCoreApplication` with no display requirement. |
| `--thread_count` | Maximum worker threads used during preprocessing and permutation testing. |
| `--loop` | Wildcard file pattern for batch execution; a `*` in `--source` also enables looping automatically and substitutes `*` in other options such as `--output`. |
| `--continue_on_error` | Continue remaining batch iterations when one file fails. |
| `--stay_open` | Keep the DSI Studio GUI open after `--action=cnt` completes instead of exiting. |

## Output Files

| File | Condition | Description |
|:--|:--|:--|
| `<prefix>.inc.tt.gz` / `<prefix>.inc.no_tract.txt` | Always | Positive-association tractogram, or an empty `.no_tract.txt` marker when zero tracts survive. |
| `<prefix>.dec.tt.gz` / `<prefix>.dec.no_tract.txt` | Always | Negative-association tractogram, or an empty `.no_tract.txt` marker when zero tracts survive. |
| `<prefix>.fdr_dist.values.txt` | Always | Tab-separated length histogram and FDR table (`voxel_dis`, `fdr_inc`, `fdr_dec`, null and observed counts). |
| `<prefix>.t_statistics.fz` | Always | FIB-compatible file storing per-fiber `inc_t*` and `dec_t*` T-statistic maps. |
| `<prefix>.report.html` | Always | HTML report of acquisition metadata, analysis parameters, and `inc`/`dec` findings. |
| `<prefix>.inc.jpg`, `<prefix>.inc_map.jpg`, `<prefix>.inc_map2.jpg`, `<prefix>.dec.jpg`, `<prefix>.dec_map.jpg`, `<prefix>.dec_map2.jpg`, `<prefix>.pos_neg.jpg` | `--no_tractogram=0` (`--action=cnt`) | Rendered 3D tractogram and ROI slice views (`.pos_neg.jpg` is written when at least one side has a finding). |

For the GUI workflow, database creation, demographic loading, longitudinal-database handling, and interpretation of FDR, see [Correlational Tractography / Connectometry](/doc/gui_cx.html).
