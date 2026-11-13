# [Slices Segment] Menu

Choose a U-Net segmentation model for the current image slice.

This menu is filled at runtime using the configured U-Net models for the selected template. Each model is added with the GUI command hint `run segment_brain`. Entries requiring T1w, T2w, or FLAIR can be disabled when the current image does not match the model's required modality. Hover over a model to see its description; the exact model list depends on the installation and template.


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui), [tracking_window.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.cpp) and [tracking_window_action.cpp](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window_action.cpp).
