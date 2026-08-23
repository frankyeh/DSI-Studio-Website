# How to Analyze dMRI

<img src="https://github.com/user-attachments/assets/9afac513-ccbd-419c-9d79-4b7dd9458294" width="800"/>

Most DSI Studio diffusion MRI workflows begin with the same three steps:

1. [Create SRC files (`.sz`) from DICOM or NIFTI diffusion data](/doc/gui_t1.html).
2. [Run SRC quality control](/doc/gui_t1.html#step-t1a-quality-control-optional) and exclude or correct problematic data before group analysis.
3. [Reconstruct FIB files (`.fz`)](/doc/gui_t2.html) using GQI for native-space analysis or QSDR when template-space reconstruction is needed.

After reconstruction, choose the analysis that matches the scientific question.

## Region-Based Analysis

<img src="https://user-images.githubusercontent.com/275569/147855916-ccd9a41d-fbfa-4011-9df9-92c4213e2fa3.png" width="800"/>

Use region-based analysis when the question concerns diffusion measurements within an anatomical region.

1. Open the subject `.fz` file in **Step T3: Fiber Tracking**.
2. [Load or define regions](/doc/gui_t3_roi_tracking.html#load-regions-from-built-in-atlases). Built-in atlases are usually the simplest choice for standardized regions.
3. If needed, use **[Slices][Insert Other Images]** to add registered measurements such as DKI, NODDI, PET, or other NIFTI data.
4. Use **[Regions][Statistics]** to obtain diffusion or other image measurements from the selected regions.

For population/template-space region analysis, [create a `.dz` connectometry database](/doc/gui_cx.html) from the subjects and open the database in Step T3. Regions used with a population database should be defined in the corresponding template space.

## Tractometry

Tractometry quantifies diffusion or other measurements along white-matter pathways.

1. Map the pathways using [automatic fiber tracking](/doc/gui_t3_atk.html) or [ROI-based fiber tracking](/doc/gui_t3_roi_tracking.html).
2. Add other image measurements with **[Slices][Insert Other Images]** when needed.
3. Use **[Tracts][Statistics]** for tract-level summary measurements.
4. Use the [tract profile](/doc/gui_t3_atk.html#tract-profile) when the spatial distribution of a measurement along the pathway is important.

For population/template-space tractometry, a `.dz` database can be opened in Step T3 and analyzed with template-space pathways.

Example study: <https://www.nature.com/articles/nn.3870>

## Differential Tractography

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population.

Use the dedicated [Differential Tractography documentation](/doc/gui_t3_dt.html) for the four common designs:

- longitudinal change in native space;
- longitudinal change in template space;
- cross-sectional comparison in native space;
- cross-sectional comparison in template space.

Example study: <https://pubmed.ncbi.nlm.nih.gov/31472253/>

## Correlational Tractography / Connectometry

Correlational tractography maps pathway segments whose diffusion measurements are associated with a study variable across a population. Connectometry uses permutation testing to estimate the statistical reliability of those findings.

The current workflow is:

1. [Create a connectometry database (`.dz`)](/doc/gui_cx.html) from the subject FIB files.
2. Load demographics and select covariates, the study variable, and diffusion index.
3. Run group connectometry and review the tract findings and FDR.

A current `.dz` database can store multiple diffusion indices, so separate database files are generally not required for QA, FA, RDI, and other available metrics.

Example studies: [1](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734) · [2](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275) · [3](https://www.sciencedirect.com/science/article/pii/S1875957218301797)

## Tract-to-Region (T2R) Connectome

The tract-to-region (T2R) connectome quantifies which named white-matter pathways innervate particular brain regions. It complements the conventional region-to-region (R2R) connectome by retaining the identity of the intervening tract.

1. Map the pathways using [AutoTrack](/doc/gui_t3_atk.html).
2. Load a brain parcellation from **[Step T3a][Atlas]**, such as HCP-MMP.
3. Use **[Regions][Tract-to-Region Connectome]** to generate the tract-by-region matrix.
4. The parcellation can be colored by T2R values for visualization.

Example study: [Yeh, Fang-Cheng. "Population-based tract-to-region connectome of the human brain and its hierarchical topology." Nature Communications 13, 4933 (2022).](https://www.nature.com/articles/s41467-022-32595-4)
