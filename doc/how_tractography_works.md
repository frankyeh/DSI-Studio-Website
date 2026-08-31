# How Tractography Really Works: Four Myths and How to Get Better Results

Diffusion MRI tractography is the only widely available noninvasive method for mapping white-matter pathways in the living human brain. It is also easy to over-interpret.

The most important idea is simple:

> **Diffusion MRI estimates local directional information. Tractography uses that local information to infer long-range pathways.**

A streamline is therefore an **inference produced by an algorithm**. It is not an individual axon, and a dense bundle of streamlines is not direct proof of a dense anatomical connection.

<img src="/images/tutorial_tractography_pipeline.svg" width="900" alt="Diffusion MRI provides local directional information, which tractography propagates into global streamlines.">

This distinction explains many common tractography misconceptions.

## Myth 1: Higher angular resolution automatically gives better tractography

Higher angular resolution can help resolve crossing fibers. A tensor model usually resolves only one dominant direction in a voxel, whereas higher-order approaches can represent multiple directions. Methods using orientation distribution functions or fiber orientation distributions can therefore recover pathways that tensor-based tracking may miss.

But **resolving more directions is not the same as mapping more true connections**.

<img src="/images/tutorial_tractography_resolution.svg" width="900" alt="Higher angular resolution can recover real crossing fibers but can also introduce spurious directions that lead to false pathways.">

A sharper orientation distribution, a higher spherical-harmonic order, or a model capable of producing more peaks can increase angular resolving power. However, the extra angular detail is only useful when it is supported by the signal and by the underlying anatomy. Noise, partial-volume effects, model assumptions, response-function mismatch, regularization, and limited spatial resolution can all produce unstable or spurious orientations.

Once a spurious local orientation is present, fiber tracking can propagate it over many millimeters and generate a convincing but anatomically incorrect pathway.

**Practical lesson:** choose a reconstruction method that resolves the anatomy supported by the data. Do not treat the number or sharpness of resolved peaks as a measure of tractography accuracy.

## Myth 2: Crossing fibers are the main tractography problem

Crossing fibers are important, but they are only one of several anatomical challenges.

Different parts of the brain create different tracking problems:

- **Superficial white matter:** fibers fan toward the cortex and often turn sharply near the gray-white matter boundary. This produces gyral bias and can cause connections to favor gyral crowns while missing sulcal banks.
- **Deep white matter:** association, projection, and commissural pathways cross, branch, fan, and run alongside one another.
- **Gray-white matter boundaries:** tracking must stop at biologically meaningful locations. Premature termination and overshoot both alter inferred connectivity.
- **Subcortical nuclei and brainstem:** small pathways, limited spatial resolution, susceptibility distortion, and low diffusion signal can be more limiting than angular resolution.

This is why tractography is fundamentally an **anatomical inference problem**, not simply an angular-resolution problem.

For anatomy-based examples of these challenges, see Yeh et al., *NeuroImage* 2021, particularly Figs. 1, 3, and 4.

## Myth 3: If the local fiber directions are correct, the pathway will be correct

Even good local orientation estimates do not guarantee a correct long-range trajectory.

<img src="/images/tutorial_tractography_error_modes.svg" width="900" alt="Three tractography error modes: angular deviation, incorrect termination, and switching between pathways.">

Three common mechanisms are:

1. **Angular deviation.** Small directional errors accumulate as a streamline propagates.
2. **Termination error.** A streamline may stop too early or continue beyond the true end of a pathway.
3. **Pathway switching.** At a crossing or branching region, a tracking algorithm can move from one real fiber population onto another real fiber population and create a connection that does not exist anatomically.

The third case is especially important: every local orientation along a false streamline can look plausible while the **complete connection is wrong**.

**Practical lesson:** evaluate the full pathway and its anatomical endpoints, not just the local orientation field.

## Myth 4: More streamlines mean better mapping

