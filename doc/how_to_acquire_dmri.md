# How to Acquire dMRI

Diffusion MRI acquisition is a tradeoff among angular sampling, diffusion weighting, signal-to-noise ratio, scan time, and scanner capabilities. These are two example protocols for advanced diffusion MRI; adapt parameters to the study and validate them locally.

<div class="aq-chooser" aria-label="Choose an acquisition protocol">
  <a class="aq-choice" href="#two-shell-protocol"><span class="aq-choice-tag">Practical option</span><strong>Two-shell DTI sequence</strong><span>90 diffusion directions · b = 1,500 / 3,000</span><small>Uses a built-in diffusion sequence. Match geometry and TE across both scans.</small><span class="aq-choice-link">View protocol →</span></a>
  <a class="aq-choice" href="#grid-protocol"><span class="aq-choice-tag">Advanced option</span><strong>258-direction grid sampling</strong><span>23 b-values · up to b = 4,000</span><small>Designed for broader q-space coverage; requires appropriate sequence support.</small><span class="aq-choice-link">View protocol →</span></a>
</div>

<details class="aq-context"><summary>Why use a different sampling distribution?</summary>
<p>HCP-style three-shell acquisitions commonly use b = 1,000, 2,000, and 3,000 s/mm² with 90 directions per shell. Higher b-values can benefit from denser angular sampling, whereas lower-b measurements often have greater directional redundancy. The distribution of directions within each shell also matters for orientation dependence. These considerations motivate nonuniform shell allocation or grid sampling, but the best protocol depends on the target measures, scanner, and sequence.</p>
</details>

