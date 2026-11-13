# [Tracts Cluster] Menu

The **[Tracts Cluster]** menu renames, sorts, colors, and splits tract bundles in the Tracts list, including unsupervised geometric clustering into sub-bundles. Depending on the command, operations apply to the highlighted bundle row, all checked bundles, or the entire Tracts list.

## Names and sorting

| Menu item | Behavior |
|---|---|
| **Open Tracts Name...** | Loads whitespace-separated bundle names from a `.txt` file and renames rows from the bottom of the Tracts list upward so newly added clusters are named without overwriting earlier bundles. Join multi-word names with underscores instead of spaces (for example, `Arcuate_Fasciculus_L`). |
| **Sort Tracts by Name** | Reorders all rows in the Tracts list alphabetically by name while preserving visibility checkboxes and streamlines. Sorting is alphabetical rather than numeric, so `cluster10` appears before `cluster2` unless names are zero-padded. |

## Cluster colors

| Menu item | Behavior |
|---|---|
| **Color All Clusters** | Assigns a distinct uniform RGB color to every bundle in the Tracts list (both checked and unchecked rows) and switches **Tract Color › Style** to **Assigned**. |
| **Select Color to Current Cluster...** | Opens a color picker to apply a uniform RGB color to the highlighted bundle row and switches **Tract Color › Style** to **Assigned**. Bundle colors store RGB only; adjust transparency via **Tract Color › Opacity**. |

## Cluster files

| Menu item | Behavior |
|---|---|
| **Open Cluster Colors...** | Reads `R G B` integer triplets (`0–255`) from a `.txt` file, assigns each color in list order to the checked bundles (skipping unchecked rows), and switches **Tract Color › Style** to **Assigned**. |
| **Open Cluster Labels...** | Splits the highlighted bundle into checked `cluster0`, `cluster1`, ... rows using one non-negative integer ID per visible streamline from a `.txt` file, then recolors all bundles. Replacing the selected row cannot be undone with **Tract Edit › Undo**. |
| **Open Cluster Values...** | Loads one scalar per checked bundle from a `.txt` file (count must match checked rows) and sets **Tract Color › Style** to **Loaded Value**. Adjust **Tract Color › Min Value** and **Max Value** outside `0–1`; per-streamline values take precedence. |
| **Save Cluster Colors...** | Saves the RGB color (`0–255` per channel) of every checked bundle in list order to a `.txt` file (`R G B` per line), omitting unchecked rows. |

## Clustering

Clustering replaces the highlighted bundle row in place with checked `cluster0`, `cluster1`, ... rows (largest first) in distinct colors; copy the bundle first with **[Tracts][Copy Tracts]**, as this replacement cannot be undone.

| Menu item | Behavior |
|---|---|
| **Single-Linkage Clustering** | Groups nearby streamlines in the highlighted bundle by endpoint and length proximity, prompting for max groups and clustering detail and placing leftovers in a hidden `others` row. Although labeled `mm`, detail is applied in **voxels** (divide desired mm by voxel size). |
| **K-means Clustering** | Partitions all streamlines in the highlighted bundle into k-means clusters by start/end endpoints, midpoints, and length, replacing the selected row with no `others` group. Prompts for the maximum group count and outputs up to `256` non-empty clusters. |
| **EM Clustering** | Partitions all streamlines in the highlighted bundle using Expectation-Maximization (Gaussian mixture) clustering by start/end endpoints, midpoints, and length, replacing the selected row with no `others` group. Prompts for the maximum group count and outputs up to `256` non-empty clusters. |

## Atlas recognition and bundle workflows

Unsupervised clustering groups streamlines purely by trajectory geometry; to classify, rename, or split bundles using DSI Studio's anatomical tractography atlas (or load per-streamline colors and values), see the [Tracts Misc menu](/doc/menu_tracts_misc.html). To filter clusters by regions or save checked clusters to `.tt.gz` or `.trk` files, see the [Tract Edit menu](/doc/menu_edit.html) and [Tracts menu](/doc/menu_tracts.html).
