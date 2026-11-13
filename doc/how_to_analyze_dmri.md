# How to Analyze dMRI

Start with your diffusion MRI scans, reconstruct fiber orientations, and then choose an analysis that answers your research question. The current DSI Studio main window organizes these operations under **Tractography**, **Correlational Tractography**, **Image Processing**, and **Settings**.

## Prepare your data

<ol class="ana-flow" aria-label="Diffusion MRI preparation stages">
  <li id="import"><span class="ana-num">1</span><h3>Import</h3><p class="ana-path">Tractography › 1 Import</p><p>NIfTI · DICOM · 2dseq/FDF</p><span class="ana-file">Output: .sz</span>
    <details markdown="1"><summary>Options &amp; quality checks</summary>
Select **NIfTI file(s)...** or **NIfTI folder...** (the latter detects BIDS datasets with `sub-*` directories and otherwise handles an ordinary NIfTI folder). For DICOM choose **DICOM file(s)...** or **DICOM folder...**. The **Rename DICOM** section contains file-moving/renaming operations, not conversion.
When needed, use **[Tractography][1 Import][More...][NIFTI quality control...]**. Review [SRC quality control](/doc/gui_t1.html) before group analyses; it is available under **[Tractography][2 Reconstruct][More...][SRC quality control...]**.
    </details></li>
  <li id="reconstruct"><span class="ana-num">2</span><h3>Reconstruct</h3><p class="ana-path">Tractography › 2 Reconstruct</p><p>GQI or QSDR</p><span class="ana-file">Output: .fz</span>
    <details markdown="1"><summary>Options &amp; quality checks</summary>
Click **SRC File(s) (*.sz)**. Choose [GQI or QSDR reconstruction](/doc/gui_t2.html), inspect the b-table, mask, orientation, and processing choices, and save the resulting FIB file (`.fz`). **More...** also offers batch SRC-to-FIB reconstruction.
    </details></li>
  <li id="track"><span class="ana-num">3</span><h3>Track &amp; analyze</h3><p class="ana-path">Tractography › 3 Track</p><p>Whole-brain · ROI · AutoTrack · connectome</p><span class="ana-file">Input: .fz</span>
    <details markdown="1"><summary>Options &amp; quality checks</summary>
Click **FIB File (*.fz)** to open the **fiber tracking window**, where you can perform whole-brain, ROI-based, automatic, differential, and connectome analyses. **More...** contains **T1W/T2W image...**, **Automatic tracking**, and **FIB quality control...**. The **Recent FIB / Recent SRC / Template FIB** controls below the stages reopen recent data or templates.
    </details></li>
</ol>

<aside class="ana-setup" role="note" markdown="1">
**Before starting.** Set the working directory using the persistent **Work Dir** bar and **Browse...** at the bottom of the main window. **AI Agents**, **Fiber Data Hub**, **XNAT Download**, and **Console...** are below the navigation list.
</aside>

## Choose an analysis workflow

Choose a workflow based on your scientific question. Each option links to a procedure or a more detailed guide.

<nav class="ana-pick" aria-label="Analysis workflows">
  <div class="ana-pick-group"><span class="step-eyebrow">ONE SUBJECT</span>
    <a class="ana-card" href="/doc/gui_t3_whole_brain.html"><strong>Fiber pathways</strong><span>Tractography › Track · whole-brain / ROI</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#region-based-analysis"><strong>Region measures</strong><span>FIB file · region statistics</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#tractometry"><strong>Bundle metrics &amp; profiles</strong><span>FIB file · tract statistics</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#structural-connectome-r2r-and-t2r"><strong>Connectivity (R2R / T2R)</strong><span>FIB file · parcellation / bundles</span><b aria-hidden="true">→</b></a>
  </div>
  <div class="ana-pick-group"><span class="step-eyebrow">CHANGE &amp; COHORTS</span>
    <a class="ana-card" href="/doc/gui_t3_atk.html"><strong>Named bundles across subjects</strong><span>Tractography › Track › Automatic tracking</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#differential-tractography"><strong>Individual change</strong><span>Differential tractography</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#correlational-tractography--connectometry"><strong>Diffusion–behavior association</strong><span>Correlational Tractography sidebar</span><b aria-hidden="true">→</b></a>
  </div>
  <div class="ana-pick-group"><span class="step-eyebrow">IMAGES</span>
    <div class="ana-card ana-static"><strong>Inspect or register structural images</strong><span>Image Processing › Image Editor › Open image...</span><span>Linear Registration › Linear registration...</span><span>Nonlinear Registration › Nonlinear registration...</span></div>
  </div>