## Protocol 1: Two-shell acquisition with a built-in DTI sequence
{: #two-shell-protocol }

The built-in DTI protocol supports multi-shell acquisition compatible with advanced methods such as GQI, QSDR, and RDI.

<div class="aq-step"><div class="aq-step-head"><span class="aq-number">1</span><div><strong>High-b shell</strong><small>60 directions · b = 3,000 s/mm²</small></div></div>
<div class="aq-facts"><div><small>Geometry</small><strong>2 mm isotropic</strong><span>FOV 256 mm · matrix 128 × 128 · 0 mm gap · AC–PC axial oblique · whole brain</span></div><div><small>Timing</small><strong>Minimum TE</strong><span>TR 8,000–12,000 ms; record the TE (e.g. 89.3 ms)</span></div><div><small>Baseline</small><strong>2 b0 images</strong><span>GE ASSET 2× or Siemens SMS 3–4×, if supported</span></div></div></div>

<div class="aq-step"><div class="aq-step-head"><span class="aq-number">2</span><div><strong>Lower-b shell</strong><small>30 directions · b = 1,500 s/mm²</small></div></div>
<div class="aq-facts"><div><small>Start from</small><strong>Clone the first scan</strong><span>Keep FOV, matrix, slice geometry, and coverage identical</span></div><div><small>Timing — essential</small><strong>Match the first TE exactly</strong><span>Enter the recorded TE manually; do not leave this scan at “Min TE”</span></div><div><small>Baseline</small><strong>2 b0 images</strong><span>Adjust NEX if additional SNR is needed</span></div></div></div>

<div class="aq-check"><strong>Before scanning</strong><p>Confirm identical coverage, geometry, and TE across the two shells. Save clearly named protocols so the shared TE is identifiable.</p><code>DTI_b3000_60dir_2mm_TE89</code><code>DTI_b1500_30dir_2mm_TE89</code></div>

## Protocol 2: 258-direction grid sampling
{: #grid-protocol }

An advanced option is the 12-minute **grid-258** acquisition, sampling 23 b-values from 0 to 4,000 s/mm² across 258 directions. Compared with HCP multi-shell acquisition, it reduces oversampling at low b-values and extends to higher b-values, improving sensitivity to restricted diffusion.

<div class="aq-spec"><div><small>Directions</small><strong>258</strong></div><div><small>b-value shells</small><strong>23</strong></div><div><small>Maximum b-value</small><strong>4,000</strong></div><div><small>Example duration</small><strong>~12 min</strong><span>2 mm, multiband factor 4; scanner-dependent</span></div></div>

### Benefits of grid sampling

* Uniform q-space coverage, avoiding shell sampling biases.
* Fewer low-b samples and denser high-b sampling.
* Compatible with DTI, ball-and-stick, NODDI, GQI, and other applicable models.
* Samples a continuous range of diffusion weightings that can improve sensitivity to complex tissue changes such as edema and cellular infiltration.

<div class="aq-warning"><strong>Sequence compatibility matters</strong><p>Grid sampling calls for bipolar diffusion encoding to limit eddy-current effects; relying only on post-processing correction is not recommended here. The grid is not directly suitable for shell-based spherical-harmonic workflows that require repeated measurements on discrete shells, including conventional CSD or MSMT-CSD.</p></div>

## Set up the grid-258 scan

### Siemens Prisma

Use the following files to set up the 12-minute 258-direction scan on Siemens Prisma. For faster acquisition, consider the 5-minute 101-direction scan.

<div class="aq-files">
<a href="/files/QSI258.exar1"><strong>EXAR file</strong><small>Sequence configuration</small></a>
<a href="/files/QSI258.exar1-journal"><strong>EXAR journal</strong><small>Companion file</small></a>
<a href="/files/QSI258_SMS.pdf"><strong>Siemens SMS protocol</strong><small>PDF</small></a>
<a href="/files/QSI258.pdf"><strong>CMRR protocol</strong><small>PDF</small></a>
<a href="/files/GRID258_VECTOR_TABLE.txt"><strong>Grid-258 table</strong><small>Recommended · TXT</small></a>
<a href="/files/GRID101_VECTOR_TABLE.txt"><strong>Grid-101 table</strong><small>Shorter option · TXT</small></a>
</div>

#### Acquisition notes

* Acquire both `dMRI_dir258_1_Siemens` (full DWI) and `dMRI_dir258_2_Siemens` (reverse-phase b0) for distortion correction.
* Required licenses: Siemens SMS EPI (MB imaging), DTI package (custom diffusion tables), and High-Performance Gradient (HCP) for high-bandwidth readout.

#### Sequence configuration

1. In the diffusion tab, set **MDDW** to **Free** mode.
2. Place the b-table under `C:\MedCom\MriCustomer\seq\`. Rename any existing `DiffusionVectors.txt`, then copy the Grid-258 table to that folder as `DiffusionVectors.txt`.
3. Set `b-value1 = 0` and `b-value2 = 4000`.

### Other scanners

Convert the [Grid-258 b-table](/files/GRID258_BVAL_BVEC.txt) to the required format for your scanner, then use these acquisition parameters:

<div class="aq-params"><div><small>Resolution</small><strong>2.0 mm isotropic</strong><span>2.4 mm if SNR is insufficient</span></div><div><small>Matrix</small><strong>104 × 104</strong></div><div><small>Slices</small><strong>72, no gap</strong><span>Fewer if cerebellar coverage is not needed</span></div><div><small>Multiband</small><strong>Factor 3 or 4</strong></div><div class="aq-wide"><small>Diffusion encoding</small><strong>Bipolar</strong><span>Eddy-current compensation; monopolar acquisition with only post-processing eddy correction is not recommended for grid sampling</span></div><div><small>TE and TR</small><strong>Minimum</strong></div><div><small>Pixel bandwidth</small><strong>≈ 1700 Hz/pixel</strong></div><div><small>Phase encoding</small><strong>A&gt;&gt;P</strong><span>Anterior to posterior</span></div><div><small>Reverse-phase scan</small><strong>b0 only, P&gt;&gt;A</strong><span>For distortion correction</span></div></div>

## Quality check for preliminary results

1. Verify that the brain contour remains visible in DWI images at **b = 4000**. If not, consider reducing the maximum b-value to **3000** to improve SNR.
2. Generate SRC files from the diffusion MRI data. In DSI Studio, open <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-card"><span class="gui-card-num" aria-hidden="true">2</span><span class="gui-b">2 </span>Reconstruct</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">More…</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-action">SRC quality control...</span></span> and select the SRC files to review (multiple files are supported). See [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control). Review the **Neighboring DWI Correlation** values; low correlations may indicate motion artifacts or acquisition errors.
