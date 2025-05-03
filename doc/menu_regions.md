# [Regions] Menu

## Overview
The `[Regions]` menu in DSI Studio provides a variety of functions for creating, editing, and managing regions of interest (ROIs). Below is a detailed list of all available actions and their corresponding internal commands.

---

## [Regions] Menu Actions Table

| **Action**                     | **Description**                                              | **Internal Command Text**           |
|---------------------------------|--------------------------------------------------------------|--------------------------------------|
| **New Region**                 | Creates a new region with dimensions defined by the slices.   | *No toolTip in the .ui file*         |
| **Open Region**                | Opens a NIFTI file as a region.                              | `run open_region`                    |
| **Open MNI Region**            | Warps MNI space regions to native diffusion space.           | `run open_mni_region`                |
| **Save Region**                | Saves the current region(s) to a file.                      | `run save_region`                    |
| **Save All Regions**           | Saves all regions as 4D NIfTI files.                        | `run save_all_regions_as_4d_nifti`   |
| **Statistics**                 | Exports region-based metrics to a text file.                | `run region_statistics`              |
| **Check All Regions**          | Checks all regions in the list.                             | `run check_all_regions`              |
| **Uncheck All Regions**        | Unchecks all regions in the list.                           | `run uncheck_all_regions`            |
| **Merge Regions**              | Combines selected regions into a single region.             | `run merge_all`                      |
| **Delete Region**              | Deletes the selected region(s).                             | `run delete_region`                  |
| **Delete All Regions**         | Deletes all regions in the list.                            | `run delete_region_all`              |
| **Copy Region**                | Copies the selected region to the clipboard.                | `run copy_region`                    |
| **Undo Edit**                  | Reverts the last editing action on a region.                | `run undo_edit`                      |
| **Redo Edit**                  | Redoes the last reverted editing action on a region.        | `run redo_edit`                      |
| **Tract to Region Connectome** | Generates a connectome based on tract and region overlap.    | `run tract_to_region_connectome`     |

---

## [Regions Misc] Additional Features

### **New Region From Thresholding**
- **Functionality:** Creates a region by applying an intensity threshold to the current slice.

### **New Region From MNI Coordinate**
- **Functionality:** Generates a spherical region centered on a specified MNI coordinate with a user-defined radius.

---

## [Regions][Edit Region]
The following functions are available for modifying regions:

- **Flip (X, Y, Z axes):**
  - `[Regions][Edit][Flip X]`
  - `[Regions][Edit][Flip Y]`
  - `[Regions][Edit][Flip Z]`
- **Shift:**
  - `[Regions][Edit][Shift X/Y/Z (Positive or Negative)]`
- **Morphological Operations:**
  - `[Regions][Edit][Erode]`
  - `[Regions][Edit][Dilate]`
  - `[Regions][Edit][Smooth]`
  - `[Regions][Edit][Defragment]`

---

## [Regions Misc][Advanced]
- **Separate Region:** Splits a region into connected sub-regions.
- **Negate Region:** Inverts the voxel selection within a region.

---

Let me know if you need further modifications or additional details.
