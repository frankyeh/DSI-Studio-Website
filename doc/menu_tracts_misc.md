# [Tracts Misc] Menu

The **[Tracts Misc]** menu provides post-tracking tools to clean up bundles, bridge or resample streamlines, mirror coordinates, manage per-streamline colors and values, and match pathways against the built-in tractography atlas. For file I/O, interactive editing, and clustering, see the [Tracts](/doc/menu_tracts.html), [Tract Edit](/doc/menu_edit.html), and [Tracts Cluster](/doc/menu_tracts_cluster.html) menus.

## Tract cleanup

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Merge Tracts by Name</span> | Combines all bundle rows—checked or unchecked—that share an identical name into the first matching row. Unlike **[Tracts][Merge All]**, this groups strictly by bundle name and cannot be undone with **[Tract Edit][Undo]**. |
| <span class="gui-menu-item">Delete Repeated Tracks</span> | Removes near-duplicate streamlines from all checked bundles within a specified **voxel** distance threshold (default `1.0` voxel; undoable with **[Tract Edit][Undo]**). Both streamlines in each matched pair are removed, so avoid routine use on sparse or whole-brain bundles. |
| <span class="gui-menu-item">Delete Short Tracts</span> | Removes streamlines shorter than a physical **millimeter (mm)** cutoff (default `0.5` mm, or two or fewer points) from all checked bundles. Unlike **Delete Repeated Tracks** (which uses voxels), this filter uses **mm** and can be undone with **[Tract Edit][Undo]**. |
| <span class="gui-menu-item">Delete Branches</span> | Prunes stray branches and prematurely stopping fibers from all checked bundles by keeping only streamlines that reach both main endpoint zones (undoable with **[Tract Edit][Undo]**). Use only on single two-endpoint pathways, never on fanning or whole-brain bundles. |

## Tract geometry

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Reconnect Tracts</span> | Bridges broken segments in the highlighted bundle when endpoint separation (**voxels**, `> 2.0`) and alignment angle (**degrees**, `> 0`; default `4 30`) meet your thresholds. Modifies streamlines in place without undo and can falsely join crossing fibers, so duplicate the bundle first. |
| <span class="gui-menu-item">Resample Step Size</span> | Resamples streamline vertices across all checked bundles to a uniform **voxel** spacing (default `0.5` voxels) while preserving start and end points. Enter a strictly positive value (never `0.0`); resampling overwrites geometry in place and cannot be undone. |

## Move tracts

Mirrors the highlighted bundle in place when its row is also checked; repeat the same flip to restore original coordinates, as flipping is not undoable.

### [Tracts Misc][Move Tracts]

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Flip X</span> | Mirrors all streamlines in the highlighted (and checked) bundle across the diffusion grid's left–right (**X**) axis in place. |
| <span class="gui-menu-item">Flip Y</span> | Mirrors all streamlines in the highlighted (and checked) bundle across the diffusion grid's anterior–posterior (**Y**) axis in place. |
| <span class="gui-menu-item">Flip Z</span> | Mirrors all streamlines in the highlighted (and checked) bundle across the diffusion grid's inferior–superior (**Z**) axis in place. |

## Tract colors and values

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Open Tract Colors...</span> | Loads per-streamline `R G B` colors (`0`–`255` triplets in a `.txt` file) into the highlighted bundle row. Automatically switches **Tract Color › Style** in **Options** to `Assigned` to display the imported colors. |
| <span class="gui-menu-item">Open Tract Values...</span> | Loads one scalar value per visible streamline from a `.txt` file into the highlighted bundle row and sets **Tract Color › Style** to `Loaded Value`. The value count must match the bundle's visible streamline count. |
| <span class="gui-menu-item">Save Tracts Colors As...</span> | Saves the highlighted bundle's assigned per-streamline `R G B` colors (`0`–`255`) to a `.txt` file, one triplet per line. Exports only stored painted or imported colors, not dynamic directional or metric colormaps. |

For per-bundle colors and scalar values across multiple checked rows, see the [Tracts Cluster](/doc/menu_tracts_cluster.html) menu.

## Tract recognition

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Recognize...</span> | Matches the highlighted bundle (or the first row if none is selected) against the built-in tractography atlas, running template registration if needed. Opens a summary of matched pathway percentages (with clipboard copy and `.txt` export) without altering the bundle. |
| <span class="gui-menu-item">Recognize and Rename</span> | Classifies each checked bundle against the built-in tractography atlas (running template registration if needed) and renames each row to its highest-matching pathway without splitting or deleting streamlines. Use **[Tracts Misc][Merge Tracts by Name]** afterward to combine identically named rows. |
| <span class="gui-menu-item">Recognize and Cluster</span> | Splits the highlighted bundle by matching each streamline to the built-in tractography atlas, replacing the original row with separate colored rows per pathway. Unrecognized streamlines are discarded and splitting cannot be undone, so duplicate the bundle first to keep unmatched fibers. |

Atlas recognition reports the closest-matching reference pathway; always verify results against underlying anatomy, or use [AutoTrack](/doc/gui_t3_atk.html) to reconstruct named pathways directly.
