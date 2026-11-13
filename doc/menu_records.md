# [Records] Menu

Use the **[Records]** menu in the Fiber Tracking window to view or export the reconstruction protocol embedded in a `.fib.gz` or `.fz` file, and to replay or batch-apply recorded GUI workflows.

## Records

| Menu item | Behavior |
|---|---|
| **FIB Protocol...** | Shows the numbered preprocessing and reconstruction protocol embedded in the loaded `.fib.gz` or `.fz` file, with controls to copy it, toggle a table view, or save `<stem>_fib_protocol.txt`. Externally converted files without an embedded log open blank. |
| **Command History...** | Opens an editable table of the window's recorded GUI steps (`Action` and `Parameter 1`–`Parameter 3`). Select rows to replay locally (**Run&nbsp;Selected**), delete steps, save or load `<stem>_commands.csv` workflows, or batch-apply across datasets (**Apply to Files...**, **Apply to Folders...**). |

## Viewing and Saving the FIB Protocol

DSI Studio embeds preprocessing and reconstruction steps inside `.src.gz`/`.sz` and `.fib.gz`/`.fz` outputs for methods reporting and quality review; datasets converted outside DSI Studio may open with an empty protocol.

## Recording, Editing, and Replaying GUI Workflows

![DSI Studio Command History interface](https://github.com/user-attachments/assets/ef9b62f5-bc29-4b3d-b22f-f7ce9a4e56d7)

Each tracking window automatically logs completed menu and panel operations starting from row 1 (opening the `.fib.gz` or `.fz` dataset), while omitting canceled dialogs, Undo/Redo actions, and freehand mouse or camera drags. Use **Reload...** to restore the live session log after editing or loading a `.csv` file, or **Clear** to reset the log back to row 1.

## Batch Replay Tips and Practical Cautions

Include row 1 (opening the `.fib.gz` or `.fz` dataset) when running **Apply to Files...** or **Apply to Folders...** across subjects so DSI Studio matches target file extensions, substitutes companion ROI and tract filenames, and automatically skips the original source file. Saved `*_commands.csv` workflows can also be dragged onto DSI Studio's main window for automatic replay; see [Command History](/doc/cli_vis.html) for batch workflow details.
