# How to Analyze dMRI

## Prepare your data

<ol class="ana-flow" aria-label="Diffusion MRI preparation stages">
  <li id="import">
    <span class="ana-num">1</span>
    <h3>Import</h3>
    <p class="ana-path"><span class="ui-key">Tractography</span><span class="ui-sep"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">1</span> Import</span></p>
    <p>Choose <strong>NIfTI file(s) or folder</strong>, <strong>DICOM file(s) or folder</strong>, or <strong>2dseq/FDF</strong> to import diffusion scans and create an SRC file (<code>.sz</code>).</p>
    <span class="ana-file">Output: .sz</span>
    <details>
      <summary>Options &amp; quality checks</summary>
      <p>Set the working directory using <span class="ui-key">Work Dir</span> and <span class="ui-key">Browse...</span>. <strong>NIfTI folder...</strong> detects BIDS datasets with <code>sub-*</code> directories and otherwise handles an ordinary NIfTI folder. <strong>Rename DICOM</strong> moves or renames files; it does not convert them.</p>
      <p>When needed, use <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">1</span> Import</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">More...</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">NIFTI quality control...</span>. Review <a href="/doc/gui_t1.html">SRC quality control</a> before group analyses; it is available under <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">2</span> Reconstruct</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">More...</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">SRC quality control...</span>.</p>
    </details>
  </li>
  <li id="reconstruct">
    <span class="ana-num">2</span>
    <h3>Reconstruct</h3>
    <p class="ana-path"><span class="ui-key">Tractography</span><span class="ui-sep"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">2</span> Reconstruct</span></p>
    <p>Open the imported <code>.sz</code> with <strong>SRC File(s) (*.sz)</strong>. Choose <a href="/doc/gui_t2.html">GQI or QSDR</a> and review the b-table, mask, and orientation before generating the FIB file (<code>.fz</code>).</p>
    <span class="ana-file">Output: .fz</span>
    <details>
      <summary>Options &amp; quality checks</summary>
      <p>Adjust processing choices as needed. For multiple SRC files, use <span class="ui-key">More...</span> for batch SRC-to-FIB reconstruction.</p>
    </details>
  </li>
  <li id="track">
    <span class="ana-num">3</span>
    <h3>Track &amp; analyze</h3>
    <p class="ana-path"><span class="ui-key">Tractography</span><span class="ui-sep"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span></p>
    <p>Open <strong>FIB File (*.fz)</strong> to enter the fiber tracking window. Run whole-brain or ROI-based tracking, AutoTrack, differential tractography, or connectivity analyses.</p>
    <span class="ana-file">Input: .fz</span>
    <details>
      <summary>Options &amp; quality checks</summary>
      <p>Under <span class="ui-key">More...</span>, find <span class="ui-key">T1W/T2W image...</span>, <span class="ui-key">Automatic tracking</span>, and <span class="ui-key">FIB quality control...</span>. Use <span class="ui-key">Recent FIB</span> / <span class="ui-key">Recent SRC</span> / <span class="ui-key">Template FIB</span> to reopen previous data or templates.</p>
    </details>
  </li>
</ol>

## Choose an analysis workflow

