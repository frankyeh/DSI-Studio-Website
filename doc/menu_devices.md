# [Devices] Menu

DSI Studio can display devices such as SEEG electrodes, DBS leads, probes, and obturators together with images, regions, and tractography.

![image](https://user-images.githubusercontent.com/275569/147839140-127aedcb-e626-422a-8a3b-c1f7749d24d5.png)

## Electrode detection from CT

1. Load the CT with **[Slices][Insert Other Images]**.
2. Verify the CT-to-MRI/diffusion registration before using the detected locations.
3. If needed, resample the CT to isotropic resolution using the image-processing tools.
4. Use **[Devices][Detect Electrodes]** to identify electrode contacts.

Detected contacts can be added as regions and displayed with the electrode/device geometry.

## Add a device manually

Use **[Devices][New Device]** to add a device, then select the device type and dimensions in the device table.

![image](https://user-images.githubusercontent.com/275569/147839144-b4bb48b1-7aed-4774-9d14-4928d81c0cf2.png)

The selected device is displayed in the 3D view.

![image](https://user-images.githubusercontent.com/275569/147839149-88ba5c23-5c45-4481-9f62-22b6a0380af5.png)

Use the 3D editing controls to move the device tip and adjust its orientation. Rotate the camera and inspect the device from multiple views before using its position for interpretation.

## Save devices

Device positions can be saved to CSV and restored later. A saved workspace can also include the current devices together with tracts, regions, slices, and rendering settings.

See the [[Slices] menu](/doc/menu_slices.html#workspaces) for workspace saving/loading.

## Custom device definitions

DSI Studio packages device definitions with the application. If a study requires a custom device geometry, edit the device-definition file in the DSI Studio package and restart the application after saving the change.

Keep a backup of the original definition file before modifying it, and preserve the segment lengths/radii in millimeters.
