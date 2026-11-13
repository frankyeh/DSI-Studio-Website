# Import: Generate SRC Files

DSI Studio converts diffusion MRI source data into an SRC file. Current SRC files use the <span class="gui-file-ext">.sz</span> extension; legacy <span class="gui-file-ext">.src.gz</span> files remain supported.

An SRC file stores the diffusion-weighted image volumes together with the b-table and other information needed for [reconstruction](/doc/gui_t2.html).

Older tutorial videos on this page may show the former main-window layout. The instructions below use current controls.

## XNAT download

In the DSI Studio main window, select <span class="gui-action">XNAT Download</span> in the left sidebar to connect to an XNAT server, browse project data, and select a local download directory. This retrieves data before SRC creation; review the downloaded series and their b-tables before importing them.

*Older XNAT screenshots may show the previous main-window layout.*

## DICOM to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=65" title="DICOM to SRC tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

In the current main window, open <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">DICOM</span></span> and choose <span class="gui-action">DICOM file(s)...</span> or <span class="gui-action">DICOM folder...</span>. DSI Studio reads the diffusion information and creates a <span class="gui-file-ext">.sz</span> file. The <span class="gui-action">▸ Rename DICOM</span> control exposes optional operations that move/rename DICOM files; these do not perform conversion.

For larger studies, the <span class="gui-action">DICOM folder...</span> option converts a folder of scans. Expand <span class="gui-action">▸ Rename DICOM</span> only when you explicitly need to reorganize the source files:

- <span class="gui-action">Rename DICOM files...</span> — organize selected DICOM files.
- <span class="gui-action">Rename DICOM folder...</span> — organize DICOM studies beneath a root folder.

The DICOM rename functions **move and rename files**. Keep an untouched backup and test on a small subset before processing a full study. For command-line organization, see [Rename DICOM CLI](/doc/cli_ren.html).

## NIFTI to SRC

<iframe width="560" height="315" src="https://www.youtube.com/embed/iuBtgGLohsg" title="NIfTI to SRC tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

A diffusion-weighted NIFTI image needs its diffusion gradient information. DSI Studio commonly reads this from FSL/BIDS `bval` and `bvec` sidecars.

For one scan:

1. Open <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI file(s)...</span></span> and select one 4D diffusion NIfTI file. To process multiple scans, use <span class="gui-action">NIfTI folder...</span>.
2. DSI Studio looks for matching `bval` and `bvec` files in the same directory.
3. If they are not found automatically, use <span class="gui-menu-path"><span class="gui-menu-item">Files</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Open bval</span></span> and <span class="gui-menu-path"><span class="gui-menu-item">Files</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Open bvec</span></span> in the **source-image / b-table parser window** opened by NIfTI import (not the DSI Studio main window).
4. Create the <span class="gui-file-ext">.sz</span> file and inspect the loaded b-table before continuing.

For folder conversion, choose <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI folder...</span></span>. The program detects a BIDS dataset from its `sub-*` directories and creates SRC outputs under `derivatives`; otherwise, it processes an ordinary NIfTI folder and places an SRC file beside each diffusion NIfTI. The older separate BIDS/single-folder buttons have been consolidated.

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

Load it using <span class="gui-menu-path"><span class="gui-menu-item">Files</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">Open b-table</span></span> in that same **source-image / b-table parser window**.

When present in the NIFTI directory, DSI Studio can also use `grad_dev.nii.gz` for gradient-nonlinearity information and `nodif_brain_mask.nii.gz` as a brain mask.

## Other supported source formats

### Bruker 2dseq

Select the `2dseq` file using <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2dseq/FDF</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2dseq / FDF file(s)...</span></span>. Keep its associated Bruker files in their expected relative directories so DSI Studio can read spatial parameters and the diffusion table.

After loading, verify the image and b-table orientation before reconstruction.

### Varian / Agilent FDF

On <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2dseq/FDF</span></span>, select the FDF files from the scan directory. DSI Studio reads the image and diffusion-gradient information and creates an SRC file.

## Folder and batch SRC creation

For multi-subject import, use the relevant main-window folder action:

| Input | Main-window action | Batch behavior |
|:--|:--|:--|
| **NIfTI / BIDS** | <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI folder...</span></span> | Detects `sub-*` BIDS directories and writes under `derivatives`, or processes a conventional folder beside the images. |
| **DICOM** | <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">DICOM</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">DICOM folder...</span></span> | Converts supported DICOM studies within the selected folder. |
| **Bruker / FDF** | <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2dseq/FDF</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2dseq / FDF file(s)...</span></span> | Accepts supported scanner-specific files; preserve accompanying scanner metadata. |

Current SRC files use <span class="gui-file-ext">.sz</span>. After conversion, run [SRC quality control](#step-t1a-quality-control) to identify scan outliers before reconstruction.

## Step T1a: Quality Control

Run <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Reconstruct</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">▸ More…</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">SRC quality control...</span></span> on the study's SRC files before reconstruction or population analysis. For NIfTI DWI quality control before SRC creation, use <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">1 Import</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIfTI</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">▸ More…</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">NIFTI quality control...</span></span>.

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

If isotropic resampling is needed, open the SRC file through <span class="gui-menu-path"><span class="gui-menu-item">Tractography</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">2 Reconstruct</span><span class="gui-menu-sep" aria-hidden="true">›</span><span class="gui-menu-item">SRC File(s) (*.sz)</span></span> and use the reconstruction image-editing/resampling tools. Resampling does not add anatomical information and should be used for a specific processing requirement rather than routinely.

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
