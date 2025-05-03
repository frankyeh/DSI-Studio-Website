# Connectometry Analysis

> Use `--action=cnt` to initiate [Step C3 Connectometry].

## Examples

**1. Use a multiple regression model with three variables (0:SEX, 1:BMI, 2:AGE) to study how BMI (the second variable) affects the brain connection in the CMU 60 connectometry database.**

```bash
dsi_studio --action=cnt --source=CMU60.db.fib.gz --demo=CMU60.txt --variable_list=0,1,2 --voi=1 --roi=FreeSurferDKT:left_precentral --output=CMU60
```

**2. Use ROI and ROA to limit connectometry analysis on subjects with specific demographics: scanner=1 and group not equal to 3.**

```bash
dsi_studio --action=cnt --source=study.db.fib.gz --demo=demo.csv --roi=ROI.nii.gz --roa=ROA.nii.gz --select="scanner=1,group/3" --variable_list=2,4,5 --voi=5
```

**3. Windows batch script to test each variable from 5 to 37, with variables 2 and 3 included as covariates.**

```batch
for /l %%G in (5,1,37) do (
dsi_studio --action=cnt --source=connectometry.sdf.db.fib.gz --demo=PANSS.csv --variable_list=2,3,%%G --voi=%%G > log%%G.txt
)
```

---

## Core Functions

| **Parameter**    | **Description**                                                                 |
|-------------------|---------------------------------------------------------------------------------|
| `source`         | Specify the `db.fib.gz` file.                                                   |
| `demo`           | Assign the path to the demographic CSV or text file.                           |
| `variable_list`  | Assign the study variables for regression. Use commas to include multiple variables. <br> Example: `--variable_list=0,1,2` includes the first three variables. |
| `voi`            | Specify the "variable of interest" for multiple regression. <br> Use `--voi=0` for the first variable, `--voi=1` for the second, etc. <br> For longitudinal analysis, use `--voi=longitudinal`. |

---

## Parameters 

| **Parameter**     | **Default**          | **Description**                                                                 |
|--------------------|----------------------|---------------------------------------------------------------------------------|
| `t_threshold`     | `2.5`               | Assign the t-threshold for correlational tracking.                             |
| `length_threshold`| `20` voxel distance | Specify the minimum tracking length for correlations.                          |
| `fdr_threshold`   | `0`                 | Set a nonzero FDR threshold (e.g., 0.05) to enable FDR control.                |
| `permutation`     | `2000`              | Specify the number of permutations used in analysis.                           |
| `thread_count`    | Hardware max        | Define the number of threads for computation.                                  |
| `tip_iteration`   | `16`                | Number of pruning iterations to reduce noise.                                  |
| `exclude_cb`      | `1`                 | Use `--exclude_cb=1` to exclude the cerebellum from analysis.                  |
| `no_tractogram`   | `1`                 | Set to `1` to avoid generating 3D tractogram images (output files are still generated). |
| `output`          | Demographics path   | Specify the prefix for all output files. Defaults to the demographics file name with the study variable appended. |

---

## Optional Functions

| **Parameter**      | **Description**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| `select`          | Select subjects for analysis. Example: `--select=Gender=1,Age>20` filters subjects with Gender=1 and Age > 20. |
| `seed, roi, roa...`| Define regions to limit the tracking areas (see `--action=trk` for details).    |

---

## Updates Based on Code Features

### Features Found in Source Code:
- **Multiple Regression**: The class `group_connectometry_analysis` handles the regression model and adjusts QA values based on statistical models.
- **FDR Analysis**: The function `calculate_FDR` computes the false discovery rate to identify significant findings.
- **Exclusion of Cerebellum**: The method `exclude_cerebellum` removes cerebellum-related data when `exclude_cb=1`.
- **SPM Generation**: Statistical Parametric Mapping is computed via `calculate_spm` and results are saved using `save_result`.
- **Report Generation**: The `generate_report` method creates a detailed HTML visualization of findings.
