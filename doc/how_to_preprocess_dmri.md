# How to Preprocess dMRI

Not every dataset needs every correction. Which steps to run depends on how the data were acquired and on how the results will be used. In DSI Studio, preprocessing is done in the reconstruction window: open an SRC file and select **Preprocessing** in the sidebar. The page asks the questions in the figure below and highlights the steps that apply to your data.

![Choosing preprocessing steps for diffusion MRI](/images/tutorial_preprocessing_flow.png)

Answer the questions from top to bottom. The first two questions choose a column; the steps in that column, followed by the shared steps at the bottom, form your preprocessing. Dashed steps are optional and run only when the question in the same row is answered as shown on its arrow.

## Susceptibility distortion
{: #susceptibility }

Echo-planar imaging is sensitive to magnetic susceptibility differences at air–tissue boundaries. The artifacts are strongest at the frontal tip, near the sinuses, and at the temporal base, where signals can be compressed, stretched, or lost. Susceptibility also distorts the entire head along the phase-encoding direction, typically by a few voxels.

<div class="aq-check"><strong>When is it needed?</strong><p>Correct susceptibility distortion when results in native space must be aligned with the subject's structural images, for example T1w-based regions or surgical planning. It may not be needed if you will use QSDR, or if the DWI does not need to be aligned with the subject's T1w image.</p></div>

* **Reverse phase-encoding b0 available:** use **FSL topup**. For shell-based data that also need eddy correction, **FSL topup/eddy** handles both in one step.
* **No reverse phase-encoding scan:** **T1w-based distortion correction** warps the b0 image to the subject's T1w image. It is optional, and the correction may not be perfect.

## Eddy current distortion
{: #eddy }

Eddy currents from the diffusion gradients shift, shear, or scale the diffusion-weighted images, differently for each gradient direction. Correct them only if the distortion is obvious, for example when edges in different DWI volumes do not line up. Many newer sequences reduce eddy currents considerably, for instance with bipolar diffusion encoding, so this correction is not always needed. DSI Studio's default answer is therefore **No**.

FSL eddy requires shell-based acquisitions. DSI Studio reads the shell scheme from the b-table and sets the shell question automatically; grid (DSI) sampling is not supported by FSL eddy.

## Motion
{: #motion }

FSL eddy also corrects motion. DSI Studio, however, calculates the **neighboring DWI correlation** to detect motion and eddy distortion, and lets you drop outlier volumes (see [SRC quality control](/doc/gui_t1.html#step-t1a-quality-control)). Instead of correcting motion-corrupted data, dropping a few outlier volumes is often a simpler solution.

<div class="aq-warning"><strong>Motion Correction is optional</strong><p>Use it to save data with heavy subject motion that you do not want to exclude, for example DSI data that cannot use FSL eddy.</p></div>

## Bias field
{: #bias-field }

The bias field is a smooth brightness variation across the image caused by the receiver coils. **Bias Field Correction** evens out the signal and is recommended unless the data were already corrected.

## Isotropic resolution
{: #isotropic }

Tractography needs an isotropic voxel size, the same in all directions. If the voxels are not isotropic, use **Resample to Isotropic**. DSI Studio sets the default answer from the voxel size of the SRC file.

## Run the preprocessing
{: #run }

1. Open the SRC file and select **Preprocessing** in the sidebar of the reconstruction window.
2. Answer each question with **Yes** or **No**. Choosing **Yes** for reverse phase-encoding b0 or for T1w-based correction asks for the corresponding image file; if no file is selected, the answer returns to **No**.
3. Click **Run Preprocessing**. DSI Studio runs the highlighted steps in order and stops if one fails or is canceled.

Steps already applied to the file are marked with ✓ and skipped. DSI Studio identifies them from the processing report of the SRC file, and questions that no longer apply are grayed out. Each step can also be run on its own with its button.
