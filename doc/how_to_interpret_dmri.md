# Diffusion metrics provided by DSI Studio

Diffusion MRI (dMRI) acquires **diffusion-sensitive** MRI data to reveal the microscopic change of biological tissues structure. The dMRI data are processed to provide diffusion metrics, such as diffusivities and anisotropy to summarize the diffusion pattern. Studies have shown that diffusion metrics could reveal neuronal change, cellularity, tissue edema.

The diffusion metrics can be categorized into model-based metrics and model-free metrics. The model-based metrics are calculated by fitting dMRI signals with a *model*, such as a tensor model, NODDI model, and the model's parameters will provide the metrics. The model-free metrics are directly calculated from the PDF of diffusion distribution (i.e., histogram of diffusion distribution) without assuming a model. 

There are pros and cons concerning each approach. Model-based metrics need fewer diffusion sampling, and the metrics are usually associated with physical or biological references. The downside is the violation of models, which is common in biological studies. Model-free metrics need more diffusion samplings, and the association between metrics and biological findings requires tissue validation. The advantage is that it does not have a model assumption.

The following is a summary of how to interpret results from diffusion metrics.


|  | Metrics | Model/Method |  Interpretation |  Changes |  Explanation |
|----|-----|-------|-----------------|----------|-------------|
| **FA** | fractional anisotropy |Diffusion tensor | FA is nonspecifically associated with axonal integrity |  decreases: demyelination (Chang, 2017), inflammation, edema, axonal loss | Fractional anisotropy (FA) is a measure of the degree of anisotropy, or directional dependence, of a diffusion process in a biological tissue. It is a scalar value between 0 and 1 that indicates how much the diffusion of water molecules in a tissue deviates from being isotropic, or evenly distributed, in all directions. A value of 0 indicates completely isotropic diffusion, while a value of 1 indicates completely anisotropic diffusion, where the diffusion is restricted to a single direction. FA is commonly used in medical imaging, particularly in diffusion tensor imaging (DTI), to measure the microstructural properties of tissues in the body, such as the white matter tracts in the brain or the tendons and ligaments in the musculoskeletal system. It is often used as a marker for the structural integrity of tissue and can be used to detect abnormalities or changes in tissue structure in various diseases or conditions. |
| **AD** | axial diffusivity* | Diffusion tensor | AD is nonspecifically associated with axonal density | decreases: axonal loss (Budde et al., 2007; Song et al., 2003) | Axial diffusivity is a measure of the rate at which water molecules diffuse along the primary diffusion direction in a tissue. It is one of three diffusivity measures that are commonly derived from diffusion tensor imaging (DTI). Axial diffusivity is a scalar value that is typically represented by the symbol "λ₁" and is expressed in units of square millimeters per second (mm²/s). Axial diffusivity is often used in combination with other diffusivity measures and DTI-derived metrics to study the microstructure and function of various tissues in the body, including the brain, spinal cord, and musculoskeletal system. |
| **RD** | radial diffusivity* | Diffusion tensor | RD is nonspecifically associated with myelination | increases: demyelination (Budde et al., 2007; Song et al., 2002; Song et al., 2005)| Radial diffusivity is a measure of the rate at which water molecules diffuse perpendicular to the primary diffusion direction in a tissue. It is a scalar value that is typically represented by the symbol "λ₂" and is expressed in units of square millimeters per second (mm²/s). It reflects the degree to which diffusion is unrestricted in the tissue perpendicular to the primary diffusion direction and can be used to characterize the structural properties of the tissue. For example, a high radial diffusivity may indicate a lack of structural organization or coherence in a tissue, while a low radial diffusivity may indicate a highly organized or densely packed tissue structure. Radial diffusivity is often used in combination with other diffusivity measures and DTI-derived metrics to study the microstructure and function of various tissues in the body, including the brain, spinal cord, and musculoskeletal system. |
| **MD** | mean diffusivity* | Diffusion tensor | MD is associated with edema and cell infiltration | increases: vesogenic edema, decreases: cytotoxic edema | Mean diffusivity is a scalar value that is typically represented by the symbol "λ̄" and is expressed in units of square millimeters per second (mm²/s). It is calculated as the average of the three eigenvalues of the diffusion tensor, which represents the degree of diffusional restriction in three orthogonal directions in a tissue. Mean diffusivity reflects the overall degree of diffusional freedom in the tissue and can be used to characterize the structural properties of the tissue. |
| **QA** | quantitative anisotropy | Q-space imaging | QA is associated with axonal density | decreases: axonal loss (Yeh 2019)(Shen 2015) | Quantitative anisotropy (QA) is a measure of anisotropy of a diffusion process in a biological tissue. It is less affected to edema, while FA and AD are also sensitive to edema (Yeh, 2013). | 
| **ISO** | isotropy | Q-space imaging | ISO is associated with edema | increases: edema | ISO is a measure of isotropic diffusion (Yeh. 2010). It represents background isotropic diffusion contributed from CSF or edema, including both restricted and non-restricted isotropic diffusion. |
| **RDI** | restricted diffusion imaging | Q-space imaging | RDI is associated with cell infiltration during inflammation (Yeh 2017)(Yeh 2021)| increases: cell infiltrations (inflammation or tumor infiltration) | RDI quantified the total amount of restricted diffusion regardless of the orientation (Yeh 2017)| 
| **NRDI** | none-restricted diffusion imaging | Q-space imaging | NRDI is associated with edema (Yeh 2017)| increases: edema due to inflammation | NRDI quantified the total amount of none-restricted diffusion regardless of the orientation (Yeh 2017)| 
| **QIR** (newly introduced, 2025) | QA-ISO ratio (qa/iso) | Q-space imaging | QIR is a measure of axonal integrity | reduced QIR may suggest inflamation or demelination | QIR is derived from QA and ISO and is more comparable across different scans because the computation removed the effect of B1 inhomogniety | 
| **VOL** (newly introduced, 2025) | template-space volume | Nonlinear spatial normalization | Vol is a volumetric measure | reduced vol suggest atrophy of the tissue | Vol reflect the relective size in the template space. | 

