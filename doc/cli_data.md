## 📁 File Formats Used in DSI Studio

DSI Studio uses a set of optimized file formats tailored for diffusion MRI analysis. The `.sz` files (formerly `.src.gz`) store preprocessed 4D diffusion-weighted images along with their associated b-table, and serve as the input for reconstruction. The `.fz` files (formerly `.fib.gz`) contain voxel-wise diffusion information, including fiber orientations (fixels), anisotropy, and other scalar metrics derived from DTI or GQI models—used for fiber tracking and connectome analysis. All formats are designed for compact storage and fast processing, and can be exported to NIfTI using DSI Studio’s built-in tools or Python scripts.

Here’s a clearer, more concise, and professionally structured rewrite of your **SRC Files** section:

## 📦 SRC Files (`*.src.gz`, `*.sz`)

SRC files are used in DSI Studio to store diffusion-weighted imaging (DWI) data along with their associated b-table for subsequent image reconstruction. Two main formats are currently in use:

* **`*.src.gz`** — Used in versions prior to the Hou update. This is a gzip-compressed MATLAB `.mat` file (version 4) that stores full-volume data.
* **`*.sz`** — Introduced in the Hou version and beyond. This is a more compact format that stores only values within a predefined mask and applies linear scaling via `slope` and `inter` parameters for compression.

Both formats follow a consistent matrix structure, but due to the use of periods (`.`) in matrix keys, `*.sz` files are **not MATLAB-compatible**. However, they can be fully accessed and processed using **Python (`scipy.io`)** or C++.

---

### 🧠 Matrix Structure Overview

| Matrix Name             | Description                                                                                                                                                                         |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dimension`             | A 1×3 vector specifying the 3D volume dimensions.                                                                                                                                   |
| `voxel_size`            | A 1×3 vector specifying voxel size in millimeters.                                                                                                                                  |
| `image0`, `image1`, ... | DWI volumes extracted from DICOM or NIfTI. In `.src.gz`, each image is a full 3D volume. In `.sz`, only values inside the mask are stored and compressed using `slope` and `inter`. |
| `b_table`               | A 4×N matrix: first row for b-values, next three rows for gradient directions.                                                                                                      |
| `mask`                  | A binary 1D array representing the spatial mask; reshape to `dimension` to get the 3D mask.                                                                                         |
| `report` *(optional)*   | A text string describing the processing steps or metadata.                                                                                                                          |


---

### 📥 Example 1: Load a `.sz` File and List Contents

```python
!wget -q https://github.com/data-hcp/lifespan/releases/download/hcp-ya-retest/103818a.sz
!gunzip -c 103818a.sz > 103818a.mat

import scipy.io
for name, shape, _ in scipy.io.whosmat('103818a.mat'):
    print(f"{name}: shape {shape}")
```

---

### 🔁 Example 2: Convert `.sz` or `.fz` Back to `.src.gz` or `.fib.gz`

```python
import scipy.io, numpy as np, gzip, shutil, os

def convert_to_mat(input_file):
    temp = input_file.replace('.sz', '.mat').replace('.fz', '.mat')
    with gzip.open(input_file, 'rb') as f_in, open(temp, 'wb') as f_out:
        shutil.copyfileobj(f_in, f_out)
    return temp

def restore_and_save(mat_file, output_file):
    data = scipy.io.loadmat(mat_file)
    mask = data.get('mask')
    if mask is not None:  # only needed for .sz
        dim = data['dimension'][0]
        nz = np.flatnonzero(mask.T)
        for k in list(data.keys()):
            if '.' in k or k.startswith('__') or k in ['dimension', 'voxel_size', 'b_table', 'mask']: continue
            v = data[k]
            if v.shape[1] != len(nz): continue
            slope = data.get(f"{k}.slope", [[1]])[0][0]
            inter = data.get(f"{k}.inter", [[0]])[0][0]
            full = np.zeros(np.prod(dim), dtype=np.float32)
            full[nz] = v.flatten() * slope + inter
            data[k] = full.reshape(dim[::-1]).T
    scipy.io.savemat(mat_file, data, format='4')
    with open(mat_file, 'rb') as f_in, gzip.open(output_file, 'wb') as f_out:
        shutil.copyfileobj(f_in, f_out)
    os.remove(mat_file)

