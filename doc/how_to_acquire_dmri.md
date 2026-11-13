# How to Acquire dMRI

Diffusion MRI acquisition is a tradeoff among angular sampling, diffusion weighting, signal-to-noise ratio, scan time, and scanner capabilities. These are two example protocols for advanced diffusion MRI; adapt parameters to the study and validate them locally.

<div class="aq-chooser" aria-label="Choose an acquisition protocol">
  <a class="aq-choice" href="#recommendation-1-multi-shell-acquisition-using-a-built-in-dti-sequence"><span class="aq-choice-tag">Practical option</span><strong>Two-shell DTI sequence</strong><span>90 diffusion directions · b = 1,500 / 3,000</span><small>Uses a built-in diffusion sequence. Match geometry and TE across both scans.</small><span class="aq-choice-link">View protocol →</span></a>
  <a class="aq-choice" href="#recommendation-2-23-shell-dsi-acquisition-b-value--0-to-4000-across-258-directions"><span class="aq-choice-tag">Advanced option</span><strong>258-direction grid sampling</strong><span>23 b-values · up to b = 4,000</span><small>Designed for broader q-space coverage; requires appropriate sequence support.</small><span class="aq-choice-link">View protocol →</span></a>
</div>

<details class="aq-context"><summary>Why use a different sampling distribution?</summary>
<p>HCP-style three-shell acquisitions commonly use b = 1,000, 2,000, and 3,000 s/mm² with 90 directions per shell. Higher b-values can benefit from denser angular sampling, whereas lower-b measurements often have greater directional redundancy. The distribution of directions within each shell also matters for orientation dependence. These considerations motivate nonuniform shell allocation or grid sampling, but the best protocol depends on the target measures, scanner, and sequence.</p>
</details>

## Recommendation 1: Multi-Shell Acquisition Using a Built-In DTI Sequence

The built-in DTI protocol supports multi-shell acquisition compatible with advanced methods such as GQI, QSDR, and RDI.

### Step 1: b = 3000 s/mm² (60 Directions, Minimum TE)

<div class="aq-step"><div class="aq-step-head"><span class="aq-number">1</span><div><strong>High-b shell</strong><small>60 directions · b = 3,000 s/mm²</small></div></div>
<div class="aq-facts"><div><small>Geometry</small><strong>2 mm isotropic</strong><span>FOV 256 mm · matrix 128 × 128 · 0 mm gap · AC–PC axial oblique · whole brain</span></div><div><small>Timing</small><strong>Minimum TE</strong><span>TR 8,000–12,000 ms; record the TE (e.g. 89.3 ms)</span></div><div><small>Baseline</small><strong>2 b0 images</strong><span>GE ASSET 2× or Siemens SMS 3–4×, if supported</span></div></div></div>

### Step 2: b = 1500 s/mm² (30 Directions, Same TE)

<div class="aq-step"><div class="aq-step-head"><span class="aq-number">2</span><div><strong>Lower-b shell</strong><small>30 directions · b = 1,500 s/mm²</small></div></div>
<div class="aq-facts"><div><small>Start from</small><strong>Clone the first scan</strong><span>Keep FOV, matrix, slice geometry, and coverage identical</span></div><div><small>Timing — essential</small><strong>Match the first TE exactly</strong><span>Enter the recorded TE manually; do not leave this scan at “Min TE”</span></div><div><small>Baseline</small><strong>2 b0 images</strong><span>Adjust NEX if additional SNR is needed</span></div></div></div>

<div class="aq-check"><strong>Before scanning</strong><p>Confirm identical coverage, geometry, and TE across the two shells. Save clearly named protocols so the shared TE is identifiable.</p><code>DTI_b3000_60dir_2mm_TE89</code><code>DTI_b1500_30dir_2mm_TE89</code></div>

## Recommendation 2: 23-Shell DSI Acquisition (b-value = 0 to 4,000 across 258 Directions)

An advanced option is the 12-minute **grid-258** acquisition, sampling 23 b-values from 0 to 4,000 s/mm² across 258 directions. Compared with HCP multi-shell acquisition, it reduces oversampling at low b-values and extends to higher b-values, improving sensitivity to restricted diffusion.

This grid scheme directly addresses the sampling and orientation-bias issues of HCP-style acquisitions.

