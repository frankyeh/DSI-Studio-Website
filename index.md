# DSI-Studio: Tractography Tool for Diffusion MRI Fiber Tracking and Connectome
 
![image](/images/dsi_studio2.jfif)
> *A screenshot of DSI Studio mapping tractography of the arcuate fasciculus*

DSI Studio is a user-friendly tractography software designed for tractography analysis. It allows researchers and clinicians to efficiently map brain connections and explore their relationships with brain disorders.  

The design of DSI Studio follows *minimalist principle* and prioritizes *simplicity, efficiency, and adaptability*:  

- **Tractography Modalities**: provide connectional tractography and also new modalities:
  - **Differential tractography** for detecting structural changes  
  - **Correlational tractography** for studying structure-function relationships  
- **Connetome Modalities**: provide conventional region-to-region connectome and novel tract-to-Region connectome for connectivity mapping
- **Minimal Preprocessing** – Uses only essential corrections (e.g., **FSL’s TOPUP and eddy**) to reduce preprocessing burden.  
- **Modeling** – Uses simple models such as **DTI, GQI, and QSDR**.

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
