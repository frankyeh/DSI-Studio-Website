# Correlational Tractography

> **Before cohort-level correlational tractography:** [Import and QC each subject's DWI](/doc/gui_t1.html) → [reconstruct with QSDR](/doc/gui_t2.html#q-space-diffeomorphic-reconstruction-qsdr) using the **same template and output resolution** → create a cohort `.dz` database as described below. Confirm subject registration and demographic data before modeling. This is a group analysis, so a single native-space FIB is not sufficient.

<iframe width="560" height="315" src="https://www.youtube.com/embed/qC8jx6XZHGI" title="Correlational tractography tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Correlational tractography identifies white-matter pathway segments whose local diffusion/connectome measurements are associated with a study variable across subjects. DSI Studio uses regression to account for selected covariates and applies local correlation statistics followed by deterministic tracking. [Connectometry](https://pubmed.ncbi.nlm.nih.gov/26499808/) uses permutation testing to estimate the false discovery rate (FDR) of the resulting tracks.

Correlational tractography is a **cohort-level** analysis. For a single subject's own before/after comparison, use [Differential Tractography](/doc/gui_t3_dt.html) instead. Group-level longitudinal connectometry is supported by first creating a database of matched per-subject changes.

- **Tractography methods including correlational tractography:** Yeh et al., *NeuroImage* 245 (2021): 118651. [PubMed](https://pubmed.ncbi.nlm.nih.gov/34673247/)
- **Connectometry:** Yeh, Fang-Cheng, David Badre, and Timothy Verstynen. NeuroImage 125 (2016): 162-171. [PDF](/ref/Connectometry.pdf)

## Current main-window workflow

Select <span class="gui-action">Correlational Tractography</span> from the left sidebar. Its three cards are **1 FIB files → 2 Prepare database → 3 Correlational Tractography**. In the first card, <span class="gui-menu-path"><span class="gui-menu-item">FIB File(s) (*.fz)...</span></span> and <span class="gui-menu-path"><span class="gui-menu-item">FIB Folder (*.fz)...</span></span> open database creation with the selected QSDR FIB data preloaded. <span class="gui-menu-path"><span class="gui-menu-item">Database (*.dz)...</span></span> in the second card opens a database for inspection/editing; <span class="gui-menu-path"><span class="gui-menu-item">Database (*.dz)</span></span> in the third card launches correlational tractography analysis. A **Recent Database** list is located below.

## Current workflow

1. **Create database:** create a connectometry database (`.dz`) from subject QSDR FIB files reconstructed into the same template space and resolution.
2. **Prepare database (optional):** inspect/modify the database or calculate longitudinal change.
3. **Correlational tractography:** load/verify demographics, select the model and study variable, and run correlational tractography/connectometry.

> **Version compatibility:** Use the current **Sun** version to create `.dz` databases. The pinned Hou release dated **2025-04-16** predates `.dz` support; some later Hou builds introduced `.dz` support but had database-creation issues. Older `.db.fz` and `.db.fib.gz` databases remain supported for compatibility.

## Create a connectometry database

A connectometry database combines diffusion measurements from multiple subjects in a common template space for population analysis.

For population connectometry, [reconstruct all subjects with QSDR](/doc/gui_t2.html#q-space-diffeomorphic-reconstruction-qsdr) using **the same template space and resolution** before database creation. Native-space GQI remains appropriate for individual/native-space analyses, but should not be treated as interchangeable input for a common-space connectometry cohort.

### Add subject FIB files

Use <span class="gui-menu-path"><span class="gui-menu-item">Correlational Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 FIB files</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">FIB File(s) (*.fz)...</span></span> or <span class="gui-menu-path"><span class="gui-menu-item">FIB Folder (*.fz)...</span></span> to open Create Database with the input list populated.

Use <span class="gui-action">Add</span>, <span class="gui-action">Search in Directory...</span>, or <span class="gui-action">Open List</span> to load the subject QSDR FIB files. Check the subject list before creating the database. For longitudinal studies, keeping repeat scans of each subject adjacent makes later pairing easier.

### Create the `.dz` file

The current database format is `.dz`. A new `.dz` database stores the available diffusion indices from the FIB files in one database, so a separate database is no longer needed for each metric.

Confirm the output name and click <span class="gui-action">Create Database</span>.

Command-line equivalent:

```bash
dsi_studio --action=db --source=*.qsdr.fz --output=study.dz
```

To embed demographics during database creation:

```bash
dsi_studio --action=db --source=*.qsdr.fz --demo=participants.tsv --output=study.dz
```

By default, current DSI Studio stores the available indices from the FIB files. Use `--index_name` only when you intentionally want a subset, for example:

```bash
dsi_studio --action=db --source=*.qsdr.fz --index_name=qa,dti_fa --output=study.dz
```

### Check registration quality

Database construction reports registration quality for each subject. Review subjects with low R2 or visibly poor alignment before group analysis. Acquisition differences, motion, incorrect b-tables, and registration failures can all create systematic effects that should not be interpreted as biological findings.

## View or modify a database

Use <span class="gui-menu-path"><span class="gui-menu-item">Correlational Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Prepare database</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Database (*.dz)...</span></span> when you need to inspect subjects, remove problematic data, load demographics, or prepare a longitudinal database.

Demographics can be stored inside the database. After loading a demographic CSV/TSV file, save the database if you want those values to be available automatically the next time it is opened.

## Longitudinal studies

For repeated scans of the same subjects:

1. Make sure baseline and follow-up scans are correctly paired.
2. Use <span class="gui-menu-path"><span class="gui-menu-item">Tools</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Longitudinal scans...</span></span> to review or define the matching.
3. Choose how the longitudinal change is calculated, such as `scan2-scan1`.
4. Save the resulting longitudinal database as a new `.dz` file.

The command line also supports paired longitudinal conversion. If scans are stored consecutively as baseline/follow-up pairs:

```bash
dsi_studio --action=db \
  --source=study.dz \
  --match=consecutive \
  --dif_type=0 \
  --filter_type=0 \
  --normalize_iso=1 \
  --output=study_longitudinal.dz
```

A text file can be supplied to `--match` when pairing is not consecutive.

For a **group-level directional longitudinal hypothesis**, a useful two-stage strategy is:

1. First use the unfiltered signed change (`filter_type=0`) and test whether the group expected to change shows a significant nonzero longitudinal effect.
2. If a directional effect is supported, rebuild the longitudinal database using `filter_type=1` for increases or `filter_type=2` for decreases, then compare the magnitude of that directional change between groups.

The directional databases store the selected change direction as a positive magnitude. They answer a different question from the initial signed-change test and should be interpreted accordingly.

After creating any derived or longitudinal database, **re-check its demographics before analysis**. Do not assume demographic columns were embedded or carried forward from the source database. Reload the demographic table if the variables needed by the model are missing.

## Group connectometry analysis

Use <span class="gui-menu-path"><span class="gui-menu-item">Correlational Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">3 Correlational Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Database (*.dz)</span></span> to open the group analysis for a `.dz` database.

### Load and verify demographics

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

Before choosing a variable of interest or running the analysis, verify that all required demographic columns and covariates are actually present in the currently opened database. Repeat this check for derived and longitudinal databases.

Missing values can be left empty; subjects missing values required by the selected model are excluded from that analysis.

### Select covariates

Select variables whose linear effects should be removed from the diffusion measurements, such as age or sex when appropriate for the study design.

Avoid adding many highly correlated covariates unless the sample size supports the model. The choice of covariates should follow the scientific question rather than a fixed preset. For multi-site studies, scanner/site may need to be considered; for longitudinal studies, the scan interval may also matter when it varies substantially across subjects.

### Select the study variable and diffusion index

Choose the study variable to test. For example, select age to map pathways associated with aging, or select a behavioral/clinical score to map pathways associated with that measure.

A current `.dz` database may contain multiple diffusion indices. Select the index appropriate for the hypothesis (for example QA, FA, RDI, or NRDI) within the analysis rather than creating a separate database for every metric.

For a longitudinal database, `longitudinal`/Intercept tests whether a nonzero change exists across the selected cohort. Selecting a demographic variable such as group instead tests whether the amount of change is associated with that variable.

### Parameters

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

### Run and review results

Run connectometry and inspect both increased and decreased associations when relevant to the hypothesis. DSI Studio reports the tract findings together with the null/permuted distributions and FDR as a function of track length. The FDR comes from the whole-brain permutation framework; it should not be interpreted as an independent tract-by-tract statistical test.

The 3D result view can be used to inspect the anatomical location of the findings. Interpret FDR together with effect size, track length, data quality, acquisition consistency, sample size, and the study design. Very small samples or systematic acquisition differences can produce unstable or misleading results even when a numerical threshold appears favorable.

## Command-line analysis

For batch correlational tractography/connectometry, use `--action=cnt`. The current command-line options and defaults are documented at [Connectometry CLI](/doc/cli_cnt.html).

Example:

```bash
dsi_studio --action=cnt \
  --source=study.dz \
  --index_name=qa \
  --demo=participants.csv \
  --variable_list=0,1,2 \
  --voi=2 \
  --effect_size=0.3
```

The numeric positions used by `--variable_list` and `--voi` are **zero-based indices for demographic variables**. In the example `ID,AGE,SEX,SCORE` table, the text subject identifiers `SUB01` etc. are recognized as a nonnumeric subject-name column, so AGE, SEX and SCORE are indices `0,1,2` and `--voi=2` selects SCORE. **Numeric IDs may instead be parsed as a demographic column** (index `0`), shifting AGE, SEX, and SCORE to `1,2,3`; inspect the parsed demographic variable list and adjust the indices to match your data. Check every available demographic column before launching a batch analysis; do not assume a fixed column layout across studies.
