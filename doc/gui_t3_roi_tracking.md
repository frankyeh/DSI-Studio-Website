# ROI-Based Fiber Tracking

ROI-based fiber tracking uses anatomical regions to include, exclude, start, or terminate streamlines. Use region constraints only when they are supported by the anatomical hypothesis.

> **Check whole-brain tractography first.** Before adding ROI constraints, confirm that the `.fz` file has reasonable fiber orientations and that major pathways can be reconstructed with the normal tracking settings.

## Step T3a: Add regions

Open the subject `.fz` file in **Step T3: Fiber Tracking**. Regions can be added from:

- NIfTI region files;
- built-in atlases;
- manually drawn regions;
- regions derived from existing tracts or images.

See the [[Regions] menu](/doc/menu_regions.html) for region-management functions.

![image](https://user-images.githubusercontent.com/275569/147854254-70bd8cf7-9a47-485e-bab2-d38bfa19a2c6.png)

## Region roles

Assign each checked region the role required by the tracking hypothesis.

| Role | Effect on tracking |
|:--|:--|
| **...** | Visualization/parcellation only. It does not constrain tracking. |
| **Seed** | Specifies where tracking starts. If no Seed region is assigned, DSI Studio uses the normal whole-brain seeding strategy. Add a Seed only when the starting area should intentionally be restricted. |
| **ROI** | Keeps only tracks that pass through the region. Multiple ROI regions are combined as AND constraints: a retained track must pass through all of them. |
| **ROA** | **Excludes tracks that pass through the region.** Use it to remove anatomically unwanted pathways. |
| **End** | Keeps only tracks whose endpoint lies in the region. This is more restrictive than ROI and does not itself terminate tracking. |
| **Terminative** | Stops a streamline when it enters the region. Use it when the tracking process should terminate at a specific anatomical boundary or target. |
| **NotEnd** | Rejects tracks that end in the region while still allowing tracks to pass through it. |
| **Limiting** | Restricts tracking to the allowed spatial region. It is used internally by atlas-guided tracking and can be used when a pathway must stay within a defined spatial extent. |

![image](https://user-images.githubusercontent.com/275569/147854494-af8a958f-ba50-4d2b-89c1-5ea37df57824.png)

### Recommended order for building a tracking protocol

1. Start with conventional whole-brain tracking and verify anatomy.
2. Add **one ROI** that is strongly supported by anatomical evidence.
3. Add a second ROI only when it is needed to define the intended pathway.
4. Add an ROA only to remove a known competing pathway.
5. Use End, Terminative, Seed, NotEnd, or Limiting roles only when their specific behavior is required.

Changing several constraints at once makes it difficult to determine which condition caused a pathway to appear or disappear.

### ROA thickness

A very thin ROA can be skipped when the tracking step size is large enough to jump across it. If an ROA is anatomically correct but does not exclude the expected streamlines, enlarge its thickness before adding additional exclusion regions.

## Load regions from built-in atlases

Use **Step T3a: Atlas** to select anatomical regions from an atlas appropriate for the subject/template. DSI Studio maps atlas regions into the tracking space as needed.

Atlas regions are convenient anatomical references, but the selected ROI/ROA roles still define the tracking hypothesis. Inspect the transformed region before using it as a constraint.

## Draw and edit regions

![image](https://user-images.githubusercontent.com/275569/147854543-9001e2d5-580b-4a86-a9cd-f931ca3973ca.png)

Regions can be drawn in the slice/region view with rectangular, freehand, polygon, sphere, and cube tools. Existing regions can be moved or edited with morphological operations such as dilation, erosion, smoothing, and defragmentation.

For detailed editing commands, see the [[Regions] menu](/doc/menu_regions.html).

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZkWBU_qnaKg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Step T3d: Run fiber tracking

Click **Fiber Tracking** after the region roles are assigned. Only checked regions constrain the result.

Inspect the tract in multiple views and compare it with known anatomy. When refining a protocol, change one condition at a time. For group studies, keep the tracking parameters and anatomical rules consistent across subjects.

For general tracking parameters, see [Whole-Brain Fiber Tracking](/doc/gui_t3_whole_brain.html). For command-line region roles, see [Fiber Tracking CLI](/doc/cli_t3.html).

# Optional: Custom templates and atlases

Most analyses should use the templates and atlases distributed with DSI Studio. Add custom resources only when the study requires a standard space or parcellation that is not already available.

## Custom template

A custom template requires the template-space images expected by DSI Studio, including anisotropic and isotropic reference maps such as:

```text
TEMPLATE_NAME.QA.nii.gz
TEMPLATE_NAME.ISO.nii.gz
```

The NIfTI headers must correctly define the template coordinate system. Additional template modalities can be supplied when needed.

Place the template resources in an appropriately named folder under the DSI Studio `atlas` directory. On macOS, the `atlas` directory is inside the DSI Studio application bundle; on Windows/Linux it is within the extracted DSI Studio package. Avoid hard-coding an executable or application-bundle name because package names can change between releases.

## Custom atlas

A parcellation atlas normally consists of:

- an integer-valued NIfTI image such as `MyAtlas.nii.gz`;
- a matching text label file such as `MyAtlas.txt` containing value/name pairs.

Place both files in the folder for the corresponding template under the DSI Studio `atlas` directory and restart DSI Studio. The atlas image must be aligned to that template space before it is used.

# Practical guidance

- Prefer anatomical evidence over trial-and-error region placement.
- Avoid adding extra ROI/ROA constraints by default.
- Keep acquisition, reconstruction, tracking, and region rules consistent when comparing subjects.
- If a pathway cannot be reconstructed without repeatedly relaxing anatomical constraints, re-check data quality and the reconstruction before accepting the tract.