<div class="aq-spec"><div><small>Directions</small><strong>258</strong></div><div><small>b-value shells</small><strong>23</strong></div><div><small>Maximum b-value</small><strong>4,000</strong></div><div><small>Example duration</small><strong>~12 min</strong><span>2 mm, multiband factor 4; scanner-dependent</span></div></div>

**Benefits of Grid Sampling:**

* Uniform q-space coverage, avoiding shell sampling biases.
* Fewer low-b samples and denser high-b sampling.
* Compatible with DTI, ball-and-stick, NODDI, GQI, and other applicable models.
* Samples a continuous range of diffusion weightings that can improve sensitivity to complex tissue changes such as edema and cellular infiltration.

<div class="aq-warning"><strong>Sequence compatibility matters</strong><p>Grid sampling calls for bipolar diffusion encoding to limit eddy-current effects; relying only on post-processing correction is not recommended here. The grid is not directly suitable for shell-based spherical-harmonic workflows that require repeated measurements on discrete shells, including conventional CSD or MSMT-CSD.</p></div>

## Steps to Install the 12-Minute Grid Scheme on Siemens Prisma Scanners

Use the following files to set up the 12-minute 258-direction scan on Siemens Prisma. For faster acquisition, consider the 5-minute 101-direction scan.

<div class="aq-files">
<a href="/files/QSI258.exar1"><strong>EXAR file</strong><small>Sequence configuration</small></a>
<a href="/files/QSI258.exar1-journal"><strong>EXAR journal</strong><small>Companion file</small></a>
<a href="/files/QSI258_SMS.pdf"><strong>Siemens SMS protocol</strong><small>PDF</small></a>
<a href="/files/QSI258.pdf"><strong>CMRR protocol</strong><small>PDF</small></a>
<a href="/files/GRID258_VECTOR_TABLE.txt"><strong>Grid-258 table</strong><small>Recommended · TXT</small></a>
<a href="/files/GRID101_VECTOR_TABLE.txt"><strong>Grid-101 table</strong><small>Shorter option · TXT</small></a>
</div>

### Acquisition Notes

* Acquire both `dMRI_dir258_1_Siemens` (full DWI) and `dMRI_dir258_2_Siemens` (reverse-phase b0) for distortion correction.
* Required licenses: Siemens SMS EPI (MB imaging), DTI package (custom diffusion tables), and High-Performance Gradient (HCP) for high-bandwidth readout.

### Sequence Configuration

1. In the diffusion tab, set **MDDW** to **Free** mode.
2. Place the b-table under `C:\MedCom\MriCustomer\seq\`. Rename any existing `DiffusionVectors.txt`, then copy the Grid-258 table to that folder as `DiffusionVectors.txt`.
3. Set `b-value1 = 0` and `b-value2 = 4000`.

## Steps to Install the 12-Minute q-Space Scheme on Other Scanners

Convert the [Grid-258 b-table](/files/GRID258_BVAL_BVEC.txt) to the required format for your scanner.

### Acquisition Parameters

1. Resolution: **2.0 mm isotropic** (increase to 2.4 mm if SNR is insufficient).
2. Matrix: **104 × 104**.
3. Slices: **72** (reduce if cerebellar coverage is not needed), no gap.
4. Multiband acceleration: **factor 3 or 4**.
5. Diffusion scheme: use **bipolar** diffusion encoding for eddy-current compensation. *Monopolar acquisition with only post-processing eddy correction is not recommended for grid sampling.*
6. Use **minimum TE and TR**.
7. Pixel bandwidth: approximately **1700 Hz/pixel**.
8. Phase encoding: **anterior-to-posterior (A>>P)**.
9. Acquire an additional b0 image with **reversed phase encoding (P>>A)** for distortion correction. Only the b0 image is needed for the reverse-phase acquisition.

## Quality Check for Preliminary Results

1. Verify that the brain contour remains visible in DWI images at **b = 4000**. If not, consider reducing the maximum b-value to **3000** to improve SNR.
2. Generate SRC files from the diffusion MRI data. In DSI Studio, open **Tractography → 2 Reconstruct → ▸ More… → SRC quality control...** and select the SRC files to review (multiple files are supported). See [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control). Review the **Neighboring DWI Correlation** values; low correlations may indicate motion artifacts or acquisition errors.

### Conclusion

Following these guidelines provides practical multi-shell and grid-sampling options for advanced diffusion MRI. Proper quality control remains essential before model fitting, reconstruction, or interpretation.
