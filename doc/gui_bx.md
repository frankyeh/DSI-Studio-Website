# Batch Processing Using the GUI

DSI Studio provides GUI batch functions for common multi-subject workflows. Test a batch operation on a small subset before running it on an entire study, especially when files will be renamed or moved.

## Step B0: XNAT Download

Use **Batch Processing → Step B0: XNAT Download** to connect to an XNAT server, select project data, and choose a local download directory.

![image](https://user-images.githubusercontent.com/275569/147839715-c06d0643-c0c8-45ea-a56b-8195ef4be4e3.png)

## Step B1: Rename and organize DICOM

The DICOM rename functions **move and rename files**. Keep a backup of the original data and test the operation first.

- **Step B1a: Rename DICOM files** — organize selected DICOM files.
- **Step B1b: Rename DICOM in Subfolders** — scan a root directory and organize the DICOM studies below it.

For command-line batch organization, see [Rename DICOM CLI](/doc/cli_ren.html).

## Step B2: Create SRC files

Use the input route that matches the source data:

- **Step B2a: NIFTI to SRC (BIDS)** for a BIDS dataset.
- **Step B2b: NIFTI to SRC (Single Folder)** for NIfTI data organized outside BIDS.
- **Step B2c: DICOM to SRC/NIFTI** for DICOM studies.

Current DSI Studio source files use the `.sz` extension.

After source creation, run [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control-optional) before reconstruction or group analysis.

## Step B3: Batch reconstruction

Use **Step B3: SRC to FIB** to reconstruct multiple `.sz` files. The normal reconstruction settings and quality checks still apply; batch processing should not be used to bypass visual QC.

Current reconstructed files use `.fz`.

See [Step T2: Reconstruction](/doc/gui_t2.html) for reconstruction choices.

For population/connectometry analysis, reconstruct the subject FIB files and then create a `.dz` database using [Correlational Tractography](/doc/gui_cx.html).

For longitudinal or cross-sectional differential analysis, follow the study-specific workflow in [Differential Tractography](/doc/gui_t3_dt.html).

## Step B4: Automatic fiber tracking

Use **Step B4: Automatic Fiber Tracking** when named pathways should be mapped across many subjects.

![image](https://user-images.githubusercontent.com/275569/147839692-9a136412-6734-4b8f-880c-c5571d464cfe.png)

### Recommended workflow

1. Select the `.fz` files to process.
2. Select the tract or tract groups.
3. Keep the FIB/template-derived defaults initially.
4. Run a small subset and inspect the anatomy in Step T3.
5. Only change tolerance, track density, pruning, or other tracking parameters when the anatomical results justify the change.
6. Apply the finalized settings consistently to the study cohort.

AutoTrack tolerance and track-to-voxel ratio are derived from the loaded FIB/template when not explicitly fixed; avoid copying old universal numeric defaults into new studies.

See [Automatic Fiber Tracking](/doc/gui_t3_atk.html) for the current workflow and troubleshooting guidance.

### Troubleshooting batch AutoTrack

If many subjects produce no result or implausible anatomy:

- review [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control-optional);
- verify b-table/image orientation and reconstruction;
- open representative `.fz` files and run [whole-brain tractography](/doc/gui_t3_whole_brain.html);
- confirm the selected tractography atlas/template is appropriate for the data;
- check whether the acquisition has sufficient spatial/angular resolution and SNR for the targeted pathway.

Do not solve a systematic acquisition or reconstruction problem by repeatedly relaxing AutoTrack constraints.

## Command-line alternatives

For automated pipelines or cluster processing, the corresponding command-line actions are:

- [SRC creation](/doc/cli_t1.html)
- [Reconstruction](/doc/cli_t2.html)
- [Automatic Fiber Tracking](/doc/cli_atk.html)
- [Fiber Tracking](/doc/cli_t3.html)
- [Connectometry](/doc/cli_cnt.html)