<nav class="ana-pick" aria-label="Analysis workflows">
  <div class="ana-pick-group"><span class="step-eyebrow">ONE SUBJECT</span>
    <a class="ana-card" href="#fiber-pathways"><strong>Fiber pathways</strong><p class="ana-desc">Reconstruct and inspect white-matter pathways using whole-brain or region-guided tracking.</p><span><span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span> · whole-brain / ROI</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#region-based-analysis"><strong>Region measures</strong><p class="ana-desc">Measure diffusion properties within selected anatomical regions.</p><span>FIB file · region statistics</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#tractometry"><strong>Bundle metrics &amp; profiles</strong><p class="ana-desc">Summarize tract measurements and examine how they vary along a pathway.</p><span>FIB file · tract statistics</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#structural-connectome-r2r-and-t2r"><strong>Connectivity (R2R / T2R)</strong><p class="ana-desc">Map connections between brain regions or between named tracts and regions.</p><span>FIB file · parcellation / bundles</span><b aria-hidden="true">→</b></a>
  </div>
  <div class="ana-pick-group"><span class="step-eyebrow">CHANGE &amp; COHORTS</span>
    <a class="ana-card" href="#named-bundles-across-subjects"><strong>Named bundles across subjects</strong><p class="ana-desc">Identify corresponding white-matter bundles across subjects with AutoTrack.</p><span><span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Automatic tracking</span></span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#differential-tractography"><strong>Individual change</strong><p class="ana-desc">Find pathway segments showing diffusion changes between scans or relative to a reference.</p><span>Differential tractography</span><b aria-hidden="true">→</b></a>
    <a class="ana-card" href="#correlational-tractography--connectometry"><strong>Diffusion–behavior association</strong><p class="ana-desc">Test associations between local diffusion measures and clinical or behavioral variables across subjects.</p><span>Correlational Tractography sidebar</span><b aria-hidden="true">→</b></a>
  </div>
  <div class="ana-pick-group"><span class="step-eyebrow">IMAGES</span>
    <a class="ana-card" href="#inspect-or-register-structural-images"><strong>Inspect or register structural images</strong><p class="ana-desc">View anatomical images and align them with diffusion data for accurate overlays and analysis.</p><span><span class="ui-key">Image Processing</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Image Editor</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Open image...</span></span><span><span class="ui-key">Linear Registration</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Linear registration...</span></span><span><span class="ui-key">Nonlinear Registration</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Nonlinear registration...</span></span><b aria-hidden="true">→</b></a>
  </div>
</nav>

## Fiber Pathways

Use fiber pathway analysis to reconstruct white-matter trajectories in an individual diffusion MRI dataset.

1. Open the subject FIB file with <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">FIB File (*.fz)</span> to enter the fiber tracking window.
2. For exploratory mapping, run [whole-brain tractography](/doc/gui_t3_whole_brain.html). Select the tracking method, seed strategy, stopping criteria, and tract length settings appropriate to the data.
3. For a particular pathway, [define or load inclusion, exclusion, and seed regions](/doc/gui_t3_roi_tracking.html) and generate ROI-based streamlines. Inspect them against the anatomy and save the resulting tract bundle.
4. Review the reconstructed paths in orthogonal slices and 3D before computing measurements or connectivity.

<aside class="ana-note" role="note"><p><strong>Check.</strong> Fiber tracking estimates plausible streamline trajectories rather than individual axons. Check image orientation, region alignment, and reconstruction quality before interpreting anatomy. Tracking thresholds and ROI placement can change the resulting pathway.</p></aside>

Full guides: [Whole-brain tracking →](/doc/gui_t3_whole_brain.html) · [ROI-based tracking →](/doc/gui_t3_roi_tracking.html)

## Region-Based Analysis

Use region-based analysis when the question concerns diffusion measurements within an anatomical region.

