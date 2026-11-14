# How to Analyze dMRI

Diffusion MRI analysis starts from the research question. Whether the interest is a pathway, a region, a connection pattern, or a change across scans or subjects decides the approach.

## From scan to analysis
{: #pipeline }

1. **Acquire** the data: [How to acquire dMRI](/doc/how_to_acquire_dmri.html)
2. **Preprocess** only what is needed: [How to preprocess dMRI](/doc/how_to_preprocess_dmri.html)
3. **Import and reconstruct**: [import](/doc/gui_t1.html) the scans as an SRC file (<span class="gui-file-ext">.sz</span>), then [reconstruct](/doc/gui_t2.html) a FIB file (<span class="gui-file-ext">.fz</span>). Use **GQI** for one subject in native space, and **QSDR** for a cohort in template space.
4. **Analyze**: choose the approach that matches your question below.

## Choose by question
{: #choose }

| Question | Approach |
|:--|:--|
| Which pathways run through this brain? | [Fiber tracking](#fiber-pathways) |
| What are the diffusion measures in a region? | [Region-based analysis](#region-based-analysis) |
| How do measures vary along a bundle? | [Tractometry](#tractometry) |
| Which regions are connected, and by which bundles? | [Connectome (R2R / T2R)](#structural-connectome-r2r-and-t2r) |
| Where is the same named bundle in each subject? | [AutoTrack](#named-bundles-across-subjects) |
| Where did a subject change? | [Differential tractography](#differential-tractography) |
| Which pathways relate to a clinical or behavioral variable? | [Correlational tractography](#correlational-tractography--connectometry) |

## Fiber tracking
{: #fiber-pathways }

Fiber tracking estimates streamline trajectories that follow the local fiber orientations. Whole-brain tracking gives an exploratory overview of all pathways; region-guided tracking uses seed, inclusion, and exclusion regions to isolate one pathway. The result depends on the tracking thresholds and on where the regions are placed.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> streamline tract files and their 3D renderings.</p><p><b>Used for:</b> visualizing anatomy, surgical planning, and defining bundles for tractometry or connectome analysis.</p></div>

Guides: [Whole-brain tracking](/doc/gui_t3_whole_brain.html) · [ROI-based tracking](/doc/gui_t3_roi_tracking.html)

## Region-based analysis
{: #region-based-analysis }

Region-based analysis summarizes diffusion measures, or other registered images such as DKI, NODDI, or PET, within anatomical regions. Atlas regions must be in the same space as the data. For a cohort in template space, the regions can be applied to a connectometry database (<span class="gui-file-ext">.dz</span>).

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> a table of diffusion measures (for example FA, MD, QA) for each region.</p><p><b>Used for:</b> comparing regions or subjects, and relating regional measures to clinical variables.</p></div>

Guide: [Load regions from atlases](/doc/gui_t3_roi_tracking.html#load-regions-from-built-in-atlases)

## Tractometry
{: #tractometry }

Tractometry quantifies measures over a bundle, either as one summary value for the bundle or as a profile along its length. Keep the profile definition the same across subjects.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> bundle statistics (mean measures, volume, length, and shape) and profiles along the bundle.</p><p><b>Used for:</b> group comparisons, imaging biomarkers, and locating changes along a pathway.</p></div>

Guide: [Tract profile](/doc/gui_t3_atk.html#tract-profile) · Example of bundle-level tractometry: [Pritschet et al., *Nature Neuroscience* (2024)](https://doi.org/10.1038/s41593-024-01741-0)

## Structural connectome (R2R and T2R)
{: #structural-connectome-r2r-and-t2r }

**R2R** (region to region) maps connections between pairs of regions. **T2R** (tract to region) maps which regions each named bundle reaches. The parcellation must be in the same space as the tracts; a native-space parcellation works with native-space tracts.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> a region-by-region connectivity matrix (R2R) or a tract-by-region matrix (T2R), with connectograms and network measures.</p><p><b>Used for:</b> network and graph analysis, and linking bundles to the cortical regions they serve.</p></div>

Guide: [Connectome (R2R and T2R)](/doc/gui_connectome.html) · Example: [Yeh FC, *Nature Communications* 13, 4933 (2022)](https://www.nature.com/articles/s41467-022-32595-4)

## Named bundles across subjects
{: #named-bundles-across-subjects }

AutoTrack identifies the same named bundles in each subject, which allows consistent tractometry across a cohort. Review each bundle: a missing or incomplete bundle is not evidence of anatomical absence.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> the named bundles of each subject as tract files, with their statistics.</p><p><b>Used for:</b> comparing the same bundles across a cohort and building tractometry datasets.</p></div>

Guide: [AutoTrack](/doc/gui_t3_atk.html)

## Differential tractography
{: #differential-tractography }

Differential tractography maps pathway segments whose diffusion measures changed, either between two scans of the same subject (longitudinal) or relative to a reference population (cross-sectional), in native or template space. Differences in scanner or protocol can mimic biological change.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> tracts showing segments with increased or decreased diffusion measures, with their volume and length.</p><p><b>Used for:</b> following disease progression or treatment effects in individual subjects.</p></div>

Guide: [Differential tractography](/doc/gui_t3_dt.html) · Example: [PubMed 31472253](https://pubmed.ncbi.nlm.nih.gov/31472253/)

## Correlational tractography (connectometry)
{: #correlational-tractography--connectometry }

Correlational tractography tests, across a cohort, which pathway segments have local diffusion measures associated with a study variable, while controlling for covariates. The false discovery rate is estimated by permutation. It uses template-space (QSDR) data with consistent resolution, collected into a connectometry database.

<div class="aq-check"><strong>What you get</strong><p><b>Output:</b> tracts with positive or negative association to the study variable, with their false discovery rate.</p><p><b>Used for:</b> identifying pathways linked to clinical or behavioral variables in group studies.</p></div>

Guide: [Correlational tractography](/doc/gui_cx.html) · Examples: [*Brain*](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734) · [*NeuroImage: Clinical*](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275) · [*NeuroImage*](https://www.sciencedirect.com/science/article/pii/S1875957218301797)

## Before interpreting results
{: #before-interpreting }

* **Same space:** data, regions, and parcellations are aligned in the same image space.
* **Same settings:** reconstruction, tracking, and measurement settings are consistent across subjects.
* **Visual review:** tracts and registrations are checked against the anatomy.
* **Streamlines are not axons:** tracking estimates plausible trajectories, and streamline counts do not directly measure connection strength.

See [How to interpret dMRI](/doc/how_to_interpret_dmri.html) and [How to interpret tractography](/doc/how_tractography_works.html).
