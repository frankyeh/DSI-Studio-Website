## Introduction

Multi-shell acquisition, particularly the HCP-style three-shell protocol, is widely adopted for advanced diffusion MRI beyond DTI. This protocol samples b-values of 1,000, 2,000, and 3,000 s/mm², each with 90 directions.

However, this approach has two key limitations:

* **Suboptimal Sampling Distribution**: The low b-value shell (b = 1,000) is oversampled, resulting in redundant measurements that can be interpolated from neighboring directions. In contrast, the high b-value shell has lower angular redundancy despite requiring denser directional sampling because neighboring high-b signals are less correlated. **An optimal design should maintain similar redundancy across shells**, allocating more directions to higher b-values and fewer to lower b-values.

* **Orientation Bias**: The 90 directions per shell are not uniformly distributed on the sphere, introducing orientation bias and reducing reproducibility when head positioning varies. This results from the HCP scheme's attempt to avoid repeating directions across shells, a constraint that is unnecessary. Each shell represents a different diffusion sensitivity, and ensuring uniform angular sampling within each shell is more important for achieving rotation invariance and reproducibility.

This document outlines recommended acquisition strategies, including (1) a practical two-shell protocol using the built-in DTI sequence and (2) an advanced 23-shell DSI protocol with 258 directions.

---

## Recommendation 1: Multi-Shell Acquisition Using a Built-In DTI Sequence

The built-in DTI protocol supports multi-shell acquisition compatible with advanced methods such as GQI, QSDR, and RDI.

### Step 1: b = 3000 s/mm² (60 Directions, Minimum TE)

* **Geometry**: FOV 256 mm, matrix 128 × 128, slice thickness 2 mm, gap 0 mm, axial oblique (AC-PC), whole-brain coverage.
* **Diffusion**: b = 3000 s/mm², 60 directions, 2 b = 0 images.
* **Timing**: TR 8000–12000 ms; TE set to *Minimum TE* (record the value, e.g., 89.3 ms).
* **Parallel Imaging**: GE ASSET 2×; Siemens SMS 3–4×.

### Step 2: b = 1500 s/mm² (30 Directions, Same TE)

* Clone the b = 3000 protocol.
* **Diffusion**: b = 1500 s/mm², 30 directions, 2 b = 0 images.
* **Timing** (**Important**): Manually set TE to exactly match the b = 3000 scan (e.g., 89.3 ms); do **not** use "Min TE".
* **Geometry**: Verify that all geometry settings remain identical.

### Final Checks

Ensure identical coverage and TE; adjust NEX if needed for SNR. Save the protocols as, for example:

* `DTI_b3000_60dir_2mm_TE89`
* `DTI_b1500_30dir_2mm_TE89`

---

## Recommendation 2: 23-Shell DSI Acquisition (b-value = 0 to 4,000 across 258 Directions)

An advanced option is the 12-minute **grid-258** acquisition, sampling 23 b-values from 0 to 4,000 s/mm² across 258 directions. Compared with HCP multi-shell acquisition, it reduces oversampling at low b-values and extends to higher b-values, improving sensitivity to restricted diffusion.

This grid scheme directly addresses the sampling and orientation-bias issues of HCP-style acquisitions.

> **With a multiband sequence (e.g., Siemens SMS or CMRR) at MB factor 4, this 2-mm, 258-direction acquisition completes in about 12 minutes.**

**Benefits of Grid Sampling:**

* Uniform q-space coverage, avoiding shell sampling biases.
* Fewer low-b samples and denser high-b sampling.
* Compatible with DTI, ball-and-stick, NODDI, GQI, and other applicable models.
* Samples a continuous range of diffusion weightings that can improve sensitivity to complex tissue changes such as edema and cellular infiltration.

**Limitations:**

* Requires bipolar diffusion encoding to correct eddy currents at the sequence level; standard *eddy* correction is insufficient because of the low directional redundancy of grid sampling.
* Not compatible with spherical-harmonic shell methods that require repeated directions on discrete shells (e.g., conventional CSD or MSMT-CSD workflows).

## Steps to Install the 12-Minute Grid Scheme on Siemens Prisma Scanners

Use the following files to set up the 12-minute 258-direction scan on Siemens Prisma. For faster acquisition, consider the 5-minute 101-direction scan.

* [EXAR File](/files/QSI258.exar1)
* [EXAR Journal](/files/QSI258.exar1-journal)
* Protocol PDF: [Siemens SMS Version](/files/QSI258_SMS.pdf) or [CMRR Version](/files/QSI258.pdf)
* b-Table: [Grid-258 (Recommended)](/files/GRID258_VECTOR_TABLE.txt) or [Grid-101](/files/GRID101_VECTOR_TABLE.txt)

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

---

## Quality Check for Preliminary Results

1. Verify that the brain contour remains visible in DWI images at **b = 4000**. If not, consider reducing the maximum b-value to **3000** to improve SNR.
2. Generate SRC files from the diffusion MRI data. In DSI Studio, run [Diffusion MRI Analysis → Step T1a: Quality Control](/doc/gui_t1.html#step-t1a-quality-control-optional) and select the folder containing the SRC files. Review the **Neighboring DWI Correlation** values; low correlations may indicate motion artifacts or acquisition errors.

### Conclusion

Following these guidelines provides practical multi-shell and grid-sampling options for advanced diffusion MRI. Proper quality control remains essential before model fitting, reconstruction, or interpretation.
