# How to Analyze dMRI

DSI Studio's current main window uses a **left navigation sidebar** rather than the older Step T1 / Step T2 / Step T3 buttons. Select **Tractography** to see the three stages **1 Import → 2 Reconstruct → 3 Track**. Other sidebar pages are **Correlational Tractography**, **Image Processing**, and **Settings**.

The **AI Agents**, **Fiber Data Hub**, **XNAT Download**, and **Console** buttons are located below the navigation list. The working directory can be selected under **Settings**. Some videos and screenshots in the detailed tutorials show an earlier main-window layout; the underlying SRC, reconstruction, and tracking operations remain applicable.

## Start here: Import → Reconstruct → Track

1. **Import diffusion data:** On **Tractography → 1 Import**, choose **NIfTI**, **DICOM**, or **2dseq/FDF**. Select **NIfTI file(s)...** or **NIfTI folder...** (the latter detects BIDS datasets with `sub-*` directories and otherwise handles an ordinary NIfTI folder). For DICOM choose **DICOM file(s)...** or **DICOM folder...**. The **▸ Rename DICOM** section contains file-moving/renaming operations, not conversion. The output is an SRC file (`.sz`).
2. **Review input quality:** On the NIfTI import section, expand **▸ More… → NIFTI quality control...** when needed. Review [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control) before group analyses; it is available under **Tractography → 2 Reconstruct → ▸ More… → SRC quality control...**.
3. **Reconstruct:** On **Tractography → 2 Reconstruct**, click **SRC File(s) (*.sz)**. Choose [GQI or QSDR reconstruction](/doc/gui_t2.html), inspect the b-table, mask, orientation, and processing choices, and save the resulting FIB file (`.fz`). **▸ More…** also offers batch SRC-to-FIB reconstruction.
4. **Track:** On **Tractography → 3 Track**, click **FIB File (*.fz)** to open the **fiber tracking window**, where you can perform whole-brain, ROI-based, automatic, differential, and connectome analyses. **▸ More…** contains **Automatic tracking** and **FIB quality control...**. The **Recent FIB / Recent SRC / Template FIB** controls below the stages reopen recent data or templates.

The **Step T3** labels used below refer to controls *inside the fiber tracking window*, not to a separate Step T3 tab in the new main window.

## Choose an analysis workflow

