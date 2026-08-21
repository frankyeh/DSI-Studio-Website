# Correlational Tractography

<iframe width="560" height="315" src="https://www.youtube.com/embed/qC8jx6XZHGI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Correlational tractography identifies white-matter pathway segments whose local diffusion/connectome measurements are associated with a study variable across subjects. DSI Studio uses regression to account for selected covariates and applies local correlation statistics followed by deterministic tracking. [Connectometry](https://pubmed.ncbi.nlm.nih.gov/26499808/) uses permutation testing to estimate the false discovery rate (FDR) of the resulting tracks.

- **Tractography methods including correlational tractography:** Yeh et al., *NeuroImage* 245 (2021): 118651. [PubMed](https://pubmed.ncbi.nlm.nih.gov/34673247/)
- **Connectometry:** Yeh, Fang-Cheng, David Badre, and Timothy Verstynen. NeuroImage 125 (2016): 162-171. [PDF](/ref/Connectometry.pdf)

## Current workflow

1. **Step C1:** create a connectometry database (`.dz`) from subject FIB files.
2. **Step C2 (optional):** inspect/modify the database or calculate longitudinal change.
3. **Step C3:** load demographics, select the model and study variable, and run correlational tractography/connectometry.

> **Use the Sun version to create `.dz` databases.** Hou versions have a known issue when creating `.dz` files. Older `.db.fz` and `.db.fib.gz` databases remain supported for compatibility.

# Step C1: Create a connectometry database

A connectometry database combines diffusion measurements from multiple subjects in a common template space for population analysis.

Use subject FIB files (`.fz`; legacy `.fib.gz` is also supported). The subjects should be processed using compatible acquisition and reconstruction settings whenever possible. Both QSDR and native-space FIB files can be used; DSI Studio maps the subject measurements to the selected template during database construction.

## C1a. Add subject FIB files

Open **[Correlational Tractography][Step C1: Create a Connectometry Database]**.

Use **[Add]**, **[Search in Directory]**, or **[Open List]** to load the subject FIB files. Check the subject list before creating the database. For longitudinal studies, keeping repeat scans of each subject adjacent makes later pairing easier.

## C1b. Create the `.dz` file

The current database format is `.dz`. A new `.dz` database stores the available diffusion indices from the FIB files in one database, so a separate database is no longer needed for each metric.

Confirm the output name and click **[Create Database]**.

Command-line equivalent:

```bash
dsi_studio --action=atl --cmd=db --source=*.fz --output=study.dz
```

To embed demographics during database creation:

```bash
dsi_studio --action=atl --cmd=db --source=*.fz --demo=participants.tsv --output=study.dz
```

By default, current DSI Studio stores the available indices from the FIB files. Use `--index_name` only when you intentionally want a subset, for example:

```bash
dsi_studio --action=atl --cmd=db --source=*.fz --index_name=qa,dti_fa --output=study.dz
```

## C1c. Check registration quality

Database construction reports registration quality for each subject. Review subjects with low R2 or visibly poor alignment before group analysis. Acquisition differences, motion, incorrect b-tables, and registration failures can all create systematic effects that should not be interpreted as biological findings.

# Step C2: View or modify a database

Open the `.dz` file in **Step C2** when you need to inspect subjects, remove problematic data, load demographics, or prepare a longitudinal database.

Demographics can be stored inside the database. After loading a demographic CSV/TSV file, save the database if you want those values to be available automatically the next time it is opened.

## Longitudinal studies

For repeated scans of the same subjects:

1. Make sure baseline and follow-up scans are correctly paired.
2. Use **[Tools][Longitudinal scans...]** to review or define the matching.
3. Choose how the longitudinal change is calculated, such as `scan2-scan1`.
4. Save the resulting longitudinal database as a new `.dz` file.

The command line also supports paired longitudinal conversion. If scans are stored consecutively as baseline/follow-up pairs:

```bash
dsi_studio --action=atl --cmd=db --source=study.dz --match=consecutive --output=study_longitudinal.dz
```

A text file can be supplied to `--match` when pairing is not consecutive.

# Step C3: Group Connectometry Analysis

Open the `.dz` database in **Step C3: Group Connectometry Analysis**.

## C3a. Load demographics

Demographics may be supplied as CSV or tab-separated text. If demographics are already embedded in the `.dz` database, DSI Studio loads them automatically.

A recommended format includes a subject identifier in the first column:

```text
ID,AGE,SEX,SCORE
SUB01,23,0,18.2
SUB02,31,1,21.4
SUB03,24,0,17.6
SUB04,36,0,25.1
```

DSI Studio can match subject identifiers in the first column against database subject names. Check the displayed table after loading demographics to make sure subjects and values are aligned correctly.

Missing values can be left empty; subjects missing values required by the selected model are excluded from that analysis.

## C3b. Select covariates

Select variables whose linear effects should be removed from the diffusion measurements, such as age or sex when appropriate for the study design.

Avoid adding many highly correlated covariates unless the sample size supports the model. The choice of covariates should follow the scientific question rather than a fixed preset.

## C3c. Select the study variable and diffusion index

Choose the study variable to test. For example, select age to map pathways associated with aging, or select a behavioral/clinical score to map pathways associated with that measure.

A current `.dz` database may contain multiple diffusion indices. Select the index appropriate for the hypothesis (for example QA, FA, RDI, or NRDI) within the analysis rather than creating a separate database for every metric.

For longitudinal databases, choose the variable that corresponds to the longitudinal hypothesis being tested.

## C3d. Parameters

| Parameter | Description |
|:--|:--|
| **Effect Size Threshold** | Current analyses can use an effect-size threshold; `0.3` is the current default. Higher values focus on stronger associations. |
| **T Threshold** | An alternative threshold based on the local T statistic. Use it when the analysis is intentionally defined by a T threshold rather than effect size. |
| **Length Threshold** | Removes short fragmented findings. The current starting value is derived from the database dimensions rather than a universal fixed length. |
| **FDR Control** | When enabled, retain findings satisfying the selected FDR threshold. When disabled, the analysis reports the FDR of the findings across track lengths. |
| **Permutation Count** | More permutations provide a smoother estimate of the null distribution at the cost of computation time. Increase it when finer FDR resolution is needed. |
| **Pruning / Region Pruning** | Removes fragmented or poorly supported tracking results. |
| **Study Region** | Whole brain is the default exploratory setting. ROI, ROA, End, Seed, or Terminative regions can restrict the hypothesis when anatomically justified. |
| **Exclude Cerebellum** | Optional. Leave it off unless excluding the cerebellum is part of the intended analysis. |

A more restrictive threshold or longer minimum length generally yields fewer findings. Parameter choices should be reported with the analysis because they define the tested tractography hypothesis.

### Study regions

Use **Whole brain** for an exploratory analysis. If the hypothesis is anatomically specific, load a region from an atlas or NIFTI file and assign the appropriate role. Adding regions changes the tested hypothesis and should be decided before interpreting the result.

### Cohort selection

Use **Select Cohort** when the analysis applies only to a subset of subjects. Always verify the selected subjects before running the permutation analysis.

## C3e. Run and review results

Run connectometry and inspect both increased and decreased associations when relevant to the hypothesis. DSI Studio reports the tract findings together with the null/permuted distributions and FDR as a function of track length. The 3D result view can be used to inspect the anatomical location of the findings.

Interpret FDR together with effect size, track length, data quality, acquisition consistency, sample size, and the study design. Very small samples or systematic acquisition differences can produce unstable or misleading results even when a numerical threshold appears favorable.

# Command-line analysis

For batch correlational tractography/connectometry, use `--action=cnt`. The current command-line options and defaults are documented at [Connectometry CLI](/doc/cli_cnt.html).

Example:

```bash
dsi_studio --action=cnt \
  --source=study.dz \
  --index_name=qa \
  --demo=participants.csv \
  --variable_list=1,2,3 \
  --voi=3 \
  --effect_size=0.3
```

The numeric positions used by `--variable_list` and `--voi` refer to the variables in the loaded demographic table. Check the demographic columns before launching a batch analysis.
