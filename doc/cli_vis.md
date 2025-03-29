# Command History

The DSI Studio "Hou" version provides a `command history` function that allows users to record and repeat GUI operations or use command line to execute GUI functions. This function also support batch processing on a large number of files to realize automated GUI-based batch processing.

## Viewing command history

After loading a .fz file, most operation will be recorded in command history, which can be inspected at [Options][Command History]

![image](https://github.com/user-attachments/assets/ef9b62f5-bc29-4b3d-b22f-f7ce9a4e56d7)

You may save or load the history to/from a CSV text file (.csv). You may also select a part of the commands to repeat the command. 

## Applying command history to other files

The [Apply to Others] allows users to apply the steps to other files if the selected commands include a file open action.

For example, to repeat all commands to different FIB files, just select all command to include the `open_fib` command and click on the [Apply to Others] button. DSI Studio will ask to specify other FIB files and execute accordingly.

**Some functions are not recorded in the command history. If you need certain functions to be included, please post the request on DSI Studio forum**

## Run command history from command line

You may run the command history from command line by passing the .csv command history file to dsi studio. The command history needs to include the `open_fib` step so that DSI Studio can run it correctly.

```
dsi_studio sub001_command.csv
```
**You may also drag and drop a .csv file into DSI Studio GUI to run it.**

## Run command history from console window

The console window in DSI Studio (click on the [Console] button at the top right corner) also allows for running the GUI commands. 

*Load each *.tt.gz tract file in the current folder, and set the view from the top and save the rendering image as 1.jpg.*
```
--action=vis --loop=*.tt.gz --cmd="open_tracts,*.tt.gz+set_view,2+save_hd_screen,1.jpg,1024 800"
```
Each command should be separated by a '+' sign to run it.
