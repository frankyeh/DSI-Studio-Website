# Reconstruction

<iframe width="560" height="315" src="https://www.youtube.com/embed/-J8qBMiHQHk?start=215" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

The reconstruction step processes an SRC file from Step T1 to generate the FIB file, which can be further used in fiber tracking.


# Step T2: Open SRC File(s)
Click on [**Step T2 Reconstruction**] button in the main project window to select one or multiple SRC files.

DSI Studio will present a reconstruction window as shown in the figure to the right.

*Tip: you can select multiple SRC files here, and DSI Studio will reconstruct each of them respectively. Even if you have additional preprocessing such as flipping images or rotating them to the MNI space, DSI Studio will apply the same procedure to each of the SRC files.*

![image](https://user-images.githubusercontent.com/275569/147804658-3d2b3442-c0dd-4383-91cf-3718670b1413.png)


# Visual Quality Inspection (Optional)

Switch the first tab [**Source Images**].

1. Check eddy current distortion and motion: click on the b-table list on the left and use the keyboard arrow key to scroll down the list. If the brain is distorted across DWI, it is likely caused by the eddy current. Subject head movement can also be seen when scrolling through the DWIs.

2. Check bad slices: click on the [**Show bad slices**] button (available after 10/2/2019 version) and DSI Studio will mark bad slices and their belonging DWI volume in red (see below). I recommend using sagittal view to check the bad slices (click on the sagittal view button on the right)

![](/images/t1_bad_slices.png)

3. Checking other problems:

<iframe width="560" height="315" src="https://www.youtube.com/embed/stL4GMeTC1I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Step T2a: Setup a Mask

![image](https://user-images.githubusercontent.com/275569/147804666-b75d4167-ce90-4722-816e-a3106046f6f0.png)

The purpose of the mask is to filter out the background region, increase the reconstruction efficacy, and facilitate further visualization. In the mask selection window, there is a track bar at the bottom for slice selection. The mask can be generated using several built-in functions provided by DSI Studio:

- "Thresholding" generates an initial selection.
- "Smoothing" smooths contour.
- "Expansion" expands the current selection.
- "Erosion" shrinks the contour.
- "Defragment" filters out small fragments.

Recommendation steps include thresholding, smoothing, and defragment.
You can open/save the mask to a txt file or nifti file.

# Preprocessing Steps

Please follow each of the steps to preprocess DWI data.

## 1. TOPUP/EDDY

 ***If you have reverse-phase encoding acquisition:***
1. On the top menu, click on [Corrections][TOPUP/EDDY]
2. Select the reverse phase encoding acquisition (nii.gz or src.gz). If you are not sure how to identify it, please refer to the [video tutorial](https://www.youtube.com/watch?v=-J8qBMiHQHk&t=130s)
3. DSI Studio will call FSL's topup and eddy to handle susceptibility artifact and eddy current distortion.

***If you do not have reverse-phase encoding acquisition:***
1. On the top menu, click on [Corrections][EDDY]
2. DSI Studio will call FSL's eddy to handle susceptibility artifact and eddy current distortion.

**This preprocessing may take a long time (in hours)**
**After the correction, it is recommended to save a new SRC file**
**When a user load the SRC file, DSI Studio will check if there are previous TOPUP/EDDY correction results and will load them if users request the TOPUP/EDDY again**


## 2. Correct image orientations (animal scans)

Most animal scans are likely to be acquired with an orientation different from the animal template in DSI Studio. Therefore, it is necessary to rotate the image volume to match the template. This will ensure the b-table checking functions correctly and the atlas function in Step T3 works properly.

The following is the template orientation for mouse, marmoset, and rhesus data.
   
![](/images/t2_default_template.png)
   
Moving the sliding bar to the "right" should go to the "top" of the brain. For rhesus and marmoset scans, the slices should be oriented in the axial view, with the top slice position on the right of the sliding bar.

![image](https://user-images.githubusercontent.com/275569/149644623-ee22e1d3-d8a6-4650-b2ae-ce10d93e11f2.png)

Use [**Edit**][**Image Flip**] or [**Image Swap**] to ensure your image volume matches the template orientation. Note that either "Image Flip" or "Image Swap" will create a mirrored image, so you may need either 2 or 4 flip/swap operations to avoid creating mirrored images. ***You can always add an additional left-right flip to ensure an even number of operations***.
      
## 3. Remove background signals or crop image volume (optional, recommended for animal studies)

Animal scans often include signals from tissues outside the brain, and they can cause a problem in the atlas or template-related analysis.
 
This can be tackled by using [**Edit**][**Erase Background Signals**] to eliminate the signals outside the mask.
   
Also use [**Edit**][**Crop Background**] to reduce image volume,

## 4. Check b-table orientation (animal scans)

If your analysis does not consider fiber orientation (e.g. just analyze FA, ADC...etc.) You can ignore the b-table flipping problem. Otherwise, you will need to pay attention to the reconstructed fiber orientations.

Apply [**B-table**][**Check b-table**] to check b-table orientation. The b-table checking function will examine a total of 24 different flip and swap conditions and figure out the one that gives the best fiber coherence. 

The b-table checking function may fail if the SNR is low. In such cases, you might need to manually test different b-table flip/swap configurations to determine which one provides the correct fiber orientations.. 

## 5. Make isotropic (optional)

Many scans have non-isotropic resolution, which can cause registration problems. You can interpolate the data to make it isotropic using [**Edit**][**Make isotropic**]


# Step T2b(1): Select a Reconstruction Method

## Diffusion Tensor Imaging (DTI)

DTI analysis will generate one fiber orientation per voxel and associated anisotropy and diffusivity measure.
GQI and QSDR are recommended because DSI Studio will also calculate DTI metrics for GQI and QSDR.
The only condition to choose DTI here is when GQI fails to produce correct results.

## Generalized Q-sampling Imaging (GQI)(Recommended)

GQI is a model-free method for resolving fiber orientations and quantifying the anisotropy of diffusing water. DSI Studio will also calculate DTI metrics even if GQI is used.

The `diffusion sampling length ratio` controls GQI's sensitivity, and it is highly recommended to optimize the ratio using a preliminary scan following these steps:

1. Reconstruct FIB files using ratios of 0.3, 0.4, 0.5, ...2.0 etc.
2. Open one FIB file in [Step T3 Fiber Tracking]
3. Switch to the coronal view [View Coronal View] and adjust the [Tracking Threshold] option at the right upper corner under **[Step T3c: Options**][**Tracking Parameters**] so that the value covers only the white matter:

![image](https://user-images.githubusercontent.com/275569/149644632-a0cb7e70-734f-4737-97e7-a888d8d7d230.png)

4. Locate non-crossing regions (e.g. mid corpus callosum marked by the light red circle) and crossing regions (e.g. lateral corpus callosum marked by light blue). The optimal value should resolve crossing patterns in crossing regions (light blue) and still have clean fiber direction at mid corpus callosum (light red).

The general principle is to "**select the highest L that has an acceptable amount of spurious fibers.**"

![image](https://user-images.githubusercontent.com/275569/149644634-d80a0e27-bfcc-43e0-ac2a-bd5bef59e5eb.png)


## Q-Space Diffeomorphic Reconstruction (QSDR)

QSDR [11] is the MNI version of GQI. Choose QSDR if you would like to have all the following analyses in the MNI space (e.g. correlation tractography and related connectometry analysis).

To carry out other images modalities to the MNI space with QSDR, click on the [**Attach Images...**] button at the right side of the output resolution box. To carry out other images modalities to the MNI space with QSDR, click on the [**Attach Images...**] button at the right side of the output resolution box. If you have T1W-based ROI to be transformed along with the T1W, add the T1W first before adding the ROI. If you have T1W-based ROI to be transformed along with the T1W, add the T1W first before adding the ROI.

The QSDR reconstruction requires the assignment of a template (e.g. human, monkey, rat, mouse).
***Please make sure that your data match the template.***

# Step T2b(2): Specify Parameters

***Output ODF***

Checking this check box will export all ODF information and allow DSI Studio to run connectometry analysis or visualize the ODFs for inspection. Note that enabling this option will greatly increase the size of the .fib file.

***Check b-table***

DSI Studio allows for checking and correcting whether the b-table is flipped and/or swapped in x, y, or z-direction. The function is realized by using a fiber coherence index to check which settings a better result [14].  The checkbox for "check b-table" is located on the bottom of the reconstruction dialog after selecting the reconstruction method (e.g. DTI, GQI, ...etc.).

*If the diffusion signals are corrupted, this b-table checking may result in a random configuration that can be easily identified from the file name of the FIB file.

***Advanced Options: Scheme balance***

Check this option if you are using a shell acquisition to make sure that the orientational sampling is homogeneous.

***Advanced Options: No high b for DTI***

Check this if your tensor estimation uses only b-values lower than 1,500.

***Advanced Options: Other output metrics***

Specify the name of the metrics (separated by comma) to be included in the output FIB file:

- fa: fractional anisotropy
- rd: radial diffusivity
- rd1: first radial diffusivity
- rd2: second radial diffusivity
- md: mean diffusivity
- helix: helix angle
- tensor: the tensor metrix in txx,txy,txz,tyy,tyz
- nqa: normalized qa
- iso: isotropic measures
- gfa: generalized fractional anisotropy
- rdi: restricted diffusion
- nrdi: non-restricted diffusion
- jdet: Jacobian determinant used in the QSDR reconstruction.
