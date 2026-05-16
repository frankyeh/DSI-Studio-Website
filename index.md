<img width="1024" alt="image" src="https://github.com/user-attachments/assets/e19f8a20-7d4b-46a2-8bd9-8863c4dba0d6" />


> ### Integrated tractography, connectomics, brain MRI segmentation, and large-scale fiber data sharing


**DSI Studio** is an open-source, cross-platform software platform for diffusion MRI and structural MRI analysis. It brings together diffusion reconstruction, fiber tracking, connectome mapping, quality control, group analysis, and brain MRI segmentation in one environment. With direct integration of the **Fiber Data Hub**, DSI Studio also serves as a data infrastructure for scalable and reproducible brain connectivity research.

**Quick links:** [Download](download.html) · [Fiber Data Hub](https://brain.labsolver.org) · [News](news.html) · [Forum](https://groups.google.com/g/dsi-studio) · [GitHub](https://github.com/frankyeh/DSI-Studio)

---

## What you can do with DSI Studio

- Reconstruct diffusion MRI using methods including **DTI**, **GQI**, and **QSDR**.
- Map white matter pathways using **deterministic** and **probabilistic tractography**.
- Study change and association using **differential tractography** and **correlational tractography**.
- Quantify brain networks using both **region-to-region connectivity matrices** and the **tract-to-region connectome**.
- Run **quality control**, **group analysis**, and population-level workflows in the same software environment.
- Perform **brain MRI segmentation** for T1w and T2w images using compact **U-Net models** integrated in DSI Studio.
- Use the same platform on **Windows**, **macOS**, **Linux**, and **Docker**.

---

## Fiber Data Hub

The **Fiber Data Hub** extends DSI Studio from a software tool to a software-and-data platform. It provides direct access to compact processed diffusion MRI derivatives that can be downloaded and analyzed without repeating the full preprocessing pipeline from raw DWI.

The Hub currently hosts **more than 50,000 processed fiber datasets** from major neuroimaging resources including **HCP**, **ABCD**, **OpenNeuro**, **INDI**, **TCIA**, and other public datasets. These derivatives are distributed in compact formats such as **.fz**, **.sz**, and **.dz**, which are typically **50–100× smaller** than raw diffusion MRI while preserving the information needed for tractography and connectomics. For example, a diffusion MRI dataset that may require roughly **500 MB** in raw form can often be represented as a **5–10 MB** fiber dataset for downstream analysis.

This allows users to:

- browse and download processed datasets directly in DSI Studio;
- start tractography and connectomics analysis more quickly;
- build reproducible pipelines using standardized derivatives;
- scale analyses across public datasets with much lower storage and transfer cost.

Explore the Hub at [brain.labsolver.org](https://brain.labsolver.org).

---

## Research impact

DSI Studio has been used in **more than 3,000 publications** spanning neuroscience, neurology, psychiatry, psychology, biomedical engineering, and neurosurgery. Studies using DSI Studio have appeared in journals including **Nature Neuroscience**, **Nature Human Behaviour**, **Nature Communications**, **Brain**, **Cerebral Cortex**, and **NeuroImage**.

The software and the data platform were recently described in *Nature Methods* as an integrated tractography platform and fiber data hub for accelerating brain research.

![image](/images/nat_rev_neuro.png)
> DSI Studio tractography on the cover of *Nature Reviews Neurology* in 2017.

---

## Clinical and translational applications

DSI Studio has supported translational and clinical research in a wide range of neurological conditions, including epilepsy, traumatic brain injury, developmental disorders, and neurodegenerative disease. At the University of Pittsburgh Medical Center, its tractography workflows have been applied in research involving **more than 200 brain tumor patients** for presurgical evaluation and structural pathway assessment.

Its applications include mapping perilesional and intralesional pathways, studying postsurgical pathway changes, and reconstructing cranial nerves for skull base surgery research. These efforts aim to improve structural assessment in settings where preservation of brain function is important.

![image](/images/af.png)
> The human language pathway: left arcuate fasciculus mapped using DSI Studio.

---

## Ex-vivo and histology applications

DSI Studio can also process high-resolution ex-vivo diffusion MRI and MRI microscopy data. This supports structural analysis of biological tissue at high spatial resolution and has been used in histology-oriented imaging studies.

![Kidney rainbow](/images/KidneyRainbow.png)
> Kidney Rainbow created using DSI Studio by Nian Wang, Center for In Vivo Microscopy, Duke University. Coverage by [Science](https://science.sciencemag.org/content/363/6427/564).

---

## Brain MRI segmentation

DSI Studio now includes **U-Net based segmentation** for structural MRI, allowing users to run compact segmentation models directly inside the software. This extends the platform beyond tractography and connectomics to a broader structural MRI workflow.

The segmentation models are designed for convenient deployment, with no need to install Python or large deep-learning environments separately. Related segmentation development is also supported by the sister project [U-Net Studio](https://unet-studio.labsolver.org).

---

## Get started

- Download the latest release from the [download page](download.html).
- Check recent features and bug notices on the [news page](news.html).
- Visit the [support forum](https://groups.google.com/g/dsi-studio) for questions and community discussions.
- Explore large-scale processed datasets on the [Fiber Data Hub](https://brain.labsolver.org).
