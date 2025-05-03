# [Edit] Menu

<iframe width="560" height="315" src="https://www.youtube.com/embed/1xfhaFQhCtY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

DSI Studio's manual tract segmentation function provides precise control over fiber tracking results. With features like "Select," "Delete," and "Cut," users can refine tractography outcomes, selecting specific tracks and cutting bundles for focused analysis. The "Track Painting" option allows for color-coded visualization, and "Cut by Slices" enables precise segmentation based on slice positions. User-friendly shortcuts, such as Ctrl+S for tract selection and Ctrl+D for deletion, enhance efficiency. 

---

## [Edit][Select]

![image](https://user-images.githubusercontent.com/275569/147838366-a6adb599-2b04-4a17-9914-214c4af2fa9f.png)

The steps are as follows:

1. [Edit][Select] or press Ctrl + S
2. Move the cursor to the starting point of the selection plane on the screen.
3. Press the left button of the mouse and move the cursor to the ending point of the selection plane. **Use the right button to further select tracts perpendicular to the selection plane within an incoming angle (about 60 degrees).**
4. Release the button and the selection will be performed immediately.

The select function selects the tracks that pass through the *selection plane* determined by the location of the mouse click position on the screen.  

---

## [Edit][Delete]

Deletion is carried out in the same manner except that the selected tracks will be deleted. The deletion can be undone by Ctrl+Z.

---

## [Edit][Cut]

Tract cutting is to cut the selected tracts into two parts. This function is useful in a condition where the users want to analyze only the right corpus callosum. The corpus callosum can be cut into two parts by a sagittally placed selection plane.  The right corpus callosum is then selected to perform further analysis.

Another condition to apply tract cutting is that only a section of the fiber bundle is to be analyzed, or the users want to align the fiber bundle. In such a case, an interesting portion of the fiber bundle can be extracted by track cutting.

---

## [Edit][Paint]

Track painting offers a way to paint tracks with an assigned color. To perform this task, press Ctrl+P. Assign the painting color and select the tracks.

---

## [Edit][Cut by slice]

Cut tracts based on slice position. The function is at [Edit][Cut by slices].

---

## [Edit][Shortcuts]

  - Ctrl+S: select tracts in the 3D window 
  - Ctrl+D: delete tracts in the 3D window
  - Ctrl+P: delete tracts in the 3D window
  - Ctrl+X: cut tracts in the 3D window (click-drag-release, cannot undo)
  - Ctrl+T: trim tracts  (a.k.a. topology-informed pruning)
  - Ctrl+Z: undo select and delete
  - Ctrl+Y: redo select and delete

---

## [Edit] Menu Actions Table

| **Action**             | **Description**                                                                                     | **Internal Command Text**      |
|-------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------|
| **Select**             | Selects tracks passing through the selection plane. Press `Ctrl+S` to activate.                     | `run select_tract`             |
| **Delete**             | Deletes selected tracks. Press `Ctrl+D` to activate. Undo with `Ctrl+Z`.                            | `run delete_tract`             |
| **Cut**                | Cuts selected tracts into two parts. Press `Ctrl+X` to activate.                                     | `run cut_tract`                |
| **Paint**              | Paints selected tracts with an assigned color. Press `Ctrl+P` to activate.                          | `run paint_tract`              |
| **Cut by Slice**       | Cuts tracts based on slice position.                                                                | `run cut_by_slice`             |
| **Undo Select/Delete** | Reverts the last selection or deletion action. Press `Ctrl+Z` to activate.                          | `run undo_action`              |
| **Redo Select/Delete** | Redoes the last reverted action. Press `Ctrl+Y` to activate.                                        | `run redo_action`              |
| **Trim Tracts**        | Trims tracts using topology-informed pruning. Press `Ctrl+T` to activate.                           | `run trim_tracts`              |
