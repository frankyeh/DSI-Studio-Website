# Diffusion metrics provided by DSI Studio

Diffusion MRI (dMRI) acquires **diffusion-sensitive** MRI data to characterize microscopic tissue structure. The data can be processed to provide diffusion metrics, including diffusivities and anisotropy measures that summarize diffusion patterns. These metrics can reveal changes associated with neuronal injury, cellularity, and tissue edema.

Diffusion metrics can be broadly categorized as model-based or model-free. Model-based metrics are calculated by fitting dMRI signals with a model, such as the diffusion tensor or NODDI, and interpreting the fitted model parameters. Model-free metrics are calculated directly from the diffusion distribution without assuming a specific tissue model.

Each approach has advantages and limitations. Model-based methods generally require fewer diffusion samples and often provide parameters with familiar physical interpretations, but their results depend on how well the model assumptions fit the tissue. Model-free methods generally require more diffusion samples, and biological interpretation still requires tissue validation, but they avoid imposing a specific tissue model.

<div class="di-overview">
<p>Read each metric as a measurement of diffusion, not a diagnosis. Changes can reflect multiple tissue processes, acquisition differences, and crossing-fiber geometry. Use complementary metrics and clinical context.</p>
</div>
<section class="di-group" aria-label="Diffusion tensor metrics">
<h2>Diffusion tensor metrics</h2>
<p class="di-group-intro">Directionality and magnitude of diffusion estimated by the diffusion tensor model.</p>
<div class="di-grid">
<article class="di-metric" id="metric-fa">
  <div class="di-metric-head"><div><span class="di-abbr">FA</span><h3>fractional anisotropy</h3></div></div>
  <p class="di-role">FA is nonspecifically associated with axonal integrity</p>
  <p class="di-change"><span>Reported change</span>decreases: demyelination (Chang, 2017), inflammation, edema, axonal loss</p>
  <details><summary>Interpretation and limitations</summary><p>Fractional anisotropy (FA) measures the degree of directional dependence of diffusion. It ranges from 0 for isotropic diffusion to 1 for highly anisotropic diffusion. FA is widely used to characterize white-matter microstructure but is sensitive to multiple biological and geometric factors, including crossing fibers.</p></details>
</article>
<article class="di-metric" id="metric-ad">
  <div class="di-metric-head"><div><span class="di-abbr">AD</span><h3>axial diffusivity</h3></div></div>
  <p class="di-role">AD is nonspecifically associated with axonal density</p>
  <p class="di-change"><span>Reported change</span>decreases: axonal loss (Budde et al., 2007; Song et al., 2003)</p>
  <details><summary>Interpretation and limitations</summary><p>Axial diffusivity is the principal diffusivity of the diffusion tensor and is represented by the largest eigenvalue, λ₁. It describes diffusion along the tensor's primary direction.</p></details>
</article>
<article class="di-metric" id="metric-rd">
  <div class="di-metric-head"><div><span class="di-abbr">RD</span><h3>radial diffusivity</h3></div></div>
  <p class="di-role">RD is nonspecifically associated with myelination</p>
  <p class="di-change"><span>Reported change</span>increases: demyelination (Budde et al., 2007; Song et al., 2002; Song et al., 2005)</p>
  <details><summary>Interpretation and limitations</summary><p>Radial diffusivity describes diffusion perpendicular to the tensor's primary direction and is defined as the mean of the two smaller eigenvalues: RD = (λ₂ + λ₃)/2.</p></details>
</article>
<article class="di-metric" id="metric-md">
  <div class="di-metric-head"><div><span class="di-abbr">MD</span><h3>mean diffusivity</h3></div></div>
  <p class="di-role">MD is associated with edema and cell infiltration</p>
  <p class="di-change"><span>Reported change</span>increases: vasogenic edema; decreases: cytotoxic edema</p>
  <details><summary>Interpretation and limitations</summary><p>Mean diffusivity is the average of the three diffusion-tensor eigenvalues: MD = (λ₁ + λ₂ + λ₃)/3. It summarizes the overall magnitude of diffusion within a voxel.</p></details>
</article>
</div>
</section>
<section class="di-group" aria-label="Q-space metrics">
<h2>Q-space metrics</h2>
<p class="di-group-intro">Diffusion measurements derived from Q-space imaging and related reconstructions.</p>
<div class="di-grid">
<article class="di-metric" id="metric-qa">
  <div class="di-metric-head"><div><span class="di-abbr">QA</span><h3>quantitative anisotropy</h3></div></div>
  <p class="di-role">QA is associated with axonal density</p>
  <p class="di-change"><span>Reported change</span>decreases: axonal loss (Yeh 2019; Shen 2015)</p>
  <details><summary>Interpretation and limitations</summary><p>Quantitative anisotropy (QA) measures anisotropic diffusion associated with each resolved fiber orientation. It is less affected by isotropic diffusion such as edema than FA (Yeh, 2013).</p></details>
</article>
<article class="di-metric" id="metric-iso">
  <div class="di-metric-head"><div><span class="di-abbr">ISO</span><h3>isotropy</h3></div></div>
  <p class="di-role">ISO is associated with edema</p>
  <p class="di-change"><span>Reported change</span>increases: edema</p>
  <details><summary>Interpretation and limitations</summary><p>ISO measures isotropic diffusion (Yeh, 2010). It represents background isotropic diffusion contributed by CSF or edema, including both restricted and non-restricted isotropic diffusion.</p></details>
