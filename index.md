---
layout: home
title: DSI Studio
---
<section class="hero">
  <div>
    <a class="paper" href="https://doi.org/10.1038/s41592-025-02762-8"><b>Nature Methods 2025</b>Platform paper →</a>
    <h1>Tractography, connectomics and fiber data, in one open platform.</h1>
    <p class="lead">Reconstruct diffusion MRI, map white-matter pathways, run group analysis and segment brain MRI on Windows, macOS, Linux or Docker. No installation needed.</p>
    <div class="btns">
      <a class="btn primary" href="/download.html">Download</a>
      <a class="btn" href="/doc/how_to_analyze_dmri.html">Start the tutorial</a>
      <a class="btn" href="https://brain.labsolver.org">Browse the Fiber Data Hub</a>
    </div>
    <div class="release">Sun {{ site.sun_version }} · Windows · macOS · Linux · Docker · <a href="https://github.com/frankyeh/DSI-Studio" style="color:inherit">source on GitHub</a></div>
  </div>
  <figure class="shot" style="margin:0"><img src="/images/af.png" alt="Left arcuate fasciculus"><figcaption>Left arcuate fasciculus, DSI Studio</figcaption></figure>
</section>

<section class="stats">
  <div><b>3,000+</b><span>publications using DSI Studio</span></div>
  <div><b>50,000+</b><span>processed datasets on the Hub</span></div>
  <div><b>29</b><span>documentation guides</span></div>
  <div><b>4</b><span>platforms: Windows, macOS, Linux, Docker</span></div>
</section>

<section class="doors">
  <div class="door">
    <h3>Learn</h3>
    <p>Concepts first, then the exact clicks.</p>
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
    <p>Questions answered by the developer and community.</p>
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
    <div class="pills"><span>Codex</span><span>Claude Code</span><span>Muse</span><span>Antigravity</span><span>Grok</span></div>
  </div>
  <div class="prompt">
    <div class="pl"><span>you</span>Track the left arcuate fasciculus in this FIB file and export FA along the tract.</div>
    <div class="pl ai"><span>agent</span>Opened subject01.fz · AutoTrack AF_L · FA along tract saved (illustrative)</div>
    <div class="pl run"><span>run</span>dsi_studio --action=atk --source=subject01.fz --track_id=AF_L</div>
  </div>
</section>

<section class="section">
  <div><img src="/images/nat_rev_neuro.png" alt="Nature Reviews Neurology cover" style="max-width:300px"></div>
  <div>
    <h2>Research applications</h2>
    <p>Cited in Nature Neuroscience, Nature Human Behaviour, Brain and NeuroImage; on the cover of Nature Reviews Neurology (2017); used in diffusion MRI, tractography and structural connectomics research.</p>
    <div class="pills"><span>DTI · GQI · QSDR</span><span>AutoTrack</span><span>Differential</span><span>Correlational</span><span>U-Net segmentation</span></div>
  </div>
</section>

<section class="section">
  <div>
    <h2>Fiber Data Hub</h2>
    <p>The <strong>Fiber Data Hub</strong> extends DSI Studio from a software tool to a software-and-data platform. It provides direct access to compact processed diffusion MRI derivatives that can be downloaded and analyzed without repeating the full preprocessing pipeline from raw DWI.</p>
    <p>The Hub currently hosts <strong>more than 50,000 processed fiber datasets</strong> from major neuroimaging resources including <strong>HCP</strong>, <strong>ABCD</strong>, <strong>OpenNeuro</strong>, <strong>INDI</strong>, <strong>TCIA</strong>, and other public datasets. DSI Studio uses compact formats such as <strong>.fz</strong> and <strong>.sz</strong> for processed diffusion data, while <strong>.dz</strong> supports population databases. These formats substantially reduce storage and transfer requirements for many downstream workflows compared with repeatedly distributing the corresponding raw diffusion MRI data.</p>
  </div>
  <div>
    <p>This allows users to:</p>
    <ul>
      <li>browse and download processed datasets directly in DSI Studio;</li>
      <li>start tractography and connectomics analysis more quickly;</li>
      <li>build reproducible pipelines using standardized derivatives;</li>
      <li>scale analyses across public datasets with much lower storage and transfer cost.</li>
    </ul>
    <p>Explore the Hub at <a href="https://brain.labsolver.org">brain.labsolver.org</a>.</p>
  </div>
</section>

<section class="section">
  <div>
    <img src="/images/af.png" alt="Left arcuate fasciculus language pathway tractography">
    <p class="caption">The human language pathway: left arcuate fasciculus mapped using DSI Studio.</p>
  </div>
  <div>
    <h2>Neuroimaging research applications</h2>
    <p>DSI Studio supports diffusion MRI research across multiple datasets and imaging protocols. Its tractography workflows can be used to reconstruct white-matter pathways and examine structural connectivity.</p>
    <p>Research applications include pathway reconstruction, regional connectivity analysis, and comparisons of tractography results across imaging datasets.</p>
  </div>
</section>

<section class="section">
  <div>
    <h2>Ex-vivo and histology applications</h2>
    <p>DSI Studio can also process high-resolution ex-vivo diffusion MRI and MRI microscopy data. This supports structural analysis of biological tissue at high spatial resolution and has been used in histology-oriented imaging studies.</p>
  </div>
  <div>
    <img src="/images/KidneyRainbow.png" alt="Kidney rainbow">
    <p class="caption">Kidney Rainbow created using DSI Studio by Nian Wang, Center for In Vivo Microscopy, Duke University. Coverage by <a href="https://doi.org/10.1126/science.363.6427.564">Science</a>.</p>
  </div>
</section>

<section class="section">
  <div>
    <div class="pills"><span>U-Net segmentation</span><span>Structural MRI</span><span>No Python needed</span><span><a href="https://unet-studio.labsolver.org">U-Net Studio →</a></span></div>
  </div>
  <div>
    <h2>Brain MRI segmentation</h2>
    <p>DSI Studio now includes <strong>U-Net based segmentation</strong> for structural MRI, allowing users to run compact segmentation models directly inside the software. This extends the platform beyond tractography and connectomics to a broader structural MRI workflow.</p>
    <p>The segmentation models are designed for convenient deployment, with no need to install Python or large deep-learning environments separately. Related segmentation development is also supported by the sister project <a href="https://unet-studio.labsolver.org">U-Net Studio</a>.</p>
  </div>
</section>

<section class="section">
  <div>
    <h2>Get started</h2>
    <ul>
      <li>Download the latest release from the <a href="/download.html">download page</a>.</li>
      <li>Check recent features and bug notices on the <a href="/news.html">news page</a>.</li>
      <li>Visit the <a href="https://groups.google.com/g/dsi-studio">support forum</a> for questions and community discussions.</li>
      <li>Explore large-scale processed datasets on the <a href="https://brain.labsolver.org">Fiber Data Hub</a>.</li>
    </ul>
  </div>
  <div>
    <div class="badges">
      <a href="https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml"><img src="https://github.com/frankyeh/DSI-Studio/actions/workflows/build.yml/badge.svg" alt="Build Release"></a>
      <a href="https://github.com/frankyeh/DSI-Studio/commits/master"><img src="https://img.shields.io/github/last-commit/frankyeh/DSI-Studio" alt="Latest source commit"></a>
      <a href="https://github.com/frankyeh/DSI-Studio/releases"><img src="https://img.shields.io/github/v/release/frankyeh/DSI-Studio" alt="Latest GitHub release"></a>
    </div>
  </div>
</section>
