# [File] Menu

Load and save FIB files, workspaces, and tracking/rendering settings.

## FIB files

| Menu item | Shortcut / GUI command hint |
|---|---|
| Open FIB Directory... | Shortcut: `Ctrl+O` |
| Save FIB As... | `run save_fib_as` |

## Workspace

| Menu item | Shortcut / GUI command hint |
|---|---|
| Load Workspace... | `run load_workspace` |
| Save Workspace... | `run save_workspace` |

## Tracking settings

| Menu item | Shortcut / GUI command hint |
|---|---|
| Load Tracking Settings... | `run load_tracking_setting` |
| Save Tracking Settings... | `run save_tracking_setting` |
| Load Parameter ID... | `run load_parameter_id` |
| Restore Tracking Settings | — |

## Rendering settings

| Menu item | Shortcut / GUI command hint |
|---|---|
| Load Rendering Settings... | `run load_rendering_setting` |
| Save Rendering Settings | `run save_rendering_setting` |
| Restore Rendering Settings | — |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
