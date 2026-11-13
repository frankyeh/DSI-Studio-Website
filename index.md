---
layout: home
title: DSI Studio
---
<section class="hero">
  <div>
    <a class="paper" href="https://doi.org/10.1038/s41592-025-02762-8"><b>Nature Methods 2025</b>Platform paper →</a>
    <h1>Tractography, connectomics and fiber data, in one open platform.</h1>
    <p class="lead">Reconstruct diffusion MRI, map white-matter pathways, run group analysis and segment brain MRI.</p>
    <div class="btns">
      <a class="btn primary" href="/download.html">Download</a>
      <a class="btn" href="/doc/how_to_analyze_dmri.html">Start the tutorial</a>
      <a class="btn" href="https://brain.labsolver.org">Browse the Fiber Data Hub</a>
    </div>
    <div class="release">Sun {{ site.sun_version }} · Windows · macOS · Linux · Docker · <a href="https://github.com/frankyeh/DSI-Studio" style="color:inherit">source on GitHub</a></div>
  </div>
  <figure class="shot" style="margin:0;aspect-ratio:1600/833"><img src="/images/af.jpg" alt="Left arcuate fasciculus tractography" width="1600" height="833"><figcaption>Left arcuate fasciculus, DSI Studio rendering</figcaption></figure>
</section>

<section class="stats two">
  <div><b>3,000+</b><span>publications using DSI Studio</span></div>
  <div><b>30,000+</b><span>processed datasets on the Hub</span></div>
</section>

<section class="doors">
  <div class="door">
    <h3>Learn</h3>
    <ul>
      <li><a href="/doc/how_to_acquire_dmri.html">How to acquire dMRI</a></li>
      <li><a href="/doc/how_to_analyze_dmri.html">How to analyze dMRI</a></li>
      <li><a href="/doc/how_tractography_works.html">How to interpret tractography</a></li>
      <li><a href="/doc/gui_t1.html">GUI manual · Command line →</a></li>
    </ul>
  </div>
  <div class="door">
    <h3>Data</h3>
    <p>Start from processed .fz / .sz / .dz files instead of raw DWI.</p>
    <ul>
      <li><a href="https://brain.labsolver.org">HCP · ABCD · OpenNeuro</a></li>
      <li><a href="https://brain.labsolver.org">INDI · TCIA · others</a></li>
      <li><a href="/doc/cli_data.html">Data formats</a></li>
      <li><a href="https://brain.labsolver.org">Open the Fiber Data Hub →</a></li>
    </ul>
  </div>
  <div class="door">
    <h3>Support</h3>
    <ul>
      <li><a href="https://groups.google.com/g/dsi-studio">Forum (Google)</a></li>
      <li><a href="https://www.reddit.com/r/dsistudio/">Forum (Reddit)</a></li>
      <li><a href="https://www.dropbox.com/request/W0hJkAIVC2keWAjqtqvU">Upload data for help</a></li>
      <li><a href="/news.html">News &amp; bug notices →</a></li>
    </ul>
  </div>
</section>

<section class="section">
  <div>
    <h2>AI agents, in the Sun version</h2>
    <p>Connect a supported local agent and ask in plain language: it reads the manuals, inspects the current data and runs reconstruction, tracking, image processing or connectometry.</p>
    <div class="agent-logos" role="group" aria-label="Supported AI agents"><span class="agent-logo"><span class="agent-logo-tile"><img src="/images/agents/codex.svg" width="38" height="38" alt=""></span><span class="agent-logo-name">Codex</span></span><span class="agent-logo"><span class="agent-logo-tile"><img src="/images/agents/claude.svg" width="38" height="38" alt=""></span><span class="agent-logo-name">Claude Code</span></span><span class="agent-logo"><span class="agent-logo-tile"><img src="/images/agents/muse.svg" width="38" height="38" alt=""></span><span class="agent-logo-name">Muse</span></span><span class="agent-logo"><span class="agent-logo-tile"><img src="/images/agents/antigravity.svg" width="38" height="38" alt=""></span><span class="agent-logo-name">Antigravity</span></span><span class="agent-logo"><span class="agent-logo-tile"><img src="/images/agents/grok.svg" width="38" height="38" alt=""></span><span class="agent-logo-name">Grok</span></span></div>
  </div>
  <div class="prompt">
    <div class="pl"><span>you</span>Track the left arcuate fasciculus in this FIB file and export FA along the tract.</div>
    <div class="pl ai"><span>agent</span>Opened subject01.fz · AutoTrack AF_L · FA along tract saved (illustrative)</div>
    <div class="pl run"><span>run</span>dsi_studio --action=atk --source=subject01.fz --track_id=AF_L</div>
  </div>
