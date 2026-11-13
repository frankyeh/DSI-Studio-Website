# [Slices Segment] Menu

Use **[Slices Segment]** to segment the active 3D image slice into anatomical tissues, subcortical nuclei, perivascular spaces, or pathological lesions using 3D U-Net models and add each structure to **Assign Regions**. Available models update automatically for the selected species template, while skull stripping and 3D mesh extraction are in the [Slices menu](/doc/menu_slices.html).

## How slice segmentation works

Select a species template (`human`, `rhesus`, `marmoset`, `rat`, or `mouse`) in **Assign Regions** to populate **[Slices Segment]** (also opened by **Segment...** in the **Assign Regions** toolbar), and choose the target volume in the 3D viewport's top-left slice selector. Contrast-flexible models work on any slice, whereas modality-specific models check the slice name (case-insensitive) for `t1`/`mpr`, `t2`/`tse`, or `flair`/`t2f` and otherwise disable with `(need T1w)`, `(need T2w)`, or `(need FLAIR)`. Model weights download automatically on first use and add regions at the active slice's native millimeter resolution and alignment with passive **Type** `...` until changed to `ROI`, `ROA`, `Seed`, or `End`.

## Human anatomy and tissue models

Shown when the `human` template is selected in **Assign Regions**.

| Menu item | Behavior |
|---|---|
| **SynthSeg V2 (10.5MB)** | Segments human brain MRI into 32 anatomical regions—including cortex, white matter, ventricles, CSF, brainstem, thalamus, basal ganglia, hippocampus, and amygdala—at native slice resolution. Works on any slice regardless of MRI contrast or resolution. |
| **SIAM Model 1 (64.1MB)** | Segments unstripped human head MRI into 39 intracerebral and extracerebral head-tissue regions in **Assign Regions**. Works on any slice across contrasts, resolutions, and pathology. |
| **SIAM Model 2 (51.8MB)** | Segments unstripped human head MRI into 12 brain-tissue regions plus extracerebral head, skull, dura, and vessel regions. Works on any slice across contrasts, resolutions, and pathology. |
| **SIAM Model 3 (21.5MB)** | Segments unstripped human head MRI into the 12 brain and extracerebral head, skull, dura, and vessel regions of **SIAM Model 2 (51.8MB)** plus an abnormal-tissue anomaly region. Works on any slice across contrasts and resolutions. |
| **GOUHFI (79.6MB)** | Segments whole-brain human MRI into 35 anatomical brain structures in **Assign Regions** using the GOUHFI 2.0 network. Works on any slice across MRI contrasts, resolutions, and field strengths. |
| **MedNet-PVS T2w (19.7MB)** | Segments white-matter perivascular spaces (PVS) into **Assign Regions**. Requires a T2-weighted slice with `t2` or `tse` in its name; otherwise disabled with `(need T2w)`. |
| **U-Net Studio Human Tissue (13MB)** | Segments human brain MRI into five major tissue regions in **Assign Regions**. Enabled on any slice across T1w, T2w, FLAIR, and related structural contrasts. |
| **U-Net Studio Human T1w Tissue (1.2MB)** | Segments T1-weighted human brain MRI into five major tissue regions using a lightweight model. Requires `t1` or `mpr` in the slice name; otherwise disabled with `(need T1w)`. |
| **U-Net Studio Human T2w Tissue (1.2MB)** | Segments T2-weighted human brain MRI into five major tissue regions using a lightweight model. Requires `t2` or `tse` in the slice name; otherwise disabled with `(need T2w)`. |
| **U-Net Studio Human FLAIR Tissue (1.3MB)** | Segments human FLAIR MRI into five major brain-tissue regions using a lightweight model. Requires `flair` or `t2f` in the slice name; otherwise disabled with `(need FLAIR)`. |

## Human lesion and disease models

Listed below the human anatomy models when the `human` template is active.

