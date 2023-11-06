# Surface visualization

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

DSI Studio is able to render cortical surface and assist localization of the fiber tracts, and the cortical surface can be added by [Slices][Add Isosurface][Full]. You may try different thresholds to the best surface rendering. A higher threshold often results in a smaller surface object, while the lower threshold may lose structural detail. You may need to repeat this step several times until you get a satisfactory result.

To use T1W as the surface, insert the T1W NIFTI file using [Slices][Insert Other images]. DSI Studio will register the image with MNI-space T1W and ask users for confirmation. If the registration is not perfect, you may adjust it using [Slice][Adjust Registration]. After registration, add the surface using [Slices][Add Isosurface][Full]. DSI Studio will ask for the confirmation of a registration result to remove the skull. After that, it will ask for the threshold for creating an isosurface. A higher threshold often results in a smaller surface object, while the lower threshold may lose structural detail. You may need to repeat this step several times until you get a satisfactory result.

*The opacity of the isosurface can be adjusted in the [Surface Rendering] option in the right-upper window.*

*You may create a cross-section of the surface by selecting the drop-down list on the top of the 3D window. The level of the cross-section is determined by the slice position. The rendering color and opacity can also be changed using the options under the item named "Surface".*


# Region visualization

<iframe width="560" height="315" src="https://www.youtube.com/embed/RdUgv9PFrks" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

DSI Studio provides line-art for coronal sections of regions using the following steps:

1. Load [HCP1065.1mm.fib.gz](http://brain.labsolver.org/) at [Step T3 Fiber Tracking]

2. Click [Step T3a][Atlas..] and select BrainSeg. Add all regions

3. [Step T3c Options][Region Windows][Show Edge] = All

4. Load MNI-space regions using [Slices][Insert T1W/T2W images]

5. On the middle top toolbar, select the region and change the background color to white and the foreground color to red/yellow, check the overlay.

# Slice visualization

Select [Slices][Insert Other Images] and assign DICOM or NIFTI files to provide the source data. Image registration will be conducted in the background. Once you load the structural images, DSI Studio starts a background registration. You may need to wait for a while until it reaches a good registration.

To save the transformation matrix, select [Slices][Save Registration]. The transformation matrix can be exported to a text file. You may also load an external transformation matrix in text format by [Slices][Load Mapping]. This will stop the background registration and overwrite it.


If the alignment is not good, you can manually adjust it using [Slices][Adjust Registration]. Use the "switch view" button to check the alignment.

![image](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

To visualize images in different color maps, click [Slices][Load Color Map] and select the PET_RGB.txt file.

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

# Device visualization

![image](https://user-images.githubusercontent.com/275569/147839140-127aedcb-e626-422a-8a3b-c1f7749d24d5.png)

DSI Studio can visualize SEEG electrodes, DBS lead, probes, or an obturator in the 3D space with tractography.


## automatic electrode detection using CT images

Insert CT images using [Slices][Insert T1W/T2W images] and wait until DSI Studio registers them with MRI. The detection needs isotropic resolution CT images. If your CT image is not isotropic, you can make it isotropic by using [Tools][O4 View Images][Edit][Make Isotropic]. After inserting the CT images, then the electrode can be detected using [Devices][Dertect electrodes]. The default parameters (value in millimeters) should work for most of the cases.

The location of the contacts will be captured as "regions," and the electrodes will be visualized, as shown in the Figure.

## manual adding devices

A new device can be added using [Device][New Device] on the top menu.

This will bring up the device list at the right lower corner:

![image](https://user-images.githubusercontent.com/275569/147839144-b4bb48b1-7aed-4774-9d14-4928d81c0cf2.png)

Then change the [Type] to specify devices and change the length to the desired length.

Specifying the [Type] will bring up the device in the 3D windows:

![image](https://user-images.githubusercontent.com/275569/147839149-88ba5c23-5c45-4481-9f62-22b6a0380af5.png)

You can change its location by first pressing Ctrl+A (Windows) or Cmd+A (Mac) and then left-clicking on the tip of the device to drag it. 

You may need to rotate the view to drag it at different directions.

To change its orientation, press Ctrl+A OR Cmd+A and left-click on the shaft to rotate it.

The slice location can also be dragged using Ctrl+A.

## save devices

The location of the devices can be saved as a CSV file. The file will include the following fields:

[Name],[Type],[Location in voxel space],[Orientation in voxel space],[Length],[Color in 32bit integer]

## adding customized devices

DSI Studio currently supports the following devices:

```
DBS Lead:Medtronic 3387
DBS Lead:Medtronic 3389
DBS Lead:Abbott Infinity
DBS Lead:Boston Scientific
SEEG Electrode:8 Contacts
SEEG Electrode:10 Contacts
SEEG Electrode:12 Contacts
SEEG Electrode:14 Contacts
SEEG Electrode:16 Contacts
Probe
Obturator:11 mm
Obturator:13.5 mm
```

To add a new device, please modify the device.txt file in the DSI Studio folder (In the Mac version, right-click on the dsi_studio.app to show content)

The following is an example for Medtronic 3387

![image](https://user-images.githubusercontent.com/275569/147839157-d999332d-f76d-415a-83a1-d0d7e6034b05.png)

The first row is the device name. Please use follow the style of [Device category]:[Device Name]

The second row is the length of each device segment in mm, whereas the third row indicates whether the segment is -1 (sphere tip), 0 (body), 1(contact), 2(3 side contacts), 3 (body with adjustable length). Each segment value is separated by a tab.

The last row is the radius in mm.

Once a new device is added, save device.txt back and restart DSI Studio to see if it takes effect.

If you would like to suggest a new type of segment, please feel free to contact me at frank.yeh@gmail.com

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
