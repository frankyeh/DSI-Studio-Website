# GUI-Based Command Line

After loading a .fz file, users can use GUI's console window for batch visualization and screen saving.
![image](https://github.com/user-attachments/assets/d0e967eb-06fc-4874-9eba-19110477a55a)

## Examples

*Load a file and add an isosurface. save the rendering image in horizontal layout.*
```
--action=vis --cmd=load_tracts,sub001.tt.gz+set_zoom,1.0+restore_rendering+add_surface,Full+save_h3view_image,sub001.png
```

*Load each *.tt.gz tract file in the current folder, and set the view from the top and save the rendering image as 1.jpg.*
```
--action=vis --cmd="load_tracts,*.tt.gz+set_view,2+save_image,1.jpg,1024 800"
```

 
## Core Functions

| Parameters            | Default | Description                                                                 |
|:-----------------|:--------|:------------------------------------------------------------------------------|
| cmd |  | specify the function to execute. Multiple commands can be combined using "+" as the separator (e.g. --cmd="add_surface+save_image") |

*The following commands are for controlling the interface:*

- `add_surface`: add an full brain isosurface (e.g. add_surface,Full). To add partial brain isosurface, replace `Full` by `Left`,`Right`,`Upper`,`Lower`,`Posterior`,`Anterior`
- `add_slice,t1w.nii.gz`: add t1w.nii.gz as the slice
- `move_slice,0,20`: move sagittal slice (0) to position 20. To move the sagittal slice, use "move_slice,1,20". To move the axial slice, use "move_slice,2,20"   
- `slice_on`: make slices visible. To enable only sagittal slice, use "slice_on,0". (0: sagittal, 1: coronal, 2:axial)
- `slice_off`: make slices invisible. To disable only sagittal slice, use "slice_off,0". (0: sagittal, 1: coronal, 2:axial)
- `set_zoom`: set the zoom-in scale. e.g., set_zoom,0.6
- `set_stereoscopic`:  set stereoscopic view
- `set_view,0` `set_view,1` `set_view,2`: set the current view to sagittal, coronal, and axial view. Call the same command twice to view from the opposite side (e.g. --cmd="set_view,2+set_view,2" will view from the top)
- `set_roi_view_index`: set the background map of the ROI window, e.g. "set_roi_view_index,0" will show the FA or QA map.
- `set_param`: configure the rendering options. A list of the rendering options can be found at https://github.com/frankyeh/DSI-Studio/blob/master/options.txt   For example, the first option is "orientation_convention", and you may set it to 0 by "set_param,orientation_convention,0" to use Radiology convention

*The following commands are for saving a file.* 

You may specify the file name. For example "add_surface+save_image,file_name.jpg"

- `save_image`: save a 3D rendering image. The image dimension can be assigned after assigning the file name, e.g., --cmd="set_view,2+save_image,1.jpg,1024 800"
- `save_lr_image`: save a left right viewed 3D rendering image.           
- `save_3view_image`, `save_h3view_image`: save 3D rendering image in 3 views
- `save_rotation_video`: save a rotation video
- `save_roi_image`: save the ROI window as an image
- `save_mapping`: save fa qa or adc mapping as a nifti image
- `save_tracks`: save current tractography as a trk file. For example, "save_tracks,tracks.tt.gz"

*The following commands are for fiber tracking, visualization, and track editing*

- `save_workspace,/my_work_folder/`: save the workspace to a folder. The directory to the folder is specified after the comma
- `load_workspace,/my_work_folder/`: load the workspace to a folder. The directory to the folder is specified after the comma
- `presentation_mode`: change the GUI to presentation mode (hide ROI window)
- `save_setting,file.ini`: save all GUI setting to the file specified
- `load_setting,file.ini`: load all GUI setting to the file specified
- `save_rendering_setting,file.ini`: save rendering setting to the file specified
- `load_rendering_setting,file.ini`: load rendering setting to the file specified
- `save_tracking_setting,file.ini`: save tracking setting to the file specified
- `load_tracking_setting,file.ini`: load tracking setting to the file specified
- `restore_rendering` : restore all rendering settings
- `run_tracking`: run tracking using the current parameters
- `load_track_color,my_track_color_file.txt`: load the color for current tract from a text file.
- `load_tract_values,my_track_value_file.txt`: load the valuer for current tract from a text file.
- `load_cluster_color,clutser_colors.txt`: load the color for all loaded tracts from a text RGB file.
- `cut_by_slice`: cut tracks by the current slice position. Specify slice orientation by 0:sagittal 1:coronal, 2:axial. and direction by 0:lesser 1:greater. For example,  `--cut_by_slice,2,1` cut tracks at Z+


