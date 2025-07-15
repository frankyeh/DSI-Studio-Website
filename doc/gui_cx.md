# Correlational Tractography

<iframe width="560" height="315" src="https://www.youtube.com/embed/qC8jx6XZHGI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
# Introduction

[*Correlational tractography (Yeh 2021)*](https://pubmed.ncbi.nlm.nih.gov/34673247/) tracks the tract segments that have anisotropy correlated with a study variable in the group studies. In detail, DSI Studio will first compute the partial correlation between diffusion metrics of interest (e.g. qa or fa) and the covariates (e.g. sex and age) to eliminate their linear effect. The corrected diffusion metrics will be correlated with the study variable (e.g. if studying aging, then the study variable is the age) using nonparametric, rank-based, Spearman correlation. The T-statistics of the correlation will be computed across each voxel of the brain. Then, deterministic fiber tracking (Yeh, 2013) will be applied using a T threshold of 2 and an anisotropy threshold to map correlational tractography. As a result, correlational tractography shows the location where a pathways has diffusion metrics substantially correlated with the study variable. 

[Connectometry (Yeh 2016)](https://pubmed.ncbi.nlm.nih.gov/26499808/)[(pdf)](/ref/Connectometry.pdf) is the statical method that uses a permutation test to get statistical inference for correlational tractography. Specifically, the analysis applied 4000 random permutations on the subject's demographics (including covariates and study variables). Each permutation round will undergo the identical pipeline of correlation tractography to derive the length distribution of correlational tracks. This approach is similar to the cluster analysis of fMRI, where a permutation test can be applied to examine the size of the clusters. The random permutation will generate a length distribution of correlational tractography, corresponding to the null hypothesis. On the other hand, the length distribution of non-permuted conditions will be derived by applying bootstrap resampling without permutation. The two distributions, namely the permuted distribution and the non-permuted distribution will be compared by computing their area under the curve, given a length threshold. A larger length threshold usually results in a smaller FDR. The users can choose to report FDR given a length threshold of 40 mm, or report significant findings given a FDR threshold of 0.05.


This documentation introduces the steps to create a connectometry database for generating correlational tractography and testing its significance. 

# Step C1: Create a connectometry database

A connectometry database aggregates multiple FIB files into a common template space that allows group-level analysis. 

You will need FIB files generated from [Step T2: Reconstruction](https://dsi-studio.labsolver.org/doc/gui_t2.html) or MNI-space NIFTI files.

The FIB file can be reconstructed using [Step T2b(1)][QSDR](**recommended**) or [Step T2b(1)][GQI] with the default setting. (1.25 sample length ratio for in-vivo and 0.6 for ex-vivo).

**After the 2022 June versions, DSI Studio can use all kinds of FIB files (not limited to QSDR FIB with ODF) to construct a connectometry database.

Click on [Correlational Tractography][Step C1: Create a Connectometry Database] to bring up the following database dialog.

![image](https://user-images.githubusercontent.com/275569/185491410-1aa4a295-748d-4a04-ad1a-b1f23ebdd98b.png)

## Step C1a: Open FIB or NIFTI files

Use the [Search in Directory] or [Add] button to load FIB files or MNI-space NIFTI files.

You may need to make sure that the file orders match that of your demographic records. Use the "sort" function to sort the files.

If you are going to study the longitudinal change, make sure that you place baseline and follow-up scans of the same subject together (e.g. subject1_scan1, subject1_sca2, subject2_scan1, subject2_scan2 ...etc.).

## Step C1b: Select analysis metric and template

The FIB file contains many diffusion metrics. The default choice is "QA" known as quantitative anisotropy. You can also study other diffusion measures such as FA, AD, RD, MD, RDI, and NRDI. Each metric will need its dedicated connectometry database file. To know more about their differences, refer to [here](\doc\how_to_interpret_dmri.html)

If you use QSDR FIB files, DSI Studio will assign the corresponding template. If you use a GQI FIB file, you may need to select the correct template.

***IMPORTANT***
The anisotropy values in **ex-vivo** tissue are substantially affected by the fixation duration

<img src="https://user-images.githubusercontent.com/275569/186562036-63224440-a77e-4931-8b78-7d99e2cb53c4.png" width=400>
source: Allan Johnson G, Wang N, Anderson RJ, Chen M, Cofer GP, Gee JC, Pratson F, Tustison N, White LE. Whole mouse brain connectomics. Journal of Comparative Neurology. 2019 Sep 1;527(13):2146-57.

## Step C1c: Create connecometry database

Confirm the output file name and make sure to use the file extension db.fib.gz

Click the "Create Database" button to create the connectometry database as a db.fib.gz file. You can add or remove subjects from the database using [Diffusion MRI Connectometry][Edit Connectometry Database]

At the End of this step, you will have a connectometry database, which is a file name with the extension db.fib.gz

After creating the database file, you can check the alignment of the database or add/remove subjects to/from the database using [**Step C2a**:**Modify a Connecometry Database**]. 
***TIP***
You can store demographic values with the connectometry DB file:
1. Open the .db.fib.gz in [Step C2: View/Modify a Connectometry Database]. 
2. Load the demographic using [Files][Open Demographics]
3. Overwrite the .db.fib.gz file by [File][Save DB as...]

# Step C2: Compute longitudinal change (skip this step in cross-sectional studies)

Skip this step if your study is a cross-sectional study (has no repeat scans of the same subjects).

1. In the main window, click [**Step C2a**:**Modify a Connecometry Database**] and select the database.

Make sure that the baseline and follow-up scans of the same subject are stored consecutively in the database (e.g. subject1_scan1, subject1_sca2, subject2_scan1, subject2_scan2 ...etc.). See the following example:

![image](https://user-images.githubusercontent.com/275569/197028837-454ebb39-ca9c-4fa0-96de-0d32c5074083.png)

2. In the top menu, select [Tools][Longitudinal scans...] to bring up the subject matching window:

![image](https://github.com/frankyeh/DSI-Studio-Website/assets/275569/ddd1fd56-cbc0-40c8-9658-a5feb3ee6cc9)

Check to make sure the scans of the same subject are matched. You may also use a text file to match scans. The text file should be the consecutive numbers of the matching pairs. The first number will be used as the baseline, and the second number will be the study scans. For example, a text file with "0 1 2 3 4 5" will match scan 0 (base) and scan 1(study), scan 2 and scan 3, scan 4 and scan 5. Please note that the first scan in the database is labeled by 0, not 1.

The "metric" group box allows you to define how the difference is calculated. "scan2-scan1" calculates the difference (recommended), whereas "(scan2-scan1)/scan1" calculates the difference as a percentage of the first scan.

The filter settings allow you to study increased metrics or decreased metrics. The default is no filter applied, and the connectometry database will include both positive and negative change. This unfilter db can be used to test whether there are longitudinal changes in the later step. If there exists significant longitudinal change, a filtered version database can be created to investigate either the positive change or negative change. For most brain diseases, we [Study decreased value in the follow-up (scan2)].

Click "Ok" to calculate the difference between scans and go back to the database window.

*Use [File][Save DB as..]  to save the differences as a "modified connectometry db".*

The new connectometry db will contain only the difference between the paired scans for further analysis.

# Step C3: Group Connectometry

Open the db.fib.gz file in [**Step C3: Group Connectometry Analysis**] in the main window.

If a dialog prompts a warning about poor image quality (low R2), you may need to remove problematic data. This can be done by (1) opening the connectometry db.fib.gz file using [**Step C2a**:**Modify a Connecometry Database**] provided in the main window and manually identifying and deleting subjects with a registration problem or motion artifact. 

![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1487609216505/Manual/diffusion-mri-connectometry/cnt_gui.png?height=236&width=400)

## Step C3a: Load demographics

The demographic file can be space-separated, tab-separated (.tsv), or comma-separated values (.csv)(recommended). The file should record any variables you would like to include in the regression model. The first row should be the name of the scalar values. The second row is the value for the first subject, and the rest follows. If there is any missing data, leave the field empty. DSI Studio will ignore subjects with missing data. You can supply only a subset of demographics or a demographics that contains additional subject information (available after the 8/21/2023 version). DSI Studio will match the first row of the demographic with the subject name in the database.

Example of a demographic file in CSV format:
```
ID,AGE,SEX
SUB01,23,0
SUB02,31,1
SUB03,24,0
SUB04,36,0
SUB05,35,1
SUB06,27,1
```

For the longitudinal study that investigates the *change* of behavior scores, you should calculate the changes by (scores at follow-up)-(scores at baseline). The name of the behavior score should add "increased" to the front. See the following example:

Example of a demographic file in CSV format:
```
ID,AGE,SEX,increased_score
SUB01,23,0,13
SUB02,31,1,-5
SUB03,24,0,3
```
 

Click on the button labeled "open subjects demographics". Load the text file that records all the scalar values that will be included in the regression model. 

## Step C3b: Select covariates

Choose the variables to be considered such as sex and age. DSI Studio will use linear regression to eliminate the effect of covariates from the diffusion measures.

There are several tips for choosing the variables in the model:

***TIPS***
If you have multiple study variables, including too many of them in the model may lead to over-fitting unless you have enough sample size. Otherwise, include basic variables like sex and age with one study variable at a time.

If your study variables are highly correlated to each other, consider using a PCA to get the principal components. (see <https://www.sciencedirect.com/science/article/pii/S1875957218301797>). You will need to interpret the meaning of each principal component by checking its coefficient.

## Step C3c: Study variable

Choose the target variable to study, and DSI Studio will find any tracks with diffusion metrics correlated with this variable. The effect of other covariates selected in C3b will be regressed out.

If your data are from a longitudinal study and want to look at longitudinal differences after considering variables selected in the previous step, choose "Longitudinal change" as the study variable.
If your data are from a longitudinal study including controls/patients and want to compare longitudinal differences between controls and patients, after considering variables selected in the previous step, choose the group id of control/patient as the study variable.


## Step C3d: Parameters

| Parameters | Descriptions |
|:-----------|:-------------|
| FDR Control | If FDR control is checked, DSI Studio will output ONLY tracks with a significant correlation (i.e., FDR lower than the threshold). <br> If unchecked, DSI Studio will report the FDR value as the significance level to examine the hypothesis <br> My experience is that an FDR of 0.05 is highly confirmatory while 0.05~0.2 suggests a high possibility of positive findings. FDR is very different from the p-value. It is unreliable if the subject number is low (e.g., less than 10). Any findings with FDR lower than 0.2 may be worth reporting since the results have much more "true positive" findings than "false positive" findings. FDR is not sensitive to sample size. This is its strength (unlike p-value constantly improves with a large sample size) and weakness (The result from a small sample size may not be reliable even if the FDR is small). |
| Length Threshold | Different length thresholds correspond to different null hypotheses, and a longer length is usually more specific (less sensitive) and tends to achieve a lower FDR (with fewer findings). <br> The length threshold can be increased to achieve a better FDR (not always). |
| T threshold | Higher values will map tracks with a more substantial correlation effect, whereas lower values for a weak correlation. <br> I would recommend running separate analyses with different T-thresholds (e.g., t=2, 2.5, and 3) to map different levels of correlation. Each threshold is viewed as a different hypothesis and will receive an FDR.|

**Study region (optional)**

Adding regions as constraints will also need to increase the permutation count (e.g., 8000) to avoid the discrete error.

The default study region for connectometry is "whole brain," which means that connectometry will look at the whole-brain region. You can choose to use an ROI in the MNI space by loading a NIFTI file of the mask. You can also choose a region from the atlas. The region type can be ROI (select tracts passing the region), ROA (discard any track passing the region), End (select track ending in the region), or Seed (Only start the seeds from the region). "Whole brain" is most suitable for the exploratory purpose. If you have a specific region of interest, limiting the computation to an ROI can give a more specific result (e.g., assign cerebral peduncle as the ROI can investigate whether CST is affected). Using only one ROI is suitable for most cases unless for a specific need in the tracking routine.

After assigning the regions, you may need to increase the seed count because the region setting will remove part or most of the findings and leave only sparse results. The best setting can be case-dependent. If the count is too low, it can risk no findings or get very sparse meaningless findings. On the other hand, a high seed count can flood findings with less significant results. There is a sweet range, and in most cases, the range is quite extensive, as long as the value is not too low, the value does not make much of a difference.

You can exclude cerebellum regions by adding cerebellum white matter and cortex as terminative regions:

![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1573051180893/Manual/diffusion-mri-connectometry/no%20cerebellum%20on%20connectometry.png)

**Select cohort (optional)**

You can include/exclude a particular group of subjects in/from the analysis. Click on the [Select Cohort] button and select criteria to [Apply]. The criteria will be listed on the [Select If] line edit. You can use the [Check] button to visualize the results.

**Advanced options**

| Parameters | Descriptions |
|:-----------|:-------------|
| Permutation count |  determines the total number of permutations applied to the FDR analysis. Higher values give a smoother and more accurate estimation of FDR curves, but it requires more computation time |
| Pruning | helps remove noisy findings. Higher values may improve FDR with the expense of sensitivity. |

## Step C3e: Run connectometry and report the results

Click on the [**Step C3d: Run connectometry**] button and wait until the computation is finished.

After the analysis, DSI Studio will output several files to report the results. You may also click on [Step C3e: View Results in 3D] button to view the tracks with a substantial difference.

**file outputs**

Results for T-statistics are stored in a FIB file:

| Output Files | Descriptions |
|:------------|:-------------|
| bmi.t_statistics.fz | A FIB file storing the t-statistics can be opened in [Step T3:fiber tracking] to visualize T-statistics with tracks. The T statistics for increased metrics is stored as "inc_t", whereas decreased metrics is stored as "dec_t" |
| bmi.length20.fdr.jpg | The FDR with respect to tracking length. |
| bmi.length20.fdr_dist.values.txt | the FDR values with respect to length. |
| *.report.html | HTML file reporting the connectometry results |

Results for increased metrics associated with the study variable will be saved with "inc" (for decreased metrics, "dec"):

| Output Files | Descriptions |
|:------------|:-------------|
| bmi.t2.inc.dist.jpg | shows the histogram of track length.|
| bmi.t2.inc.tt.gz | tractography file stores tracks that are increased metrics correlated with the study variable. It can be opened with the t-statistics FIB file |
| bmi.t2.inc.jpg | figure showing the tracks in four different views. |


***TIPS***

1. To change how DSI Studio visualizes tracks, open any FIB file in [Step T3 Fiber Tracking] and change [Step T3(c) Option]. After closing the fiber tracking window, DSI Studio will memorize the setting and apply it.

2. There are several ways to improve the FDR, with the expense of losing sensitivity. You may increase the length threshold (e.g. 30 mm), increase the T threshold, and increase pruning iterations in the advanced options.


***Troubleshooting***

1. If you have very different FDR results at different permutation counts, increase the permutation count until the FDR converges. 
2. If the finding only shows very short tracks, try lowing the T threshold 
3. If the finding only contains very few tracks, please increase the permutation count in the advance option (may need more computation time).
4. If the finding has many short fragments, consider increasing the length threshold or pruning count.

## Report Connectometry Results

Please note that correlation does not necessarily imply causality. The results can be the "cause" or the "response" of the study variable.

The following are the strategies to report results.

1. Report FDR at different T-scores gives an overview of findings from high sensitivity (low T) to high specificity (high T) 

You can visualize the results at different T thresholds (see the next section about visualization). The FDR value will be different for different track lengths. An FDR < 0.05 indicates a highly confirmative finding. An FDR > 0.2 suggests that there are substantial amounts of false findings. The result is thus inconclusive and not reliable. An FDR between 0.05 and 0.2 covers a wide spectrum of reliability. 

2. Report FDR at different subject subgroups (e.g., male group versus female group)

3. Convert the finding to ROI using [Tracts][Tract to ROI] to track the entire pathways. This allows you to confirm the anatomical structure of the findings.

# Post Connectometry Analyses

There are additional analyses that could be done after connectometry to show more details about the findings. The following are some recommendations.

## 1. Segment findings into bundles

The connectometry results often include multiple pathways, and they can be segmented into different bundles manually [(video)](https://www.youtube.com/watch?v=1xfhaFQhCtY&t=13s) or through [Tract Misc][Recognize and Cluster].

I would recommend checking [Tract Misc][Recognize and Cluster] and fine-tuning it using manual segmentation.

## 2. Report FDR for each bundle

The FDR in report.html is for the overall results, but findings with a longer distance will have a lower FDR than the others. You can report the FDR of each bundle based on its length.

To do this, first, open the 3D interface at Step C4f or open *.t_statistics.fib.gz in Step T3 and load the inc.tt.gz or dec.tt.gz files.

Segment findings into bundles as mentioned above. Get the length using [Tracts][Statistics]. If the length is 60mm, then the voxel distance is 30.

Last, check the FDR values in *.fdr_dist.values.txt and look up the row with voxel distance=30 to get the FDR values. There is one FDR for increased metrics and one for decrease. Make sure you get the right one.

## 3. Scatter plots of bundle statistics and study variable

We can further plot a scatter plot between the study variable and the diffusion metrics (e.g. QA or the metrics stored in the .db.fib.gz file) averaged from the bundle.

To do this, open the connectometry database (*.db.fib.gz) in [Step T3: Fiber Tracking] and load the tracks from tt.gz (i.e. tracks associated with the study variable) using [Tracts][Open Tracts]. Then use [Tracts][Statistics] to get along track metrics for each subject in the connectometry database. The metric (here, use QA as an example) has raw QA values (used if you check [normalize QA] in the advance option). This step will take a while.

The QA value can be copied to the clipboard and pasted into an Excel sheet. We can place them with the demographics values and plot a scatter plot with other study variables.

![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1556206471892/Manual/diffusion-mri-connectometry/scatter%20plot.png)

## 4. Visualize bundle in mosaic T1w

The finding can be visualized in slices using the following steps:

1. Switch the slice image to "T1w".
2. In the main menu, add tracks to ROI by [Tracks][Tracks to ROI]
3. In the Options window (right upper corner), under the "Region Window" node. Change the slice layout to Mosaic 2
4. Change the contrast of the slices using the slider on the top of the region window to enhance the track's regions.

[![](https://sites.google.com/a/labsolver.org/dsi-studio/_/rsrc/1468760876407/Manual/diffusion-mri-connectometry/sleep4.jpg)](https://sites.google.com/a/labsolver.org/dsi-studio/Manual/diffusion-mri-connectometry/sleep4.jpg?attredirects=0)


## 5. Get SDF values along the track

Once you get tracks that show significant differences or correlations. You can open the connectometry db at [STEP 3 Fiber tracking] and load the resulting tracks. Click on track statistics and you will get SDF values along the tracks for "each subject". This allows you to plot the difference between the groups.

