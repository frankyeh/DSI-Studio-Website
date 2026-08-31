# Practical Tips for Tractography: Common Pitfalls and What to Watch For

Diffusion MRI tractography is a powerful noninvasive way to study white-matter pathways in the living human brain, but it is also easy to over-interpret. The suggestions below summarize how I usually think about tractography when choosing a method, checking a result, or deciding how much confidence to place in a pathway.

The most useful starting point is simple:

> **Diffusion MRI estimates local directional information. Tractography uses that local information to infer long-range pathways.**

A streamline is therefore an **inference produced by an algorithm**. It is not an individual axon, and a dense bundle of streamlines is not direct proof of a dense anatomical connection.

<img src="/images/tutorial_tractography_pipeline.svg" width="900" alt="Diffusion MRI provides local directional information, which tractography propagates into global streamlines.">

This distinction helps explain several common tractography misconceptions.

## Myth 1: Higher angular resolution automatically gives better tractography

Higher angular resolution can help resolve crossing fibers. DTI usually resolves one dominant orientation in a voxel, whereas GQI-based diffusion ODFs and FODs can represent multiple orientations. This additional resolving power can recover pathways that tensor-based tracking may miss.

But **resolving more directions is not the same as mapping more true connections**.

<img src="https://ars.els-cdn.com/content/image/1-s2.0-S1053811921009241-gr3.jpg" width="950" alt="Comparison of DTI, GQI diffusion ODF, and FOD fiber orientations from Yeh et al. NeuroImage 2021.">

*Figure 3 from Yeh et al., NeuroImage 2021. The same diffusion data are reconstructed using the tensor model, GQI diffusion ODFs, and FODs.*

This comparison is useful because it shows both the benefit and the limitation of increasing angular resolving power. DTI may miss crossing branches such as lateral callosal fibers. GQI can recover additional fiber populations. FOD can provide still higher resolving power, yet the extra angular detail does not automatically translate into better anatomical accuracy. In superficial white matter, the FOD result can also produce orientations perpendicular to the gyrus and arc-shaped spurious tracks that are not supported by the corresponding histology.

The important distinction is therefore:

> **Better local fiber resolving does not automatically mean better global anatomical mapping.**

A sharper orientation distribution, a higher spherical-harmonic order, or a model capable of producing more peaks should not by itself be treated as evidence of better tractography.

**Practical suggestion:** use enough angular resolution to resolve the anatomy required by the scientific question, then judge the result against anatomy and the complete pathway rather than the number or sharpness of local peaks.

## Myth 2: Crossing fibers are the main tractography problem

Crossing fibers are important, but they are only one of several anatomical challenges. Different parts of the brain create very different tracking problems.

<img src="https://ars.els-cdn.com/content/image/1-s2.0-S1053811921009241-gr1.jpg" width="950" alt="White-matter anatomy and tractography challenges in superficial white matter, deep white matter, and subcortical nuclei from Yeh et al. NeuroImage 2021.">

*Figure 1 from Yeh et al., NeuroImage 2021. Histology illustrates three distinct anatomical settings that challenge tractography.*

The figure highlights three particularly useful examples:

- **Superficial white matter and gyral blades:** fibers fan toward the cortex and can turn sharply near the gray-white matter boundary. Tracking algorithms tend to favor less sharply turning trajectories, producing gyral bias and missed sulcal terminations.
- **Deep white matter:** association, projection, and commissural pathways intersect, branch, and fan in the centrum semiovale. This is the classic crossing-fiber problem, but it is only one region of difficulty.
- **Subcortical nuclei:** small pathways intermingle with large projection bundles, while limited spatial resolution and low diffusion signal can make local orientations difficult or impossible to resolve reliably.

The lesson I would keep in mind is:

> **Tractography is an anatomical inference problem, not simply an angular-resolution problem.**

Improving crossing-fiber resolution helps one part of the problem, but it does not solve gyral bias, incorrect termination, limited spatial resolution, poor signal, or ambiguous long-range routing.

**Practical suggestion:** before changing reconstruction or tracking parameters, first identify *where* the pathway is difficult anatomically. The appropriate solution depends on whether the main limitation is crossing, turning, termination, spatial resolution, signal quality, or pathway ambiguity.

## Myth 3: If the local fiber directions are correct, the pathway will be correct

Even good local orientation estimates do not guarantee a correct long-range trajectory.

<img src="/images/tutorial_tractography_error_modes.svg" width="900" alt="Three tractography error modes: angular deviation, incorrect termination, and switching between pathways.">

Three common mechanisms are:

1. **Angular deviation.** Small directional errors accumulate as a streamline propagates.
2. **Termination error.** A streamline may stop too early or continue beyond the true end of a pathway.
3. **Pathway switching.** At a crossing or branching region, a tracking algorithm can move from one real fiber population onto another real fiber population and create a connection that does not exist anatomically.

The third case is especially important: every local orientation along a false streamline can look plausible while the **complete connection is wrong**.

**Practical suggestion:** evaluate the full pathway and its anatomical endpoints, not just the local orientation field.

## Myth 4: More streamlines mean better mapping

Generating more streamlines usually increases sensitivity. It can also increase false positives.

<img src="/images/tutorial_tractography_streamlines.svg" width="900" alt="Dense tractography can increase sensitivity while also producing more false positive pathways.">

A tractogram with millions of streamlines may look impressive, but streamline count is affected by seeding strategy, stopping criteria, angular threshold, anisotropy threshold, tracking algorithm, reconstruction method, and post-processing.

Therefore:

> **Streamline count is an algorithm output, not an axon count.**

A useful tractography result balances sensitivity with anatomical specificity. For many scientific questions, a smaller set of reproducible and anatomically plausible pathways is more informative than a visually dense whole-brain tractogram.

## Practical checks I find useful

Good tractography rarely comes from one "best" reconstruction or tracking algorithm. It usually comes from several sources of information agreeing with one another.

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

I find it more useful to ask:

> **Which combination of data, reconstruction, tracking, anatomical constraints, and validation best answers this particular anatomical or scientific question?**

This also explains why higher angular resolution does not automatically win. Better local directional information is valuable, but tractography accuracy depends on the entire chain:

**diffusion acquisition → preprocessing → local orientation estimation → tracking → termination → anatomical selection → validation → interpretation**

An improvement at one step cannot automatically correct errors introduced at another.

## References

1. Yeh FC, Irimia A, Bastos DCA, Golby AJ. Tractography methods and findings in brain tumors and traumatic brain injury. *NeuroImage*. 2021;245:118651. https://doi.org/10.1016/j.neuroimage.2021.118651
2. Maier-Hein KH, Neher PF, Houde JC, et al. The challenge of mapping the human connectome based on diffusion tractography. *Nature Communications*. 2017;8:1349. https://doi.org/10.1038/s41467-017-01285-x
3. Reveley C, Seth AK, Pierpaoli C, et al. Superficial white matter fiber systems impede detection of long-range cortical connections in diffusion MR tractography. *PNAS*. 2015;112:E2820-E2828. https://doi.org/10.1073/pnas.1418198112
