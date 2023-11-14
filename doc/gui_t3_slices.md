# [Slices] Menu

## [Slices][Insert Other Images]

Select [Slices][Insert Other Images] and assign DICOM or NIFTI files to provide the source data. Image registration will be conducted in the background. Once you load the structural images, DSI Studio starts a background registration. You may need to wait for a while until it reaches a good registration.

To save the transformation matrix, select [Slices][Save Registration]. The transformation matrix can be exported to a text file. You may also load an external transformation matrix in text format by [Slices][Load Mapping]. This will stop the background registration and overwrite it.


If the alignment is not good, you can manually adjust it using [Slices][Adjust Registration]. Use the "switch view" button to check the alignment.

![image](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

To visualize images in different color maps, click [Slices][Load Color Map] and select the PET_RGB.txt file.


## [Slices][Add Isosurface]

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

DSI Studio is able to render cortical surface and assist localization of the fiber tracts, and the cortical surface can be added by [Slices][Add Isosurface][Full]. You may try different thresholds to the best surface rendering. A higher threshold often results in a smaller surface object, while a lower threshold may lose structural detail. You may need to repeat this step several times until you get a satisfactory result.

To use T1W as the surface, insert the T1W NIFTI file using [Slices][Insert Other images]. DSI Studio will register the image with MNI-space T1W and ask users for confirmation. If the registration is not perfect, you may adjust it using [Slice][Adjust Registration]. After registration, add the surface using [Slices][Add Isosurface][Full]. DSI Studio will ask for the confirmation of a registration result to remove the skull. After that, it will ask for the threshold for creating an isosurface. A higher threshold often results in a smaller surface object, while a lower threshold may lose structural detail. You may need to repeat this step several times until you get a satisfactory result.

*The opacity of the isosurface can be adjusted in the [Surface Rendering] option in the right-upper window.*

*You may create a cross-section of the surface by selecting the drop-down list on the top of the 3D window. The level of the cross-section is determined by the slice position. The rendering color and opacity can also be changed using the options under the item named "Surface".*

## Slice overlay

![image](https://user-images.githubusercontent.com/275569/147838851-2a802ca0-600c-4e6b-a35d-489b8667f654.png)

The following steps create an overlay from the added image:

1. Choose the [Slice] at [Step T3b Draw Region] to view the slices that will serve as an overlay image.

2. Change the contrast color by clicking the contrast color button and adjusting the contrast value. The color for the maximum value can be changed to red or yellow. The two-value input box controls the minimum and maximum contrast values. Any value between the maximum and minimum range will be an overlay on other images. DSI Studio supports multiple overlays (e.g. red or positive value blue for negative value). The one assigned later will be on the top overlay.

3. Check the "overlay" checkbox and switch to other image modalities to see the overlay result:

  The mosaic overlay can be created by changing the [region window][slice layout] in the Options Window to the right
 

# "Burn" tracts/regions on DICOM files for clinical navigation systems

First, use [Slices][Insert Other Images] and assign DICOM or NIFTI files to provide the source data.

Make sure that the current slice is the T1W DICOM you loaded, and then use [Slices][Mark Tracts on Slices] to create bright spots on the slices. You can also mark the region on T1W/T2W using [Slices][Mark Regions on Slices]. 

![image](https://user-images.githubusercontent.com/275569/147839247-2d07edb1-88c9-4c06-a75a-7a60d451031c.png)

To save slices back to the original DICOM, use [Slices][Save Slices to DICOM] and select the original DICOM images. DSI Studio will output new DICOM files with "mod" in the prefix of the file name.

# Saving/Loading Workspace

The workspace function allows users to save tracts, regions, devices, slices and rendering settings into a folder that can be loaded later.

The function is under the [View][Save Workspace...] at [Step T3 Fiber tracking].


DSI Studio will ask for an output folder to save the following files/folders:

| Files/Folders | Descriptions|
|:--------------|:------------|
| /tracts | subfolder storing tracts as tt.gz files |
| /regions | subfolder storing regions as nii.gz files |
| /devices | subfolder storing all devices in a device.dv.csv file (when loading back, DSI Studio will look for all dv.csv files) |
| /slices | subfolder storing current slices (if not the QA map) as a nii.gz file (when loading back, DSI Studio will look for all nii.gz files) |
| camera.txt |   file storing the view orientation |
| command.txt |  file storing additional command to restore the slice position. Additional commands can be added to change visualization (see command line --action=vis for details) |
| setting.ini  | file storing the rendering setting |

The above data and settings can be loaded back using the [View][Load Workspace...] function.

# Presentation Mode

DSI Studio can be used as a standalone 3D presentation viewer.

To use this "presentation mode," first create a workstation folder using the [View][Save Workspace] function in [Step T3 Fiber Tracking] as mentioned above.

Then copy the workstation folder to the DSI Studio folder (Windows version) and rename it as a folder named "presentation." 

![image](https://user-images.githubusercontent.com/275569/147838970-180575fd-56cd-4fe4-8156-b895fb4250a1.png)

For Mac versions, right-click on the dsi_studio.app to show content. Then copy the workstation folder to the /Contents/MacOS/ and rename it as "presentation"

![image](https://user-images.githubusercontent.com/275569/147838974-91d6c528-a31e-482d-bc74-8efd7131c586.png)

Last, copy the associated fib.gz file to the presentation folder.

The above steps change DSI Studio into a viewer for the workspace. You can start DSI Studio, and it immediately loads the workspace.

For Windows users, you may zip the entire dsi studio folder (which contains the presentation folder) as a zip file and share it with others.

For Mac users, the dsi_studio.app is a complete viewer app for the workspace.

**size reduction for presentation mode**

There are several ways to reduce the size and facilitate sharing of the presentation mode.

1. replacing fib.gz file with a *_qa.nii.gz file: Use [Step T3][Export][Save qa...] to save a *_qa.nii.gz file and move it to the /presentation folder. This also disables the tracking function in the presentation mode. 

2. remove hcp1065_2mm.fib.gz and mni_icbm152 files

3. remove animal templates in the /template folder


# Shortcuts and Graphic Control

## Region view (left lower corner)

Click on the brain buttons at the bottom row of the ROI window to switch between different slice views

- **Q** and **A**: move sagittal slide
- **W** and **S**: move coronal slide
- **E** and **D**: move axial slide
- **Z**: switch to sagittal view
- **X**: switch to coronal view
- **C**: switch to axial view
- press **Shift** to change drawing to erase
- Use **mouse wheel** to zoom in or zoom out in the left ROI window
- **Right double click** in the ROI window to move slices to the pointed location.
- **Middle mouse button** to drag a slice or a region in the ROI window to the left.


## 3D View (middle)

- **Mouse left button**: press and rotate the object
- **Mouse right button**: press and change the zoom scale
- **Mouse mid button**: press and move the object (you may also use direction keys to move the object)
- **Left double click** on a region: select it in the region list
- **mouse wheel** to zoom in or zoom out in the 3D window
- **Ctrl+M** to drag a slice or a region in the 3D window.
- **Alt+1**: remember the current viewport and slice position to memory slot 1 and **1**: return to the viewport and slice position recorded in memory slot 1

  The same function applies to **Alt+2**,...**Alt+9**, and **2**,**3**,...**9**

The following four shortcuts are for track editing. To edit the tracts, 1) hit the shortcut 2) press left mouse button 3) drag the cursor 4) release the mouse button. If the track selection further considers the income angle, use right mouse button instead. (please refer to here for details)

- **Ctrl+S**: select tracts in the 3D window 
- **Ctrl+D**: delete tracts in the 3D window
- **Ctrl+P**: delete tracts in the 3D window
- **Ctrl+X**: cut tracts in the 3D window (click-drag-release)(cannot undo)
- **Ctrl+T**: trim tracts
- **Ctrl+Z**: undo select and delete
- **Ctrl+Y**: redo select and delete