# Example:
# convert_to_mat("file.sz") → restore_and_save → "file.src.gz"
mat = convert_to_mat("103818a.sz")
restore_and_save(mat, "103818a.src.gz")
```

---

### 🧠 Example 3: Convert `.sz` to 4D NIfTI + `.bval` + `.bvec`

```python
import scipy.io
import numpy as np
import gzip
import shutil
import os
import nibabel as nib

def convert_sz_to_nifti_bval_bvec(sz_filepath, output_prefix):
    """Converts a DSI Studio .sz file to 4D NIfTI, bval, and bvec files."""

    temp_mat = sz_filepath.replace('.sz', '.mat')
    with gzip.open(sz_filepath, 'rb') as f_in, open(temp_mat, 'wb') as f_out:
        shutil.copyfileobj(f_in, f_out)

    data = scipy.io.loadmat(temp_mat)
    dimensions = data['dimension'][0]
    voxel_size = data['voxel_size'][0]
    b_table = data['b_table']

    mask = data.get('mask')
    if mask is not None and mask.size > 0:
        mask_3d = mask.reshape(dimensions[::-1]).T
        nz_indices = np.flatnonzero(mask_3d)
        
        num_dwi_volumes = sum(1 for k in data.keys() if k.startswith('image') and not ('.' in k or k.startswith('__')))
        dwi_data_4d = np.zeros((*dimensions, num_dwi_volumes), dtype=np.float32)

        for i in range(num_dwi_volumes):
            img_key = f"image{i}"
            compressed_data = data[img_key].flatten()
            slope = data.get(f"{img_key}.slope", [[1]])[0][0]
            inter = data.get(f"{img_key}.inter", [[0]])[0][0]
            
            full_1d_volume = np.zeros(np.prod(dimensions), dtype=np.float32)
            full_1d_volume[nz_indices] = compressed_data * slope + inter
            dwi_data_4d[..., i] = full_1d_volume.reshape(dimensions[2], dimensions[1], dimensions[0]).transpose(2,1,0)
    else: # Likely an old .src.gz format (full volumes)
        image_volumes = [data[k] for k in data.keys() if k.startswith('image') and not ('.' in k or k.startswith('__'))]
        if not image_volumes:
            raise ValueError("No image data found in the .sz file.")
        dwi_data_4d = np.stack([vol.T for vol in image_volumes], axis=-1)

    affine = np.diag(np.append(voxel_size, 1))
    affine[:3, 3] = -voxel_size * (np.array(dimensions) - 1) / 2.0
    
    nib.save(nib.Nifti1Image(dwi_data_4d, affine), f"{output_prefix}.nii.gz")
    np.savetxt(f"{output_prefix}.bval", b_table[0, :].reshape(1, -1), fmt='%.6f')
    np.savetxt(f"{output_prefix}.bvec", b_table[1:4, :], fmt='%.6f')

    os.remove(temp_mat)

