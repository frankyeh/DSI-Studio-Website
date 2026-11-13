# [View] Menu

Set orthogonal views and save or recall camera positions.

## Orthogonal views

| Menu item | Shortcut / GUI command hint |
|---|---|
| Sagittal View | Shortcut: `Z` |
| Coronal View | Shortcut: `X` |
| Axial View | Shortcut: `C` |

## Camera files

| Menu item | Shortcut / GUI command hint |
|---|---|
| Open Camera Position... | `run open_camera` |
| Save Camera Position... | `run save_camera` |

## Camera presets

| Menu item | Shortcut / GUI command hint |
|---|---|
| **Store Camera Position 3** submenu | — |
| **Recall Camera Position 2** submenu | — |

### Store Camera Position 3 submenu

| Menu item | Shortcut / GUI command hint |
|---|---|
| Slot 1 | Shortcut: `Alt+1` · `run store_camera1` |
| Slot 2 | Shortcut: `Alt+2` · `run store_camera2` |
| Slot 3  | Shortcut: `Alt+3` · `run store_camera3` |
| Slot 4 | Shortcut: `Alt+4` · `run store_camera4` |

### Recall Camera Position 2 submenu

| Menu item | Shortcut / GUI command hint |
|---|---|
| Slot 1 | Shortcut: `1` · `run restore_camera1` |
| Slot 2 | Shortcut: `2` · `run restore_camera2` |
| Slot 3 | Shortcut: `3` · `run restore_camera3` |
| Slot 4 | Shortcut: `4` · `run restore_camera4` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