</article>
<article class="di-metric" id="metric-rdi">
  <div class="di-metric-head"><div><span class="di-abbr">RDI</span><h3>restricted diffusion imaging</h3></div></div>
  <p class="di-role">RDI is associated with cell infiltration during inflammation (Yeh 2017; Garic 2021)</p>
  <p class="di-change"><span>Reported change</span>increases: cell infiltration associated with inflammation or tumor infiltration</p>
  <details><summary>Interpretation and limitations</summary><p>RDI quantifies restricted diffusion regardless of orientation (Yeh, 2017).</p></details>
</article>
<article class="di-metric" id="metric-nrdi">
  <div class="di-metric-head"><div><span class="di-abbr">NRDI</span><h3>non-restricted diffusion imaging</h3></div></div>
  <p class="di-role">NRDI is associated with edema (Yeh 2017)</p>
  <p class="di-change"><span>Reported change</span>increases: edema due to inflammation</p>
  <details><summary>Interpretation and limitations</summary><p>NRDI quantifies non-restricted diffusion regardless of orientation (Yeh, 2017).</p></details>
</article>
<article class="di-metric" id="metric-qir">
  <div class="di-metric-head"><div><span class="di-abbr">QIR</span><h3>QA-to-ISO ratio (QA/ISO)</h3></div><span class="di-new">Introduced 2025</span></div>
  <p class="di-role">QIR is a measure of axonal integrity</p>
  <p class="di-change"><span>Reported change</span>reduced QIR may suggest inflammation or demyelination</p>
  <details><summary>Interpretation and limitations</summary><p>QIR is derived from QA and ISO and is intended to improve comparability across scans by reducing the effect of B1 inhomogeneity.</p></details>
</article>
</div>
</section>
<section class="di-group" aria-label="Spatial normalization">
<h2>Spatial normalization</h2>
<p class="di-group-intro">A volume-related measure obtained from nonlinear spatial normalization.</p>
<div class="di-grid">
<article class="di-metric" id="metric-vol">
  <div class="di-metric-head"><div><span class="di-abbr">VOL</span><h3>template-space volume</h3></div><span class="di-new">Introduced 2025</span></div>
  <p class="di-role">VOL is a volumetric measure</p>
  <p class="di-change"><span>Reported change</span>reduced VOL may suggest tissue atrophy</p>
  <details><summary>Interpretation and limitations</summary><p>VOL reflects relative tissue size in template space after nonlinear spatial normalization.</p></details>
</article>
</div>
</section>
*The diffusivity values (AD, RD, and MD) reported by DSI Studio use units of 10^-3 mm²/s.*

## Change of metrics in neurological disorders

<p class="di-legend">↓ decrease &nbsp; · &nbsp; ↑ increase &nbsp; · &nbsp; – no consistent change</p>

<div class="di-comparisons">
<section class="di-condition"><h3>Acute axonal injury with inflammation</h3><p class="di-examples">stroke (&lt; 3 months), TBI (&lt; 3 months), MS relapse, tumor mass effect</p><dl class="di-signals"><div><dt>FA</dt><dd>↓</dd></div><div><dt>AD</dt><dd>↑ or -</dd></div><div><dt>RD</dt><dd>↑</dd></div><div><dt>MD</dt><dd>↑</dd></div><div><dt>QA</dt><dd>-</dd></div><div><dt>ISO</dt><dd>↑</dd></div><div><dt>RDI</dt><dd>↑ (at locations with cell infiltration)</dd></div><div><dt>NRDI</dt><dd>↑ (at edema locations)</dd></div></dl></section>
<section class="di-condition"><h3>Axonal loss without inflammation</h3><p class="di-examples">ALS, Huntington's disease, TBI (&gt; 6 months), stroke (&gt; 6 months)</p><dl class="di-signals"><div><dt>FA</dt><dd>↓</dd></div><div><dt>AD</dt><dd>↓</dd></div><div><dt>RD</dt><dd>↑</dd></div><div><dt>MD</dt><dd>-</dd></div><div><dt>QA</dt><dd>↓</dd></div><div><dt>ISO</dt><dd>-</dd></div><div><dt>RDI</dt><dd>-</dd></div><div><dt>NRDI</dt><dd>-</dd></div></dl></section>
</div>
<p class="di-caveat">These are illustrative patterns, not diagnostic criteria. The direction and magnitude of change depend on tissue, stage, measurement method, and study design.</p>

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

16. Yeh FC, Wedeen VJ, Tseng WYI. Generalized q-sampling imaging. IEEE Trans Med Imaging. 2010;29(9):1626–1635. doi:10.1109/TMI.2010.2045126.
17. Yeh FC, et al. Deterministic diffusion fiber tracking improved by quantitative anisotropy. PLoS One. 2013;8(11):e80713.

## Difference between model-based and model-free

Here we focus on the differences between DTI and GQI, although other model-based and model-free methods exist.

<img src="https://user-images.githubusercontent.com/275569/156826203-e8c4ece1-1b45-4193-8257-aa8e64dfe57c.png" width="400" alt="Illustration comparing diffusion-model metrics">


The DTI model does not explicitly separate restricted and non-restricted diffusion. Consequently, DTI-derived metrics such as FA, AD, RD, and MD can be sensitive to a wide range of biological and geometric changes, including edema, inflammation, and crossing fibers (see Yeh F-C et al. PLoS ONE 8(11):e80713, 2013). More advanced methods aim to provide complementary metrics. GQI separates isotropic and anisotropic diffusion components, reducing the contribution of isotropic free water to anisotropy measurements. The anisotropic component is further quantified for each resolved diffusion direction, reducing the effect of crossing fibers on direction-specific measurements. A neurosurgery study showed that QA is robust against peritumoral edema and can contribute to more reliable tractography (Zhang et al., Neurosurgery 73(6), 1044-1053, 2013). A phantom study also showed that QA is more robust to free-water effects and partial-volume effects from crossing fibers (Yeh et al., PLoS ONE 8(11), 2013).
