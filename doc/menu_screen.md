# [Screen] Menu

Use the **[Screen]** menu in the Fiber Tracking window to save 3D and 2D slice images, copy cropped views or galleries to the clipboard, export `.obj` or `.glb` 3D meshes, and record `.avi` videos. Set camera viewpoints beforehand in the [View menu](/doc/menu_view.html) or switch single, double, and stereoscopic viewports in the [Layout menu](/doc/menu_layout.html).

## Screen captures

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Save 3D Screen...</span> | Saves the live 3D view at its current window size without cropping (`.png`, `.bmp`, `.jpg`, or `.tif`), rendering dense fiber bundles completely and preserving any active **Layout › 3D Views** split. Choose `.png` or `.tif` for lossless quality. |
| <span class="gui-menu-item">Save 3D Screen in High Resolution..</span> | Prompts for custom pixel dimensions (`width height`, default `1024 800`) and saves a high-resolution 3D image (`.png`, `.bmp`, `.jpg`, or `.tif`) at the current camera angle before restoring the window view. Maximum resolution depends on your graphics hardware. |
| <span class="gui-menu-item">Save 3D Screen Left/Right Views...</span> | Saves a side-by-side image of the standard Left and Right sagittal 3D views at the current zoom level (`.png`, `.bmp`, `.jpg`, or `.tif`). Overwrites custom camera angles and leaves the live 3D view on the Right sagittal preset. |

## Multi-view captures

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Save 3D Screen in 3 Views..</span> | Saves a 2 × 2 composite (`.png`, `.bmp`, `.jpg`, or `.tif`) combining Left sagittal, Posterior coronal, and Superior axial 3D views with the active **Draw Regions** 2D view. Overwrites the live 3D camera to Superior axial and vertically clips tall 2D mosaics. |
| <span class="gui-menu-item">Save 3D Screen in 3 Views (Horizontal)...</span> | Captures four standard 3D views (Left sagittal, Right sagittal, Posterior coronal, and Superior axial), crops each tightly around the foreground, and saves a horizontal strip (`.png`, `.bmp`, `.jpg`, or `.tif`). Overwrites the live 3D camera to Superior axial. |
| <span class="gui-menu-item">Save 3D Screen in 3 Views (Vertical)...</span> | Captures four uncropped standard 3D views (Left sagittal, Right sagittal, Posterior coronal, and Superior axial) and stacks them into a vertical strip (`.png`, `.bmp`, `.jpg`, or `.tif`). Overwrites the live 3D camera to Superior axial. |

## ROI and 3D export

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Save ROI Screen...</span> | Saves the **Draw Regions** 2D view (`.png`, `.bmp`, or `.jpg`) using the active **ROI › Zoom** and layout (**Single Slice**, **3 Slices**, or **Mosaic**). In **3 Slices** layout, the embedded 3D quadrant is blank; use **Screen › Copy to Clipboard › ROI** to include it. |
| <span class="gui-menu-item">Save 3D Model...</span> | Exports checked tracts, checked regions, visible slices, and isosurfaces as centered millimeter meshes in `.glb` (single file) or `.obj` (with `.mtl` and `.jpg` slice textures). Requires at least one checked tract or region, excludes ODFs, labels, and unconverted devices, and stops if any checked bundle exceeds 3,000 visible streamlines after auto-pruning. |

## Clipboard

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Copy to Clipboard</span> | Opens the submenu for copying cropped 3D views, multi-angle 3D composites, per-tract or per-region galleries, and 2D slice views directly to the system clipboard. |

### [Copy to Clipboard] submenu

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">3D Screen</span> | Captures the current 3D view, crops uniform background margins tightly around the foreground, and copies the image to the clipboard without changing the camera angle. |
| <span class="gui-menu-item">3D Screen Left/Right Views</span> | Renders uncropped Left and Right sagittal 3D views side by side at the current zoom and copies the combined image to the clipboard. Overwrites the live 3D camera to the Right sagittal view. |
| <span class="gui-menu-item">3D Screen 3 Views</span> | Copies a 2 × 2 composite of Left sagittal, Posterior coronal, and Superior axial 3D views plus the active **Draw Regions** 2D view to the clipboard. Overwrites the live 3D camera to Superior axial. |
| <span class="gui-menu-item">3D Screen 3 Views Horizontal</span> | Captures four standard 3D views (Left sagittal, Right sagittal, Posterior coronal, and Superior axial), tightly crops each view, and copies the horizontal strip to the clipboard. Overwrites the live 3D camera to Superior axial. |
| <span class="gui-menu-item">3D Screen 3 Views Vertical</span> | Captures four uncropped standard 3D views (Left sagittal, Right sagittal, Posterior coronal, and Superior axial) and copies them as a vertical strip to the clipboard. Overwrites the live 3D camera to Superior axial. |
| <span class="gui-menu-item">3D Screen Each Tract</span> | Prompts for column count (`1`–`50`, default `5`), renders and crops each checked tract bundle individually from the current 3D view, and copies the multi-column gallery to the clipboard. Requires enabled **Tract Rendering**; canceling the prompt still uses `5` columns. |
| <span class="gui-menu-item">3D Screen Each Region</span> | Prompts for column count (`1`–`50`, default `5`), renders and crops each checked region individually from the current 3D view, and copies the multi-column gallery to the clipboard. Requires enabled **Region Rendering**; canceling the prompt still uses `5` columns. |
| <span class="gui-menu-item">ROI</span> | Copies the currently displayed **Draw Regions** 2D view—including **Single Slice**, **3 Slices** (with its live embedded 3D quadrant), or **Mosaic** layout—directly to the clipboard. |
| <span class="gui-menu-item">Track Report</span> | Performs no action in the current build and leaves the clipboard unchanged. Copy report text directly from the **Report** box in the **Tracts** dock, or use **Tracts › Statistics...** instead. |

## Rotation and video

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Auto Rotate</span> | Continuously rotates the live 3D view around the vertical axis at 10° per second while keeping zoom and rendering controls interactive (checkable toggle; select again to stop). |
| <span class="gui-menu-item">Record Video...</span> | Starts or stops live 10 fps `.avi` recording of interactive 3D motion at the current viewport size (avoid resizing the window while recording). Ignore the legacy <kbd><kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>R</kbd></kbd> prompt and select **Screen › Record Video...** again to stop and save. |
| <span class="gui-menu-item">Save Rotation Video...</span> | Renders a smooth 360° turntable `.avi` video (`1,800` frames at `30 fps`, `1980 × 1080` on Windows/Linux or current view size on macOS) and restores the camera afterward. Canceling early stops rendering and may leave a partial `.avi` file. |

## Capture and export notes

- **Preserving custom viewpoints:** All **Left/Right Views** and **3 Views** commands overwrite the live 3D camera with orthogonal presets, so store custom angles first in the [View menu](/doc/menu_view.html) (<kbd><kbd>Alt</kbd>+<kbd>1</kbd></kbd>–<kbd><kbd>Alt</kbd>+<kbd>4</kbd></kbd>).
- **Including devices in 3D exports:** Convert electrode contacts to spherical regions via **[Devices][Leads to ROI...]** ([Devices menu](/doc/menu_devices.html)) before selecting **[Screen][Save 3D Model...]** if you want leads included in `.glb` or `.obj` meshes.
