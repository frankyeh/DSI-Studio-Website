# [Region Edit] Menu

Edit region masks and apply morphology or set operations.

## Checked regions

| Menu item | Shortcut / GUI command hint |
|---|---|
| Modify All Checked | — |

## Morphology

| Menu item | Shortcut / GUI command hint |
|---|---|
| Dilation | Shortcut: `Ctrl+Shift+D` · `run region_action_dilation` |
| Dilation By Voxel... | `run region_action_dilation_by_voxel` |
| Dilation By Threshold... | `run region_action_dilation_by_threshold` |
| Erosioin By Threshold... | `run region_action_erosion_by_threshold` |
| Erosion | Shortcut: `Ctrl+Shift+E` · `run region_action_erosion` |

## Shape cleanup

| Menu item | Shortcut / GUI command hint |
|---|---|
| Opening | Shortcut: `Ctrl+Shift+O` · `run region_action_opening` |
| Closing | Shortcut: `Ctrl+Shift+C` · `run region_action_closing` |

## Threshold

| Menu item | Shortcut / GUI command hint |
|---|---|
| Smoothing | Shortcut: `Ctrl+Shift+S` · `run region_action_smoothing` |
| Defragment | Shortcut: `Ctrl+Shift+F` · `run region_action_defragment` |
| Separate | Shortcut: `Ctrl+Shift+P` · `run region_action_separate` |
| Negate | Shortcut: `Ctrl+Shift+N` · `run region_action_negate` |

## Set operations

| Menu item | Shortcut / GUI command hint |
|---|---|
| Threshold | Shortcut: `Ctrl+Shift+T` · `run region_action_threshold_current` |

## Other actions

| Menu item | Shortcut / GUI command hint |
|---|---|
| All Intersect First | Shortcut: `Ctrl+Shift+1` · `run region_action_all_inter_1st` |
| All Exclude First | Shortcut: `Ctrl+Shift+2` · `run region_action_all_ex_1st` |
| First Excludes All | Shortcut: `Ctrl+Shift+3` · `run region_action_1st_ex_all` |
| All Fit into First | Shortcut: `Ctrl+Shift+4` · `run region_action_all_to_1st` |
| Refine All | Shortcut: `Ctrl+Shift+5` · `run region_action_refine_all` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
