# Import: Generate SRC Files

DSI Studio converts diffusion MRI source data into an SRC file. Current SRC files use the `.sz` extension; legacy `.src.gz` files remain supported.

An SRC file stores the diffusion-weighted image volumes together with the b-table and other information needed for reconstruction.

Older tutorial videos on this page may show the former main-window layout. The instructions below use current controls.

## XNAT download

In the DSI Studio main window, select **XNAT Download** in the left sidebar to connect to an XNAT server, browse project data, and select a local download directory. This retrieves data before SRC creation; review the downloaded series and their b-tables before importing them.

*Older XNAT screenshots may show the previous main-window layout.*

## DICOM to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=65" title="DICOM to SRC tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

In the current main window, open **Tractography → 1 Import → DICOM** and choose **DICOM file(s)...** or **DICOM folder...**. DSI Studio reads the diffusion information and creates a `.sz` file. The **▸ Rename DICOM** control exposes optional operations that move/rename DICOM files; these do not perform conversion.

For larger studies, the **DICOM folder...** option converts a folder of scans. Expand **▸ Rename DICOM** only when you explicitly need to reorganize the source files:

- **Rename DICOM files...** — organize selected DICOM files.
- **Rename DICOM folder...** — organize DICOM studies beneath a root folder.

The DICOM rename functions **move and rename files**. Keep an untouched backup and test on a small subset before processing a full study. For command-line organization, see [Rename DICOM CLI](/doc/cli_ren.html).

## NIFTI to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/iuBtgGLohsg" title="NIfTI to SRC tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

A diffusion-weighted NIFTI image needs its diffusion gradient information. DSI Studio commonly reads this from FSL/BIDS `bval` and `bvec` sidecars.

For one scan:

1. Open **Tractography → 1 Import → NIfTI → NIfTI file(s)...** and select one 4D diffusion NIfTI file. To process multiple scans, use **NIfTI folder...**.
2. DSI Studio looks for matching `bval` and `bvec` files in the same directory.
3. If they are not found automatically, use **Files → Open bval** and **Files → Open bvec** in the **source-image / b-table parser window** opened by NIfTI import (not the DSI Studio main window).
4. Create the `.sz` file and inspect the loaded b-table before continuing.

For folder conversion, choose **Tractography → 1 Import → NIfTI → NIfTI folder...**. The program detects a BIDS dataset from its `sub-*` directories and creates SRC outputs under `derivatives`; otherwise, it processes an ordinary NIfTI folder and places an SRC file beside each diffusion NIfTI. The older separate BIDS/single-folder buttons have been consolidated.

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

Load it using **Files → Open b-table** in that same **source-image / b-table parser window**.

When present in the NIFTI directory, DSI Studio can also use `grad_dev.nii.gz` for gradient-nonlinearity information and `nodif_brain_mask.nii.gz` as a brain mask.

## Other supported source formats

### Bruker 2dseq

Select the `2dseq` file using **Tractography → 1 Import → 2dseq/FDF → 2dseq / FDF file(s)...**. Keep its associated Bruker files in their expected relative directories so DSI Studio can read spatial parameters and the diffusion table.

After loading, verify the image and b-table orientation before reconstruction.

### Varian / Agilent FDF

On **Tractography → 1 Import → 2dseq/FDF**, select the FDF files from the scan directory. DSI Studio reads the image and diffusion-gradient information and creates an SRC file.

## Folder and batch SRC creation

For multi-subject import, use the relevant main-window folder action:

| Input | Main-window action | Batch behavior |
|:--|:--|:--|
| **NIfTI / BIDS** | **Tractography → 1 Import → NIfTI → NIfTI folder...** | Detects `sub-*` BIDS directories and writes under `derivatives`, or processes a conventional folder beside the images. |
| **DICOM** | **Tractography → 1 Import → DICOM → DICOM folder...** | Converts supported DICOM studies within the selected folder. |
| **Bruker / FDF** | **Tractography → 1 Import → 2dseq/FDF → 2dseq / FDF file(s)...** | Accepts supported scanner-specific files; preserve accompanying scanner metadata. |

Current SRC files use `.sz`. After conversion, run [SRC quality control](#step-t1a-quality-control) to identify scan outliers before reconstruction.

## Step T1a: Quality Control

Run **Tractography → 2 Reconstruct → ▸ More… → SRC quality control...** on the study's SRC files before reconstruction or population analysis. For NIfTI DWI quality control before SRC creation, use **Tractography → 1 Import → NIfTI → ▸ More… → NIFTI quality control...**.

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

## B-table orientation

Incorrect b-table orientation produces incorrect fiber orientations. B-table flip/swap operations are available during source creation/reconstruction, but they should be applied only when QC and anatomical inspection indicate a problem.

For batch b-table checking and command-line correction, see [Reconstruction CLI](/doc/cli_t2.html).

## Isotropic resampling

If isotropic resampling is needed, open the SRC file through **Tractography → 2 Reconstruct → SRC File(s) (*.sz)** and use the reconstruction image-editing/resampling tools. Resampling does not add anatomical information and should be used for a specific processing requirement rather than routinely.

See [Step T2: Reconstruction](/doc/gui_t2.html#make-isotropic).

## Combining multiple scans

When multiple diffusion acquisitions were collected in the same image space and should be combined into one SRC file, the command line supports `--other_source`:

```bash
dsi_studio --action=src \
  --source=scan1.nii.gz \
  --other_source=scan2.nii.gz,scan3.nii.gz \
  --output=combined.sz
```

Do not combine acquisitions that require inter-scan spatial realignment as if they were already in the same image space. Process alignment/motion appropriately for the study design first.
