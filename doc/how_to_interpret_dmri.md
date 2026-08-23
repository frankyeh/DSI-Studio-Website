# Diffusion metrics provided by DSI Studio

Diffusion MRI (dMRI) acquires **diffusion-sensitive** MRI data to characterize microscopic tissue structure. The data can be processed to provide diffusion metrics, including diffusivities and anisotropy measures that summarize diffusion patterns. These metrics can reveal changes associated with neuronal injury, cellularity, and tissue edema.

Diffusion metrics can be broadly categorized as model-based or model-free. Model-based metrics are calculated by fitting dMRI signals with a model, such as the diffusion tensor or NODDI, and interpreting the fitted model parameters. Model-free metrics are calculated directly from the diffusion distribution without assuming a specific tissue model.

Each approach has advantages and limitations. Model-based methods generally require fewer diffusion samples and often provide parameters with familiar physical interpretations, but their results depend on how well the model assumptions fit the tissue. Model-free methods generally require more diffusion samples, and biological interpretation still requires tissue validation, but they avoid imposing a specific tissue model.

The following table summarizes commonly used diffusion metrics and their interpretation.

|  | Metrics | Model/Method | Interpretation | Changes | Explanation |
|----|-----|-------|-----------------|----------|-------------|
| **FA** | fractional anisotropy | Diffusion tensor | FA is nonspecifically associated with axonal integrity | decreases: demyelination (Chang, 2017), inflammation, edema, axonal loss | Fractional anisotropy (FA) measures the degree of directional dependence of diffusion. It ranges from 0 for isotropic diffusion to 1 for highly anisotropic diffusion. FA is widely used to characterize white-matter microstructure but is sensitive to multiple biological and geometric factors, including crossing fibers. |
| **AD** | axial diffusivity* | Diffusion tensor | AD is nonspecifically associated with axonal density | decreases: axonal loss (Budde et al., 2007; Song et al., 2003) | Axial diffusivity is the principal diffusivity of the diffusion tensor and is represented by the largest eigenvalue, λ₁. It describes diffusion along the tensor's primary direction. |
| **RD** | radial diffusivity* | Diffusion tensor | RD is nonspecifically associated with myelination | increases: demyelination (Budde et al., 2007; Song et al., 2002; Song et al., 2005) | Radial diffusivity describes diffusion perpendicular to the tensor's primary direction and is defined as the mean of the two smaller eigenvalues: RD = (λ₂ + λ₃)/2. |
| **MD** | mean diffusivity* | Diffusion tensor | MD is associated with edema and cell infiltration | increases: vasogenic edema; decreases: cytotoxic edema | Mean diffusivity is the average of the three diffusion-tensor eigenvalues: MD = (λ₁ + λ₂ + λ₃)/3. It summarizes the overall magnitude of diffusion within a voxel. |
| **QA** | quantitative anisotropy | Q-space imaging | QA is associated with axonal density | decreases: axonal loss (Yeh 2019; Shen 2015) | Quantitative anisotropy (QA) measures anisotropic diffusion associated with each resolved fiber orientation. It is less affected by isotropic diffusion such as edema than FA (Yeh, 2013). |
| **ISO** | isotropy | Q-space imaging | ISO is associated with edema | increases: edema | ISO measures isotropic diffusion (Yeh, 2010). It represents background isotropic diffusion contributed by CSF or edema, including both restricted and non-restricted isotropic diffusion. |
| **RDI** | restricted diffusion imaging | Q-space imaging | RDI is associated with cell infiltration during inflammation (Yeh 2017; Yeh 2021) | increases: cell infiltration associated with inflammation or tumor infiltration | RDI quantifies restricted diffusion regardless of orientation (Yeh, 2017). |
| **NRDI** | non-restricted diffusion imaging | Q-space imaging | NRDI is associated with edema (Yeh 2017) | increases: edema due to inflammation | NRDI quantifies non-restricted diffusion regardless of orientation (Yeh, 2017). |
| **QIR** (introduced in 2025) | QA-to-ISO ratio (QA/ISO) | Q-space imaging | QIR is a measure of axonal integrity | reduced QIR may suggest inflammation or demyelination | QIR is derived from QA and ISO and is intended to improve comparability across scans by reducing the effect of B1 inhomogeneity. |
| **VOL** (introduced in 2025) | template-space volume | Nonlinear spatial normalization | VOL is a volumetric measure | reduced VOL may suggest tissue atrophy | VOL reflects relative tissue size in template space after nonlinear spatial normalization. |

*The diffusivity values (AD, RD, and MD) reported by DSI Studio use units of 10^-3 mm²/s.

## Change of metrics in neurological disorders

↓: decrease; ↑: increase; -: no change

| Condition | Example | FA | AD | RD | MD | QA | ISO | RDI | NRDI |
|----------|---------|----|----|----|----|----|----|----|----|
| Acute axonal injury with inflammation | stroke (< 3 months), TBI (< 3 months), MS relapse, tumor mass effect | ↓ | ↑ or - | ↑ | ↑ | - | ↑ | ↑ (at locations with cell infiltration) | ↑ (at edema locations) |
| Axonal loss without inflammation | ALS, Huntington's disease, TBI (> 6 months), stroke (> 6 months) | ↓ | ↓ | ↑ | - | ↓ | - | - | - |

