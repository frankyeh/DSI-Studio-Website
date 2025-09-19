# Automatic Fiber Tracking (AutoTrack)

<iframe width="560" height="315" src="https://www.youtube.com/embed/Hzeb_q6ux-Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

***If you are using DSI Studio dated between Feb 8 2022 and June 27 2022, please update it to handle the "no result" issue in difficult tracking cases.***

Automatic fiber tracking aims to map a target pathway (e.g. optic radiation, arcuate fasciculus) and is most suitable for pre-surgical planning or tract-of-interest analysis. The function uses a pathway recognition based on the a tractography atlas (Yeh et al. 2018) to filter out false tracks and unrelated tracks. The detail of how it works is the following:

1. non-linear registration of subject data to MNI space
2. seeds placed within the atlas tract volume (i.e. in any voxel corresponding to any tract
3. each of the generated streamlines is compared to the streamlines associated with each fiber tract from the HCP tractography atlas (Yeh 2018) using Hausdorff distances
4. determine which of the generated streamlines are the best match to the streamlines from the target structures in the HCP tractography atlas
5. streamlines matching to the target track in the HCP tractography atlas are retained and all other streamlines are discarded.

To use this function, click on [**Step T3: Fiber Tracking**] to select a FIB file generated from Step T1-T2, and DSI Studio will bring up the tracking window.

Follows the steps below to run automatic fiber tracking.

***Before using auto track, make sure you run a whole-brain tracking as a visual quality check.***

## 1. Click on the [Step T3d:Tracts][Enable auto track...]

DSI Studio will normalize the subject's QA/ISO map to the template QA/ISO map and allow track recognition based on the bring tractography atlas.

## 2. Select Tract of Interest and Run [Fiber Tracking]

After normalization, select the target tracks and click fiber tracking to get the results.

You may change parameters at [Step T3c: Options] such as setting [Terminate if] = 10,000 [Tracts] or 1,000,000 [Seeds] to get satisfactory results.

[Min Length] helps remove fragments.

[Topology-Informed Pruning] will help remove singular fiber.

[Autotrack Tolerance (mm)] controls the tolerance to spurious fibers.

For detailed about tracking parameters, please refer to [whole brain tracking](/doc/t3_whole_brain.html)

## Troubleshooting AutoTrack

Automatic fiber tracking may fail to generate tracks if the data are not optimally reconstructed, or if the data has a quality problem. If "no result" happens very often (e.g. > 50%), then likely there is a post-processing problems.

Here is the checking list:

1. run [quality control for SRC files](/doc/gui_t1.html#batch-quality-control) and remove problematic dataset.

2. Check if the b-table is flipped, or if the image volume is flipped. For more details, please check the documentation at reconstruction.

   For b-table problems, uncheck the "check b-table" function at [Step T2b(2)], and manually flip the b-table in y-direction if you use bval and bvec from FSL.

The above issues will affect all pathways. If "no result" happens more often in small pathways (e.g. right AF, SLF I, corticopontine tract...etc) or in only some subjects (see examples reported in [1](https://groups.google.com/g/dsi-studio/c/wfXcuxk1I3g),[2](https://groups.google.com/g/dsi-studio/c/oOVyL9MN7PQ)), then likely your data acquisition is not perfect enough to map all small tracts in all individuals. 

The following is a list of common acquisition issues that may lead to poor results, and unfortunately, **there is not much you can do**.

1. nonisotropic spatial resolution (e.g., slice thickness much larger than in-plane resolution)
2. b-value not enough (e.g., b-value less than 1,500)
3. insufficient diffusion sampling directions (e.g., less than 100 directions)

If you still have concerns or unexpected issues, please upload the problematic SRC file(s) using the upload link provided on the left navigation bar and notify me at the Discussion forum.

## Customize a tract atlas

The built-in tract atlas can be modified (e.g. merge or add tracts) for study needs. 

The trat atlas are stored as ICBM152_adult.tt.gz (tracts) and ICBM152_adult.tt.gz.txt (labels) under /atlas/ICBM152_adult under the DSI Studio pacakage. In the Mac version, right-click on dsi_studio_64.app and look for atlas folder under /Contents/MacOs. 


To modify it, open HCP1065_1mm.fib.gz (can be found [here](https://brain.labsolver.org/hcp_template.html)) in [Step T3 Fiber Tracking], and load the tract atlas using [Tracts][Open Tracts].

After modifying the tracts, check all the tracts and save them back using [Tracts][Save Tracts][Save All Tracts...]

Restart DSI Studio to take effect.

# Tractometry

[Tracts][Statistics] provides values extracted from shape analysis and diffusion metrics.

[Shape analysis](https://www.sciencedirect.com/science/article/pii/S1053811920308156) uses the topology information from tractography streamlines to derive length, area, volume, and shape descriptors. The analysis quantifies macroscopic structural features of fiber pathways.

The details of the diffusion metrics are documented at [Diffusion MRI Metrics](https://sites.google.com/a/labsolver.org/dsi-studio/Manual/diffusion-mri-indices)

***Shape Metrics***

| Metrics | Description |
|:--------|:------------|
| mean length  | multiplying number of coordinates in the streamlines with the distance between the coordinates |
| span(mm) | distance between two end regions |
| curl | length divided by span |
| elongation |    |
| diameter(mm) |  |
| volume (mm^3) | multiplying number of voxels passed by all streamlines with the voxel size (in mm cubic) |
| trunk volume(mm^3) |  |
| branch volume(mm^3) |  |
| total surface area(mm^2) |  |
| total radius of end regions(mm) | |
| total area of end regions(mm^2) |  |
| irregularity 12.0779  | |
| area of end region 1(mm^2) |  |
| radius of end region 1(mm) |  |
| irregularity of end region 1 |  |
| area of end region 2(mm^2) |  |
| radius of end region 2(mm) |  |
| irregularity of end region 2 |  |

***Diffusion Metrics***

| Metrics | Description |
|:--------|:------------|
| qa | quantitative anisotropy |
| nqa  | normalized QA (maximum QA of a scan=1) |
| dti_fa | fractional anisotropy|
| md | mean diffusivity |
| ad | axial diffusivity |
| rd | radial diffusivity |
| iso | isotropic diffusion |
| rdi | restricted diffusion |
| nrdi02L | non-restricted diffusion at 0.2 diffusion sampling length ratio |
| nrdi04L | non-restricted diffusion at 0.4 diffusion sampling length ratio |

You can insert an external volume (e.g., T1W, T2W, PET...etc.) using [Slices][Insert T1W/T2W...] and use track statistics to sample them along the fiber pathways.

## Tractography file formats

The supported tractography format in DSI Studio include the TT format (DSI Studio default), Text format, TRK format (TracVis), TCK format (Mrtrix), and MAT format (MATLAB):

***TT format***

The Tiny-Tract (TT) format stores tractography with a resolution limit of 1/32 voxel spacing. It is a gz compressed mat file. To load it in MATLAB, ungzip it and rename it as .mat. There will be matrices including dimension, parameter_id, report, track, voxel_size.

The tract matrix can be parsed using the following code:

```mat
function track = parse_tt(track)
buf1 = uint8(track);
buf2 = int8(track);
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


***Text format***

The output format can be a text file that stores the coordinates of each fiber track. The coordinates of each fiber trajectory are stored in one line. The x y z coordinates are listed sequentially:

```
x1 y1 z1 x2 y2 z2 ... xn yn zn               <---first tract
x1 y1 z1 x2 y2 z2 ... xn yn zn               <---second tract
```

***Along-tract metrics format***

The fiber trajectories are sequences of 3D coordinates. DSI Studio uses these coordinates to sample the index like FA and ADC. The along-track sampling samples one value for each coordinate in a trajectory. The data arrangement is similar to that of the tract coordinate TXT file.
   
For each coordinate on a trajectory, DSI Studio calculates the index using trilinear interpolation. The calculated values of a trajectory are exported as a sequence of numbers in a line. The values are separated by space. The first value is the first coordinate of the first trajectory, as shown in the following.
     

```
v11 v12 v13 v14...
v21 v22 v23 v24...
```

Here v11 is the value corresponding to the coordinate (x1,y1,z1) of the first trajectory line (show above). v12 corresponds to (x2,y2,z2)...etc.

***MAT Format***

The trajectories can be saved in a Matlab MAT version 4 format. The coordinates of all tracts are stored in a matrix named "tracts". The numbers of coordinates for each fiber are stored in a matrix named "length". The coordinates of the first trajectory are stored in tracts(:,1:length(1)), and the second in tracts(:,length(1)+1,length(1)+length(2)).

To save the tracts data in MAT version 4 format. Use the command save tract.mat -v4


# Tract Profile

![image](https://user-images.githubusercontent.com/275569/147835769-2d0eb159-2c26-4820-8037-410e95226eac.png)

The tract profile is similar to [Automated Fiber Quantification (AFQ)](https://github.com/yeatmanlab/AFQ). The function is located at [Tracts][Tract Profile]. DSI Studio provides a reporting interface that visualizes the quantitative index of a generated tractography. The generated plot and data can be exported as image or text value data.

## X direction, Y direction, and Z direction

![image](https://user-images.githubusercontent.com/275569/147835776-b20e26dd-ab69-4e23-829e-41df14133c22.png)

**An example of corticospinal tract viewed from the anterior**

![image](https://user-images.githubusercontent.com/275569/147835782-ba03fd19-b335-405d-9ccf-58f644471cb9.png)

**Result obtained from X-direction sampling**

For X-direction, the coordinates of all tracks are projected to x-axis simultaneously, and the averaged values along the x-direction are estimated using kernel density estimator with the bandwidth specified in the interface. The unit of the bandwidth is at the scale of voxel size. The sampling strategies for y-direction and z-direction are conducted similarly.

## Fiber orientation

All fiber tracts are stretched to the same length, and the sampling starts from one end to another. The sampled values are regressed using a kernel density estimator with the bandwidth specified in the interface.

![image](https://user-images.githubusercontent.com/275569/147835790-a16c9d8a-0c91-4131-9b0d-8ca1954e6673.png)

**Illustration of the fiber stretch strategy for index sampling.**

## Mean of each fiber

the mean value of the index is calculated for each fiber tract first, and all the values are plotted.

# Tract Density Imaging

Track density imaging (TDI) was introduced by Calamante et al. [1]. TDI was shown to achieve higher resolution and facilitate the visualization of smaller structures. 

DSI Studio offers the function under [Tracts][Save Track Density] to export tract density imaging after tractography is generated. To generate TDI, smaller step size is required to produce a good result. The default setting of step size used in DSI Studio is half of the voxel size. This setting has to be changed to 1/8 voxel size in order to achieve a subvoxel resolution of 1/4 voxel size. A Larger amount of fiber tracts (e.g. 10,000 for human study) tends to generate better TDI. 

Another way to include more fiber is to generate multiple TDI and average them together. This may offer the same effect of generating a huge equivalent amount of fiber tracts. You may want to export TDI in MATLAB's mat format and use Matlab to perform the average.


Use [Tracts][Save Track Density] to save TDI. There are three options under this submenu

- **TDI in diffusion space**

The first option is exporting the TDI in the diffusion space, which does not use the subvoxel resolution.

- **TDI in subvoxel diffusion space**

The second one exports TDI in a 4-times higher spatial resolution than the current diffusion space.

- **TDI in current slice space**

You may load a T1w image in DSI Studio and export the TDI in the T1 space.


---

## 🧠 Tract-to-Region (T2R) Connectome in DSI Studio

---

<iframe width="560" height="315" src="https://www.youtube.com/embed/7Ns9vHl21J8?si=GxfaKk8cz4FCpGMQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The **tract-to-region (T2R) connectome** represents structural connectivity as an **n-by-m matrix**, where **n** is the number of white matter tract bundles, and **m** is the number of brain parcellation regions. Each matrix entry quantifies the probability or strength of a tract's connection to a specific cortical region. This method offers a more anatomically precise mapping compared to conventional region-to-region connectomes, which often overlook the specific white matter pathways involved.

---

## 🛠️ Steps to Generate a T2R Connectome

### 1. Map Tract Bundles Using AutoTrack

Utilize [AutoTrack](https://dsi-studio.labsolver.org/doc/gui_t3_atk.html) to automatically identify and map tract bundles of interest, such as the arcuate fasciculus, cingulum, or IFOF. This can be performed via the GUI or command line.

> ⚠️ **Important:** Avoid using whole-brain tractography for T2R connectome construction, as it may introduce false positives and lacks specificity.

![AutoTrack Interface](https://github.com/user-attachments/assets/6532c9d2-9516-4374-973d-206df90f3764)

### 2. Load Brain Parcellations

In \[Step T3a]\[Atlas...], load the desired brain parcellation atlas, such as HCP-MMP or Brodmann. Ensure all regions from the selected parcellation are included for comprehensive coverage.

![Loading Parcellations](https://github.com/user-attachments/assets/9251ab6a-f2f2-4608-be7a-bfed5b86e492)

### 3. Generate the T2R Matrix

Navigate to \[Regions] > \[Tract-to-Region Connectome] to compute the T2R matrix. This function calculates the overlap between each tract bundle and cortical region, outputting a text-based matrix that can be imported into spreadsheet software like Excel for further analysis.

![T2R Matrix Output](https://github.com/user-attachments/assets/ce5bbfbc-a17b-459f-81db-c596ffccf4f0)

### 4. Visualize T2R Connectivity

To visualize the connectivity of a specific tract bundle:

* Go to \[Step T3c Option] > \[Region Rendering] > \[Region Color].
* Set **Style** to `Metrics` and **Metrics** to `Current Tract`.
* Adjust **Max Value** to a lower threshold (e.g., 0.2) to enhance visibility.
* Select the tract bundle of interest from the list to display its cortical projections.

![Arcuate Fasciculus Connectivity](https://github.com/user-attachments/assets/89d3407d-fbb2-444c-b848-2b5c89321248)

---

## 📌 Additional Notes

* **Data Consistency:** Approximately 85% of tract-to-region connections are consistent across individuals, while the remaining 15% exhibit variability, underscoring the importance of individualized mapping in certain contexts.&#x20;

* **Clinical Applications:** The T2R connectome is particularly useful in clinical scenarios, such as assessing the impact of white matter lesions on cortical function or planning neurosurgical interventions.

* **Research Utility:** This approach facilitates a deeper understanding of the brain's structural organization, enabling studies on functional connectivity, development, and neurological disorders.

---

### 🧠 Tract-to-Region Connectome: Command-Line Examples

DSI Studio offers versatile command-line options to compute T2R connectomes, enabling efficient batch processing and integration into automated pipelines.

#### 1. Map the Left Arcuate Fasciculus and Derive Its T2R Connectome with HCP-MMP Parcellation

This command performs deterministic tracking of the left arcuate fasciculus and computes its connectivity with the HCP-MMP atlas:([DSI Studio Documentation][1])

```bash
dsi_studio --action=trk --source=subject.fib.gz --track_id=ArcuateFasciculusL --connectivity=HCP-MMP
```



* `--action=trk`: Initiates deterministic fiber tracking.
* `--track_id=ArcuateFasciculusL`: Specifies the tract of interest.
* `--connectivity=HCP-MMP`: Computes the tract-to-region connectome using the HCP-MMP parcellation.([DSI Studio Documentation][1], [DSI Studio Documentation][2])

*Note*: Avoid using whole-brain tractography for T2R connectome construction, as it may introduce false positives and lacks specificity.

#### 2. Load Pre-Derived Tracts and Compute Their T2R Connectome with Brodmann Parcellation

If you have previously generated tract files (e.g., via AutoTrack or manual segmentation), you can compute their T2R connectome as follows:

```bash
dsi_studio --action=ana --source=my.fz --tract=bundle1.tt.gz,bundle2.tt.gz,bundle3.tt.gz --connectivity=Brodmann
```



* `--action=ana`: Initiates analysis on existing tracts.
* `--tract=...`: Specifies the tract files to analyze.
* `--connectivity=Brodmann`: Computes the connectome using the Brodmann parcellation.([DSI Studio Documentation][3], [DSI Studio Documentation][2])

*Note*: This approach is suitable for analyzing multiple tracts simultaneously.

#### 3. Use AutoTrack to Map Recommended Pathways and Derive Their T2R Connectome with HCP-MMP and Brodmann Parcellations

To automatically map a set of major white matter pathways and compute their T2R connectomes with multiple parcellations:

```bash
dsi_studio --action=atk --source=*.fz --track_id=Arcuate,Cingulum,Aslant,InferiorFronto,InferiorLongitudinal,SuperiorLongitudinal,Uncinate,Fornix,Corticos,ThalamicR,Optic,Lemniscus,Reticular,Corpus --connectivity=HCP-MMP,Brodmann
```

* `--action=atk`: Initiates automatic fiber tracking using predefined tract IDs.
* `--source=*.fz`: Applies the operation to all `.fz` files in the directory.
* `--track_id=...`: Specifies the list of tracts to map.
* `--connectivity=HCP-MMP,Brodmann`: Computes connectomes using both HCP-MMP and Brodmann parcellations.([DSI Studio Documentation][4])

*Note*: Ensure that your `.fz` files are reconstructed using QSDR to align with the template space required for AutoTrack.([DSI Studio Documentation][5])

