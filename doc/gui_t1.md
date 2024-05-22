# Parsing Images

DSI Studio accepts major image formats, including DICOM and NIFTI. The followings are steps to convert them to DSI Studio formats.

# Convert DICOM Files to SRC Files

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=65" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

To rename DICOM files in DSI Studio, you have two options: [Batch processing][Step B1a: Rename DICOM files] and [Batch processing][Step B1b: Rename DICOM in Subfolders].

For the former option, you need to select all the DICOM files that are ready to be renamed. Once selected, the files will be moved into their respective folders.

The latter option requires you to assign a root folder that contains all the DICOM files to be sorted. DSI Studio will then rename the DICOM files and place them in different folders based on their pulse sequences. You can refer to the figure on the right for a visual representation of this process.

After renaming the DICOM files, you can proceed to generate src.gz files for the next step by using [Batch processing][Step B2c: DICOM to SRC]. Simply select the root folder that contains all the scan data.

Alternatively, you can also convert DICOM to SRC by using [Diffusion MRI Analysis][Step T1: Open Source Images] in the main window. Manually select all the dMRI files, and DSI Studio will present a new window with a b-value table, as shown in the figure.

![](/images/t1_btable.png)

DSI Studio will show b-table extracted from the DICOM headers. You can replace the b-table by using the top [Files] menu.

Click [**Ok**] button to generate an SRC file for further reconstruction.

*TIP: GE, Philips, and Toshiba scanners output numerous DICOM images for dMRI acquisitions. Instead of selecting all files, you can select only one of the DICOM files in [**Step T1: Open Source Images**], and DSI Studio will ask whether you would like to load all other DICOM images. Select yes to specify that you want all DICOM files to be loaded.

# Convert NIFTI File to SRC Files

<iframe width="560" height="315" src="https://www.youtube.com/embed/iuBtgGLohsg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

The NIFTI files include no b-table information, and we will also a bval and a bvec file to create an SRC file.

If your data are stored in BIDS structure, a quick way to convert all NIFTI files to SRC files is [**Batch processing**][**Step B2a: NIFTI to SRC (BIDS)**].
You may also use [**Batch processing**][**Step B2b: NIFTI to SRC (Single Folder)**] to batch convert NIFTI files to SRC files.

To convert one subject's data, click on [**Step T1: Open Source Images**] in the main window and select the NIFTI file. DSI Studio will try to locate its associated bval and bvec files in the same folder. 

If DSI Studio cannot find bval or bvec file, you may specify them using the top menu [**Files**][**Open bval**] and [**Files**][**Open bvec**] 

![](/images/t1_mac_menu.png)

DSI Studio also has its own b-table text file format, arranged in bvalue bvecx bvecy bvecz. The first column is the b-value, and the rest is a unit vector of diffusion gradient direction.

> 3000    0.994200    -0.000000   -0.107600<br>
> 3000    0.985100    -0.130800   0.111300<br>
> 3000    0.985100    0.130800    0.111300<br>
> 3000    0.963800    -0.245300   -0.104400<br>
> 3000    0.963800    0.245300    -0.104400<br>
> 3000    0.994200    -0.000000   -0.107600<br>
> 3000    0.985100    -0.130800   0.111300<br>
> 3000    0.985100    0.130800    0.111300<br>
> 3000    0.963800    -0.245300   -0.104400<br>
> 3000    0.963800    0.245300    -0.104400<br>

The file can be loaded using the top menu [**Files**][**Open b-table**]


DSI Studio will also search under the NIFTI file directory for a file named "grad_dev.nii.gz" to include gradient nonlinearity information and also "nodif_brain_mask.nii.gz" as the brain mask.


# Convert Bruker 2dseq Files to SRC Files

To read 2dseq file, click [**Step T1: Open Source Images**] in the main window and select the "2dseq" file for creating the SRC file.

Note that DSI Studio will obtain spatial parameters from other accessory files including "3dproc" and "reco", which are stored in the same directory as 2dseq. In addition to these two files, the b-table will be obtained from the "method" file, which should be stored in the upper directory.  Make sure that these files exist and are in the correct relative directories.

Once the image is loaded, the b-table may need additional flipping (e.g. swap x-y, y-z, or x-z). For example, you may need to swap x-y to get the correct result.

# Convert Varian and Agilent FDF Files to SRC Files

Click [**Step T1: Open Source Images**] in the main window and select all fdf files in the folder. DSI Studio will extract b-table information and the DWI from the files.

# Step T1a: Quality Control (Optional)

It is recommended that the SRC files are examined by a quality control procedure to ensure its integrity and quality. The reference for this procedure is documented in Yeh, Fang-Cheng, et al. "Differential tractography as a track-based biomarker for neuronal injury." NeuroImage 202 (2019): 116131.

To do this, use [**Diffusion MRI Analysis**][**Step T1a: Quality Control**] and select the folder that contains SRC files (DSI Studio will search for all SRC files in the subdirectories). DSI Studio will provide a report in the following format, which can be paste in Excel:

<img src="https://github.com/frankyeh/DSI-Studio-Website/assets/275569/daf1e77f-5774-4284-9211-480c6a37a08a" width="600"/>

