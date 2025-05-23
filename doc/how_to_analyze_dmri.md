# How to analyze dMRI

<img src="https://github.com/user-attachments/assets/9afac513-ccbd-419c-9d79-4b7dd9458294" width=800/>

The following are common preprocessing steps for almost all analysis in DSI Studio.

1. [Create SRC files (.sz) from diffusion data (DICOM files or NIFTI)](/doc/gui_t1.html).

2. [SRC file quality control](/doc/gui_t1.html#step-t1a-quality-control-optional)(/doc/gui_t1.html#batch-quality-control) to exclude problematic data.

3. [Reconstruct FIB files (.fz) from SRC files](/doc/gui_t2.html) using GQI (native space) or QSDR(template space) to get a FIB file.


## Region-Based Analysis

<img src="https://user-images.githubusercontent.com/275569/147855916-ccd9a41d-fbfa-4011-9df9-92c4213e2fa3.png" width=800/>

The simplest way to analyze diffusion data is to assign a region in the brain (either manual drawing or from an atlas) and get statistics on its diffusion indices. The following are the steps for ROI-based analysis in the subject space.

1. Open the FIB file in [Step T3 Fiber Tracking]. Check out [how to assign regions](/doc/gui_t3_roi_tracking.html#Load-Regions-From-Built-In-Atlases). Loading the region from built-in atlases will be the most efficient.

2. If you would like to analyze other measures (e.g. DKI, NODDI, or PET), and insert their NIFTI files by [Slices][Insert T1W/T2W images]. DSI Studio will register newly added images to the diffusion data set. The added metrics will be analyzed in the following steps.

3. Use [Region][Statistics] to get region statistics. Click on the "show details" button to see the results. You may load the results in Excel to do further analysis. Each subject should have a diffusion index value calculated from a region.

  Another way to do region-based analysis is to reconstruct/normalize all data in the MNI space and extract region statistics from all subjects in one shot. Here are the steps

1. [Construct a connectometry database](/doc/gui_cx.html) from all subjects.

2. Open the database file in Step T3, and follow the above-mentioned steps 3 to 6. Please note that this db.fib.gz file is already in the MNI space, and the regions should be MNI regions.

## Tractometry

Tractometry is a term that refers to the study and analysis of white matter tracts in the brain. White matter tracts are made up of axons (long, thin fibers) that connect different areas of the brain and facilitate communication between neurons. Tractometry involves using techniques such as diffusion tensor imaging (DTI) to visualize and quantify these white matter tracts in the brain. This can be used to study the structure and function of the brain, as well as to identify abnormalities or changes in the white matter tracts that may be associated with various neurological conditions.

Example studies: <https://www.nature.com/articles/nn.3870>

The tractography-based analysis, a.k.a. tractometry, uses diffusion fiber tracking (manual or automatic) to define the trajectories of fiber pathways and extract tract-associated metrics. The advantage of tractography-based analysis is that it is fiber specific, whereas region-based analysis does not specify which fiber pathways to be analyzed. The following list is the steps for tractography-based analysis.

1. Fiber tracking: I would suggest using [automated fiber tracking](/doc/gui_t3_atk.html) to target your tracts of interest. Alternatively, you may need to use the conventional [region-based fiber tracking](/doc/gui_t3_roi_tracking.html) to obtain the exact fiber pathways you are interested in.

2. If you would like to analyze other measures (e.g. DKI, NODDI, or PET), and insert their NIFTI files by [Slices][Insert T1W/T2W images]. DSI Studio will register newly added images to the diffusion data set. The added metrics will be analyzed in the following steps.

3. Use [Tracts][Statistics] to get the average diffusion indices along the fiber pathways. Click on "show details" to get information and copy it to Excel for further analysis.

4. Alternatively, use [track profile](/doc/gui_t3_atk.html#Tract-Profile) to get the distributed pattern of a diffusion index along the fiber pathways.

Another way to do region-based analysis is to reconstruct/normalize all data in the MNI space and extract tract statistics from all subjects in one shot. Here are the steps

1. [Construct a connectometry database](/doc/gui_cx.html) from all subjects.

2. Open the database file in Step T3, and follow the above-mentioned steps 3 to 5. Please note that this db.fib.gz file is already in the MNI space, and the regions should be MNI regions.

## Differential Tractography

Example study: <https://pubmed.ncbi.nlm.nih.gov/31472253/>

Differential tractography is a new type of tractography that compares repeat scans of the same individuals to capture neuronal injury reflected by a decrease of anisotropy.

The following is the steps

1. Follow the steps at [Differential tractography for longitudinal data](http://dsi-studio.labsolver.org/Manual/differential-tractography)

*Individual connectometry* aims to find the difference between the longitudinal study of a single subject. The method tracks the deviate pathways of an individual by comparing the subject with a normal population, an atlas, or the subject's previous scan. It is a powerful tool to map the tracks that are damaged. The steps to get the group or individual connectometry is described in the following documentation:

## Correlational Tractography

Example studies: ([1](https://academic.oup.com/brain/article-abstract/143/8/2532/5875734))[(](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C39&q=Rahmani+connectometry&btnG=)[2](https://www.sciencedirect.com/science/article/pii/S2213158220301571#b0275))([3](https://www.sciencedirect.com/science/article/pii/S1875957218301797))

Correlational tractography is a tractography modality that shows pathways correlated with a study variable (e.g. age). The method to derive correlational tractography and test its reliability is called "group connectometry." There are two types of connectometry analyses are available in DSI Studio: group connectometry and individual connectometry.

*Group connectometry* aims to find trajectories associated with a study variable in a group study. The method allows you to identify the exact segment/subcomponent/branches of tracks that are correlated to group differences or any study variable (age, sex, ...etc).

The following are the steps for doing connectometry analysis.

For group connectometry see:

1. See [Create a connectometry database](/doc/gui_cx.html) to create a connectometry database

2. [Run group connectometry analysis](/doc/gui_cx.html)

## Tract-to-region (T2R) connectome

Example studies: [Yeh, Fang-Cheng. "Population-based tract-to-region connectome of the human brain and its hierarchical topology." Nature communications 13.1 (2022): 4933.](https://www.nature.com/articles/s41467-022-32595-4)

The tract-to-region (T2R) connectome maps the probability that specific white matter tracts innervate particular cortical regions, providing a detailed anatomical linkage between tracts and regions. This contrasts with the traditional region-to-region (R2R) connectome, which outlines connections between cortical areas without specifying the underlying white matter pathways. 

The following list is the steps for generating a connectivity matrix.

1. Map a list of fiber bundles using [AutoTrack](https://dsi-studio.labsolver.org/doc/gui_t3_atk.html)
2. Choose a brain parcellation using the [Step T3a][Atlas] button to load brain parcellation (e.g. HCP-MMP or Broadman)
3. At top menu, [Region][Tract-to-Region Connectome] to get the n-by-m T2R matrix.
4. The T2R connectome can be visualized by coloring the parcellation region with T2R values. See instruction [here](https://dsi-studio.labsolver.org/doc/gui_t3_atk.html).
