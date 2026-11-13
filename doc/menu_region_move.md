# [Region Move] Menu

Flip, shift, or interactively move the selected region.

## Axis flips

| Menu item | Shortcut / GUI command hint |
|---|---|
| Flip X | Shortcut: `Ctrl+7` · `run region_action_flipx` |
| Flip Y | Shortcut: `Ctrl+8` · `run region_action_flipy` |
| Flip Z | Shortcut: `Ctrl+9` · `run region_action_flipz` |

## Axis shifts

| Menu item | Shortcut / GUI command hint |
|---|---|
| Shift +X | Shortcut: `Ctrl+1` · `run region_action_shiftx` |
| Shift -X | Shortcut: `Ctrl+2` · `run region_action_shiftnx` |
| Shift +Y | Shortcut: `Ctrl+3` · `run region_action_shifty` |
| Shift -Y | Shortcut: `Ctrl+4` · `run region_action_shiftny` |
| Shift +Z | Shortcut: `Ctrl+5` · `run region_action_shiftz` |
| Shift -Z | Shortcut: `Ctrl+6` · `run region_action_shiftnz` |

## Interactive placement

| Menu item | Shortcut / GUI command hint |
|---|---|
| Move Region at 3D | Shortcut: `Ctrl+A` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