</nav>

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

Streamline-order tract profiles use 100 sampling positions; image-axis profiles instead depend on image geometry. Keep profile definition and bandwidth consistent across subjects. For template-space tractometry, a compatible `.dz` database may be used with template-space pathways.

Example study of **bundle-level** QA tractometry (not a 100-node along-tract profile): [Pritschet et al., *Nature Neuroscience* (2024)](https://doi.org/10.1038/s41593-024-01741-0).

## Differential Tractography

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population. It is configured within the fiber tracking window after opening the appropriate FIB, rather than in the main window's Correlational Tractography page.

See [Differential Tractography](/doc/gui_t3_dt.html) for the four workflows: longitudinal native space, longitudinal template space, cross-sectional native space, and cross-sectional template space. The input images and reference maps must be appropriately aligned; scan/protocol differences can mimic biological change.

Example study: <https://pubmed.ncbi.nlm.nih.gov/31472253/>

## Correlational Tractography / Connectometry

**Correlational Tractography** is a separate sidebar page for cohort-level associations between local diffusion measures and study variables. Its three stages are:

1. **FIB files:** Select QSDR FIB inputs via **FIB File(s) (*.fz)...** or **FIB Folder (*.fz)...** to populate the database-creation workflow. The import stage also offers **▸ More... → Create population average...** for reference-population workflows.
2. **Prepare database:** Open **Database (*.dz)...** to inspect or edit a connectometry database, subjects, and demographics.
3. **Correlational Tractography:** Select **Database (*.dz)** to open group correlational tractography/connectometry analysis. Choose variables, covariates and diffusion index, then review association tracts and permutation-derived FDR.

For cohort data, use compatible QSDR/template-space FIB files with consistent resolution. A current `.dz` database can store several diffusion indices, so separate database files are generally not required for QA, FA, RDI and other available metrics. A **Recent Database** list is shown beneath the stages.

See the [full connectometry tutorial](/doc/gui_cx.html).

Example studies: [*Brain* cohort study](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734) · [NeuroImage: Clinical study](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275) · [NeuroImage study](https://www.sciencedirect.com/science/article/pii/S1875957218301797)

## Structural Connectome (R2R and T2R)

**R2R** represents connectivity between pairs of brain regions. **T2R** records the relationship between named white-matter pathways and regions; both are distinct from the group-level diffusion association tested by Correlational Tractography.

**R2R workflow:**

1. Open a FIB with **Tractography → 3 Track → FIB File (*.fz)**, and generate or load the tracts to be analyzed.
2. In the tracking window, open **[Tracts][Connectivity Matrix]**.
3. Choose a parcellation in the **same coordinate space** as the tractography, and select endpoint/pass-through rules and matrix values.
4. Calculate and export the matrix, connectogram, or network measures.

**T2R workflow:**

1. Map named bundles using [AutoTrack](/doc/gui_t3_atk.html) or load an appropriate set of named tracts.
2. Load a matching parcellation using the **Regions** atlas controls in the fiber tracking window.
3. Select **[Regions][Tract-to-Region Connectome]** to generate the tract-by-region matrix and optionally visualize the parcellation using T2R values.

Native-space parcellations are supported when aligned to native-space tracts; QSDR is not universally required. Keep the tract set, parcellation and counting/weighting rules consistent for comparisons. Streamline counts do not directly measure axons or biological connection strength.

For more information, see [Whole-Brain Tractography and Connectivity Matrices](/doc/gui_t3_whole_brain.html) and [AutoTrack / T2R](/doc/gui_t3_atk.html).

Example study: [Yeh FC, *Nature Communications* 13, 4933 (2022)](https://www.nature.com/articles/s41467-022-32595-4).