Generating more streamlines usually increases sensitivity. It can also increase false positives.

<img src="/images/tutorial_tractography_streamlines.svg" width="900" alt="Dense tractography can increase sensitivity while also producing more false positive pathways.">

A tractogram with millions of streamlines may look impressive, but streamline count is affected by seeding strategy, stopping criteria, angular threshold, anisotropy threshold, tracking algorithm, reconstruction method, and post-processing.

Therefore:

> **Streamline count is an algorithm output, not an axon count.**

A useful tractography result balances sensitivity with anatomical specificity. For many scientific questions, a smaller set of reproducible and anatomically plausible pathways is more informative than a visually dense whole-brain tractogram.

## What makes tractography work well?

Good tractography does not come from one "best" reconstruction or tracking algorithm. It comes from several sources of information agreeing with one another.

| Step | Question to ask |
|---|---|
| **1. Start with anatomy** | What pathway, cortical territory, or network am I trying to study? What course and endpoints are anatomically plausible? |
| **2. Check acquisition and preprocessing** | Is the spatial resolution, angular sampling, SNR, distortion correction, and motion correction adequate for the target anatomy? |
| **3. Resolve only supported angular detail** | Does the reconstruction recover necessary crossing fibers without introducing unstable peaks? |
| **4. Use appropriate tracking constraints** | Are turning angle, step size, anisotropy/termination threshold, and seeding strategy appropriate for the pathway? |
| **5. Use anatomical information** | Can ROI, ROA, endpoint, terminating, or other anatomical constraints reduce implausible routes? |
| **6. Inspect the complete pathway** | Does the tract follow plausible anatomy and terminate where expected? Are there obvious false continuations or missing branches? |
| **7. Test robustness** | Does the finding persist across reasonable parameter changes, repeated scans, subjects, templates, or atlases? |
| **8. Interpret conservatively** | Is the conclusion supported by the measurement? Avoid equating streamlines with axons or streamline counts with connection strength without validation. |

## A useful way to think about tractography

Instead of asking:

> **Which tractography method is the most advanced?**

ask:

> **Which combination of data, reconstruction, tracking, anatomical constraints, and validation best answers this particular anatomical or scientific question?**

This also explains why higher angular resolution does not automatically win. Better local directional information is valuable, but tractography accuracy depends on the entire chain:

**diffusion acquisition → preprocessing → local orientation estimation → tracking → termination → anatomical selection → validation → interpretation**

An improvement at one step cannot automatically correct errors introduced at another.

## In DSI Studio

For practical implementation, continue with:

- [Whole-brain tractography](/doc/gui_t3_whole_brain.html)
- [ROI-based tracking](/doc/gui_t3_roi_tracking.html)
- [Automatic fiber tracking](/doc/gui_t3_atk.html)
- [How to acquire dMRI](/doc/how_to_acquire_dmri.html)
- [How to analyze dMRI](/doc/how_to_analyze_dmri.html)
- [How to interpret dMRI](/doc/how_to_interpret_dmri.html)

The purpose of these tools is not to make the densest tractogram. The goal is to obtain a result that is **anatomically plausible, reproducible, and appropriate for the scientific question**.

## References

1. Yeh FC, Irimia A, Bastos DCA, Golby AJ. Tractography methods and findings in brain tumors and traumatic brain injury. *NeuroImage*. 2021;245:118651. https://doi.org/10.1016/j.neuroimage.2021.118651
2. Maier-Hein KH, Neher PF, Houde JC, et al. The challenge of mapping the human connectome based on diffusion tractography. *Nature Communications*. 2017;8:1349. https://doi.org/10.1038/s41467-017-01285-x
3. Reveley C, Seth AK, Pierpaoli C, et al. Superficial white matter fiber systems impede detection of long-range cortical connections in diffusion MR tractography. *PNAS*. 2015;112:E2820-E2828. https://doi.org/10.1073/pnas.1418198112
