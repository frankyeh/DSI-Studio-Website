# [Tracts Cluster] Menu

Organize tract clusters, their labels and colors, and run clustering methods.

## Names and sorting

| Menu item | Shortcut / GUI command hint |
|---|---|
| Open Tracts Name... | `run open_tract_name` |
| Sort Tracts by Name | `run sort_tract_by_name` |

## Cluster colors

| Menu item | Shortcut / GUI command hint |
|---|---|
| Color All Clusters | `run color_all_cluster` |
| Select Color to Current Cluster... | `run select_cluster_color` |

## Cluster files

| Menu item | Shortcut / GUI command hint |
|---|---|
| Open Cluster Colors... | `run load_cluster_color` |
| Open Cluster Labels... | `run cluster_tract_by_label` |
| Open Cluster Values... | `run load_cluster_values` |
| Save Cluster Colors... | `run save_cluster_color` |

## Clustering

| Menu item | Shortcut / GUI command hint |
|---|---|
| Single-Linkage Clustering | `run cluster_tract_by_hy` |
| K-means Clustering | `run cluster_tract_by_km` |
| EM Clustering | `run cluster_tract_by_em` |


The `run ...` strings above, where shown, are **GUI command hints from the UI file**, not public `dsi_studio --action=...` CLI flags.

Source: [tracking_window.ui](https://github.com/frankyeh/DSI-Studio/blob/master/tracking/tracking_window.ui).