# Example usage:
# If you don't have it already, download the example .sz file:
# !wget -q https://github.com/data-hcp/lifespan/releases/download/hcp-ya-retest/103818a.sz
# convert_sz_to_nifti_bval_bvec('103818a.sz', 'output_dwi')
```


## 📦 FIB Files (`*.fib.gz`, `*.fz`)

DSI Studio uses `.fib.gz` (older versions) and `.fz` (Hou versions and later) to store fiber tracking results, including fiber orientation vectors and anisotropy metrics.

* `.fib.gz` is a GZip-compressed MATLAB `.mat` file (v4 format).
* `.fz` is a compressed and masked version of `.fib.gz` that stores only voxels inside the mask. Data is scaled using `matrix_name.slope` and offset by `matrix_name.inter`.

> ⚠️ Note: Due to `.` in variable names, `.fz` cannot be read directly in MATLAB, but it can be processed using `scipy.io` in Python or with C++ code.

There is also a `mask` matrix (present in `*.fz` files) that specifies the voxel locations where fiber data (`fa0`, `fa1`, `fa2`, `index0`, `index1`, `index2`) and diffusion metrics (e.g., `dti_fa`, `ad`, `rd`, etc.) are stored. Except for `indexN` matrices (which store integers), all other values are stored in compressed form using linear scaling:

```text
restored_value = raw * slope + inter
```

These scaling factors are stored in `matrix_name.slope` and `matrix_name.inter`.

---

### 🧠 Matrix Contents (Key Variables)

| Variable                           | Description                                                                                                                 |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `dimension`                        | `[x, y, z]` — image dimensions                                                                                              |
| `voxel_size`                       | Voxel size in millimeters                                                                                                   |
| `mask`                             | A binary 3D mask indicating valid voxels. Use it to reconstruct full 3D volumes from vector data.                           |
| `fa0`, `fa1`, ...                  | Anisotropy values (e.g., QA or FA) for each resolved fiber. Stored as vectors. Use `reshape(faN, dimension)` to view in 3D. |
| `index0`, `index1`, ...            | Orientation indices for each fiber, pointing to directions in `odf_vertices`. Values are integers (not scaled).             |
| `dir0`, `dir1`, ... (optional)     | Full directional vectors (if saved). Reshape to `[3, x, y, z]` using `reshape(dir0, [3, dimension])`              |
| `odf1`, `odf2`, ... (optional)     | Raw ODF values. Matrix shape is `[#vertices, #voxels]`                                                            |
| `dti_fa`, `dti_ad`, `dti_rd`, etc. | Diffusion tensor metrics stored as vectors within the mask and scaled by `.slope` and `.inter`                              |


**Example: Load a .fz file (Python)**

```python
!wget -q https://github.com/data-hcp/lifespan/releases/download/hcp-ya-retest/103818a.fz
!gunzip -c 103818a.fz > 103818a.mat
import scipy.io
# Use whosmat to list variables (name, shape, type)
variables = scipy.io.whosmat('103818a.mat')
for name, shape, _ in variables:
    print(f"{name}: dimension {shape}")
```

Here is a clean and efficient rewrite of both examples in **Python**, using `scipy.io` and handling both `.fib.gz` and `.fz` files. It also considers optional `mask`, `slope`, and `inter` matrices for value restoration.

---

### ✅ Example: Load `.fib.gz` or `.fz` and Extract FA, Index, and fiber orientation index

```python
import gzip, os
import numpy as np
import scipy.io
def load_fib(filename):
    # Unzip .gz to .mat
    mat_file = filename.replace('.gz', '')
    with gzip.open(filename, 'rb') as f_in, open(mat_file, 'wb') as f_out:
        f_out.write(f_in.read())

    # Load MATLAB v4 format
    data = scipy.io.loadmat(mat_file)
    os.remove(mat_file)

    dimension = tuple(data['dimension'][0])
    max_fib = sum(f'fa{i}' in data for i in range(10))
    
    fa = np.stack([
        reshape_and_restore(data, f'fa{i}', dimension, data.get('mask'))
        for i in range(max_fib)
    ], axis=-1)

    index = np.stack([
        reshape_and_restore(data, f'index{i}', dimension, data.get('mask'), dtype=int)
        for i in range(max_fib)
    ], axis=-1)

    return fa, index

def reshape_and_restore(data, key, dim, mask, dtype=float):
    raw = data[key].flatten()
    slope = data.get(f"{key}.slope", [[1]])[0][0]
    inter = data.get(f"{key}.inter", [[0]])[0][0]
    restored = raw * slope + inter if dtype == float else raw.astype(int)

    if mask is not None:
        mask_3d = mask.reshape(dim[::-1]).T
        full = np.zeros(mask.size, dtype=restored.dtype)
        full[np.flatnonzero(mask)] = restored
        return full.reshape(dim)
    else:
        return restored.reshape(dim)
```

---


## TT file (*.tt.gz) 

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

## Load MRtrix3 FOD in DSI Studio

Convert FOD.mif using the following command:

```
sh2peaks fod.mif peak.mif
mrconvert peak.mif -stride 1,2,3,4 peak.nii.gz
```

The peak.nii.gz can be loaded in DSI Studio [Step T3 Fiber Tracking] to run fiber tracking.

![image](https://user-images.githubusercontent.com/275569/158298172-0fd06ad6-ac08-4f2f-9736-db85a95289cf.png)
