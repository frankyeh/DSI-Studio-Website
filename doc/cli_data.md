# Data Export/Import to MATLAB

DSI Studio saves most files in MAT -v4 format. You can ungzip the fib.gz, src.gz file and rename it to view the content in MATLAB.

## Load MRtrix3 FOD in DSI Studio

Convert FOD.mif using the following command:

```
sh2peaks fod.mif peak.mif
mrconvert peak.mif -stride 1,2,3,4 peak.nii.gz
```

The peak.nii.gz can be loaded in DSI Studio [Step T3 Fiber Tracking] to run fiber tracking.

![image](https://user-images.githubusercontent.com/275569/158298172-0fd06ad6-ac08-4f2f-9736-db85a95289cf.png)




## Load/Save SRC files in MATLAB

|   matrix | description|
|:---------|:-----------|
| *dimension* | A 1-by-3 vector storing the dimension of the containing image. |
| *voxel_size* | A 1-by-3 vector storing the voxel size in mm. |
| *image0,image1,...* | The images extracted from DICOM or NIFTI files. These matrices can be restored to a 3-dimensional image volume by the command "reshape(image0,dimension);" |
| *b-table* | The b-table of the extracted images. b(1,:) stores b-value, whereas b(2:4,:) stores gradient vector. |


[![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1468760874347/Manual/export-data-to-matlab/src.PNG)](https://sites.google.com/a/labsolver.org/dsi-studio/Manual/export-data-to-matlab/src.PNG?attredirects=0)

The SRC file (*.src, *.src.gz) stores the diffusion weighted images and b-table that can be used to do reconstruction. The SRC files are MATLAB matrix file stored in V4 version. The src.gz is the SRC file compressed by gzip. To load an SRC file in MATLAB, uncompress the file and rename it as .mat.

An SRC file stores the raw image volumes in addition to image dimension, voxel size, and b-table ([see how to get .src file](https://sites.google.com/a/labsolver.org/dsi-studio/Manual/Parse-DICOM)). The SRC files are MATLAB matrix file stored in V4 version. To load an SRC file into Matlab, extract .src.gz file to create an uncompressed .src file. Then rename the .src file to .mat file and load it in Matlab. The meanings of each exported matrix are explained as follows.

The following is a list of matrix used in the SRC file.


After loading the .src file in Matlab, the user can process the data and save it back to an SRC file using "save('filename.src','-v4');" Note that you have to specify **'-v4' **so that DSI Studio can read the .src file.


**Example: Load the DWI as a 4D image from a src.gz file**

```matlab
load('DSI-253d_PA.nii.gz.mat')
bsize = size(b_table);
for i=1:bsize(2)
eval(strcat('images(:,:,:,i) = reshape(image',int2str(i-1),',dimension);'));
end
```

**Example: Load the DWI as a 4D image from a src.gz file**

```matlab
function [image btable vs]= read_src(file_name)
if ~exist('file_name')
    file_name =  uigetfile('*.src.gz');
end
if file_name == 0
    image = [];
    return
end
gunzip(file_name);
[pathstr, name, ext] = fileparts(file_name);
movefile(name,strcat(name,'.mat'));
load(strcat(name,'.mat'));
bsize = size(b_table);
image = zeros([dimension bsize(2)]);
for i = 1:bsize(2)
    eval(strcat('image(:,:,:,',int2str(i),')=reshape(image',int2str(i-1),',dimension);'));
    eval(strcat('clear image',int2str(i-1)));
end
delete(strcat(name,'.mat'));
btable = b_table;
vs = voxel_size;
end
```

**Example: change the DWI image orientation in the src file. The axial slice is rotated to sagittal direction.**

```matlab
%change permute_dim and num_dwi according to your case 
permute_dim = [3 1 2];

num_dwi = 120;

for i = 0:num_dwi

% reodering the dim to z x y
eval(strcat('image',int2str(i),' = permute(reshape(image',int2str(i),',dimension),permute_dim);'));
% flip the third dimension
eval(strcat('image',int2str(i),' = image',int2str(i),'(:,:,128:-1:1);'));
eval(strcat('image',int2str(i),' = reshape(image',int2str(i),',1,[]);'));

end
dimension = size(permute(reshape(image0,dimension),permute_dim));

% copy the b_value
new_b_table(1,:) = b_table(1,:);

% rotate the b_vector
for i = 1:3
   new_b_table(i+1,:) = b_table(permute_dim(i)+1,:);
end

b_table = new_b_table;

clear new_b_table;
clear i;

save output.src -v4
```

**Example: Save a 4D matrix to the src file**

```matlab
%dbsi_data is a 4D matrix arranged in [x,y,b_vector,z]

dim = size(dbsi_data);

num_dwi = dim(3);
dim(3) = [];

% save image matrix
for i=1:99
eval(strcat('image',int2str(i-1),'=dbsi_data(:,:,',int2str(i),',:);'));
eval(strcat('image',int2str(i-1),'=reshape(squeeze(image',int2str(i-1),'),1,[]);'));
end

% input voxel size in mm
voxel_size = [1,1,1];

% the dimension of each DWI
dimension=dim;

b_table = zeros(4,num_dwi);
% input b_value
b_table(1,:) = b_value; 
b_table(2:4,:) = b_vector; 

save example.src -v4
```

## Load/Save FIB files in MATLAB

 ![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1468760871354/Manual/export-data-to-matlab/mat_export.jpg)

The FIB file (*.fib.gz) stores the vector field (fiber orientations) and anisotropy information (the magnitude) that can be used by DSI Studio to conduct fiber tracking. The FIB files are MATLAB matrix file stored in V4 version. The fib.gz files are the FIB files compressed by gzip. To load an FIB file into Matlab, extract the *.fib.gz file into an uncompressed .fib file. Then rename the .fib file to .mat file and load it using Matlab.


The following is a list of matrix used in the SRC file.

| matrix | description|
|:---------|:-----------|
| *dimension* | A 1-by-3 vector storing the dimension of the containing image. |
| *voxel_size* | A 1-by-3 vector storing the voxel size in mm. |
| *fa0, fa1, fa2,...* | The fa0, fa1,... matrices are image volume recording the anisotropy values of the first fiber (fa0), second fiber (fa1) ...etc. Note that a voxel can have multiple fiber orientations. These FA matrices are originally three-dimensional matrices but stored as one-dimensional vectors. To restore the matrix back to three-dimensional, use the following command. <br> `fa0 = reshape(fa0,dimension);` <br> If there is no fiber resolved in a voxel, the corresponding FA value is assigned by zero. Note that in DSI, QBI, and GQI reconstruction, the fa matrices stores the QA values (The definition of QA is documented in Yet et al., "generalized q-sampling imaging", IEEE TMI, 2010), not the FA value. These fa matrices will be used as the fiber termination threshold to determine the end point of the tracks. <br> Any matrix stored with a dimension of 1-by-*n* will be viewed as the quantitative measurements if *n* matches the total image volume. You can store any mapping such as "gfa", "diffusivity", "t1_map" and DSI Studio can use them in track specific analysis |
| *index0, index1, index2,...* | These index matrices store the "directional index" of the fiber orientation instead of the full orientation vector. <br> For example, the 1st fiber orientation at coordinate (10,10,10) can be obtained by <br> `index0 = reshape(index0,dimension); dir0 = odf_vertices(:,index0(10,10,10)+1);`.<br> The 2nd fiber orientation at coordinate (10,10,10): <br> `index1 = reshape(index1,dimension); <br> dir1 = odf_vertices(:,index1(10,10,10)+1);` |
| *odf_vertices* | The fiber orientation table used by the index matrices. For example: <br> The index is zero based. For instance, if a voxel's *index0* value is 5, the voxel's first fiber has an orientation of *odf_vertices*(:,5+1). |
| *dir0, dir1, dir2, ...* | The vectors of fiber orientations instead of directional index. By default, DSI Studio uses "directional index" (*index0,index1,...* etc.), and thus won't save directional vectors in the FIB file. <br> Each directional matrix has a dimension of 3xN. <br> dir0 is the directional vector of the most prominent fiber, and dir1 is the vector of the second most prominent fiber. These matrices have to be "reshaped" to restore to its original form: <br> ` dir0 = reshape(dir0,[3 dimension]);` <br> This will make it a 3-by-x-by-y-by-z matrix. <br> To determine the number of fibers in a voxel, we need to refer to *fa0*, *fa1*, *fa2*...etc. For example, if a voxel has two fibers, its *fa0* and *fa1* have nonzero values, whereas* fa2*, *fa3*,... are zero. |
| *odf1, odf2, ....* | The ODF values of each voxel can also be exported from DSI Studio. To get the ODF data, check the "output ODF" check box in advance option of the reconstruction window. DSI Studio will store a series of matrices named *odf1, odf2,.... * For each these odf matrices, the dimension is e.g. 321-by-20000. 321 is the dimension of an ODF vector (the actual value depends on the ODF order), and 20000 is the number of the ODFs stored. Note that only the ODFs from the voxels with QA \> 0 or FA \> 0 are stored. <br> For more detail about how to use the ODF matrix and visualize them. If you are using the ODFs calculated from QSDR, you may also need to look into this document to handle the discrepancy between ODF counts and a number of voxels with qa \> 0. <br> Example: load a fib.gz file and get the information <br> The odf_vertices matrix stores the ODF sampling orientations table. This table stores sampling orientations that are equally-distributed (almost) on a sphere with the radius of one. You can use the table generated from DSI Studio or any table you prefer. The odf_face matrix indicates which three orientations forms a triangle on an ODF. YOU may use the one generated from DSI Studio. The exemplary table can be download at the bottom of this page. |

**Example: load a fib.gz file and get the information**

```matlab
function [fa index odf_vertices odf_faces]= read_fib(file_name)
if ~exist('file_name')
    file_name =  uigetfile('*.fib.gz');
end
if file_name == 0
    image = [];
    return
end
gunzip(file_name);
[pathstr, name, ext] = fileparts(file_name);
movefile(name,strcat(name,'.mat'));
fib = load(strcat(name,'.mat'));

max_fib = 0;
for i = 1:10
    if isfield(fib,strcat('fa',int2str(i-1)))
        max_fib = i;
    else
        break;
    end
end

fa = zeros([fib.dimension max_fib]);
index = zeros([fib.dimension max_fib]);

for i = 1:max_fib
    eval(strcat('fa(:,:,:,i) = reshape(fib.fa',int2str(i-1),',fib.dimension);'));
    eval(strcat('index(:,:,:,i) = reshape(fib.index',int2str(i-1),',fib.dimension);'));
end

odf_vertices = fib.odf_vertices;
odf_faces = fib.odf_faces;
delete(strcat(name,'.mat'));
end
```

**Example: get the fiber orientation at coordinate (x,y,z)**

```matlab
% decompress the fib.gz to fib file
% the (x,y,z) coordinates starts from (0,0,0)
load xxx.fib
index0 = reshape(index0,dimension);
fiber_orientation = odf_vertices(:,index0(x+1,y+1,z+1)+1);
```


**Save an FIB file**

Similar to saving .src file, users can save the .fib file using `save xxx.fib -v4`. The generated .fib files are readily loadable in DSI Studio. Note that some matricies are required in a .fib file, including the dimension (matrix named "dimension"), voxel size (matrix named "voxel_size"), fiber directions (dir0, dir1, dir2 or index0, index1, index2), and fiber anisotropy (fa0, fa1, fa2). ODF information is in optional and can be left out.

## Load/Save TT files in MATLAB

The TinyTrack (*.tt.gz) file is a track format used in DSI Studio to store track coordinates. The TT files are MATLAB matrix file stored in V4 version. The tt.gz files are the TT files compressed by gzip. To load a TT file into Matlab, extract the *.tt.gz file into an uncompressed .tt file. Then rename the .tt file to .mat file and load it using Matlab.

The following is a list of matrix used in TT file:

|   matrix | description|
|:---------|:-----------|
| *dimension* | A 1-by-3 vector storing the dimension of the containing image. |
| *voxel_size* | A 1-by-3 vector storing the voxel size in mm. |
| *track* | Please use the following code to parse the "track" matrix |


```matlab
function track = parse_tt(track)
buf1 = uint8(track);
buf2 = typecast(buf1,'int8');
pos = [];
i = 1;
while(i <= length(track))
    pos = [pos i];
    i = i + typecast(buf1(i:i+3),'uint32')+13;
end

track = cell(1,length(pos));
parfor i = 1:length(pos)
    p = pos(i);
    size = typecast(buf1(p:p+3),'uint32')/3;
    x = typecast(buf1(p+4:p+7),'int32');
    y = typecast(buf1(p+8:p+11),'int32');
    z = typecast(buf1(p+12:p+15),'int32');
    tt = zeros(size,3);
    tt(1,:) = [x y z];
    p = p+16;
    for j = 2:size
        x = x+int32(buf2(p));
        y = y+int32(buf2(p+1));
        z = z+int32(buf2(p+2));
        p = p+3;
        tt(j,:) = [x y z];
    end
    track{i} = single(tt)/32;
end
end
```

