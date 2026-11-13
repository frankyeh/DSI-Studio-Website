# [Export] Menu

Export slice-derived maps and metrics from the loaded FIB file.

The entries are populated from the loaded FIB data rather than fixed in the UI file. Available metrics come from `handle->get_index_list()`; the menu also offers `fiber`, and `odfs` when ODFs are present. Non-template-space data may offer metric exports in MNI space. When the `iso` metric exists, `Correct Bias Field` is added.

| Selection | GUI command hint |
|---|---|
| Loaded metric, `fiber`, optional `odfs` | `run save_slice_image` |
| Metric in MNI space, when applicable | `run save_slice_mni_image` |
| Correct Bias Field, when available | `run correct_bias_field` |

The metric names and available options depend on the current FIB data.


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui), [tracking_window.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.cpp) and [tracking_window_action.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window_action.cpp).