- Check consistency in image dimension, resolution, DWI count, shell count. It is likely that certain scans are incomplete, and the total DWI count will be different (see subject 102008 in the example).

- Check if there is a `low-quality outlier` label. DSI Studio will examine 'neighboring DWI correlation', which decreases if there is a prominent eddy current artifact, head motion, or any head coil issues that may impact the diffusion signals.

- Diffusion contrast estimates the contrast generated by the diffusion gradient. Higher values suggest better resolving power for fiber orientations.

<1.1 Poor   (unlikely to resolve fiber)
1.1~1.3 Fair (okay for resolving single fiber)
> 1.3 Good (can resolve multiple fibers)

DSI Studio will use an outlier checking function (e.g. 3 median absolute deviations) to label problematic data with 'low-quality outlier'.  The example on the top identifies a dataset with a neighboring DWI correlation of only 0.892282, which is substantially lower than the other. Another dataset 102008 has a different b-table, which cannot be used together with the others. If a dataset has a very low neighboring DWI correlation, you will need to examine the raw DWI images (see video above) to see if the problem is correctable. Otherwise, the problematic datasets should be excluded from the analysis.

# Flip b-table (Optional)

![](/images/t1_mac_menu2.png)

DSI Studio allows for flipping the b-table before creating the SRC file. The functions are under the [Edit] menu.

# Resample dMRI Signals (Optional)

DSI Studio can up-sample the dMRI signals to achieve a high spatial resolution. Once an SRC file is created (following the steps mentioned above), you may use the following steps to get a new SRC file with interpolated dMRI signals that achieves x2 or x4 upsampling

1. Open the created SRC file in [step 2 reconstruction]
2. Export a 4D NIFTI file using [Files][Save 4D nifti] and a b-table file using [Files][save b-table]    (For Mac and Linux users, the [Files] menu is on the very top of the system menu)
3. Open the 4D NIFTI file in step 1 open source images.
4. Load the created b-table file using [Files][Open b-table]
5. In the left bottom corner, select upsample 2 or upsampling 4
6. Create a new SRC file that has interpolated dMRI signals (which leads to interpolated ODF)

# Aggregating Multiple Scans (Optional)

- ***To merge data from the same scan section*** (i.e. no head rotation or movement):

1. Click on [**Step T1: Open source images**] and select the files from the first scan. DSI Studio will open up a dialog showing the DWI loaded. 
2. Then in the top menu, click on the item [**Files**][**Open images...**] and select the files from the second scan. The dialog will append these new DWIs to the existing list. If you load 4D NIFTI files here, you may need to manually concatenate the b-table using [**Files**] menu.
3. Create a joint SRC file.

- ***To merge scans from two SRC files:***

1. open the SRC file using [**Step T2: Reconstruction**] and export a 4D NIFTI file using the top menu [**File**][**Save 4D NIFTI**]. You also need to export the b-table as a single text file here.
2. repeat the same step for another SRC file.
3. manually merge two b-table text files using a text editor. Adding one after another.
4. Back to the DSI Studio main window. Click [**Step T1: Open source images**] and open the first exported 4D nifti file. DSI Studio will open up a new dialog. Add the second 4D NIFTI using the top menu item [**Files**][**Open Images...**].
5. Load the merged b-table created from (3) using the top menu item [**Files**][**Load b-table...**]
6. Create a joint SRC file.

- ***To merge scans at multiple time points:*** (Make sure you have DSI Studio with version dated after April/2022)

1. **Create SRC files** for each of the scans (see instruction on the top of this page) 
2. **Create a common image space:** Find the SRC file that has the best quality (see [visual troubleshooting](https://dsi-studio.labsolver.org/doc/gui_t2.html)), and save the sum of DWI as a NIFTI file using [**File**][**Save DWI Sum...**]. Before svaing, you may opt to apply [**Edit**][**Crop background volume**] to reduce image volume. The DWI sum can be interpolated to a higher resolution using [**Tools**][**O1: View Image**][**Edit**][**Make Isotropic**]. 

3. **Align SRC to the common space**: Open first SRC file using [**Step T2: Reconstruction**] and rotate the volume to a the "DWI sum" using the top menu item [**Edit**][**Resample to T1w/T2w...**]. Specify the NIFTI files for the DWI sum. DSI Studio will show a manual registration interface allowing you to rerun registration or adjust registration. Click ok once the alignment is good. Export the rotated volume using the top menu [**File**][**Save 4D nifti**]. DSI Studio will save the resampled NIFTI file as well as rotated bvec and bval. Repeat this step to output NIFTI file for each SRC file. You may use the following command can proceed multiple SRC files and saved as NIFTI files:
```
dsi_studio --action=rec --source=*.src.gz --rotate_to=dwi_sum.nii.gz --save_nii=*_aligned.nii.gz
```
4. **Merge all scans**: In DSI Studio main window. Click [**Step T1: Open source images**] and open all resampled NIFTI files and create a joint SRC file. You may also use the command line to merge the NIFTI file into a new SRC:
```
dsi_studio --action=src --source=scan1.nii.gz --other_source=scan2.nii.gz,scan2.nii.gz,scan3.nii.gz --output=combined.src.gz
```
