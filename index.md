<img width="1024" alt="image" src="https://github.com/user-attachments/assets/837427ff-b7f9-402f-bd21-3be9eb1d0346" />

> ### Integrated tractography, connectomics, brain MRI segmentation, and large-scale fiber data sharing
> Yeh, Fang-Cheng. “DSI Studio: An Integrated Tractography Platform and Fiber Data Hub for Accelerating Brain Research.” Nature Methods, July 2025, https://doi.org/10.1038/s41592-025-02762-8.

**DSI Studio** is a cross-platform software platform with source code available on [GitHub](https://github.com/frankyeh/DSI-Studio) for diffusion MRI and structural MRI analysis. It brings together diffusion reconstruction, fiber tracking, connectome mapping, quality control, group analysis, and brain MRI segmentation in one environment. With direct integration of the **Fiber Data Hub**, DSI Studio also serves as a data infrastructure for scalable and reproducible brain connectivity research.

**Quick links:** [Download](download.html) · [Fiber Data Hub](https://brain.labsolver.org) · [News](news.html) · [Forum](https://groups.google.com/g/dsi-studio) · [GitHub](https://github.com/frankyeh/DSI-Studio)

---

## What you can do with DSI Studio

- Reconstruct diffusion MRI using methods including **DTI**, **GQI**, and **QSDR**.
- Map white-matter pathways using **deterministic tractography**, **ROI-based tracking**, and **atlas-based automatic fiber tracking**.
- Study change and association using **differential tractography** and **correlational tractography**.
- Quantify brain networks using both **region-to-region connectivity matrices** and the **tract-to-region connectome**.
- Run **quality control**, **group analysis**, and population-level workflows in the same software environment.
- Perform **brain MRI segmentation** using compact **U-Net models** integrated in DSI Studio.
- Use **Codex** or **Claude Code** through the Sun version's **AI Agent** integration to operate DSI Studio workflows from natural-language requests.
- Use the same platform on **Windows**, **macOS**, **Linux**, and **Docker**.

---

## AI Agent in the Sun version

The current **Sun** version integrates AI agents directly with DSI Studio. After installing **Codex** or **Claude Code** on the same computer, the agent can read the DSI Studio manuals, inspect the current data and application state, and operate workflows such as reconstruction, image processing, fiber tracking, and connectometry from natural-language requests.

See the [download page](download.html) for the current Sun release and setup requirement.

---

## Fiber Data Hub

The **Fiber Data Hub** extends DSI Studio from a software tool to a software-and-data platform. It provides direct access to compact processed diffusion MRI derivatives that can be downloaded and analyzed without repeating the full preprocessing pipeline from raw DWI.

The Hub currently hosts **more than 50,000 processed fiber datasets** from major neuroimaging resources including **HCP**, **ABCD**, **OpenNeuro**, **INDI**, **TCIA**, and other public datasets. DSI Studio uses compact formats such as **.fz** and **.sz** for processed diffusion data, while **.dz** supports population databases. These formats substantially reduce storage and transfer requirements for many downstream workflows compared with repeatedly distributing the corresponding raw diffusion MRI data.

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
