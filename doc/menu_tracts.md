# Documentation: `[Tracts]`, `[Tracts Cluster]`, and `[Tracts Misc]` Menus

This document provides a detailed overview of the `[Tracts]`, `[Tracts Cluster]`, and `[Tracts Misc]` menus, their features, functionalities, and associated commands based on the source code and UI definitions.

---

## `[Tracts]` Menu

The `[Tracts]` menu provides comprehensive tools for managing, saving, and analyzing fiber tracts. It includes options for loading, saving, and processing tracts, as well as generating reports and analyzing connectivity.

### Main Actions

| **Menu Item**                         | **Command**                          | **Description**                                                                 |
|---------------------------------------|------------------------------------------------|---------------------------------------------------------------------------------|
| Open Tract                            | `open_tract`                               | Load a tract file for visualization and analysis.                              |
| Open MNI Space Tracts                 | `open_mni_tracts`                          | Load tract files aligned with MNI space coordinates.                           |
| Load Built-In Atlas                   | `load_built_in_atlas`                      | Import tracts from a pre-defined atlas.                                        |
| Save Tract As                         | `save_tract_as`                            | Save the currently selected tract to a file.                                   |
| Save All Tracts As                    | `save_all_tracts_as`                       | Save all loaded tracts into a single file.                                     |
| Save All Tracts As Multiple Files     | `save_all_tracts_as_multiple_files`        | Save each tract as an individual file.                                         |
| Check All Tracts                      | `check_all_tracts`                         | Mark all tracts for processing or visualization.                               |
| Separate Deleted                      | `separate_deleted`                         | Separate deleted tracts into a different group.                                |
| Copy Track                            | `copy_track`                               | Duplicate the selected tract.                                                  |
| Merge All                             | `merge_all`                                | Merge all tracts into a single tract.                                          |
| Delete Tract                          | `delete_tract`                             | Delete the selected tract.                                                     |
| Delete All Tracts                     | `delete_tract_all`                         | Delete all loaded tracts.                                                      |
| Tracts to Region                      | `tracts_to_region`                         | Convert selected tracts into a region of interest (ROI).                       |
| Endpoints to Region                   | `endpoints_to_region`                      | Convert tract endpoints into a region of interest (ROI).                       |
| Filter by ROI                         | `filter_by_roi`                            | Filter tracts based on their overlap with a region of interest (ROI).          |
| Tract Analysis Report                 | `tract_analysis_report`                    | Generate a detailed analysis report for the selected tracts.                   |
| Connectivity Matrix                   | `connectivity_matrix`                      | Generate a connectivity matrix based on tracts.                                |
| Open Connectivity Matrix              | `open_connectivity_matrix`                 | Load and visualize a saved connectivity matrix.                                |
| Statistics                            | `statistics`                               | Calculate statistical properties of the selected tracts.                       |

---

### Submenus

#### Save Tracts (Misc)
This submenu provides advanced options for saving tracts and their associated data.

| **Menu Item**                         | **Command**                          | **Description**                                                                 |
|---------------------------------------|------------------------------------------------|---------------------------------------------------------------------------------|
| Save Tracts in Current Mapping        | `save_tracts_in_current_mapping`           | Save tracts in the current coordinate system.                                  |
| Save Tracts in Template Space         | `save_tracts_in_template_space`            | Export tracts aligned with the template space.                                 |
| Save Tracts in MNI Coordinates        | `save_tract_in_mni_coordinates`            | Save tracts transformed into MNI coordinates.                                  |
| Save End Points As                    | `save_end_points_as`                       | Export tract endpoints as a separate file.                                     |
| Save Endpoints in Current Mapping     | `save_endpoints_in_current_mapping`        | Save endpoints in the current mapping.                                         |
| Save Endpoints in MNI Coordinates     | `save_endpoints_in_mni_coordinates`        | Save endpoint data in MNI space.                                               |
| TDI Diffusion Space                   | `tdi_diffusion_space`                      | Generate tract density imaging (TDI) in diffusion space.                       |
| TDI Subvoxel Diffusion Space          | `tdi_subvoxel_diffusion_space`             | Create TDI in subvoxel resolution.                                             |

#### Save Along Tract Index
This submenu provides options for saving tracts categorized by specific indices.

| **Menu Item**                         | **Command (Tooltip)**                          | **Description**                                                                 |
|---------------------------------------|------------------------------------------------|---------------------------------------------------------------------------------|
| Save Along Tract Index                | `save_along_tract_index`                   | Save tract data categorized by specific indices (e.g., FA, MD).                |

---

## `[Tracts Cluster]` Menu

The `[Tracts Cluster]` menu provides tools for clustering and organizing tracts based on different criteria.

### Actions

| **Menu Item**                  | **Command**                          | **Description**                                                                 |
|--------------------------------|------------------------------------------------|---------------------------------------------------------------------------------|
| Open Tracts Name               | `open_tracts_name`                         | Open a file containing tract names for display or modification.                |
| Sort Tracts By Names           | `sort_tracts_by_names`                     | Sort loaded tracts alphabetically by their names.                              |
| Assign Colors For Each         | `assign_colors_for_each`                   | Automatically assign a unique color to each tract.                             |
| Set Cluster Color              | `set_cluster_color`                        | Set a specific color for a selected cluster of tracts.                         |
| Open Cluster Colors            | `open_cluster_colors`                      | Load a predefined color scheme for tract clusters.                             |
| Open Cluster Labels            | `open_cluster_labels`                      | Load cluster labels from a file.                                               |
| Open Cluster Values            | `open_cluster_values`                      | Load cluster-specific values (e.g., statistical properties).                   |
| Save Cluster Colors            | `save_cluster_colors`                      | Save the current cluster color scheme to a file.                               |
| Hierarchical Clustering        | `hierarchical_clustering`                 | Perform hierarchical clustering to group tracts based on similarity.           |
| K-Means Clustering             | `k_means_clustering`                       | Use k-means clustering to partition tracts into `k` clusters.                  |
| EM Clustering                  | `em_clustering`                            | Apply expectation-maximization (EM) clustering for probabilistic grouping.     |

---

## `[Tracts Misc]` Menu

The `[Tracts Misc]` menu provides additional tools for refining, modifying, and organizing tracts.

### Actions

| **Menu Item**                  | **Command**                          | **Description**                                                                 |
|--------------------------------|------------------------------------------------|---------------------------------------------------------------------------------|
| Trim Tracts                    | `trim_tracts`                              | Remove excess parts of tracts based on topology.                               |
| Cut Tracts                     | `cut_tracts`                               | Cut selected tracts using a defined plane or region.                           |
| Split Tracts                   | `split_tracts`                             | Divide a tract into smaller sub-tracts.                                        |
| Merge Tracts                   | `merge_tracts`                             | Combine multiple tracts into a single bundle.                                  |
| Negate Tracts                  | `negate_tracts`                            | Invert the selection of tracts.                                                |
| Extract Subset                 | `extract_subset`                           | Extract a subset of tracts based on specific criteria.                         |
| Generate ROI from Tract        | `generate_roi_from_tract`                  | Create a region of interest (ROI) from selected tracts.                        |
| Visualize Tract Density        | `visualize_tract_density`                  | Generate a visual representation of tract density in a volume.                 |

---

## Notes
- Ensure that all required datasets (e.g., tracts, atlases) are loaded before using these menu options.
- Some features, such as clustering and density visualization, may require additional computational resources.
- For additional details or troubleshooting, refer to the source files:
  - `tracking_window_action.cpp`
  - `tracking_window.ui`