| Scientific question | Where to start | Further instructions |
|:--|:--|:--|
| Fiber pathways in one subject | **Tractography → 3 Track → FIB File (*.fz)** | [Whole-Brain Tractography](/doc/gui_t3_whole_brain.html), [ROI-Based Tracking](/doc/gui_t3_roi_tracking.html) |
| Named fiber bundles across subjects | **Tractography → 3 Track → ▸ More… → Automatic tracking** | [AutoTrack and Tractometry](/doc/gui_t3_atk.html) |
| Diffusion measures within anatomical regions | Open a FIB in the tracking window | [Region-Based Analysis](#region-based-analysis) |
| Bundle metrics and along-tract profiles | Open a FIB and map or load tracts | [Tractometry](#tractometry) |
| Individual longitudinal or patient-versus-reference changes | Open the appropriate FIB in the tracking window | [Differential Tractography](/doc/gui_t3_dt.html) |
| Diffusion–behavior associations across a cohort | **Correlational Tractography** sidebar page | [Correlational Tractography](/doc/gui_cx.html) |
| Region-to-region or tract-to-region connectivity | Open a FIB in the tracking window | [Connectome](#structural-connectome-r2r-and-t2r) |
| Structural image inspection or registration | **Image Processing** sidebar page | Open image, linear registration, or nonlinear registration |

## Region-Based Analysis

Use region-based analysis when the question concerns diffusion measurements within an anatomical region.

1. On **Tractography → 3 Track**, open the subject `.fz` file.
2. [Load or define regions](/doc/gui_t3_roi_tracking.html#load-regions-from-built-in-atlases). Built-in atlases are useful for standardized regions, provided they match the image space.
3. If needed, use **[Slices][Insert Other Images]** in the tracking window to add registered measurements such as DKI, NODDI, PET, or other NIfTI data.
4. Use **[Regions][Statistics]** to obtain measurements for selected regions.

For population/template-space region analysis, a [`.dz` connectometry database](/doc/gui_cx.html) can be opened in the tracking window with template-space regions. A `.dz` is for group diffusion/connectometry data; it is **not** a region-to-region connectivity matrix.

## Tractometry

Tractometry quantifies diffusion or other measurements along white-matter pathways.

1. Map the pathways using [AutoTrack](/doc/gui_t3_atk.html) or [ROI-based fiber tracking](/doc/gui_t3_roi_tracking.html).
2. Add other registered measurements with **[Slices][Insert Other Images]** when needed.
3. Use **[Tracts][Statistics]** for bundle-level scalar and shape measurements.
4. Use the [tract profile](/doc/gui_t3_atk.html#tract-profile) when the spatial distribution of a measurement along the pathway is important.

Along-tract profiles currently use 100 sampling positions; keep profile definition and bandwidth consistent across subjects. For template-space tractometry, a compatible `.dz` database may be used with template-space pathways.

Example study: <https://www.nature.com/articles/nn.3870>

## Differential Tractography

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population. It is configured within the fiber tracking window after opening the appropriate FIB, rather than in the main window's Correlational Tractography page.

See [Differential Tractography](/doc/gui_t3_dt.html) for the four workflows: longitudinal native space, longitudinal template space, cross-sectional native space, and cross-sectional template space. The input images and reference maps must be appropriately aligned; scan/protocol differences can mimic biological change.

Example study: <https://pubmed.ncbi.nlm.nih.gov/31472253/>

## Correlational Tractography / Connectometry

**Correlational Tractography** is a separate sidebar page for cohort-level associations between local diffusion measures and study variables. Its three stages are:

1. **FIB files:** Select QSDR FIB inputs via **FIB File(s) (*.fz)...** or **FIB Folder (*.fz)...** to populate the database-creation workflow.
2. **Prepare database:** Open **Database (*.dz)...** to inspect or edit a connectometry database, subjects, and demographics.
3. **Run analysis:** Select **Database (*.dz)** to open group correlational tractography/connectometry analysis. Choose variables, covariates and diffusion index, then review association tracts and permutation-derived FDR.

For cohort data, use compatible QSDR/template-space FIB files with consistent resolution. A current `.dz` database can store several diffusion indices, so separate database files are generally not required for QA, FA, RDI and other available metrics. A **Recent Database** list is shown beneath the stages.

See the [full connectometry tutorial](/doc/gui_cx.html).

Example studies: [1](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734) · [2](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275) · [3](https://www.sciencedirect.com/science/article/pii/S1875957218301797)

## Structural Connectome (R2R and T2R)

**R2R** represents connectivity between pairs of brain regions. **T2R** records the relationship between named white-matter pathways and regions; both are distinct from the group-level diffusion association tested by Correlational Tractography.

**R2R workflow:**

1. Open a FIB with **Tractography → 3 Track → FIB File (*.fz)**, and generate or load the tracts to be analyzed.
2. In the tracking window, open **[Tracts][Connectivity Matrix]**.
3. Choose a parcellation in the **same coordinate space** as the tractography, and select endpoint/pass-through rules and matrix values.
4. Calculate and export the matrix, connectogram, or network measures.

**T2R workflow:**

1. Map named bundles using [AutoTrack](/doc/gui_t3_atk.html) or load an appropriate set of named tracts.
2. Load a matching parcellation from **[Step T3a][Atlas]** within the tracking window.
3. Select **[Regions][Tract-to-Region Connectome]** to generate the tract-by-region matrix and optionally visualize the parcellation using T2R values.

Native-space parcellations are supported when aligned to native-space tracts; QSDR is not universally required. Keep the tract set, parcellation and counting/weighting rules consistent for comparisons. Streamline counts do not directly measure axons or biological connection strength.

For more information, see [Whole-Brain Tractography and Connectivity Matrices](/doc/gui_t3_whole_brain.html) and [AutoTrack / T2R](/doc/gui_t3_atk.html).

Example study: [Yeh FC, *Nature Communications* 13, 4933 (2022)](https://www.nature.com/articles/s41467-022-32595-4).
