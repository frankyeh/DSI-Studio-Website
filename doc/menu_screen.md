# [Screen] Menu

Save 3D and ROI views, copy views, and record rotations or videos.

## Screen captures

| Menu item | Shortcut / GUI command hint |
|---|---|
| Save 3D Screen... | `run save_screen` |
| Save 3D Screen in High Resolution.. | `run save_hd_screen` |
| Save 3D Screen Left/Right Views... | `run save_lr_screen` |

## Multi-view captures

| Menu item | Shortcut / GUI command hint |
|---|---|
| Save 3D Screen in 3 Views.. | `run save_3view_screen` |
| Save 3D Screen in 3 Views (Horizontal)... | `run save_h3view_screen` |
| Save 3D Screen in 3 Views (Vertical)... | `run save_v3view_screen` |

## ROI and 3D export

| Menu item | Shortcut / GUI command hint |
|---|---|
| Save ROI Screen... | `run save_roi_screen` |
| Save 3D Model... | `run save_3d_model` |

## Clipboard

| Menu item | Shortcut / GUI command hint |
|---|---|
| **Copy to Clipboard** submenu | — |

### Copy to Clipboard submenu

| Menu item | Shortcut / GUI command hint |
|---|---|
| 3D Screen | — |
| 3D Screen Left/Right Views | — |
| 3D Screen 3 Views | — |
| 3D Screen 3 Views Horizontal | — |
| 3D Screen 3 Views Vertical | — |
| 3D Screen Each Tract | — |
| 3D Screen Each Region | — |
| ROI | — |
| Track Report | — |

## Rotation and video

| Menu item | Shortcut / GUI command hint |
|---|---|
| Auto Rotate | — |
| Record Video... | — |
| Save Rotation Video... | `run save_rotation_video` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
