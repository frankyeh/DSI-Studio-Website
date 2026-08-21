# Differential Tractography

![image](https://user-images.githubusercontent.com/275569/147860577-35e5b242-2991-4cdf-b6f7-91161d8b0c73.png)

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population. Instead of tracking where anisotropy exists, it adds a tracking criterion that follows where a selected diffusion metric differs between two conditions.

[Original differential tractography study](https://pubmed.ncbi.nlm.nih.gov/31472253/): Yeh et al., *NeuroImage* 202 (2019): 116131.

![image](https://user-images.githubusercontent.com/275569/170849962-ef2f90af-748a-4011-8610-508fa8e24645.png)

Differential tractography can be applied to DTI, multi-shell, and DSI acquisitions. Higher b-value acquisitions can improve sensitivity to axonal changes, whereas low-SNR or acquisition differences can increase false findings. Always evaluate data quality and use the same processing choices across the scans or groups being compared.

## Four common study designs

| Type | Design | Tracking space | Typical use |
|---|---|---|---|
| **1** | Longitudinal | Native subject space | Repeat scans with good native-space tractography and little deformation between scans |
| **2** | Longitudinal | Template space | Repeat scans when native-space tractography/alignment is difficult |
| **3** | Cross-sectional | Native patient space | Compare an individual patient with an age/sex or otherwise matched reference population |
| **4** | Cross-sectional | Template space | Case-control/reference comparison performed in template space |

Current files use `.sz` for SRC, `.fz` for FIB, and `.dz` for connectometry databases. Legacy `.src.gz`, `.fib.gz`, `.db.fz`, and `.db.fib.gz` files remain readable in compatible workflows.

> **For Type 3 and Type 4 reference databases, use the Sun version to create `.dz` files.** Hou versions have a known issue when creating `.dz` databases.

# Type 1: Longitudinal change in native space

Use this design for repeated scans of the same subject when both scans can be compared directly in native diffusion space.

## 1. Reconstruct baseline and follow-up FIB files

```bash
dsi_studio --action=rec --source=*_ses-01_dwi.sz --method=4 --param=1.25 --output=*_ses-01_dwi.fz
dsi_studio --action=rec --source=*_ses-02_dwi.sz --method=4 --param=1.25 --output=*_ses-02_dwi.fz
```

In the GUI, create/QC the `.sz` files and reconstruct the scans using the same GQI settings.

## 2. Export the follow-up metric

For FA:

```bash
dsi_studio --action=exp --source=*_ses-02_dwi.fz --export=dti_fa
```

The same approach can be used for QA or another metric appropriate for the acquisition and hypothesis.

## 3. Check whole-brain tracking first

Before differential tracking, open a representative baseline `.fz` in **Step T3: Fiber Tracking** and confirm that conventional whole-brain tracking is anatomically reasonable. Resolve acquisition, b-table, reconstruction, or tracking problems before interpreting differential findings.

## 4. Run differential tracking

Example using FA:

```bash
dsi_studio --action=trk \
  --source=subject_baseline.fz \
  --other_slices=subject_followup.fz.dti_fa.nii.gz \
  --dt_metric1=dti_fa \
  --dt_metric2=dti_fa \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=16 \
  --output=subject.diff.tt.gz
```

In the GUI, open the baseline FIB, use **[Slices][Insert Other Images]** to add the follow-up metric, select the baseline and follow-up metrics under **Differential Tracking**, set the change threshold, and run fiber tracking.

# Type 2: Longitudinal change in template space

Use this design when repeated scans should be compared in a common template space.

## 1. Reconstruct the scans with QSDR

```bash
dsi_studio --action=rec --source=*.sz --method=7 --output=*.fz
```

## 2. Export the metric from each scan

```bash
dsi_studio --action=exp --source=*.fz --export=dti_fa
```

## 3. Run differential tracking in the template

For the default human template example:

```bash
dsi_studio --action=trk \
  --source=0 \
  --template=0 \
  --other_slices=subject_baseline.dti_fa.nii.gz,subject_followup.dti_fa.nii.gz \
  --dt_metric1=subject_baseline \
  --dt_metric2=subject_followup \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=16 \
  --output=subject.template.diff.tt.gz
```

Use the template appropriate for the species/study. The available template list is provided by the current DSI Studio version; do not rely on an old hard-coded template list when building a new pipeline.

# Type 3: Cross-sectional change in native patient space

Use this design when each patient's native-space metric is compared with a matched reference population.

## 1. Reconstruct patient and control FIB files

Patients can be reconstructed in native space with GQI. Reference controls should be processed consistently; QSDR controls can be used to construct the population reference database.

```bash
dsi_studio --action=rec --source=*.patients.sz --method=4 --param=1.25 --output=*.patients.fz
dsi_studio --action=rec --source=*.controls.sz --method=7 --output=*.controls.fz
```

## 2. Create the reference `.dz` database

For a differential analysis using FA, creating a database restricted to `dti_fa` makes the reference metric explicit:

```bash
dsi_studio --action=atl \
  --cmd=db \
  --source=*.controls.fz \
  --index_name=dti_fa \
  --demo=controls_age_sex.csv \
  --output=control.dz
```

The demographic file stored in the reference database defines the variables used to generate a subject-matched reference value.

The general database workflow is documented in [Correlational Tractography / Connectometry](/doc/gui_cx.html).

## 3. Run patient-versus-reference differential tracking

For batch processing, `--subject_demo` supplies the patient's demographic values used to create the matched reference image from the `.dz` database.

Example subject demographics file:

```text
SUB01,36,1
SUB02,42,0
```

The numeric values after the subject ID must match the demographic variables stored in the reference database, in the same order.

```bash
dsi_studio --action=trk \
  --source=SUB01.patient.fz \
  --other_slices=control.dz \
  --subject_demo=patient_age_sex.csv \
  --dt_metric1=control \
  --dt_metric2=dti_fa \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=16 \
  --output=SUB01.cross_sectional.tt.gz
```

In the GUI, open the patient's `.fz`, insert `control.dz` using **[Slices][Insert Other Images]**, enter the patient's demographic values when requested, select the control/reference metric as Metric 1 and the patient's metric as Metric 2, then run differential tracking.

A decrease analysis assumes the reference metric is larger than the patient metric. Reverse the comparison or threshold interpretation when the biological hypothesis concerns an increase.

Reference databases from external datasets can be useful, but diffusion measurements are sensitive to acquisition and processing differences. A large apparent patient/reference difference can reflect scanner, protocol, b-value, spatial resolution, or reconstruction differences rather than pathology. Prefer a reference population acquired and processed as comparably as possible.

# Type 4: Cross-sectional change in template space

Use this design when the patient metric and reference population will be compared in a common template space.

## 1. Reconstruct controls with QSDR

```bash
dsi_studio --action=rec --source=*.controls.sz --method=7 --output=*.controls.fz
```

## 2. Export the patient metric in template space

Export the metric from a template-space patient FIB or otherwise prepare the patient's metric in the same template space.

```bash
dsi_studio --action=exp --source=patient.fz --export=dti_fa
```

## 3. Create the control `.dz` database

```bash
dsi_studio --action=atl \
  --cmd=db \
  --source=*.controls.fz \
  --index_name=dti_fa \
  --demo=controls_age_sex.csv \
  --output=control.dz
```

## 4. Run differential tracking in template space

For one subject, demographic values may be supplied directly to `--subject_demo`; their order must match the variables embedded in the control database.

```bash
dsi_studio --action=trk \
  --source=0 \
  --template=0 \
  --other_slices=patient.dti_fa.nii.gz,control.dz \
  --subject_demo="36 1" \
  --dt_metric1=control \
  --dt_metric2=patient \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=16 \
  --output=patient.template.cross_sectional.tt.gz
```

In the GUI, open the appropriate template in Step T3, insert the patient metric and the control `.dz`, enter the patient's demographics, then select the matched control metric as Metric 1 and the patient metric as Metric 2.

# Choosing thresholds and tracking parameters

The differential threshold describes the required difference between Metric 1 and Metric 2. For a fractional change definition such as `(m1-m2)/m1`, values of `0.1`, `0.2`, and `0.3` correspond to 10%, 20%, and 30% decreases.

Normal scan-to-scan and individual variation can be substantial. Evaluate a scientifically justified range rather than choosing a threshold only because it produces visually appealing tracks. Larger thresholds and longer minimum track lengths are generally more specific but less sensitive.

Before comparing subjects, keep the relevant tracking parameters fixed. Always check conventional tractography first; differential tracking cannot rescue poor diffusion acquisition, an incorrect b-table, failed reconstruction, or severe registration errors.

## ROI and ROA constraints

An ROI/ROA can be used when the hypothesis is anatomically specific. Adding a region changes the tested hypothesis and can substantially change the number of findings, so region constraints should be chosen from anatomical evidence rather than tuned after viewing the result.

## Smoothing

If repeat scans are noisy or registration differences create unstable voxelwise metrics, modest image smoothing may improve robustness. Apply the same processing to both conditions and document it in the analysis methods.

# Interpreting common metrics

For detailed interpretation, see [How to Interpret dMRI Metrics](/doc/how_to_interpret_dmri.html).

- **FA / FA0:** sensitive to many tissue changes, including edema, inflammation, demyelination, and axonal loss; useful as a broad screening metric but not highly specific.
- **QA:** often more specific to changes affecting anisotropic restricted diffusion and axonal structure.
- **RDI:** requires appropriate multi-shell/DSI sampling and can reflect changes in restricted diffusion.
- **NRDI:** requires appropriate multi-shell/DSI sampling and can be sensitive to tissue-water changes.

Different metrics answer different biological questions. A multi-metric differential analysis can provide complementary information, but each metric should be interpreted within what the acquisition can support.

# False discovery rate for differential findings

A control or sham comparison can be used to estimate how much differential tracking is produced in the absence of the biological effect of interest.

A simple group-level estimate used in differential tractography is:

```text
FDR = average volume of findings in controls / average volume of findings in patients
```

Apply identical processing, thresholds, and tracking parameters to the patient and control/sham analyses. Report the FDR together with the differential threshold, minimum track length, metric, acquisition, and other tracking parameters used to generate the result.
