# Differential Tractography

![image](https://user-images.githubusercontent.com/275569/147860577-35e5b242-2991-4cdf-b6f7-91161d8b0c73.png)

---

Differential tractography compares one scan to another to map neuronal changes reflected by a decrease of anisotropy. Differential tractography integrates the "tracking-the-difference" paradigm as opposed to the "tracking-the-existence" used in conventional settings. This is realized by adding one tracking criterion to screen if trajectories have a decrease in anisotropy. Differential tractography greatly increases the sensitivity and specificity of diffusion metrics by aggregating results along white matter pathways.

![image](https://user-images.githubusercontent.com/275569/170849962-ef2f90af-748a-4011-8610-508fa8e24645.png)

Differential tractography can be applied to DTI data, multi-shell data, and DSI data. The anisotropy can be derived from DTI or GQI. Higher b-value signals will be more sensitive to early-stage neuronal changes, whereas low b-value signals may include a lot of physiological fluctuations, potentially leading to a higher false discovery rate (FDR). In the [original differential tractography study](https://pubmed.ncbi.nlm.nih.gov/31472253/), we used 256-direction grid sampling with b-max=4000 or even up to 7000 to get excellent FDR values lower than 0.05. Using DTI data may increase FDR up to 0.2.

![image](https://user-images.githubusercontent.com/275569/147860680-74abdce8-81a3-47a6-9d01-3d93be355b0d.png)

There are 4 types of differential tractography, and the applicable types depend on the experiment design.

# Type 1: mapping longitudinal change in the native space

**Requirements**:
- repeat scans of the same subject, e.g. before/after treatment
- good DWI data for fiber tracking (if not, choose type 2)
- no deformation between repeats scans (if not, choose type 2)

**Example**:
- human subject study with repeated scans before and after treatment

**Steps**: Use command-line options for batch processing, or follow the GUI steps for individual processing.

| **Steps** | Details |
|-----------|------------|
| 1. **Generate FIB Files** | Command-line: <br> ```dsi_studio --action=rec --source=*_ses-01_dwi.sz (or .src.gz for older versions) --method=4 --param0=1.25 --output=*_ses-01_dwi.fz (or .fib.gz for older versions)``` <br> ```dsi_studio --action=rec --source=*_ses-02_dwi.sz (or .src.gz for older versions) --method=4 --param0=1.25 --output=*_ses-02_dwi.fz (or .fib.gz for older versions)``` <br> GUI: <br> 1a. [creating SRC files](/doc/gui_t1.html): make sure to have a quality check to ensure the quality is good. Use .sz files (or .src.gz). <br> 1b. [generating FIB files](/doc/gui_t2.html): at Step T2b(1), use the GQI method to get native space .fz files (or .fib.gz). |
| 2. **Export Metrics** | Command-line: <br> ```dsi_studio --action=exp --source=*_ses-02_dwi.fz (or .fib.gz for older versions) --export=dti_fa``` <br> GUI: <br> 2a. Open the .fz file (or .fib.gz) of each **follow-up** scan at **[Step T3: Fiber Tracking]**. <br> 2b. Use the **[Export]** function to save the dti_fa or qa map in a NIFTI file (e.g. sub1.followup.fa.nii.gz). Note that `fa0` is often preferred over `fa`. |
| 3. **Optimize Tractography** | (optional manual checking) <br> GUI: <br> 3a. Open one baseline scan's .fz file (or .fib.gz) at **[Step T3 Fiber Tracking]**. <br> 3b. Restore default fiber tracking settings using **[Options][Restore Tracking Settings]**. <br> 3c. Set **[Step T3c Options][Tracking Parameters][Terminate if]** to `1,000,000` seeds. <br> 3d. Click the **[Step T3d: Tracts][Fiber Tracking]** button to see if you can get good quality whole-brain fiber tracking. If not, check out the troubleshooting section of the [whole brain fiber tracking document](https://dsi-studio.labsolver.org/doc/gui_t3_whole_brain.html). <br> 3e. Find out the best parameters until getting good quality whole-brain track. These parameters (`--seed_count`, `--min_length`, etc.) should be used in the command line for Step 4. |
| 4. **Differential Tracking** | Command-line: <br> ```dsi_studio --action=trk --source=*_ses-01_dwi.fz (or .fib.gz for older versions) --other_slices=*_ses-02_dwi.fib.gz.dti_fa.nii.gz --dt_metric1=dti_fa --dt_metric2=dti_fa --dt_threshold=0.2 --seed_count=1000000 --min_length=30 --tip_iteration=16 --output=*.tt.gz``` <br> GUI: <br> 4a. For each subject, open their baseline scan's .fz file (or .fib.gz) at **[Step T3 Fiber Tracking]**. <br> 4b. Use **[Slice][Insert Other Images]** and select the NIFTI file of the follow-up scan metric (e.g. sub1.followup.fa.nii.gz created above). <br> 4c. Select the desired metric (e.g., `dti_fa` or `qa`) at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics1]**. This assumes that the anisotropy in the baseline scan is larger than the follow-up. <br> 4d. Select the loaded metric at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics2]**. <br> 4e. Adjust thresholds by setting **[Differential Tracking][Metric1>Metric2 Threshold]** (e.g., `0.1`, `0.2`, or `0.3`), which specify 10%, 20%, and 30% differences if [Threshold Type] is (m1-m2)/m1. For most metrics, the normal individual differences are around 10~20%. A higher threshold gives more specific results against individual variations. To use the absolute value as the threshold, set the [Threshold Type] to **m1-m2**. This threshold value should be used for the `--dt_threshold` parameter in the command line. <br> 4f. Click on **[Step T3d Tracts][Fiber Tracking]** to get differential tractography. |

# Type 2: mapping longitudinal change in template space (using QSDR for template-space analysis)

**Requirements:**
- repeat scans of the same subject, e.g. before/after treatment

**Example:**
- animal in-vivo study with repeated scans before and after treatment
- human study where native space tractography is challenging

**Steps:** Use command-line options for batch processing, or follow the GUI steps for individual processing.

| **Steps** | Details |
|-----------|------------|
| 1. **Generate QSDR FIB Files** | Command-line: <br> ```dsi_studio --action=rec --source=*.sz (or .src.gz for older versions) --method=7 --output=*.fz (or .fib.gz for older versions)``` <br> GUI: <br> 1a. [creating SRC files](/doc/gui_t1.html): make sure to have a quality check to ensure the quality is good. Use .sz files (or .src.gz). <br> 1b. [generating FIB files](/doc/gui_t2.html): at Step T2b(1), use the QSDR method to get native space .fz files (or .fib.gz). |
| 2. **Export Metrics** | Command-line: <br> ```dsi_studio --action=exp --source=*.fz (or .fib.gz for older versions) --export=dti_fa``` <br> GUI: <br> 2a. Open the .fz file (or .fib.gz) of **all** scans at **[Step T3: Fiber Tracking]**. <br> 2b. Use the **[Export]** function to save the dti_fa or qa map in a NIFTI file (e.g. sub1.fa.nii.gz). Note that `fa0` is often preferred over `fa`. |
| 3. **Differential Tracking** | Command-line: <br> ```dsi_studio --action=trk --source=0 --template=0 --tolerance=20 --other_slices=sub1_baseline.dti_fa.nii.gz,sub1_followup.dti_fa.nii.gz --dt_metric1=sub1_baseline --dt_metric2=sub1_followup --dt_threshold=0.2 --seed_count=1000000 --min_length=30 --tip_iteration=16 --output=*.tt.gz``` <br> *(Note: `0` for --source and --template refers to the ICBM152_adult template. Other template IDs are listed below.)*<br> Template IDs: <br> 0: ICBM152_adult <br> 1: C57BL6_mouse <br> 2: dHCP_neonate <br> 3: INDI_rhesus <br> 4: Pitt_marmoset <br> 5: WHS_SD_rat <br> GUI: <br> 4a. At **[Step T3 Fiber Tracking]**, choose the appropriate template (e.g., ICBM152_adult template for human studies) and click on the Step T3 button. <br> 4b. Use **[Slice][Insert Other Images]** and select the NIFTI files of the baseline and follow-up metrics (dti_fa or qa) of a subject. <br> 4c. Select the baseline metrics at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics1]**. This assumes that the anisotropy in the baseline scan is larger than the follow-up. <br> 4d. Select the follow-up metric at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics2]**. <br> 4e. Adjust thresholds by setting **[Differential Tracking][Metric1>Metric2 Threshold]** (e.g., `0.1`, `0.2`, or `0.3`), which specify 10%, 20%, and 30% differences if [Threshold Type] is (m1-m2)/m1. For most metrics, the normal individual differences are around 10~20%. A higher threshold gives more specific results against individual variations. To use the absolute value as the threshold, set the [Threshold Type] to **m1-m2**. This threshold value should be used for the `--dt_threshold` parameter in the command line. <br> 4f. Click on **[Step T3d Tracts][Fiber Tracking]** to get differential tractography. |

# Type 3: mapping cross sectional change in the native space

**Requirements:**
- age-sex-matched controls
- DWI data good enough for fiber tracking

**Example:**
- human case-control studies

**Steps:** Use command-line options for batch processing, or follow the GUI steps for individual processing.

| **Steps** | Details |
|-----------|------------|
| 1. **Generate FIB Files** | Command-line: <br> ```dsi_studio --action=rec --source=*.patients.sz (or .src.gz for older versions) --method=4 --param0=1.25 --output=*.patients.fz (or .fib.gz for older versions)``` <br> ```dsi_studio --action=rec --method=7 --source=*.controls.sz (or .src.gz for older versions) --output=*.controls.fz (or .fib.gz for older versions)``` <br> GUI: <br> 1a. [creating SRC files](/doc/gui_t1.html): make sure to have a quality check to ensure the quality is good. Use .sz files (or .src.gz). <br> 1b. [generating FIB files](/doc/gui_t2.html): at Step T2b(1), **choose GQI for patients and choose QSDR for controls**. Use .fz files (or .fib.gz). |
| 2. **Create Database File for Controls** | Command-line: <br> ```dsi_studio --action=atl --source=*.control.fz (or .fib.gz for older versions) --cmd=db --template=0 --demo=controls_age_sex.txt --output=control.db.fz (or .db.fib.gz for older versions)``` <br> GUI: <br> 2a. [Create a database](https://dsi-studio.labsolver.org/doc/gui_cx.html) and include **only controls**. <br> 2b. Prepare a demographic file for the controls. The file can be a space, tab, or comma-separated text file (e.g., `age sex` on the first line, followed by data like `32 1`). <br> 2c. Associate database with demographics: Open the connectometry database file (e.g., control.db.fz) in **[Step C2]** and load control subjects' demographics using **[File][Open demographics]**. This step associates demographics with the controls in the database. Save the database using the [File] menu. **Before doing this step, it is recommended to use [Step C3] to open the .db.fz file and load the demographics, just to check for any mismatch.** |
| 3. **Differential Tracking** | Command-line: <br> ```dsi_studio --action=trk --source=patient.fz (or .fib.gz for older versions) --other_slices=control.db.fz (or .db.fib.gz for older versions) --dt_metric1=control --dt_metric2=dti_fa --demo=patient_age_sex.txt --select="age=<patient_age>,sex=<patient_sex>" --dt_threshold=0.2 --seed_count=1000000 --min_length=30 --tip_iteration=16 --output=*.cross_sectional.tt.gz``` <br> *(Note: `patient_age_sex.txt` is a demographics file for patients, potentially with an additional first column storing partial filenames for matching, although the `--select` option with `--demo` is preferred if demographics can be linked by file order or naming conventions.)* <br> GUI: <br> 3a. For each patient subject, open their .fz file (or .fib.gz) at **[Step T3 Fiber Tracking]**. <br> 3b. Click **[Slice][Insert Other Images]** and select the database file (e.g., control.db.fz) created in the previous step. Enter the patient's age and sex (e.g., `36 1`). You may rename the patient's FIB file to inform DSI Studio of the subject's age and sex. For example, `20220808_M029Y_XXXX.src.gz.gqi.fib.gz` will inform DSI Studio that the subject is a 29-year-old male. <br> 3c. Select the desired metric (e.g., `dti_fa` or `qa`) at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics2]** for the patient data. <br> 3d. Select the database metric at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics1]**. This assumes that the anisotropy in the control is larger than the patient. <br> 3e. Adjust thresholds by setting **[Differential Tracking][Metric1>Metric2 Threshold]** (e.g., `0.1`, `0.2`, or `0.3`), which specify 10%, 20%, and 30% differences if [Threshold Type] is (m1-m2)/m1. For most metrics, the normal individual differences are around 10~20%. A higher threshold gives more specific results against individual variations. To use the absolute value as the threshold, set the [Threshold Type] to **m1-m2**. This threshold value should be used for the `--dt_threshold` parameter in the command line. <br> 3f. Click on **[Step T3d Tracts][Fiber Tracking]** to get differential tractography. |

**TIPS**
If you don't have control subjects, there are publicly available connectometry databases available at [https://brain.labsolver.org](https://brain.labsolver.org). However, most dMRI metrics (especially DTI metrics) are very sensitive to acquisition parameters, and likely you will get many false positive results just due to acquisition differences. The following are publicly available connectometry databases:
- [HCP Aging](https://pitt-my.sharepoint.com/:f:/g/personal/yehfc_pitt_edu/EvdTx_lhJJBCmek2G0IfNhkBmr7CkGKU79H6JC1OH2aWmA?e=xtSbtb) (age: 36+ y)
- [HCP Development](https://pitt-my.sharepoint.com/:f:/g/personal/yehfc_pitt_edu/EgTq8mpY5zZEhKhwHhZMbPABMzRLckaiaRnwm4tMWSg3Fw?e=4m95Mf) (age: 5-21 y)
- [developing HCP (neonate)](https://pitt-my.sharepoint.com/:f:/g/personal/yehfc_pitt_edu/EgTq8mpY5zZEhKhwHhZMbPABMzRLckaiaRnwm4tMWSg3Fw?e=4m95Mf) (age: 20-44 weeks post-conception)
- HCP young adult (to be constructed)
- Grid258 (under construction)

# Type 4: mapping cross sectional change in template space (using QSDR for template-space analysis)

**Requirements:**
- age-sex-matched controls

**Example:**
- animal in-vivo or ex-vivo studies
- human case-control studies where native space tractography is challenging

**Steps:** Use command-line options for batch processing, or follow the GUI steps for individual processing.

| **Steps** | Details |
|-----------|------------|
| 1. **Generate QSDR FIB Files for Controls** | Command-line: <br> ```dsi_studio --action=rec --method=7 --source=*.controls.sz (or .src.gz for older versions) --output=*.controls.fz (or .fib.gz for older versions)``` <br> GUI: <br> 1a. [creating SRC files](/doc/gui_t1.html): make sure to have a quality check to ensure the quality is good. Use .sz files (or .src.gz). <br> 1b. [generating FIB files](/doc/gui_t2.html): at Step T2b(1), choose QSDR. Use .fz files (or .fib.gz). |
| 2. **Export Patient Metrics** | Command-line: <br> ```dsi_studio --action=exp --source=*.patient.fz (or .fib.gz for older versions) --export=dti_fa``` <br> GUI: <br> 2a. Open each patient's .fz file (or .fib.gz) at **[Step T3: Fiber Tracking]**. <br> 2b. Use the **[Export]** function to save the dti_fa or qa map in a NIFTI file (e.g. sub1.fa.nii.gz). Note that `fa0` is often preferred over `fa`. |
| 3. **Create Database File for Controls** | Command-line: <br> ```dsi_studio --action=atl --source=*.control.fz (or .fib.gz for older versions) --cmd=db --template=0 --demo=controls_age_sex.txt --output=control.db.fz (or .db.fib.gz for older versions)``` <br> GUI: <br> 3a. [Create a database](https://dsi-studio.labsolver.org/doc/gui_cx.html) and include **only controls**. <br> 3b. Prepare a demographic file for the controls. The file can be a space, tab, or comma-separated text file (e.g., `age sex` on the first line, followed by data like `32 1`). <br> 3c. Associate database with demographics: Open the connectometry database file (e.g., control.db.fz) in **[Step C2]** and load control subjects' demographics using **[File][Open demographics]**. This step associates demographics with the controls in the database. Save the database using the [File] menu. **Before doing this step, it is recommended to use [Step C3] to open the .db.fz file and load the demographics, just to check for any mismatch.** |
| 4. **Differential Tracking** | Command-line: <br> ```dsi_studio --action=trk --source=0 --template=0 --tolerance=20 --other_slices=patient.dti_fa.nii.gz,control.db.fz (or .db.fib.gz for older versions) --dt_metric1=control --dt_metric2=patient --dt_threshold=0.2 --seed_count=1000000 --min_length=30 --tip_iteration=16 --output=*.tt.gz``` <br> *(Note: `0` for --source and --template refers to the ICBM152_adult template. Other template IDs are listed above.)* <br> GUI: <br> 4a. At **[Step T3 Fiber Tracking]**, choose the appropriate template (e.g., ICBM152_adult template for human studies) and click on the Step T3 button. <br> 4b. Use **[Slice][Insert Other Images]** and select a patient's dti_fa or qa NIFTI file generated from Step 2. <br> 4c. Click **[Slice][Insert Other Images]** and select the control subjects database file (e.g., control.db.fz) created in the previous step. Enter the patient's age and sex (e.g., `36 1`). <br> 4d. Select the control metrics at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics1]**. This assumes that the anisotropy in the control is larger than the patient. <br> 4e. Select the patient's metric at **[Step T3c: Options][Tracking Parameters][Differential Tracking][Metrics2]**. <br> 4f. Adjust thresholds by setting **[Differential Tracking][Metric1>Metric2 Threshold]** (e.g., `0.1`, `0.2`, or `0.3`), which specify 10%, 20%, and 30% differences if [Threshold Type] is (m1-m2)/m1. For most metrics, the normal individual differences are around 10~20%. A higher threshold gives more specific results against individual variations. To use the absolute value as the threshold, set the [Threshold Type] to **m1-m2**. This threshold value should be used for the `--dt_threshold` parameter in the command line. <br> 4g. Click on **[Step T3d Tracts][Fiber Tracking]** to get differential tractography. |

## Advanced Features & Suggestions

**Reporting**

It is recommended to check a range of **[Metric1>Metric2 Threshold]** values (e.g. 0.1, 0.2, 0.3, 0.4) and eliminate fragments by increasing **[Step T3c:Options][Tracking Parameters][Min Length]**. The goal is to maximize sensitivity while retaining specificity. These correspond to the `--dt_threshold` and `--min_length` command-line parameters.

**Add ROA or ROI to limit findings**

You can map the affected pathways that pass through the internal capsule by assigning the internal capsule as the ROI. The number and length of tracks can be compared between patients if other tracking parameters are fixed (be sure to fix the seed count using the `--seed_count` parameter).

**Exclude cerebellum**

Many false results may be generated just because of different slice coverage at the cerebellum. To handle this issue, add a region from [Atlas][BrainSeg][Cerebellum] and assign it as ROA. This corresponds to using a mask file with the `--mask` command-line option.

**Segment results into bundles**

You can use [Tracts][Miscellaneous][Recognize Track] to recognize the name of the bundles.

**Apply smoothing to metrics**

If tracking results change significantly in repeated analyses, it is likely that the image acquisition is not optimal (e.g., too noisy, has very thick slices), and thus registration errors are amplified. To minimize this variance, export both baseline and follow-up NQA images (or other metrics) and smooth them using [Tool][O41 View image][Signals][Smoothing] (make sure to update DSI Studio to use this function). Save the smoothed images as new files (e.g., NIFTI) to run differential tracking. After smoothing, add the smoothed image back using [Slices][Add Other images] and continue with further analysis. Command-line options for smoothing metrics might also be available in recent versions.

**Multi-metrics analysis**
The metrics we usually use include DTI's FA and GQI's QA, RDI, NRDI (see [interpretations](https://dsi-studio.labsolver.org/doc/how_to_interpret_dmri.html)). The following is the implication behind them:

-   **FA** decreases during acute neuronal injury or chronic neurodegeneration. It has high sensitivity and low specificity because the decreases of FA can be due to vasogenic edema, demyelination, inflammation, or axonal loss. We often use FA as the first-pass screening. Note that `fa0` is the preferred metric name in recent versions.
-   **QA** decreases when there is demyelination or axonal loss. It usually does NOT change in acute axonal injury or edema and thus is much more specific to axonal loss. In acute neuronal injury or inflammation, we may see FA decreasing with QA staying the same, potentially indicating that the axonal injury is reversible.
-   **RDI** increases when there is cell infiltration, which happens in tumor or during inflammation. You need to have multi-shell or DSI acquisitions to use this metric.
-   **NRDI** increases when there is tissue edema. You need to have multi-shell or DSI acquisitions to use this metric.

We usually run differential fiber tracking on FA (or fa0), QA, RDI, and NRDI, respectively. This will give a comprehensive clinical picture of the neuronal change. For more detailed discussion, please refer to [how to interpret dMRI metrics](/doc/how_to_interpret_dmri.html).

## False discovery rate and statistical testing

One key question for differential tractography is the significance of the findings. For example, if we observe a lot of tracks showing up in differential tractography, how many of them are false positive? A way to quantify this reliability is by calculating the false discovery rate from a group of patients and a group of control subjects. The following steps illustrate how this can be carried out in DSI Studio.

**Example: calculate the false discovery rate using control subjects**

1. Apply **identical** steps and parameters to each patient and matched control, respectively.
2. Estimate the average volume of findings in patients using the [Tracts][Statistics] menu in Step T3.
3. Estimate the average volume of findings in controls.
4. False discovery rate (FDR) = (averaged volume in control) / (averaged volume in patient)

For example, if there is an average of 100 mm³ of track findings from patients and 10 mm³ track findings from controls, then the false discovery rate of the findings in patients is 10/100 = 10%. An FDR lower than 0.05 can be considered significant.
