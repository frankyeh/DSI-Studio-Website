## Introduction

Multishell acquisition, particularly the HCP-style 3-shell protocol, is widely adopted for advanced diffusion MRI beyond DTI. This protocol samples b-values of 1,000, 2,000, and 3,000 s/mm², each with 90 directions.

However, this approach has two key limitations:

* **Suboptimal Sampling Distribution**: The low b-value shell (b=1,000) is oversampled, resulting in redundant measurements that can be interpolated from neighboring directions. In contrast, the high b-value shell lacks sufficient angular coverage, despite requiring more directions due to lower correlation between signals. **An optimal design should maintain similar redundancy across shells**, allocating more directions to higher b-values and fewer to lower b-values.

* **Orientation Bias**: The 90 directions per shell are not uniformly distributed on the sphere, introducing orientation bias and reducing reproducibility when head positioning varies. This results from the HCP scheme’s attempt to avoid repeating directions across shells, a constraint that is unnecessary. Each shell represents a different diffusion sensitivity, and ensuring uniform angular sampling within each shell is more important for achieving rotation invariance and reproducibility.

This document outlines recommended acquisition strategies, including (1) a practical two-shell protocol using the built-in DTI sequence, and (2) an advanced 23-shell DSI protocol with 258 directions.

---

## Recommendation 1: Achieving Multishell Acquisition Using Built-In DTI Sequence

The built-in DTI protocol supports multishell acquisition compatible with advanced methods like GQI, QSDR, and RDI.

---

### Step 1: b = 3000 s/mm² (60 Directions, Minimum TE)

* **Geometry**: FOV 256 mm, Matrix 128×128, Slice 2 mm, Gap 0 mm, Axial oblique (AC-PC), Whole brain.
* **Diffusion**: b = 3000 s/mm², 60 directions, 2 b=0 images.
* **Timing**: TR 8000–12000 ms, TE set to *Minimum TE* (record value, e.g., 89.3 ms).
* **Parallel Imaging**: GE ASSET 2×, Siemens SMS 3–4×.

---

### Step 2: b = 1500 s/mm² (30 Directions, Same TE)

* Clone the b=3000 protocol.
* **Diffusion**: b = 1500 s/mm², 30 directions, 2 b=0 images.
* **Timing** (**Important**): Manually set TE to exactly match b=3000 (e.g., 89.3 ms); do **not** use "Min TE".
* **Geometry**: Verify all settings remain identical.

---

### Final Checks

Ensure identical coverage and TE; adjust NEX if needed for SNR. Save protocols as:

* `DTI_b3000_60dir_2mm_TE89`
* `DTI_b1500_30dir_2mm_TE89`


---

## Recommendation 2: 23-Shell DSI Acquisition (b-value = 0 to 4,000 across 258 Directions)

An advanced option is the 12-minute **grid-258** acquisition, sampling 23 b-values from 0 to 4,000 s/mm² across 258 directions. Compared to HCP multishell, it reduces oversampling at low b-values and extends to higher b-values, improving sensitivity to restricted diffusion.

This grid scheme directly addresses the sampling and orientation bias issues of HCP-style acquisitions.

> **With a multiband sequence (e.g., Siemens SMS or CMRR) at MB factor 4, this 2-mm, 258-direction acquisition completes in 12 minutes.**

**Benefits of Grid Sampling:**

* Uniform q-space coverage, avoiding shell sampling biases.
* More efficient: fewer low b-value samples, denser high b-value coverage.
* Compatible with DTI, ball-and-stick, NODDI, GQI, and other models.
* Captures a continuous range of diffusion patterns, improving detection of complex tissue changes such as edema and cellular infiltration, outperforming multishell schemes limited to 2–3 b-values.

**Limitations:**

* Requires bipolar diffusion encoding to correct eddy currents at the sequence level; standard *eddy* correction is insufficient due to low redundancy.
* Not compatible with spherical harmonics methods (e.g., CSD, MSMT-CSD).

## Steps to Install the 12-Minute Grid Scheme on Siemens Prisma Scanners

Use the following files to set up the 12-minute 258-direction scan on Siemens Prisma. For faster acquisition, consider the 5-minute 101-direction scan.

* [EXAR File](/files/QSI258.exar1)
* [EXAR Journal](/files/QSI258.exar1-journal)
* Protocol PDF: [Siemens SMS Version](/files/QSI258_SMS.pdf) or [CMRR Version](/files/QSI258.pdf)
* b-Table: [Grid-258 (Recommended)](/files/GRID258_VECTOR_TABLE.txt) or [Grid-101](/files/GRID101_VECTOR_TABLE.txt)

### Acquisition Notes

* Acquire both `dMRI_dir258_1_Siemens` (full DWI) and `dMRI_dir258_2_Siemens` (b0 reverse phase) for distortion correction.
* Required licenses: Siemens SMS EPI (MB imaging), DTI package (custom diffusion tables), and High-Performance Gradient (HCP) for high bandwidth readout.

### Sequence Configuration

1. In the diffusion tab, set **MDDW** to **Free** mode.
2. Place the b-table under `C:\MedCom\MriCustomer\seq\`. * Rename any existing `DiffusionVectors.txt` and copy the Grid-258 table as `DiffusionVectors.txt`.
3. Set `b-value1 = 0` and `b-value2 = 4000`.


## Steps to Install the 12-Minute q-Space Scheme on Other Scanners

Convert the [Grid-258 b-table](/files/GRID258_BVAL_BVEC.txt) to the required format for your scanner.

### Acquisition Parameters

1. Resolution: **2.0 mm isotropic** (increase to 2.4 mm if SNR is insufficient).
2. Matrix: **104 × 104**.
3. Slices: **72** (reduce if cerebellum coverage is not needed), no gap.
4. Multiband Acceleration: **Factor 3 or 4**.
5. Diffusion Scheme: Use **Bipolar** for eddy current compensation. *Monopolar with FSL's eddy correction is not recommended for grid sampling.*
6. Use **Minimum TE and TR**.
7. Pixel Bandwidth: \~**1700 Hz/pixel**.
8. Phase Encoding: **Anterior to Posterior (A>>P)**.
9. Acquire an additional b0 image with **Phase Encoding reversed (P>>A)** for distortion correction. Only the b0 image is needed for this.

---

## Quality Check for Preliminary Results

1. Verify that the brain contour is still visible in the DWI images at **b = 4000**. If not, consider reducing the maximum b-value to **3000** to improve SNR.
2. Generate SRC files from the diffusion MRI data. In DSI Studio, run [Diffusion MRI Analysis → Step T1a: Quality Control](/doc/gui_t1.html#step-t1a-quality-control-optional) and select the folder containing the SRC files. Review the **Neighboring DWI Correlation** values; low correlations may indicate motion artifacts or acquisition errors.

### Conclusion

Following these guidelines ensures efficient and high-quality multishell diffusion MRI acquisitions suitable for advanced analyses beyond DTI. Proper quality control is essential to verify data integrity before proceeding to model fitting and interpretation.


