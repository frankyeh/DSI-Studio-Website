# Step T3 Whole Brain Fiber Tracking

Open the main window and click a button named [**Step T3: Fiber Tracking**] to select a FIB file generated from Step T1-T2. DSI Studio will bring up the tracking window.

**Click on [Step T3d: Tracts][Fiber Tracking] to get whole brain fiber tracks.**
 
DSI Studio will seed the whole brain region and perform fiber tracking. If successful, you will see whole-brain tractography:
 
  ![image](https://user-images.githubusercontent.com/275569/147802869-07d8b7e9-aea9-4fc3-8af5-29286787841f.png)

Troubleshooting:

1. **No tract showing up**: reduce [**Step T3c: Options**][**Tracking Parameters**][**Min Length**]
2. **Too many fragments**: increase [**Step T3c: Options**][**Tracking Parameters**][**Min Length**]
3. **Tracts do not reach cortex**: reduce [**Step T3c: Options**][**Tracking Parameters**][**Tracking Threshold**]
4. **Tracts appear odd or noisy**: increase [**Step T3c: Options**][**Tracking Parameters**][**Tracking Threshold**]. Also could be due to acquisition problem, distortion, or reconstruction error. Please refer to the troubleshooting sections at Step T2.

Examples of poor-quality whole-brain tractography:

<img src="https://user-images.githubusercontent.com/275569/195244541-d6b033f4-d50f-4563-985d-8a2cdd3c14cd.png" width=200><img src="https://user-images.githubusercontent.com/275569/195244700-4ef138e2-2538-4d73-ad4e-740b97831839.png" width=200>![image](https://user-images.githubusercontent.com/275569/147860570-f12b4cae-5725-4d34-ac9e-9a4078d664b2.png)

b-table flipped (left 1), motion or eddy distortion problem (left 2), [Tracking Threshold] is too low (left 3), or too high (left 4). 

## Fine-Tuning Parameters

To get the best quality of whole brain tractography, you may need to change [**Step T3c: Options**][**Tracking Parameters**], including [**Tracking Threshold**] (The default is `0` which is a randomized value around 0.6 otsu thresholds), [**Min Length**], [**Terminate if**] to get better results. 

The details of each parameter:

| Parameters | Default | Descriptions|
|:-----------|:--------|:------------|
|***Tracking Index*** | `0` (randomized) | Choose which metric to use as the termination threshold. In DTI, FA is used as the index for the filter to determine the fiber threshold. In DSI, QBI, GQI, the fiber threshold is based on the quantitative anisotropy (QA), which is defined for each resolved fiber orientation. The definition of QA is documented in the GQI paper [3]. |
| ***Tracking Threshold*** | `0` (randomized) | This parameter determines the threshold for terminating fiber tracking. If a threshold of 0 is assigned, DSI Studio will use a randomly selected threshold from 0.5 and 0.7 of Otsu's threshold. Otsu's method calculates the optimal separation threshold that maximizes the variance between the background and foreground. <br> If you input a non-zero value and switch between different tracking indexes, the value will be automatically determined using 0.6 of Otsu's threshold. <br> The general principle for choosing the threshold is to select the lowest value that has acceptable spurious fibers. |
| ***Angular Threshold*** | `0` (randomized) | This threshold (in degrees) serves as a termination criterion. If two consecutive moving directions have a crossing angle above this threshold, the tracking will be terminated. <br> The default `0` will be a random selection of a value from 15 degrees to 90 degrees.|
| ***Step size*** | `0` (randomized) | Step size defines the moving distance in each tracking iteration. This unit is in millimeter-scale. The default value `0` will be a random selection of the step size from 0.5 to 1.5 voxel distance. |
| ***Min Length*** | `30` | Length constraint that filters out the tracks that are either too short |
| ***Max Length*** | `300` | Length constraint that filters out the tracks that are either too long |
| ***Terminate if*** | `500` | The total number of `Tracts` generated or `Seeds` placed |
|                    | `Tracts` | If `Tracts` is selected, DSI Studio will keep seeding and tracking until the number of tracts reaches the above number because a seed may not always result in a track due to [Min Lnegth][Max Length] or ROI filtering. If `Seeds` is selected, DSI Studio will stop once the total seeding count reaches the number. <br> The choice of the value depends on the application. The rule of thumb is that the final result of the pipeline (e.g. averaged fa, tract area...etc.) should not change more than 5% when you add 1,000 more tracts. A preliminary test is often needed to find the minimum required number of tracts that result in stable results.|
| ***Topology-Informed Pruning (TIP)***| 16 | Number of iteration for removing false connections using the TIP method (Yeh Neurotherapeutics 2018)[5] Used in automatic fiber tracking. Used in autotrack |
| ***Autotrack tolerance (mm)***| 16 | The distance tolerance for track recognition. A larger number results in more results but increases the chance of false findings autotrack  |

***Advance Options***

| Parameters | Default | Descriptions|
|:-----------|:--------|:------------|
| ***Tracking Algorithm*** | `Streamline(Euler)` | The fiber tracking algorithm used for deterministic fiber tracking. <br> Runge-Kutta method is a higher-order tracking method similar to the default Euler approach.  |
| ***Smoothing*** | `0` (off) | Smoothing serves like a "momentum". For example, if smoothing is 0, the propagation direction is independent of the previous incoming direction. If the smoothing is 0.5, each moving direction remains 50% of the "momentum", which is the previous propagation vector. This function makes the tracks appear smoother. In implementation detail, there is a weighting sum on every two consecutive moving directions. For smoothing value 0.2, each subsequent direction has 0.2 weightings contributed from the previous moving direction and 0.8 contributed from the income direction. To disable smoothing set its value to 0. <br> Assign 1.0 to do a random selection of the value from 0% to 95%. |
| **Seed Orientation*** | `Primary` | Specify the starting orientation of the seeds. The `Primary` approach always starts the tracking from the most prominent fiber in a voxel. The advantage is the stableness and consistency of the results. A way to compensate for this drawback is using whole-brain seeding to explore all possible connections. <br> The `Random` approach starts the tracking a randomly selected fiber orientation, so the results have random factors. The advantage is that it can explore all possibilities, but the drawback is that the results may not be reproduced exactly. The tracking results are also sensitive to noisy fibers because the false fiber orientation can be selected as the starting direction. <br> The `All` approach starts a track for each fiber orientation resolved in a voxel. It aims to explore all possible connections and there are no stochastic factors that may hurt the reproducibility. The drawback is that it is sensitive to noisy fibers because all resolved fiber orientations will be used as the starting directions.|
| ***Check Ending*** | `Off` | If `On`, DSI Studio will check whether the tracking terminates with a location that has anisotropy higher than the threshold. If higher, the fiber is probably terminated in the white matter and will be removed.|
| ***Default Otsu*** | `0.6` | The ratio of otsu threshold to derive default anisotropy threshold.|
| ***Output Format*** | `tt.gz` | The default tractography output file format.|





## 3. Save Tractography

The fiber trajectories can be saved using [Tracts][Save Tracts As]. One should note that [Tracts][Save Tracts As] saves only the current selected fiber bundles. To save tracts of different clusters at one, use [Tracts][Save All Tracts As...].


DSI Studio saves tracks in the native diffusion voxel space rotated to "LPS". The coordinates are voxel coordinates started from (0,0,0) at the most left/anterior/bottom point of the image volume. The orientation is (+right,+posterior, +top). For example, (1,2,3) = [the left most 1st voxel, the most anterior second voxel, the bottom 3rd voxel].


# Connectivity Matrix and Network Measures 

***Recommendation: Do not use connectivity matrix and network measure unless you are really familiar with its methodological utilization, interpretation, and limications. Choose [other analysis approaches](https://dsi-studio.labsolver.org/doc/how_to_analyze_dmri.html) for better sensitivity and specificity.***


The following are steps to generate connectivity matricies:

1. Generate whole brain tracking using the instructions above. Make sure to have at least 1 million tracks (i.e. [**Step T3c: Options**][**Tracking Parameters**][**Terminate if**] to a least 1,000,000 [**Tracks**]).
2. If you would like to use default regions from an atlas to construct the connectivity matrices, skip this step. If you would like to customize a combination of regions (e.g. to include subcortical regions), use the [**Step T3a**][**Atlas**] to add regions from atlases. 
3. Open the connectivity matrix dialog by [**Tracts**][**Connectivity matrix**]. DSI Studio will conduct a spatial normalization to ensure that the built-in parcellation is registered with the subject data. 
4. In the connectivity matrix dialog at [**Parcellation Atlas**] choose the atlas. If you use customized regions, choose `ROIs`. Click [**Recalculate**] to get the connectivity matrices.

Here are some tips for getting a good connectivity matrix:

- Decide whether to include subcortical regions.
- Avoid using a white matter atlas as the parcellation atlas (e.g., HCP842/HCP1065 tractography atlas, JHU white matter atlas)

There are settings in the connectivity matrix dialog:

| Parameters | Description|
|:-----------|:-----------|
| ***Parcellation Atlas*** | DSI Studio provides built-in atlases as the brain parcellation or choose `ROIs` for using customized regions. <br> You can also customize your own parcellation by loading regions from  the [Step T3a][Atlas] button or from a NIFTI file.   |
| ***pass region vs end in region*** | Determine whether the connecting track is counted by `passing` or `ending` in the parcellation region. `ending` is more restrictive because `passing` allows for passing through. |
| ***value*** | The connectivity matrix can be calculated by accounting for the number of tracts that pass two ROIs (select "count" as matrix value). This number can be normalized by the median length (select "ncount" as matrix value) or multiplied by the sum of the inverse of the length (select "ncount2"). <br> The FA, QA, or ADC sampled by the tracks can also be used as the matrix entry (e.g. select "fa", "qa", or "adc" as the matrix value). There is a drop-down list in the connectivity window. One is "end in region", which counts only the final connecting region. Another one is "pass region", which counts all the regions passed. |
| ***threshold*** | The threshold is used to filter out matrix entries with a small number of connecting tracks. It is a ratio to the maximum connecting tracks in the connectivity matrix. |

The calculated matrix can be exported as a mat file or a figure. In the mat file, the connectivity matrix is stored as an n-by-n *connectivity* matrix, whereas the region labels are stored in the *name* matrix. To recover the list of the region labels, use the following command in Matlab

```
labels = textscan(char(name),'%s');
```

Here's the python code for getting the labels:

```python
from scipy.io.matlab import loadmat
m = loadmat("connectivity.mat")
names = "".join(m["name"].view("S4")[``0]).split("\n")
```

## Network measures

Network measures will be calculated and listed in the second tab for further analysis (group comparison, regression...etc.)

![image](https://user-images.githubusercontent.com/275569/147804365-22f3bf97-4d5c-4b97-94af-8ea511aa795f.png)

Graph theoretical analysis views brain connections as a *graph *and applied graph-based measures to analyze it. A graph is defined as a set of nodes or vertices and the edges or lines between them. Its topology can be quantitatively described by a wide variety of measures, including network characteristic path length, clustering coefficient, global efficiency, and local efficiency.

The network measures in DSI Studio follow the implementation of the brain connectivity toolbox (https://sites.google.com/site/bctnet/). The explanation of the network measures can be found [here](https://sites.google.com/site/bctnet/measures/list#TOC-Clustering-and-Community-Structure).  You may also refer to Bullmore, E. and O. Sporns (2009). "Complex brain networks: graph theoretical analysis of structural and functional systems." Nat Rev Neurosci 10(3): 186-198. for details.

For binary measures, the connectivity matrix will be thresholded by a track count in proportional to the max count in the connectivity matrix to make it binary. For example, if the largest count in the connectivity matrix is 100,000 and the threshold is 0.01, then an entry with at least 1,000 track count will be set to 1, and any value fewer will be zeroed.

For weighted measures, the connectivity matrix will be normalized so that the maximum value of the matrix is one.

After getting the network measures in the text file, you may use python scripts to extract information. e.g., <https://github.com/frankyeh/DSI-Studio/issues/61>

## Graph Visualization

![image](https://user-images.githubusercontent.com/275569/147804418-4d8920b8-521f-4fcf-ab48-3d3eff73bbfb.png)

DSI Studio provides a 3D graph presentation to visualize the graph structure. There are two ways to present this graph

**Approach1: using MAT file generated from DSI Studio**

1. Save the connectivity matrix from the connectivity dialog using the "save matrix" button. The output will be a MATLAB mat file. 

2. Back to the fiber tracking window, on the top menu, use [View][Visualize Graph] to load the connectivity matrix mat file.

**Approach2: using customized MAT file or Text file with user-defined regions**

1. In [step T3 Fiber tracking], load regions using [Region][Open Region]. If your matrix is 100 by 100, then there should be 100 regions.

2. Prepare connectivity matrix in a text file (space-separated or tab-separated values) or a mat file (use -v4 to save the mat file, and the matrix should be named "connectivity")

3. Load the matrix using [View][Visualize Graph]

*TIP: You can assign positive and negative values to the matrix, DSI Studio will show them in red and blue colors.*

*TIP: There are rendering options for graph under [Step T3c: Option][Region Rendering]*

## Plot connectogram

To create the connectogram, in the connectivity dialog, click on the button named "save connectogram" and save a text file. Open the website: http://mkweb.bcgsc.ca/tableviewer/visualize/. In "2A. UPLOAD YOUR FILE", choose the text file and check  "col with row size" and "row with col size" (see below). Click on visualize table to get the connectogram figure.

![image](https://user-images.githubusercontent.com/275569/147804446-3f65b9f7-d1a6-47b1-a165-904948d45da9.png)

![](http://circos.ca/tutorials/lessons/recipes/cortical_maps/img/02.png)

