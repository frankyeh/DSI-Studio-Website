# Connectometry Analysis

Use `--action=cnt` to run group connectometry from a `.dz` connectometry database.

## Examples

### Multiple regression

Use a regression model with three variables (`0:SEX`, `1:BMI`, `2:AGE`) and test BMI as the variable of interest:

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

## Core Parameters

| Parameter | Description |
|:--|:--|
| `--source` | Connectometry database (`.dz`; legacy `.db.fz` and `.db.fib.gz` remain supported). |
| `--index_name` | Diffusion index to analyze, such as `qa` or `dti_fa`. If omitted, the first available index in the database is used. |
| `--demo` | Demographic CSV or text file. This is required when demographics are not already stored in the database. |
| `--variable_list` | Comma-separated **zero-based column indices** included in the regression model, for example `0,1,2`. |
| `--voi` | **Zero-based index** of the variable of interest within the demographic variables. For longitudinal analysis, use `--voi=longitudinal`. |

DSI Studio prints the available index names and demographic-variable indices when the analysis starts. Check **all available demographic columns** before launching a batch analysis rather than assuming a fixed column layout.

For a derived or longitudinal database, do not assume demographics from the source database were embedded or carried forward. Verify the available variables again and supply `--demo=<file>` when the required study variable or covariates are missing.

## Analysis Parameters

| Parameter | Default | Description |
|:--|:--|:--|
| `--effect_size` | `0.3` | Correlation/effect-size threshold when `--t_threshold` is not specified. |
| `--t_threshold` | optional | Use a local T threshold instead of `--effect_size`. |
| `--length_threshold` | data dependent | Minimum tracking length in voxel distance; derived from the database dimensions when omitted. |
| `--fdr_threshold` | `0` | Set a nonzero value such as `0.05` to enable FDR thresholding. |
| `--permutation` | `2000` | Number of permutations. |
| `--exclude_cb` | `0` | Set to `1` to exclude the cerebellum. |
| `--no_tractogram` | `1` | Do not generate 3D tractogram images in command-line mode. |
| `--normalize_iso` | `1` | Normalize QA/RDI by ISO for non-longitudinal databases. |
| `--tip_iteration` | `16` | Topology-informed pruning iterations. |
| `--region_pruning` | `1` | Remove fragmented findings using tract-coverage information. |

## Cohort and Region Options

| Parameter | Description |
|:--|:--|
| `--select` | Select a subset of subjects using demographic conditions, for example `--select="Gender=1,Age>20"`. |
| `--seed`, `--roi`, `--roa`, `--end`, `--ter`, `--nend`, `--lim` | Anatomical constraints using the same region syntax as [`--action=trk`](/doc/cli_t3.html). |
| `--output` | Prefix for output files. If omitted, DSI Studio derives an output name from the demographics/study variable. |

If no seed is provided, DSI Studio uses whole-brain seeding. Region constraints change the tested anatomical hypothesis and should be chosen before interpreting the result.

For the GUI workflow, database creation, demographic loading, longitudinal-database handling, and interpretation of FDR, see [Correlational Tractography / Connectometry](/doc/gui_cx.html).
