# ROI-based Fiber Tracking

Region-based fiber tracking is a fiber tracking approach for mapping white matter tracts in the brain using user-defined regions. It involves identifying specific regions of interest (ROIs) in the brain and then tracking the connections between these ROIs to understand the structural organization of the brain.

Example protocol for different pathways can be found in the [TrackEM project](https://my.vanderbilt.edu/tractem/protocol/) and recent [TRACULA update](https://www.biorxiv.org/content/10.1101/2021.06.28.450265v2.full)

# Step T3a: Assign Regions

First, open the main window and click a button named [**Step T3: Fiber Tracking**] to select a FIB file generated from Step T1-T2. DSI Studio will bring up the tracking window.

![image](https://user-images.githubusercontent.com/275569/147854254-70bd8cf7-9a47-485e-bab2-d38bfa19a2c6.png)

The upper left corner shows a region list under [**Step T3a: Assign Regions**]. A *region* is a set of voxels. It can serve as the no function type `...`, or `seed`, `ROI`, `ROA`...etc. to facilitate fiber tracking.

There are several ways to assign regions:

## Load Regions From NIFTI Files

For details, please check out the [[Regions] menu section](/gui_t3_regions.html)

## Load Regions From Built-In Atlases

DSI Studio provides a list of atlases that can be added to the region list. Users can add anatomical landmarks by clicking on the ![image](https://user-images.githubusercontent.com/275569/147854300-d08bfff8-0ef0-480e-893b-582c8ed9097b.png) button in [Step T3a]. DSI Studio will perform a nonlinear registration to bring the atlas to the subject space.

# Specify Region Types

![image](https://user-images.githubusercontent.com/275569/147854494-af8a958f-ba50-4d2b-89c1-5ea37df57824.png)

---

There are several region types available to control fiber tracking, including ROI, ROA, Seed, End, and Terminative. Each of them is explained in the following sections.

| Type | Function |
|:-----|:---------|
| ***...*** | This region type is only used for visualization and parcellation. It has no effect on fiber tracking. |
| ***Seed*** | *DO NOT* assign a seed region unless you want to speed up fiber tracking or refine tracking results. If no seed region is assigned, DSI Studio will use the whole brain region as the seed region. A common mistake is to assign cortical regions as seed regions. Consequently, DSI Studio will not initiate fiber tracking at the cortical region and resulting in poor tracking results. <br><br> The seed regions are locations where the tracking algorithm will initiate fiber points. Ideally, it should be located in the white matter, and the algorithm will track in two opposite directions until it reaches the cortex. The actual seeding points are "uniformly distributed" within the seeding voxel. For example, a seed voxel placed at (53,87,68) can have a subvoxel seeding point located within (52.5 to 53.5, 86.5 to 87.5, 67.5 to 68.5). Within the voxel region (52.5 to 53.5, 86.5 to 87.5, 67.5 to 68.5), DSI Studio draws a point within the voxel range using a uniform distribution. The point is then used as the starting point within the selected voxel. <br><br> To refine tracking result, a new seed region can be created from the tracks by [Tracts][Tract to ROI] function. I would also enlarge this new seed region by [Regions][Modify Region][Dilation]. <br><br> Users can specify a seeding point file to override the subvoxel seeding routine and guide the tracking algorithm to start at specific points. To do this, in the tracking parameter, assign "Voxel Center" to the "Seed Position" item and assign "Primary" to the "Seed Orientation". A text file storing a list of point coordinates is needed. For example, to start racking at (53.42, 87.34, 68.43), (53.41, 87.32, 68.32), and (53.67, 87.21, 68.21), you need to have a text file with the following content: <br><br>   > 5342 8734 6843 <br> > 5341 8732 6832 <br> > 5367 8721 6821 <br> > 100 -1 -1 <br><br> Here the coordinates are scaled by 100. The largest number accepted is 32767. The number of points will determine the number of tracks generated (In tracking parameters, please make sure that "Terminate if" has a number larger than your point count).|
| ***ROI*** | The region-of-interest (ROI) is used to *filter* tracks. It is NOT the starting point of the fiber tracking algorithm. <br> If there are two ROIs, they will function together. The final track will have to pass both of them. |
| ***ROA*** | The region-of-interest (ROA) is used used to *select* the tracks that pass through the region. It is NOT the starting point of the fiber tracking algorithm. <br><br>   A thin-slice `ROA` may have the *leap-across* problem. Fiber tracking can jump across a `ROA` slice if the step size is large. If this happens, enlarge the ROA. |
| ***End*** | *DO NOT* assign an `End` region unless you have tried assigning it as `ROI`. <br><br> An "End" region selects tracts that are ended (not passing) in the "end" region. It is much more restrictive than `ROI` and often generates no results. <br><br> If one ending region is assigned, then only the tracks ended within the regions are preserved. <br> If two end regions are assigned, then the tracks ended in both regions are preserved.  <br><br>Please note that the "end" region, unlike the terminative region, does not affect the termination of the tracking algorithm. It simply selects the tracts that end in it. |
| ***Terminative*** | A terminative region will intercept fiber tracking by terminating any tracts as soon as they enter it. It changes the behavior of a tracking algorithm and forces tracking to terminate. A terminative region is useful if one is to study the tracts that project to a nucleus or a specific cortical area. A terminative region does not allow a tract to pass through it, which is very different from an "end" region. <br> A terminative region can be used to terminate a track if the anisotropy level is greater than a threshold. The steps are the following: <br><br> 1. In the options window, set the anisotropy threshold to the maximum value <br> 2. Click on [Region][Whole Brain seeding]. This creates a region with FA greater than the threshold. <br> 3. Change the region type to "terminative" <br> 4. Setting the Fanisotropy threshold back to the minimum value <br> 5. Start fiber tracking. |
| ***NotEnd*** | Similar to ROA but only excludes tracts that end in the region (allows passing). |

## Tips

- Assign `Seed` ***only if*** you want to speed up fiber tracking by limiting the starting region of fiber tracking. 
- Always start with only one ROI and gradually add more restrictions, such as the second `ROI`, `ROA`, `End`...etc.
- Assign `ROA` to eliminate unwanted pathways.
- Assign `End` ***only if*** you have tried assigning it as `ROI` and want more restricted results in the endpoints.
- Assign `Terminative` ***only if*** you specifically want tracks to stop at a certain location.

# Step T2b: Draw Regions

![image](https://user-images.githubusercontent.com/275569/147854543-9001e2d5-580b-4a86-a9cd-f931ca3973ca.png)


***Tip: check out shortcuts at the bottom of this page.***

You can manually draw or edit a region at the window to the left-bottom widget, where a toolbar on its top shows different drawing tools. 

To draw a region in the region window, **left-click** and **drag** to create a new region. Any further click will add voxels to the existing region. To remove part of a region, **right-click** to assign the region to be erased.

The function of each tool is detailed as follows:

| Tool | Function |
|:-----|:---------|
| ![image](https://user-images.githubusercontent.com/275569/147854173-3cd05ebd-551c-4e9c-842f-935015c66c9d.png) | draws a rectangular region. |
| ![image](https://user-images.githubusercontent.com/275569/147854177-d991d6c1-8ea4-45de-9136-4a707f2dd48c.png) | draws a shape using the cursor trajectory. |
| ![image](https://user-images.githubusercontent.com/275569/147854181-7e12e978-b427-4086-b2b5-f5c212473292.png) | draws a polygon region. |
| ![image](https://user-images.githubusercontent.com/275569/147854183-b871cfa1-9320-4ed8-96e5-3ed00731f5ac.png) | draws a ball in the 3D space. |
| ![image](https://user-images.githubusercontent.com/275569/147854187-83303766-ad81-4f75-bcde-c357b20b1fff.png) | draws a cubic in the 3D space. |
| ![image](https://user-images.githubusercontent.com/275569/147854192-76a728bf-de78-42a2-a670-6419613651cd.png) | allows for dragging a region, or the slices. |
| ![image](https://user-images.githubusercontent.com/275569/147854206-77569995-ddf3-4df7-8631-a9bfa9f206d3.png) | can be used to estimate the distance in mm. |

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZkWBU_qnaKg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Modifying Regions (Optional) 

You can modify a region using [Regions Misc][Modify Regions] or [Move Regions]. 

The modifications includes moving the regions in the x, y, or z-direction. Flip x, flip y, or flip z correct the orientation problem. 

There are also morphology operators that can dilate, smooth, or erode the regions.

[All Exclude First] will erase the location of the first region from all other regions.
[First Excludes All] will erase other regions from the first region.
[All Intercept First] will intercept all other regions with the first region.
[All to First] will assign regions to the locations of the first region. This is often used in [creating a parcellation](https://twitter.com/FangChengYeh/status/1549549617699868677).

# Step T3d: Tracts

Click on the [Fiber Tracking] button to start fiber tracking. Only the checked regions will affect tracking results.

# (Optional) ROI files to FSL

The ROI saved by DSI Studio may not align perfectly in FSLeyes because of the different  translocation matrix in the NIFTI headers.

The following steps will convert DSI Studio ROIs to FSL space.

1. Prepare an FSL-generated mask or ROI file (e.g., nodif_brain_mask.nii.gz)
2. Click on [Tools][O6: Linear Registration Toolbox]
3. Select the DSI Studio ROI file
4. Select the FSL mask or ROI file
5. In the registration window, click on the [File] button on the left bottom corner and select [Save Transformed Image]
6. Save the converted ROI as a NIFTI file.

# (Optional) Add a new template

A *template* is a population-average image volume (e.g. T1W, FA...etc.) that defines a standard space. The template has its imaging modality, such as T1W, T2W, anisotropy map.

DSI Studio can include a new anisotropy template for different animal species or different subject populations (e.g., neonatal, elder...etc.).

To add a template, first, create a folder under the /atlas folder (e.g. /atlas/NAME_OF_TEMPLATE) and then copy the anisotropy map to the folder and name it NAME_OF_TEMPLATE.QA.nii.gz

DSI Studio also needs an "ISO" map to utilize dual-modality normalization. The file should also be placed under /atlas/NAME_OF_TEMPLATE and should be saved as NAME_OF_TEMPLATE.ISO.nii.gz

An ISO map is optional, but having an ISO template with the QA template will greatly increase the normalization accuracy.

If you only have a T1W template (e.g. child or elder population), then you can warp ICBM152 QA and ISO template to your template space using the following procedure:

1. [O6: Linear registration box] select "\atlas\ICBM152\ICBM152.QA.nii.gz" as the subject image and "\atlas\ICBM152\ICBM152.T1W.nii.gz" (provided in DSI Studio package, windows version) as the reference image. 

2. Click on [Save Warped Image] and save it as a new file. Let's name it new.QA.nii.gz

3. Repeat steps 1 and 2 but select "\atlas\ICBM152\ICBM152.ISO.nii.gz" to create new.ISO.nii.gz

4. [07: Nonlinear registration box], [Open Subject]->"\atlas\ICBM152\ICBM152.T1W.nii.gz" and [Open Reference]->your new t1w template. I would suggest using smoothness=1.0 or higher to avoid over-distorted results. Click [Run]. You may experiment with different smoothness values to get the best result.

5. After registration, select the top menu [File][Apply Warpping]->new.QA.nii.gz and overwrite it.

6. Repeat 5 but select new.ISO.nii.gz and overwrite it.

7. Now you have new QA and ISO templates in your T1W template space. After adding them to \atlas\NAME_OF_TEMPLATE, restart DSI Studio to see the template added to the menu.

# (Optional) Add a new atlas

An *atlas* is an integer-valued parcellation that records the location of each brain region. It usually has a corresponding value-name list in the text format.

1. **Prepare the atlas in NIFTI**: For other formats, please convert them to the NIFTI format as .nii.gz. 
2. **A .txt text file records the labels**: An example of the text can be find [here](https://github.com/frankyeh/DSI-Studio-atlas/blob/main/ICBM152/HCP-MMP.txt). Each line has a value-name pair separated by a tab or space. The file name should match the atlas (e.g. HCP-MMP.nii.gz and HCP-MMP.txt)
3. **Locate the target template folder**: A list of template space can be found [here](https://github.com/frankyeh/DSI-Studio-atlas), including ICBM152 (human young adult), neuonate, CIVM_mouse,...etc. You will find the same folders in the DSI Studio package. In windows, they are in under the \dsi_studio_64\atlas folder. In Mac, those folders are stored in the app package (Right-click on dsi_studio_64.app to open the DSI Studio package /Content/MacOS/atlas). 
4. **Convert atlas to the target template space**: click on [Tools][R1: Linear Registration Toolbox], first select the atlas file (from step 1) and then select the [TEMPLATE NAME].QA.nii.gz in the template folder (identified in Step 3). Once the two volume matches, save the atlas using [Files][Save Transformed Image]
5. **Copy atlas and its text labels to the template folder**: For the human atlas, please copy the .nii.gz and corresponding .txt files to the template folder (e.g., \atlas\ICBM152 in the dsi studio package). For animal atlas, please find the corresponding folder, such as the one for mouse, rat, marmoset, or rhesus. 

After copying a new atlas to the template space folder, restart DSI Studio to see the new atlas added to the ICBM152 menu.


# Shortcuts 

| Location | Shortcut | Function |
|:---------|:-----|:---------|
| 3D window | left button | drag to rotate view |
| 3D window | right button | drag to zoom |
| 3D window | middle button | drag to move |
| 3D window | wheel | zoom in or zoom out |
| 3D window | double left-clicks on a region | select it in the region list |
| 3D window | Ctrl+A | drag a slice or a region in the 3D window. |
| 3D window | Alt+1, Alt+2,...etc | remember the current viewport and slice position to memory slot 1 |
| 3D window | "1", "2",...etc. | return to the viewport and slice position recorded in memory slot 1 |
| ROI window | right double click |  move slices to the pointed location. |
| ROI window | middle button | drag a slice or a region in the ROI window to the left. |
| ROI window | wheel | zoom in or zoom out |
| Any | "Q" and "A" | move sagittal slide |
| Any | "W" and "S" | move coronal slide |
| Any | "E" and "D" | move axial slide |
| Any | "Z" | switch to sagittal view |
| Any | "X" | switch to coronal view |
| Any | "C" | switch to axial view |

***example: 3D eraser region***

1. Create a new cubic region as an "eraser" and move it to the top
2. In the 3D window, use Ctrl+A to drag the eraser region to regions to be erased
3. Use [Regions][Modified Checked Regions][All Excludes First] or Ctrl+Shift+2 to apply the erasing effect
