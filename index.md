# DSI-Studio: Tractography Software for Diffusion MRI, Fiber Tracking, and Connectome
 
![image](/images/dsi_studio2.jfif)
> *A screenshot of DSI Studio mapping tractography of the arcuate fasciculus*

**DSI Studio** is an open-source, cross-platform software platform for analyzing brain connectivity using diffusion-weighted MRI (dMRI). It supports widely used diffusion models including diffusion tensor imaging (DTI) and generalized q-sampling imaging (GQI) for robust mapping of white matter pathways and brain networks. The tool enables both neuroscience researchers and clinical neuroimaging specialists to reconstruct, quantify, and visualize white matter architecture, as well as study its relationship to cognition, behavior, and neurological conditions such as epilepsy, Alzheimer's disease, traumatic brain injury, and developmental disorders.

We also recommend other tools, including MRtrix3, Dipy, TRACULA, SlicerDMRI, ExploreDTI, TrackVis, FSL probtrackx.

### Major Functionalities

* **Multiple Tractography Approaches**:
  Supports deterministic and probabilistic **fiber tracking** algorithms, as well as novel modalities:

  * **Differential tractography** – for detecting longitudinal or group-level microstructural changes in white matter.
  * **Correlational tractography** – for identifying connections associated with behavioral, cognitive, or clinical variables.
* **Comprehensive Connectome Mapping**:
  Includes both traditional **region-to-region connectivity matrices** and the novel **tract-to-region connectome**, enabling high-resolution structural network analysis.
* **Lightweight Preprocessing Pipeline**:
  Employs only essential corrections (e.g., **motion correction**, **susceptibility distortion correction** using tools like FSL TOPUP and eddy), avoiding overfitting and preserving native image features.
* **Support for Multiple Diffusion Models**:
  Built-in support for DTI, GQI, and **Q-space diffeomorphic reconstruction (QSDR)**, allowing flexibility across different acquisition protocols and datasets.

Whether you're conducting brain connectomics research, analyzing neurodevelopment, or building machine learning pipelines on white matter features, DSI Studio provides an efficient and validated environment for structural MRI analysis and neuroinformatics applications.

---

### Publications in Neuroscience

DSI Studio has facilitated and been used in more than 3,000 publications. Research studies using DSI Studio have been published in top-tier journals including Nature Neuroscience, Nature Human Behavior, Nature Communication, Brain, Cerebral Cortex, and NeuroImage 

![image](/images/nat_rev_neuro.png)
> DSI Studio tractography on the cover of "Nature Reviews Neurology" for the whole year of 2017. 
> About the cover. Brains and beauty — the 2017 cover. Nat Rev Neurol 13, 1 (2017)

---

### Research in Clinical Medicine for Tumor Patients

Under the research projects, DSI Studio has helped more than 200 brain tumor patients at the University of Pittsburgh Medical Center:

Patient's story: [1](https://www.youtube.com/watch?v=gEZlzkxb-LE), [2](https://www.youtube.com/watch?v=vULJxiuO6lo), [3](https://www.youtube.com/watch?v=7WQ-Dej4_dM)

Fiber tracking in DSI Studio provides a superior presurgical evaluation of the fiber tracts for patients with complex brain lesions, including low-grade and high-grade gliomas. Presurgical studies are built upon precise and accurate neuroanatomical knowledge, which allows doctors to reconstruct perilesional or intralesional fiber tracts, design the less invasive trajectory into the target lesion and apply more effectively intraoperative electrical mapping techniques for maximal and safe tumor resection in eloquent cortical and subcortical regions. 

Our clinical experience applying DSI Studio fiber tracking has been reported in Neurosurgery, Journal of Neurosurgery, and Neuro-oncology among others. We are actively investigating its potential for not only presurgical planning and intraoperative navigation but also for neurostructural damage assessment, estimation of postsurgical neural pathways damage and recovery, and tracking of postsurgical changes and responses to rehabilitation therapy.

The latest innovation is the reconstruction of cranial nerves for presurgical evaluation in skull base surgery, with very promising results. The ultimate goal is to facilitate brain function preservation and recovery in patients undergoing complex brain surgery.

![image](/images/af.png)
> The human language pathway: left arcuate fasciculus mapped using DSI Studio

---

### Histology Studies

DSI Studio can process high-resolution ex-vivo diffusion "MRI microscopy" acquired at a large volume of imaging data, makes it possible to estimate the location, orientation, and anisotropy of the tracts in any histological specimens. This provides a powerful tool to investigate the structural topology of biological tissue to shade insight into its physiological function.

![Kidney rainbow](/images/KidneyRainbow.png)
> Kidney Rainbow created using DSI Studio by Nian Wang, Center for In Vivo Microcopy (Directed by Dr. G. Allan Johnson), Duke University, U.S (Curtesy of Dr. Wang) <br>
> Coverage by [Science](https://science.sciencemag.org/content/363/6427/564)

The tissue segmentation functoin in DSI Studio is provided by its sister tool [U-Net Studio](https://unet-studio.labsolver.org)
