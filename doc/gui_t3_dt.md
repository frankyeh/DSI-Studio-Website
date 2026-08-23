# Differential Tractography

![image](https://user-images.githubusercontent.com/275569/147860577-35e5b242-2991-4cdf-b6f7-91161d8b0c73.png)

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population. Instead of tracking where anisotropy exists, it adds a tracking criterion that follows where a selected diffusion metric differs between two conditions.

[Original differential tractography study](https://pubmed.ncbi.nlm.nih.gov/31472253/): Yeh et al., *NeuroImage* 202 (2019): 116131.

![image](https://user-images.githubusercontent.com/275569/170849962-ef2f90af-748a-4011-8610-508fa8e24645.png)

Differential tractography can be applied to DTI, multi-shell, and DSI acquisitions. Higher b-value acquisitions can improve sensitivity to axonal changes, whereas low-SNR or acquisition differences can increase false findings. Always evaluate data quality and use the same processing choices across the scans or groups being compared.

## Four common study designs

| Type | Design | Tracking space | Typical use |
|---|---|---|---|
| **1** | Longitudinal | Native subject space | Baseline versus follow-up in the subject's native diffusion space |
| **2** | Longitudinal | Template space | Baseline versus follow-up after QSDR/template normalization |
| **3** | Cross-sectional | Native patient space | An individual patient compared with a population-average control reference |
| **4** | Cross-sectional | Template space | Cross-sectional comparison performed in a common QSDR/template space |

The **Type 1–4 number describes the study design and analysis space**. It is separate from the numeric `dt_threshold_type`, which selects the mathematical formula used to compare Metric 1 (`m1`) and Metric 2 (`m2`).

Current files use `.sz` for SRC and `.fz` for FIB. A `.dz` connectometry database is useful for cohort-level correlational tractography, but it is no longer the preferred default reference for Type 3 differential tractography.

# Type 1: Longitudinal change in native space

Use this design for repeated scans of the same subject when the comparison should remain in native diffusion space.

## 1. Reconstruct baseline and follow-up FIB files

Use GQI for both scans with consistent reconstruction settings:

```bash
dsi_studio --action=rec --source=subject_baseline.sz --method=4 --param=1.25 --output=subject_baseline.fz
dsi_studio --action=rec --source=subject_followup.sz --method=4 --param=1.25 --output=subject_followup.fz
```

## 2. Export only the follow-up metric

The opened baseline FIB already contains its own metric, so do not export the baseline metric and insert it again.

For FA:

```bash
dsi_studio --action=exp --source=subject_followup.fz --export=dti_fa
```

This produces a file such as `subject_followup.fz.dti_fa.nii.gz`.

## 3. Check whole-brain tracking first

Open the baseline `.fz` in **Step T3: Fiber Tracking** and confirm that conventional whole-brain tracking is anatomically reasonable. Resolve acquisition, b-table, reconstruction, or tracking problems before interpreting differential findings.

## 4. Add the follow-up metric and wait for registration

Insert the exported follow-up map using **[Slices][Insert Other Images]**. DSI Studio may run rigid-body registration even when the scans have the same matrix size and voxel size. Wait until registration is complete before selecting the differential metrics.

Metric names used by differential tracking are the names shown by the current tracking window, not arbitrary abbreviations. For a NIfTI custom slice, the name is derived from the file stem. Verify the displayed names before a batch run.

## 5. Run differential tracking

For a baseline-normalized decrease, use baseline `dti_fa` as `m1`, the follow-up custom slice as `m2`, and formula type `0`, `(m1-m2)/m1`.

Example:

```bash
dsi_studio --action=trk \
  --source=subject_baseline.fz \
  --other_slices=subject_followup.fz.dti_fa.nii.gz \
  --dt_metric1=dti_fa \
  --dt_metric2=subject_followup \
  --dt_threshold_type=0 \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=0 \
  --output=subject.diff.tt.gz
```

With `dt_threshold=0.2`, this example maps locations satisfying a baseline-normalized decrease greater than 20%.

# Type 2: Longitudinal change in template space

Use this design when repeated scans should be compared in a common template space.

## 1. Reconstruct both scans with QSDR

```bash
dsi_studio --action=rec --source=subject_baseline.sz --method=7 --output=subject_baseline.fz
dsi_studio --action=rec --source=subject_followup.sz --method=7 --output=subject_followup.fz
```

## 2. Export the metric from both scans

```bash
dsi_studio --action=exp --source=subject_baseline.fz --export=dti_fa
dsi_studio --action=exp --source=subject_followup.fz --export=dti_fa
```

## 3. Run differential tracking in the template

```bash
dsi_studio --action=trk \
  --source=0 \
  --template=0 \
  --other_slices=subject_baseline.fz.dti_fa.nii.gz,subject_followup.fz.dti_fa.nii.gz \
  --dt_metric1=subject_baseline \
  --dt_metric2=subject_followup \
  --dt_threshold_type=0 \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=0 \
  --output=subject.template.diff.tt.gz
```

Use the template appropriate for the species/study. Inspect the alignment of both scalar maps in template space because QSDR/nonlinear-registration errors can create local metric differences that resemble biological change, especially near tissue boundaries, ventricles, lesions, or atrophy.

# Type 3: Cross-sectional comparison in native patient space

Type 3 compares an individual patient's native-space metric with a control reference while keeping tractography in the patient's native diffusion space.

The current preferred reference is a **direct population-average scalar map** rather than an age/sex-matched `.dz` model. Age/sex matching alone can introduce model assumptions without necessarily providing a better estimate of the expected control value.

## 1. Reconstruct the patient and controls

- Reconstruct the patient with GQI for native-space tractography.
- Reconstruct all controls with QSDR using the same template space and resolution.

## 2. Export and average the control metric

Export the same scalar metric from every QSDR control FIB. Because the control maps are already in the same QSDR/MNI space, they can be averaged voxelwise.

For example, after exporting control FA maps:

```bash
dsi_studio --action=tmp \
  --source=control_*.fz.dti_fa.nii.gz \
  --output=control_avg.mni.nii.gz
```

The source images must already be QSDR/MNI aligned. Do not average native-space control images voxelwise.

## 3. Compare the native-space patient with the MNI control average

When the tracking FIB is GQI/native space, the control average is an MNI-space reference and must be mapped into the patient diffusion space. In the GUI, load it as an **MNI-space slice** and verify the mapped alignment before differential tracking.

For command-line tracking, including `.mni.` in the control-average filename marks it as an MNI-space image so DSI Studio maps it to the native patient space:

```bash
dsi_studio --action=trk \
  --source=SUB01.patient.fz \
  --other_slices=control_avg.mni.nii.gz \
  --dt_metric1=control_avg \
  --dt_metric2=dti_fa \
  --dt_threshold_type=0 \
  --dt_threshold=0.2 \
  --seed_count=1000000 \
  --min_length=30 \
  --tip_iteration=0 \
  --output=SUB01.cross_sectional.tt.gz
```

For a decrease analysis, the control reference is `m1` and the patient's metric is `m2`. Use the reverse/increase formula when the biological hypothesis concerns an increase.

Reference controls should be acquired and processed as comparably as possible. Scanner, protocol, b-value, spatial resolution, and reconstruction differences can appear as patient/control differences.

# Type 4: Cross-sectional comparison in template space

Type 4 performs the comparison in a common QSDR/template space. The subject/group metric and the control reference must use the same template space and resolution, and their alignment should be inspected before interpretation.

A QSDR/MNI population-average control map can be used directly in the corresponding template-space tracking framework. The exact Type 4 setup depends on whether the comparison is an individual, a group-derived map, or another template-space reference. Use the same differential formula rules described above and verify the runtime metric names before tracking.

# Choosing the differential formula and threshold

Current formula indices are:

```text
0  (m1-m2)/m1
1  (m1-m2)/m2
2  m1-m2
3  (m2-m1)/m1
4  (m2-m1)/m2
5  m2-m1
6  m1/max(m1)
7  m2/max(m2)
```

For a fractional change definition such as `(m1-m2)/m1`, values of `0.1`, `0.2`, and `0.3` correspond to 10%, 20%, and 30% decreases.

Normal scan-to-scan and individual variation can be substantial. Consider a scientifically justified threshold series rather than choosing one threshold because it produces visually appealing tracks. For example, 10%, 20%, 30%, and 40% can show whether the anatomical pattern persists as the required effect size increases.

# Tracking and pruning guidance

Keep the relevant tracking parameters fixed when comparing subjects. Always check conventional tractography first; differential tracking cannot rescue poor acquisition, an incorrect b-table, failed reconstruction, or severe registration errors.

For the initial differential run, use **`tip_iteration=0`** or otherwise keep pruning very light so the unpruned differential result can be inspected. If topology-informed pruning is needed, apply it incrementally after tracking and inspect the bundle after each round. One or two rounds are often sufficient for review. Stop or undo pruning if valid differential trajectories are being removed.

A remaining tract count around 5,000–10,000 can be convenient for inspection and visualization, but it is not a biological or statistical cutoff.

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

A control or sham cohort can be used to estimate how much differential tracking is produced in the absence of the biological effect of interest. Complete the differential analysis for **all controls and all patients** before estimating cohort-level FDR.

At each differential threshold, quantify the differential-tract volume for each subject and estimate:

```text
FDR = mean differential-tract volume in controls / mean differential-tract volume in patients
```

If a threshold series is reported, calculate this separately at each threshold. Apply identical processing, differential formulas, thresholds, tracking parameters, and pruning rules to controls and patients.