## References

1. Budde MD, Xie M, Cross AH, Song SK. Axial diffusivity is the primary correlate of axonal injury in the experimental autoimmune encephalomyelitis spinal cord: a quantitative pixelwise analysis. J Neurosci. 2009;29(9):2805-13.
2. Budde MD, Kim JH, Liang HF, Schmidt RE, Russell JH, Cross AH, et al. Toward accurate diagnosis of white matter pathology using diffusion tensor imaging. Magn Reson Med. 2007;57(4):688-95.
3. Sun SW, Liang HF, Trinkaus K, Cross AH, Armstrong RC, Song SK. Noninvasive detection of cuprizone induced axonal damage and demyelination in the mouse corpus callosum. Magn Reson Med. 2006;55(2):302-8.
4. Song SK, Yoshino J, Le TQ, Lin SJ, Sun SW, Cross AH, et al. Demyelination increases radial diffusivity in corpus callosum of mouse brain. Neuroimage. 2005;26(1):132-40.
5. Song SK, Sun SW, Ju WK, Lin SJ, Cross AH, Neufeld AH. Diffusion tensor imaging detects and differentiates axon and myelin degeneration in mouse optic nerve after retinal ischemia. Neuroimage. 2003;20(3):1714-22.
6. Song SK, Sun SW, Ramsbottom MJ, Chang C, Russell JH, Cross AH. Dysmyelination revealed through MRI as increased radial (but unchanged axial) diffusion of water. Neuroimage. 2002;17(3):1429-36.
7. Kono K, Inoue Y, Nakayama K, Shakudo M, Morino M, Ohata K, et al. The role of diffusion-weighted imaging in patients with brain tumors. AJNR Am J Neuroradiol. 2001;22(6):1081-8.
8. Gauvain KM, McKinstry RC, Mukherjee P, Perry A, Neil JJ, Kaufman BA, et al. Evaluating pediatric brain tumor cellularity with diffusion-tensor imaging. AJR Am J Roentgenol. 2001;177(2):449-54.
9. Sugahara T, Korogi Y, Kochi M, Ikushima I, Shigematu Y, Hirai T, et al. Usefulness of diffusion-weighted MRI with echo-planar technique in the evaluation of cellularity in gliomas. J Magn Reson Imaging. 1999;9(1):53-60.
10. Chang EH, Argyelan M, Aggarwal M, Chandon TS, Karlsgodt KH, Mori S, et al. The role of myelination in measures of white matter integrity: Combination of diffusion tensor imaging and two-photon microscopy of CLARITY intact brains. Neuroimage. 2017;147:253-61.
11. Yeh FC, Irimia A, Bastos DCA, Golby AJ. Tractography methods and findings in brain tumors and traumatic brain injury. Neuroimage. 2021;245:118651.
12. Yeh FC, Zaydan IM, Suski VR, Lacomis D, Richardson RM, Maroon JC, et al. Differential tractography as a track-based biomarker for neuronal injury. Neuroimage. 2019;202:116131.
13. Garic D, Yeh FC, Graziano P, Dick AS. In vivo restricted diffusion imaging (RDI) is sensitive to differences in axonal density in typical children and adults. Brain Struct Funct. 2021;226(8):2689-705.
14. Yeh FC, Liu L, Hitchens TK, Wu YL. Mapping immune cell infiltration using restricted diffusion MRI. Magn Reson Med. 2017;77(2):603-12.
15. Shen CY, Tyan YS, Kuo LW, Wu CW, Weng JC. Quantitative Evaluation of Rabbit Brain Injury after Cerebral Hemisphere Radiation Exposure Using Generalized q-sampling imaging. PLoS One. 2015;10(7):e0133001.

# Difference between model-based and model-free

Here we focus on the differences between DTI and GQI, although other model-based and model-free methods exist.

<img src="https://user-images.githubusercontent.com/275569/156826203-e8c4ece1-1b45-4193-8257-aa8e64dfe57c.png" width="400">

<img src="https://user-images.githubusercontent.com/275569/156826048-40933f38-339d-442e-ab5d4fbb999.png" width="400">

The DTI model does not explicitly separate restricted and non-restricted diffusion. Consequently, DTI-derived metrics such as FA, AD, RD, and MD can be sensitive to a wide range of biological and geometric changes, including edema, inflammation, and crossing fibers (see Yeh F-C et al. PLoS ONE 8(11):e80713, 2013). More advanced methods aim to provide complementary metrics. GQI separates isotropic and anisotropic diffusion components, reducing the contribution of isotropic free water to anisotropy measurements. The anisotropic component is further quantified for each resolved diffusion direction, reducing the effect of crossing fibers on direction-specific measurements. A neurosurgery study showed that QA is robust against peritumoral edema and can contribute to more reliable tractography (Zhang et al., Neurosurgery 73(6), 1044-1053, 2013). A phantom study also showed that QA is more robust to free-water effects and partial-volume effects from crossing fibers (Yeh et al., PLoS ONE 8(11), 2013).
