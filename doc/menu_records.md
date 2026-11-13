# [Records] Menu

Use the **[Records]** menu in the Fiber Tracking window to inspect and export the preprocessing and reconstruction protocol embedded in the loaded `.fib.gz` or `.fz` dataset, or to review, edit, save, load, and replay interactive GUI workflows across one or many subjects.

## Records

| Menu item | Behavior |
|---|---|
| **FIB Protocol...** | Opens the modal **FIB Protocol** dialog (`600 × 400` minimum size) displaying the numbered step-by-step preprocessing and reconstruction log stored inside the loaded `.fib.gz` or `.fz` file (parameter assignments formatted as `Set key=value`, top-menu operations as `At the top menu, select ...`, and button or processing steps as `Click ...`). Provides **Copy to Clipboard**, **Save as...** (plain-text `.txt`, defaulting to `<stem>_fib_protocol.txt`), **Show Table** / **Hide Table** (splitting tab-delimited entries into a lower table pane), and **Close**. Opens with an empty text pane if the dataset does not contain a recorded DSI Studio reconstruction log. |
| **Command History...** | Opens (or refreshes and raises) the modeless **Command History** dialog (`1024 × 600`) listing the current tracking window's recorded GUI operations in an editable 4-column table (`Action`, `Parameter 1`, `Parameter 2`, `Parameter 3`). Supports multi-row selection to replay selected steps in the current window (**Run&nbsp;Selected**), batch-apply selected workflows to multiple datasets (**Apply to Files...** or **Apply to Folders...**), prune unwanted rows (**Delete**), reset or reload the live session log (**Clear**, **Reload...**), and exchange reusable macro files in comma-separated `.csv` format (**Open...**, **Save...** defaulting to `<stem>_commands.csv`). |

## Viewing and Saving the FIB Protocol

When DSI Studio creates a `.src.gz` or `.sz` file and reconstructs a `.fib.gz` or `.fz` volume, it stores each preprocessing and reconstruction step inside the output file. Choosing **[Records][FIB Protocol...]** formats that embedded record as a numbered checklist suitable for methods documentation and quality review:

- **Copy to Clipboard:** Copies the complete numbered protocol text to the system clipboard.
- **Save as...:** Saves the numbered protocol to a `.txt` file (defaulting to `<stem>_fib_protocol.txt` in the dataset folder). Saving from this dialog is also appended to the active session's command history.
- **Show Table / Hide Table:** Toggles a resizable lower table view below the read-only text summary.
- **Close:** Closes the **FIB Protocol** dialog.

Datasets imported or converted outside DSI Studio's reconstruction pipeline may not carry an embedded step log; for those files, the dialog opens with a blank summary.

## Recording, Editing, and Replaying GUI Workflows

![DSI Studio Command History interface](https://github.com/user-attachments/assets/ef9b62f5-bc29-4b3d-b22f-f7ce9a4e56d7)

Each Fiber Tracking window automatically logs supported GUI operations as you work, starting in row 1 with opening the current `.fib.gz` or `.fz` dataset and appending completed region, tractography, slice, rendering, camera, device, and export operations in execution order. Canceled dialogs, failed operations, and internal sub-steps triggered during replay are omitted automatically, and consecutive adjustments of the same continuous setting replace the immediately preceding entry when applicable.

Choose **[Records][Command History...]** to open the **Command History** window. Every row displays an **Action** name and up to three editable arguments (**Parameter 1** through **Parameter 3**). Click rows (using `Shift`-click or `Ctrl`/`Cmd`-click for range or multi-row selection) and use the bottom toolbar controls:

- **Open...:** Loads a previously saved comma-separated `.csv` workflow file (`<stem>_commands.csv` by default) into the dialog table for inspection, editing, or replay without overwriting the window's live session log.
- **Save...:** Writes all rows currently shown in the dialog table—including any manual cell edits or row deletions—to a `.csv` macro file (`<stem>_commands.csv` by default).
- **Reload...:** Restores the dialog table from the current tracking window's live recorded history, discarding unsaved table edits or loaded `.csv` rows. Choosing **[Records][Command History...]** while the dialog already exists also refreshes the table automatically.
- **Clear:** Resets the live session history back to row 1 (the initial FIB/FZ open step) and refreshes the table.
- **Run&nbsp;Selected:** Enabled when one or more rows are selected; executes the selected rows sequentially inside the current Fiber Tracking window and confirms completion.
- **Apply to Files...:** Enabled when one or more rows are selected; saves current tracking and rendering parameters, prompts you to pick one or more target files matching the file suffix of the first file-loading step in the selection, and replays the selected sequence across every chosen file.
- **Apply to Folders...:** Enabled when one or more rows are selected; saves current tracking and rendering parameters, prompts for a root folder, recursively finds all files matching the file suffix of the first file-loading step in the selection, and replays the selected sequence across every discovered dataset.
- **Delete:** Enabled when one or more rows are selected; removes the highlighted rows from the dialog table so exploratory or unwanted steps can be excluded before saving or replaying (click **Reload...** at any time to restore the full live log).
- **Close:** Hides the **Command History** dialog while keeping live recording active in the tracking window.

## Batch Replay Tips and Practical Cautions

- **Include the initial file-loading row for multi-subject batches:** Both **Apply to Files...** and **Apply to Folders...** inspect the first file-opening step in your selected rows to determine which file extension to filter or search for (for example, `.fz`, `.fib.gz`, `.tt.gz`, or `.nii.gz`) and how to substitute subject filenames. When applying a full tracking workflow to other subjects' FIB/FZ datasets, include row 1 (opening the `.fib.gz` or `.fz` file) in your selection; DSI Studio opens a temporary tracking window for each target subject, executes the remaining selected steps, and closes the temporary window automatically before moving to the next subject.
- **Automatic skipping of the original dataset:** If the file list or folder selected for batch replay includes the exact source file from which the workflow was recorded, DSI Studio automatically skips that original file so existing outputs are not overwritten unintentionally.
- **Companion filename matching and missing-file prompts:** When a selected workflow loads or saves additional files (such as subject-specific ROIs, tracts, slices, or statistics), DSI Studio automatically maps the original subject's filename stem and folder pattern onto each target subject. If a matched input file is missing at the expected path, DSI Studio searches the target directory for a file sharing the same suffix and matching name substring, logs a check warning if substituted, and prompts whether to skip that subject or cancel the batch if no match exists.
- **Three-parameter table width:** The **Command History** dialog displays and serializes four columns (`Action` plus `Parameter 1`–`Parameter 3`). Editing, replaying, or saving from this table retains up to three comma-separated parameters per step.
- **Drag-and-drop CSV replay on the main window:** A saved `*_commands.csv` file whose first row opens a `.fib.gz` or `.fz` dataset can also be dragged and dropped directly onto DSI Studio's main launcher window to open that dataset and execute every step in the CSV automatically.
- **Interactive-only UI actions:** Direct slot operations and freehand mouse interactions—such as **[Regions][Undo]**, **[Regions][Redo]**, **[Tract Edit][Undo]**, or manual 2D/3D brush and camera drags—are not recorded as replayable rows. For batch execution outside the interactive GUI, see [Command History](/doc/cli_vis.html).
