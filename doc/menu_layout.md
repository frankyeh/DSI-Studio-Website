# [Layout] Menu

Use the **[Layout]** menu to restore the default Fiber Tracking window arrangement or switch the central 3D view between single, side-by-side, and stereoscopic display modes.

## Window and 3D views

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Restore Window Layout</span> | Resets the window size, re-docks floating panels, and reopens closed standard docks from the start of the session. Does not change the active **Layout › 3D Views** mode, camera angle, or tracking and rendering settings. |
| <span class="gui-menu-item">3D Views</span> | Opens the submenu for switching the central 3D view between **Single**, **Double**, and **Stereoscopic** display modes. Actions in this menu do not use keyboard shortcuts or active-mode checkmarks. |

### [3D Views] submenu

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">Single</span> | Displays one full-width 3D view controlled by a single camera (the default mode). Choose **Single** to exit **Double** or **Stereoscopic** split view, which **Layout › Restore Window Layout** leaves unchanged. |
| <span class="gui-menu-item">Double</span> | Splits the 3D view into two side-by-side panels with independent mouse rotation, zoom, and panning over the same loaded scene. Orthogonal presets in **View** re-synchronize both sides, while saved camera positions and **3D Zoom** apply to the left view. |
| <span class="gui-menu-item">Stereoscopic</span> | Displays coupled left-eye and right-eye 3D views that move together for side-by-side stereoscopic viewing, separated by **Options › Rendering › Stereoview Angle** (`0.0°`–`5.0°`, default `1.0°`). Formatted for side-by-side 3D displays, each half appears horizontally narrowed on a standard 2D monitor. |

## Dock panels and 3D-focused workspace

Five dockable panels surround the central 3D view: **Assign Regions** and **Draw Regions** on the left, plus **Options**, **Tracts**, and the initially hidden **Devices** panel on the right. Drag, float, or close docks to expand the 3D canvas, or right-click any visible dock title bar to show or hide individual panels (use **[Layout][Restore Window Layout]** if all docks are closed).

## Related view and reset controls

For camera presets and saved viewpoints see [View](/doc/menu_view.html), for 3D or stereo captures see [Screen](/doc/menu_screen.html), and for resetting parameters or saving workspaces see [File](/doc/menu_file.html).
