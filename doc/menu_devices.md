# [Devices] Menu

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
