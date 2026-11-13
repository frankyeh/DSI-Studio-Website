# [Regions Misc] Menu

Create, sort, and manage additional region and mapping operations.

## Create regions

| Menu item | Shortcut / GUI command hint |
|---|---|
| New Region From Thresholding... | `run new_region_from_threshold` |
| New Region From MNI Coordinate... | `run new_region_from_mni` |

## Region colors

| Menu item | Shortcut / GUI command hint |
|---|---|
| Load Region Color... | `run load_region_color` |
| Save Region Color... | `run save_region_color` |

## Sort regions

| Menu item | Shortcut / GUI command hint |
|---|---|
| Sort By Name | `run region_action_sort_name` |
| Sort By Size | `run region_action_sort_size` |
| Sort By X | `run region_action_sort_x` |
| Sort By Y | `run region_action_sort_y` |
| Sort By Z | `run region_action_sort_z` |

## Seeding and voxel data

| Menu item | Shortcut / GUI command hint |
|---|---|
| Whole Brain Seeding | `run new_region_whole_brain_seed` |
| Save Voxel Data As... | `run save_region_info` |

## Mapping and alignment

| Menu item | Shortcut / GUI command hint |
|---|---|
| Move Slices To Current Region | `run move_slice_to_region` |
| Load MNI mapping... | `run open_mapping` |
| Manual Atlas Alignment... | — |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
