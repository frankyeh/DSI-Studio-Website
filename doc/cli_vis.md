# Command History

DSI Studio records many GUI operations in **Command History**. The recorded commands can be reviewed, repeated, saved to a CSV file, applied to other datasets, or executed from the command line.

## View command history

After opening an `.fz` file, inspect recorded operations at **[Options][Command History]**.

![image](https://github.com/user-attachments/assets/ef9b62f5-bc29-4b3d-b22f-f7ce9a4e56d7)

The history can be saved to or loaded from a CSV file. Selected commands can also be repeated directly from the history window.

## Apply a workflow to other files

**Apply to Others** repeats the selected commands on other files when the selected history includes a file-open command.

For example, select a sequence containing `open_fib`, choose **Apply to Others**, and select the other FIB files to process with the same recorded steps.

Not every GUI function is recorded. If a required operation is missing from Command History, report it on the [DSI Studio forum](https://groups.google.com/g/dsi-studio).

## Run a saved command history from the command line

A saved CSV command history can be passed directly to DSI Studio. The history should include the file-open step required by the workflow.

```bash
dsi_studio sub001_command.csv
```

A command-history CSV file can also be dragged into the DSI Studio GUI to run it.

## Run GUI commands from the console

The DSI Studio console can execute GUI commands directly. The following example loops through tractography files, opens each file, changes the view, and saves a rendered image:

```text
--action=vis --loop=*.tt.gz --cmd="open_tracts,*.tt.gz+set_view,2+save_hd_screen,1.jpg,1024 800"
```

Commands in `--cmd` are separated by `+`.