</section>

<section class="research" id="research">
  <div><img src="/images/nat_rev_neuro.png" alt="Nature Reviews Neurology covers featuring DSI Studio tractography"></div>
  <div>
    <h2>Research</h2>
    <p>DSI Studio has been cited in Nature Neuroscience, Nature Human Behaviour, Brain and NeuroImage, and its tractography appeared on the cover of Nature Reviews Neurology (2017).</p>
    <div class="pills"><span>DTI · GQI · QSDR</span><span>AutoTrack</span><span>Differential</span><span>Correlational</span><span>U-Net segmentation</span></div>
    <div class="cite-card">
      <small>Nature Methods · July 2025</small>
      <span>Yeh, F.-C. DSI Studio: An integrated tractography platform and Fiber Data Hub for accelerating brain research.</span>
      <a href="https://doi.org/10.1038/s41592-025-02762-8">doi:10.1038/s41592-025-02762-8</a> · <a href="/citation.html">How to cite</a>
    </div>
  </div>
</section>

<section class="hub">
  <div>
    <h2>Fiber Data Hub</h2>
    <p>Compact processed diffusion MRI derivatives (<b style="color:#16181d">.fz</b>, <b style="color:#16181d">.sz</b>, <b style="color:#16181d">.dz</b>) that open directly in DSI Studio, without repeating preprocessing from raw DWI.</p>
  </div>
  <ul>
    <li><a href="https://brain.labsolver.org/#repository-list">Browse repositories <span>→</span></a></li>
    <li><a href="https://brain.labsolver.org/qc.html">Search by quality control <span>→</span></a></li>
    <li><a href="https://brain.labsolver.org/download.html">Download guide <span>→</span></a></li>
  </ul>
</section>

<section class="gallery">
  <figure>
    <div class="img"><img src="/images/KidneyRainbow.png" alt="Kidney rainbow"></div>
    <figcaption>
      <h3>Ex-vivo and histology</h3>
      <p>High-resolution ex-vivo diffusion MRI and MRI microscopy, for structural analysis of biological tissue.</p>
      <small>Kidney Rainbow by Nian Wang, Center for In Vivo Microscopy, Duke University. Coverage by <a href="https://doi.org/10.1126/science.363.6427.564">Science</a>.</small>
    </figcaption>
  </figure>
  <figure>
    <div class="img light"><img src="/images/unet_examples.png" alt="U-Net segmentation of mouse, marmoset and rhesus MRI"></div>
    <figcaption>
      <h3>Brain MRI segmentation</h3>
      <p>U-Net based segmentation for structural MRI runs inside DSI Studio.</p>
      <small>Segmentation examples from <a href="https://unet-studio.labsolver.org">U-Net Studio</a>, the sister project.</small>
    </figcaption>
  </figure>
</section>

<section class="start">
  <div>
    <h2>Get started</h2>
    <div class="badges">
      <a href="https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml"><img src="https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg" alt="Build"></a>
      <a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio" alt="Last commit"></a>
      <a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio" alt="Release"></a>
    </div>
  </div>
  <div class="opts">
    <a href="/download.html">Download<small>Sun {{ site.sun_version }}</small></a>
    <a href="/news.html">News<small>features · fixes</small></a>
    <a href="https://groups.google.com/g/dsi-studio">Forum<small>questions</small></a>
    <a href="https://brain.labsolver.org">Data Hub<small>datasets</small></a>
  </div>
</section>
