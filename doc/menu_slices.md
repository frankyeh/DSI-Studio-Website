# [Slices] Menu

The **Slices** menu loads additional image volumes into the Step T3 tracking window and provides registration, overlay, surface, and export functions.

## Insert Other Images

Use **[Slices][Insert Other Images]** to add DICOM or NIfTI data such as T1w, T2w, quantitative MRI, PET, statistical maps, or other aligned measurements.

DSI Studio registers an inserted image to the diffusion data when needed. Inspect the alignment before using the image for quantitative sampling, ROI definition, or visualization.

- **Save Registration** exports the current mapping.
- **Load Mapping** loads an existing mapping and replaces the current registration.
- **Adjust Registration** allows manual alignment when needed.

![image](https://user-images.githubusercontent.com/275569/147839242-16c74c18-1f17-4b7c-bb80-eaee611fccd2.png)

## Add Isosurface

<iframe width="560" height="315" src="https://www.youtube.com/embed/PdjfjRW7bLw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Use **[Slices][Add Isosurface]** to render a surface from the current image. The selected intensity threshold determines the extracted surface; inspect the result and adjust the threshold when needed.

Surface color, opacity, and clipping/cross-section settings are available in the rendering options.

## Slice overlays

An inserted image can be shown as an overlay:

1. Select the image in the slice view.
2. Adjust its contrast range and color mapping.
3. Enable **Overlay**.
4. Switch to another slice image to view the overlay on top of it.

Multiple overlays can be displayed. Verify image registration before interpreting their anatomical relationship.

## Mark tracts or regions on DICOM slices

For clinical-navigation export workflows:

1. Insert the source DICOM/T1w image with **Insert Other Images**.
2. Select that image as the current slice.
3. Use **[Slices][Mark Tracts on Slices]** or **[Slices][Mark Regions on Slices]**.
4. Use **[Slices][Save Slices to DICOM]** to create modified DICOM output.

Keep the original clinical DICOM data unchanged and export marked images as separate files.

# Workspaces

A workspace saves the current visualization state, including tracts, regions, devices, added slices, camera orientation, and rendering settings.

Use **[View][Save Workspace]** to save the workspace to a folder and **[View][Load Workspace]** to restore it later.

Typical workspace contents include:

| Item | Purpose |
|:--|:--|
| `tracts/` | Saved tractography groups. |
| `regions/` | Saved region files. |
| `devices/` | Device definitions/positions. |
| `slices/` | Added image volumes. |
| `camera.txt` | View orientation. |
| `command.txt` | Commands used to restore additional view state. |
| `setting.ini` | Rendering settings. |

When sharing a workspace, include the FIB/image data required by the saved state and use current `.fz`/`.tt.gz` formats where applicable.

# Shortcuts and graphic control

## Slice/region view

| Shortcut | Action |
|:--|:--|
| `Q` / `A` | Move sagittal slice. |
| `W` / `S` | Move coronal slice. |
| `E` / `D` | Move axial slice. |
| `Z` | Sagittal view. |
| `X` | Coronal view. |
| `C` | Axial view. |
| Mouse wheel | Zoom. |

## 3D view

- **Left drag:** rotate.
- **Right drag:** zoom.
- **Middle drag:** move the scene.
- **Mouse wheel:** zoom.
- **Double-click a region:** select it in the region list.

### Tract editing shortcuts

| Shortcut | Action |
|:--|:--|
| `Ctrl+S` | Select tracts. |
| `Ctrl+D` | Delete tracts. |
| `Ctrl+P` | **Paint tracts.** |
| `Ctrl+X` | Cut tracts. |
| `Ctrl+T` | Trim tracts. |
| `Ctrl+Z` | Undo selection/deletion. |
| `Ctrl+Y` | Redo selection/deletion. |

See the [[Edit] menu](/doc/menu_edit.html) for tract-editing details.
