# [Regions] Menu

## Overview
The `[Regions]` menu in DSI Studio provides functions for creating, editing, and managing regions of interest (ROIs).

## [Regions] Menu Actions

| **Action** | **Description** | **Internal Command Text** |
|:--|:--|:--|
| **New Region** | Creates a new region with dimensions defined by the slices. | `run new_region` |
| **Open Region** | Opens a NIFTI file as a region. | `run open_region` |
| **Open MNI Region** | Warps MNI-space regions to native diffusion space. | `run open_mni_region` |
| **Save Current Region As...** | Saves the current region to a file. | `run save_region` |
| **Save Checked Regions to one 4D NIFTI...** | Saves checked regions together as a 4D NIfTI file. | `run save_4d_region` |
| **Statistics** | Exports region-based metrics to a text file. | `run show_region_statistics` |
| **Check All Regions** | Checks all regions in the list. | `run check_all_regions` |
| **Uncheck All Regions** | Unchecks all regions in the list. | `run uncheck_all_regions` |
| **Merge Checked Regions** | Combines checked regions into one region. | `run merge_regions` |
| **Delete** | Deletes the selected region(s). | `run delete_region` |
| **Delete All Regions** | Deletes all regions in the list. | `run delete_all_regions` |
| **Copy** | Copies the selected region to the clipboard. | `run copy_region` |
| **Undo Edit** | Reverts the last region-editing action. | *No documented `run` command binding* |
| **Redo Edit** | Redoes the last reverted editing action. | *No documented `run` command binding* |
| **Tract to Region Connectome** | Generates a connectome based on tract and region overlap. | `run show_t2r` |

## [Regions Misc] Additional Features

### New Region From Thresholding
Creates a region by applying an intensity threshold to the current slice.

### New Region From MNI Coordinate
Creates a spherical region centered on an MNI coordinate with a user-defined radius.

## [Regions][Edit Region]

The following functions modify regions:

- **Flip:** X, Y, or Z axis.
- **Shift:** positive or negative shifts along X, Y, or Z.
- **Morphological operations:** Erode, Dilate, Smooth, and Defragment.

## [Regions Misc][Advanced]

- **Separate Region:** splits a region into connected sub-regions.
- **Negate Region:** inverts the voxel selection within a region.