1. On <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span>, open the subject `.fz` file.
2. [Load or define regions](/doc/gui_t3_roi_tracking.html#load-regions-from-built-in-atlases). Built-in atlases are useful for standardized regions, provided they match the image space.
3. If needed, use **[Slices][Insert Other Images]** in the tracking window to add registered measurements such as DKI, NODDI, PET, or other NIfTI data.
4. Use **[Regions][Statistics]** to obtain measurements for selected regions.

<aside class="ana-note" role="note"><p><strong>Check.</strong> Built-in atlases must match the image space. For population/template-space region analysis, a <a href="/doc/gui_cx.html"><code>.dz</code> connectometry database</a> can be opened in the tracking window with template-space regions. A <code>.dz</code> is for group diffusion/connectometry data; it is <strong>not</strong> a region-to-region connectivity matrix.</p></aside>

## Tractometry

Tractometry quantifies diffusion or other measurements along white-matter pathways.

1. Map the pathways using [AutoTrack](/doc/gui_t3_atk.html) or [ROI-based fiber tracking](/doc/gui_t3_roi_tracking.html).
2. Add other registered measurements with **[Slices][Insert Other Images]** when needed.
3. Use **[Tracts][Statistics]** for bundle-level scalar and shape measurements.
4. Use the [tract profile](/doc/gui_t3_atk.html#tract-profile) when the spatial distribution of a measurement along the pathway is important.

<aside class="ana-note" role="note"><p><strong>Check.</strong> Streamline-order tract profiles use 100 sampling positions; image-axis profiles instead depend on image geometry. Keep profile definition and bandwidth consistent across subjects. For template-space tractometry, a compatible <code>.dz</code> database may be used with template-space pathways.</p></aside>

Example study of **bundle-level** QA tractometry (not a 100-node along-tract profile): [Pritschet et al., *Nature Neuroscience* (2024)](https://doi.org/10.1038/s41593-024-01741-0).

## Structural Connectome (R2R and T2R)

**R2R** represents connectivity between pairs of brain regions. **T2R** records the relationship between named white-matter pathways and regions; both are distinct from the group-level diffusion association tested by Correlational Tractography.

<div class="ana-two">
  <div>
    <h3>R2R: Region to region</h3>
    <ol>
      <li>Open a FIB with <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">FIB File (*.fz)</span>, and generate or load the tracts to be analyzed.</li>
      <li>In the tracking window, open <span class="ui-key">Tracts</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Connectivity Matrix</span>.</li>
      <li>Choose a parcellation in the <strong>same coordinate space</strong> as the tractography, and select endpoint/pass-through rules and matrix values.</li>
      <li>Calculate and export the matrix, connectogram, or network measures.</li>
    </ol>
  </div>
  <div>
    <h3>T2R: Tract to region</h3>
    <ol>
      <li>Map named bundles using <a href="/doc/gui_t3_atk.html">AutoTrack</a> or load an appropriate set of named tracts.</li>
      <li>Load a matching parcellation using the <strong>Regions</strong> atlas controls in the fiber tracking window.</li>
      <li>Select <span class="ui-key">Regions</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Tract-to-Region Connectome</span> to generate the tract-by-region matrix and optionally visualize the parcellation using T2R values.</li>
    </ol>
  </div>
</div>

<aside class="ana-note" role="note"><p><strong>Check.</strong> Native-space parcellations are supported when aligned to native-space tracts; QSDR is not universally required. Keep the tract set, parcellation and counting/weighting rules consistent for comparisons. Streamline counts do not directly measure axons or biological connection strength.</p></aside>

Full guide: [Connectome (R2R and T2R) →](/doc/gui_connectome.html)

Example study: [Yeh FC, *Nature Communications* 13, 4933 (2022)](https://www.nature.com/articles/s41467-022-32595-4).

## Named Bundles Across Subjects

Use AutoTrack to identify corresponding named white-matter pathways in multiple subjects and support consistent tractometry.

1. Open each subject FIB file using <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">FIB File (*.fz)</span>.
2. Select <span class="ui-key">Tractography</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key ana-step-key"><span class="ana-inline-num">3</span> Track</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">More...</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Automatic tracking</span> to open AutoTrack and choose the target bundles.
3. Run automatic tracking, review the resulting bundles against the subject anatomy, and save the tract files.
4. For quantitative comparisons, use **[Tracts][Statistics]** or the [tract profile](/doc/gui_t3_atk.html#tract-profile) with the same measurement definitions and profile settings across subjects.

<aside class="ana-note" role="note"><p><strong>Check.</strong> Automatic bundle identification still requires quality review. Compare homologous bundles in compatible image spaces and use consistent reconstruction, tracking, and tractometry settings. Missing or incomplete bundles should not be interpreted as an anatomical absence without checking the data.</p></aside>

[Full guide →](/doc/gui_t3_atk.html)

## Differential Tractography

Differential tractography maps pathway segments showing changes in diffusion measurements between scans or relative to a reference population. It is configured within the fiber tracking window after opening the appropriate FIB, rather than in the main window's Correlational Tractography page.

<div class="ana-four" aria-label="Differential tractography workflow options">
  <span><strong>Longitudinal</strong> · Native space</span>
  <span><strong>Longitudinal</strong> · Template space</span>
  <span><strong>Cross-sectional</strong> · Native space</span>
  <span><strong>Cross-sectional</strong> · Template space</span>
</div>

[Full guide →](/doc/gui_t3_dt.html)

<aside class="ana-note" role="note"><p><strong>Check.</strong> The input images and reference maps must be appropriately aligned; scan/protocol differences can mimic biological change.</p></aside>

Example study: [PubMed 31472253](https://pubmed.ncbi.nlm.nih.gov/31472253/)

## Correlational Tractography / Connectometry

**Correlational Tractography** is a separate sidebar page for cohort-level associations between local diffusion measures and study variables.

<ol class="ana-flow ana-flow-small" aria-label="Correlational tractography analysis stages">
  <li>
    <span class="ana-num">1</span><h3>FIB files</h3>
    <p>Select QSDR/template-space <strong>FIB File(s) (*.fz)...</strong> or <strong>FIB Folder (*.fz)...</strong> to collect subjects for a connectometry database.</p><span class="ana-file">.fz</span>
    <details><summary>Optional: reference population</summary><p>For reference-population workflows, use <span class="ui-key">More...</span><span class="ui-sep" aria-hidden="true"></span><span class="ui-key">Create population average...</span>.</p></details>
  </li>
  <li>
    <span class="ana-num">2</span><h3>Prepare database</h3>
    <p>Open <strong>Database (*.dz)...</strong> to inspect or edit the database, including the subject list and demographic variables.</p><span class="ana-file">.dz</span>
    
  </li>
  <li>
    <span class="ana-num">3</span><h3>Correlational tractography</h3>
    <p>Select <strong>Database (*.dz)</strong> to run group correlational tractography. Choose the study variable, covariates and diffusion index, then assess association tracts and permutation-derived FDR.</p><span class="ana-file">FDR</span>
    
  </li>
</ol>

<aside class="ana-note" role="note"><p><strong>Check.</strong> For cohort data, use compatible QSDR/template-space FIB files with consistent resolution. A current <code>.dz</code> database can store several diffusion indices, so separate database files are generally not required for QA, FA, RDI and other available metrics. A <strong>Recent Database</strong> list is shown beneath the stages.</p></aside>

[Full guide →](/doc/gui_cx.html)

Example studies: [*Brain* cohort study](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734) · [NeuroImage: Clinical study](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275) · [NeuroImage study](https://www.sciencedirect.com/science/article/pii/S1875957218301797)

## Inspect or Register Structural Images

Use the **Image Processing** sidebar to inspect anatomical images and align additional modalities with diffusion MRI data.

1. Select **[Image Processing][Image Editor][Open image...]** to inspect a structural image.
2. Use **[Image Processing][Linear Registration][Linear registration...]** to align images with a linear transform when appropriate.
3. Use **[Image Processing][Nonlinear Registration][Nonlinear registration...]** when deformable registration is needed.
4. Verify the alignment visually, including anatomical landmarks and orientation, before using the registered image for region definition, tract visualization, or measurement overlays.

<aside class="ana-note" role="note"><p><strong>Check.</strong> Registration quality depends on image contrast, anatomy, and transformation direction. Inspect the result before applying regions or measurements in another image space; an incorrectly aligned overlay can produce misleading tract or regional statistics.</p></aside>

For registered measurements in the tracking window, see [Region-Based Analysis](#region-based-analysis) and [Tractometry](#tractometry) above.