| Menu item | Behavior |
|---|---|
| **TumorSynth (20.7MB)** | Segments whole-tumor extent as a single combined `Tumor` region in **Assign Regions**. Enabled on any slice across T1w, contrast-enhanced T1w, T2w, and FLAIR; use **U-Net Studio Human Tumor Lesion V2 (13MB)** for separate tumor subcompartments. |
| **MindGlide MS Lesion (25MB)** | Segments multiple sclerosis (MS) lesions alongside major brain structures (19 regions total) into **Assign Regions**. Works on any slice across clinical MRI contrasts and scan qualities. |
| **U-Net Studio Human Tumor Lesion V2 (13MB)** | Segments brain tumors into separate `Enhancing Tumor`, `Necrosis`, and `Peritumoral Edema` regions alongside five surrounding brain-tissue regions. Enabled on any slice across T1w, contrast-enhanced T1w, T2w, and FLAIR. |
| **U-Net Studio Human T1w Tumor Lesion (1.3MB)** | Segments brain-tumor compartments and surrounding tissues on non-contrast T1-weighted MRI using a lightweight model. Requires `t1` or `mpr` in the slice name; otherwise disabled with `(need T1w)`. |
| **U-Net Studio Human T1w-gd Tumor (1.3MB)** | Segments brain-tumor compartments on gadolinium contrast-enhanced T1-weighted MRI using a lightweight model. Requires `t1` or `mpr` in the slice name (`(need T1w)` otherwise); confirm manually that the T1w scan is contrast-enhanced. |
| **U-Net Studio Human FLAIR Tumor (1.3MB)** | Segments brain tumors and peritumoral hyperintensity on FLAIR MRI using a lightweight model. Requires `flair` or `t2f` in the slice name; otherwise disabled with `(need FLAIR)`. |
| **U-Net Studio Human Stroke Lesion V2 (13MB)** | Segments acute or chronic stroke lesions into **Assign Regions**. Enabled on any slice across T1w, T2w, FLAIR, and related structural stroke MRI. |
| **U-Net Studio Human MS Lesion V2 (13MB)** | Segments multiple sclerosis white-matter and parenchymal lesions into **Assign Regions**. Enabled on any slice across T1w, T2w, FLAIR, and related structural MRI. |
| **U-Net Studio Human T1w Stroke Lesion (1.2MB)** | Segments stroke lesions on human T1-weighted MRI using a lightweight model. Requires `t1` or `mpr` in the slice name; otherwise disabled with `(need T1w)`. |

## Animal template models

Shown when `rhesus`, `marmoset`, `rat`, or `mouse` is selected as the template in **Assign Regions**.

| Menu item | Behavior |
|---|---|
| **U-Net Studio Rhesus Tissue V2 (13MB)** | Segments rhesus macaque brain MRI into tissue regions in **Assign Regions**. Shown for the `rhesus` template and enabled on any slice across contrasts. |
| **U-Net Studio Marmoset Tissue V2 (13MB)** | Segments marmoset brain MRI into tissue regions in **Assign Regions**. Shown for the `marmoset` template and enabled on any slice across T1w, T2w, and FLAIR. |
| **U-Net Studio Marmoset T1w Tissue (1.3MB)** | Segments T1-weighted marmoset brain MRI into tissue regions using a lightweight model. Shown for the `marmoset` template; requires `t1` or `mpr` in the slice name (`(need T1w)` otherwise). |
| **U-Net Studio Marmoset T2w Tissue (1.2MB)** | Segments T2-weighted marmoset brain MRI into tissue regions using a lightweight model. Shown for the `marmoset` template; requires `t2` or `tse` in the slice name (`(need T2w)` otherwise). |
| **U-Net Studio Rat Tissue V2 (13MB)** | Segments rat brain MRI into tissue regions in **Assign Regions**. Shown for the `rat` template and enabled on any slice across contrasts. |
| **U-Net Studio Rat T2w Tissue (1.2MB)** | Segments T2-weighted rat brain MRI into tissue regions using a lightweight model. Shown for the `rat` template; requires `t2` or `tse` in the slice name (`(need T2w)` otherwise). |
| **U-Net Studio Mouse Tissue V2 (13MB)** | Segments mouse brain MRI into tissue regions in **Assign Regions**. Shown for the `mouse` template and enabled on any slice across contrasts. |
| **U-Net Studio Mouse T2w Tissue (1.3MB)** | Segments T2-weighted mouse brain MRI into tissue regions using a lightweight model. Shown for the `mouse` template; requires `t2` or `tse` in the slice name (`(need T2w)` otherwise). |

## Practical recommendations and cautions

Prefer ~1 mm isotropic 3D structural scans over thick-slice 2D series, and verify slice-to-diffusion alignment (**[Slices][Adjust Slice Registration...]**) before segmenting because generated regions inherit the active slice's registration. Multi-label models add empty (`0`-voxel) rows when a compartment is absent; always inspect generated boundaries across all three slice planes and clean up false-positive islands in the [Region Edit menu](/doc/menu_region_edit.html) before quantitative reporting or tract filtering.