*The diffusivity (either RD, MD, or AD) calculated in DSI Studio has a unit of 10^-3 mm^2/s.

## Change of metrics in neurological disorders

↓: decrease ↑: increase -: no change

|Condition | Example | FA | AD | RD | MD | QA | ISO | RDI | NRDI | 
|----------|---------|----|----|----|----|----|----|----|----|
| Acute axonal injury with inflammation | stroke ( < 3 month), TBI (< 3 month), MS relapse, tumor mass effect | ↓ | ↑ or - | ↑ | ↑ | - | ↑ | ↑ (at locations with cell infiltrations) | ↑ (at edema location) |
| Axonal loss without inflammation | ALS, Huntingtons Diseases, TBI (> 6 month), stroke ( > 6 month) | ↓ | ↓ | ↑ | - | ↓ | - | - | - |



## References
1. Budde MD, Xie M, Cross AH, Song SK. Axial diffusivity is the primary correlate of axonal injury in the experimental autoimmune encephalomyelitis spinal cord: a quantitative pixelwise analysis. J Neurosci. 2009;29(9):2805-13.
2. Budde MD, Kim JH, Liang HF, Schmidt RE, Russell JH, Cross AH, et al. Toward accurate diagnosis of white matter pathology using diffusion tensor imaging. Magn Reson Med. 2007;57(4):688-95.
3. Sun SW, Liang HF, Trinkaus K, Cross AH, Armstrong RC, Song SK. Noninvasive detection of cuprizone induced axonal damage and demyelination in the mouse corpus callosum. Magn Reson Med. 2006;55(2):302-8.
4. Song SK, Yoshino J, Le TQ, Lin SJ, Sun SW, Cross AH, et al. Demyelination increases radial diffusivity in corpus callosum of mouse brain. Neuroimage. 2005;26(1):132-40.
5. Song SK, Sun SW, Ju WK, Lin SJ, Cross AH, Neufeld AH. Diffusion tensor imaging detects and differentiates axon and myelin degeneration in mouse optic nerve after retinal ischemia. Neuroimage. 2003;20(3):1714-22.
6. Song SK, Sun SW, Ramsbottom MJ, Chang C, Russell J, Cross AH. Dysmyelination revealed through MRI as increased radial (but unchanged axial) diffusion of water. Neuroimage. 2002;17(3):1429-36.
7.  Kono K, Inoue Y, Nakayama K, Shakudo M, Morino M, Ohata K, et al. The role of diffusion-weighted imaging in patients with brain tumors. AJNR Am J Neuroradiol. 2001;22(6):1081-8.
8.  Gauvain KM, McKinstry RC, Mukherjee P, Perry A, Neil JJ, Kaufman BA, et al. Evaluating pediatric brain tumor cellularity with diffusion-tensor imaging. AJR Am J Roentgenol. 2001;177(2):449-54.
9. Sugahara T, Korogi Y, Kochi M, Ikushima I, Shigematu Y, Hirai T, et al. Usefulness of diffusion-weighted MRI with echo-planar technique in the evaluation of cellularity in gliomas. J Magn Reson Imaging. 1999;9(1):53-60.
10. Chang EH, Argyelan M, Aggarwal M, Chandon TS, Karlsgodt KH, Mori S, et al. The role of myelination in measures of white matter integrity: Combination of diffusion tensor imaging and two-photon microscopy of CLARITY intact brains. Neuroimage. 2017;147:253-61.
11. Yeh FC, Irimia A, Bastos DCA, Golby AJ. Tractography methods and findings in brain tumors and traumatic brain injury. Neuroimage. 2021;245:118651.
12. Yeh FC, Zaydan IM, Suski VR, Lacomis D, Richardson RM, Maroon JC, et al. Differential tractography as a track-based biomarker for neuronal injury. Neuroimage. 2019;202:116131.
13. Garic D, Yeh FC, Graziano P, Dick AS. In vivo restricted diffusion imaging (RDI) is sensitive to differences in axonal density in typical children and adults. Brain Struct Funct. 2021;226(8):2689-705.
14. Yeh FC, Liu L, Hitchens TK, Wu YL. Mapping immune cell infiltration using restricted diffusion MRI. Magn Reson Med. 2017;77(2):603-12.
15. Shen CY, Tyan YS, Kuo LW, Wu CW, Weng JC. Quantitative Evaluation of Rabbit Brain Injury after Cerebral Hemisphere Radiation Exposure Using Generalized q-Sampling Imaging. PLoS One. 2015;10(7):e0133001.


# Difference between model-based and model-free

Here we focus on the differences between DTI and GQI, although other model-based and model-free methods exist.

<img src="https://user-images.githubusercontent.com/275569/156826203-e8c4ece1-1b45-4193-8257-aa8e64dfe57c.png" width="400">

<img src="https://user-images.githubusercontent.com/275569/156826048-40933f38-339d-442e-ab5d-2fa5d4fbb999.png" width="400">

The DTI model does not consider restricted diffusion and thus FA, AD, MD are sensitive to a wide range of biological changes including edema, inflammation, or just a superimposing crossing fiber (see  Yeh F-C et al. PLoS ONE 8(11): e80713.2013). More advanced methods aim to provide more specific metrics. GQI separates isotropic diffusion and anisotropic diffusion, thus minimizing the partial volume of free water. The anisotropic diffusion is further quantified per each principal diffusion direction, thus minimizing the partial volume of crossing fibers. A neurosurgery study has shown that QA is robust against peritumoral edema and contributes to more reliable tractography (Zhang, et al. Neurosurgery, 73(6), 1044-1053. 2013). A phantom study has shown that QA is more robust to the free water effect and partial volume of crossing fibers. (Yeh, PloS one 8.11, 2013). 

