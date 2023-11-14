# [Regions] Menu

DSI Studio's region menu provides functions for loading regions, modifying the regions, and computing region-based statistics.

## [Regions][New Region]

This function creates a new region in a size and resolution defined by the current slices. Inserting T1W images using the [Slices] menu will allow for creating a new region in the space defined by the inserted image.

## [Regions][Open Region]

DSI Studio can open a NIFTI file as a region:

***If your FIB file is in the native space (i.e., reconstructed by DTI or GQI):***

1. If the image size of the NIFTI file matches the diffusion data, then no image resampling is applied. DSI Studio will load it directly.
2. If the ROI file originated from the subject's own T1W or T2W (e.g. a FreeSurfer segmentation in T1-weighted or T2-weighted image space), then you need to first insert the original structural image (e.g. T1-weighted or T2-weighted images) by [Slices][Insert T1/T2]. DSI Studio will work out a background registration, which may take 5 minutes to converge. After the registration is stabilized, load the ROI file, and DSI Studio will automatically apply the registration from the previous structural image. The transformation matrix in the nifti header will not be used here.
3. If the NIFTI file is in the MNI space, use [Region][Load MNI Region] to import the region. A background nonlinear registration will be conducted that allows wrapping the ROI to the subject space.

***If your FIB file is in the template space (i.e., reconstructed by QSDR):***

1. If the dimension of the NIFTI file matches that of the raw diffusion data, DSI Studio will automatically warp the region to the QSDR space.
2. If the NIFTI file was generated from T1W (e.g. manually drawing or from Freesurfer), the NIFTI file has to be converted to the original diffusion space. Then DSI Studio can load it and apply transformation automatically. DSI Studio can take the nifti file with multiple values as multiple ROIs. You can supply a label file with the nifti file. An example of the label file can be found under the "atlas" folder in the DSI Studio package.

DSI Studio can load ROIs from a text file of the ROI coordinates. The coordinates should be integers (floating point will be rounded up) because the coordinates indicate the "voxel" of the seeding regions. Users should be noted that the actual seeding points are uniformly distributed "within" the seeding region. The coordinates here may not necessarily match the exact seeding points in the tracking algorithm.

## [Regions][Open MNI Region]

This function will take an MNI space region and warp it into the native space.

## [Regions][Statistics]

DSI Studio can export metrics associated with a region (e.g. size, location, FA, ADC, ...etc). The exported information is a text file including the coordinates and the corresponding values for the indices.

## [Regions Misc][New Region From Thresholding]

This function creates a new region by thresholding the current slice.

## [Regions Misc][New Region From MNI Coordinate]

This function creates a sphere given the MNI Coordinate and the radius of the sphere.
