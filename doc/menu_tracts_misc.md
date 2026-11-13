# [Tracts Misc] Menu

Process, transform, color, and recognize tract groups.

## Tract cleanup

| Menu item | Shortcut / GUI command hint |
|---|---|
| Merge Tracts by Name | `run merge_tract_by_name` |
| Delete Repeated Tracks | `run delete_repeated_tract` |
| Delete Short Tracts | `run delete_tract_by_length` |
| Delete Branches | `run delete_branch` |

## Tract geometry

| Menu item | Shortcut / GUI command hint |
|---|---|
| Reconnect Tracts | `run reconnect_tract` |
| Resample Step Size | `run resample_tract` |

## Move tracts

| Menu item | Shortcut / GUI command hint |
|---|---|
| **Move Tracts** submenu | — |

### Move Tracts submenu

| Menu item | Shortcut / GUI command hint |
|---|---|
| Flip X | `run flip_tract_x` |
| Flip Y | `run flip_tract_y` |
| Flip Z | `run flip_tract_z` |

## Tract colors and values

| Menu item | Shortcut / GUI command hint |
|---|---|
| Open Tract Colors... | `run load_tract_color` |
| Open Tract Values... | `run load_tract_values` |
| Save Tracts Colors As... | `run save_tract_color` |

## Tract recognition

| Menu item | Shortcut / GUI command hint |
|---|---|
| Recognize... | `run show_tract_recognition` |
| Recognize and Rename | `run recognize_and_rename_tract` |
| Recognize and Cluster | `run recognize_and_cluster_tract` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
