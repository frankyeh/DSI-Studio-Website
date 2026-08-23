# Step T1: Generate SRC Files

DSI Studio converts diffusion MRI source data into an SRC file. Current SRC files use the `.sz` extension; legacy `.src.gz` files remain supported.

An SRC file stores the diffusion-weighted image volumes together with the b-table and other information needed for reconstruction.

## DICOM to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=65" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

For a single diffusion scan, use **Step T1: Open Source Images** and select the DICOM folder or files. DSI Studio reads the diffusion information and creates a `.sz` file.

For larger studies, the **Batch Processing** functions can organize DICOM files and create SRC files:

- **Step B1a: Rename DICOM files** — organize selected DICOM files.
- **Step B1b: Rename DICOM in Subfolders** — organize DICOM studies below a root directory.
- **Step B2c: DICOM to SRC/NIFTI** — batch-create SRC or NIFTI files.

The DICOM rename functions move/rename files, so keep a backup and test them on a small subset first. See [GUI Batch Processing](/doc/gui_bx.html) and [Rename DICOM CLI](/doc/cli_ren.html).

## NIFTI to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/iuBtgGLohsg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

A diffusion-weighted NIFTI image needs its diffusion gradient information. DSI Studio commonly reads this from FSL/BIDS `bval` and `bvec` sidecars.

For one scan:

1. Click **Step T1: Open Source Images** and select the 4D diffusion NIFTI file.
2. DSI Studio looks for matching `bval` and `bvec` files in the same directory.
3. If they are not found automatically, load them from **[Files][Open bval]** and **[Files][Open bvec]**.
4. Create the `.sz` file and inspect the loaded b-table before continuing.

For batch conversion:

- **Step B2a: NIFTI to SRC (BIDS)** — process a BIDS dataset.
- **Step B2b: NIFTI to SRC (Single Folder)** — process NIFTI files organized outside BIDS.

For command-line workflows, see [SRC Creation CLI](/doc/cli_t1.html).

### DSI Studio b-table format

DSI Studio can also read a four-column text b-table:

```text
b-value  bvec-x  bvec-y  bvec-z
```

Example:

```text
3000 -0.994200 -0.000000 -0.107600
3000 -0.985100 -0.130800  0.111300
3000 -0.985100  0.130800  0.111300
```

Load it using **[Files][Open b-table]**.

When present in the NIFTI directory, DSI Studio can also use `grad_dev.nii.gz` for gradient-nonlinearity information and `nodif_brain_mask.nii.gz` as a brain mask.

## Other supported source formats

### Bruker 2dseq

Select the `2dseq` file in **Step T1: Open Source Images**. Keep its associated Bruker files in their expected relative directories so DSI Studio can read spatial parameters and the diffusion table.

After loading, verify the image and b-table orientation before reconstruction.

### Varian / Agilent FDF

Select the FDF files from the scan directory. DSI Studio reads the image and diffusion-gradient information and creates an SRC file.

# Step T1a: Quality Control

Run **Diffusion MRI Analysis → Step T1a: Quality Control** on the study's SRC files before reconstruction or population analysis.

The QC report is designed to identify subjects that differ substantially from the rest of the study. Review at least:

- image dimensions and voxel size;
- number of diffusion volumes and shells;
- b-table consistency;
- **Neighboring DWI Correlation**, which is sensitive to motion, eddy-current artifacts, signal dropout, and other acquisition problems;
- **Diffusion Contrast**, which summarizes diffusion-weighted signal contrast relevant to resolving fiber orientations;
- any `low-quality outlier` warning.

A flagged subject should be inspected directly rather than excluded automatically. Determine whether the cause is correctable (for example, an orientation or b-table problem) or reflects acquisition failure that requires exclusion.

The neighboring-DWI-correlation QC method is described in:

> Yeh FC, et al. *Differential tractography as a track-based biomarker for neuronal injury.* NeuroImage 202 (2019): 116131.

See the [citation page](/citation.html) for the full reference.

# B-table orientation

Incorrect b-table orientation produces incorrect fiber orientations. B-table flip/swap operations are available during source creation/reconstruction, but they should be applied only when QC and anatomical inspection indicate a problem.

For batch b-table checking and command-line correction, see [Reconstruction CLI](/doc/cli_t2.html).

# Isotropic resampling

If isotropic resampling is needed, open the SRC file in **Step T2: Reconstruction** and use the reconstruction image-editing/resampling tools. Resampling does not add anatomical information and should be used for a specific processing requirement rather than routinely.

See [Step T2: Reconstruction](/doc/gui_t2.html#make-isotropic).

# Combining multiple scans

When multiple diffusion acquisitions were collected in the same image space and should be combined into one SRC file, the command line supports `--other_source`:

```bash
dsi_studio --action=src \
  --source=scan1.nii.gz \
  --other_source=scan2.nii.gz,scan3.nii.gz \
  --output=combined.sz
```

Do not combine acquisitions that require inter-scan spatial realignment as if they were already in the same image space. Process alignment/motion appropriately for the study design first.
