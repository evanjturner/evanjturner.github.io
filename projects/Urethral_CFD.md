---
layout: page
title: "Urethral Stricture CFD (M.S. Project)"
---

### Project Overview
Urethral stricture disease (USD) is common among aging male populations, often leading to urinary tract infections and incontinence before surgical treatments are considered. Multichannel urodynamics studies, where a pressure catheter is inserted into the urethra, are the primary means to identify lower urinary tract dysfunction but are not possible with urethral stricture or obstruction. Retrograde urethrography is the current gold standard for monitoring/diagnosing USD, however, it is invasive and provides little anatomical information. 

**Aim:** Develop a non-invasive MRI-based computational fluid dynamics (CFD) tool to comprehensively assess the lower urinary tract in USD patients. 

### Previous Work
Previous work in the lab has developed a Urodynamic MRI protocol, which allows dynamic 3D imaging of the lower urinary tract throughout the course of a void. To briefly summarize, a male subject is injected with gadolinium contrast, then equipped with a condom catheter that drains into a urine collection bag strapped to their leg. The subject is instructed to lay in the MRI scanner and void without strain during the imaging session. A tuned 3D MRI sequence collects a stack of images that encompass the entire lower abdomen every ~5s (depending on the specific sequence). The regions on interest in these image stacks can then be manually or semi-automatically traced and used to generate a 3D volume in a process called segmentation. By segmenting the bladder, a change in bladder volume over time can be used to determine the urinary flow rate with sufficient accuracy.  

### Methods
In addition to segmenting the bladder to obtain the urinary flow rate, the urethral geometry also needed to be segmented to simulate flow through each urethra. Slice by slice segmentation of the urethra is shown below on the left, while the resulting geometry is shown on the right.

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Seg1.gif' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Seg2.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> For the purposes of this study, only the geometry from the time point at the maximum flow rate was used. CFD was performed in SimVascular (an open-source cardiovascular modeling tool) as much of the same principles apply for modeling the lower urinary tract as cardiovascular vessels. The urinary flow rate (calculated from change in bladder volume over time) was imposed as the transient volumetric inflow, while a zero-pressure outlet was imposed at the outlet since the urinary tract exits to the atmosphere. A mesh refinement assessment was conducted to ensure results were independent of mesh resolution. Pressure, velocity, and wall shear stress (WSS) were calculated natively in SimVascular and visualized in Paraview. </p>

### Results
<div class="row">
    <div class="6u 12u$(small)">
        <p> In this study, 4 subjects were recruited: 3 healthy and 1 USD. The urinary flow rate from Urodynamic MRI for each subject is shown on the right, where it can be observed that the void time varies for each. In the clinic, USD patients are generally characterized by reduced flow, which our result agrees with here. The healthy subjects exhibit a range of flow expressions, and in our case, all are characterized by a higher maximum and average flow, while also voiding overall more volume (area under the curve) than the USD patient. </p>
    </div>
    <div class="6u$ 12u$(small)">
        <p> </p>
        <img src="{{ '/images/fulls/UroFlow.png' | relative_url }}" alt="UroFlow" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> We extracted the spatial maximum and spatially averaged pressure and WSS values for each subject throughout the void and plotted them against standardized time for visual comparison (on left: max on top, mean on bottom). Values were normalized to maximum flow (middle) and average flow (right). </p>

<!-- Gallery -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css" />
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

<style>
  .mySwiper { width: 100%; height: 400px; border-radius: 8px; }
  .swiper-slide { position: relative; }
  .swiper-slide img { width: 100%; height: 100%; object-fit: contain; }
</style>

<div class="swiper mySwiper">
  <div class="swiper-wrapper">
    <div class="swiper-slide">
      <img src="{{ '/images/fulls/UroP.png' | relative_url }}">
    </div>
    <div class="swiper-slide">
      <img src="{{ '/images/fulls/UroWSS.png' | relative_url }}">
    </div>
  </div>

  <div class="swiper-button-next"></div>
  <div class="swiper-button-prev"></div>
</div>

<script>
  window.addEventListener('load', function () {
    new Swiper('.mySwiper', {
      loop: true,
      navigation: { nextEl: '.swiper-button-next', prevEl: '.swiper-button-prev' },
    });
  });
</script>

<p style="margin-top: 2em;"> Notice how as you move from left to right through the plots, the healthy subjects all display a similar behavior relative to each other, however, the USD patient becomes more distinguishable relative to the others. Regarding WSS, we see that walls of the urethra for the stricture patient experience much more shearing relative to its lower flow than for the healthy subjects. </p>

Investigating this further, we can see in the animation below that most of the WSS/Q<sub>avg</sub> is very small over the course of a void, but higher values appear during peak flow and are concentrated near constricted regions. See how healthy subject 2 has no visible areas of high relative WSS, while the USD patient cycles constantly in the stricture region. 

<span class="image fit">
    <img src="{{ '/images/fulls/WSS.gif' | relative_url }}" alt="WSS/Qavg" />
</span>

This work shows that dynamic imaging combined with CFD techniques used in cardiovascular assessment can be applied to non-invasive urodynamics, allowing biomechanical assessment of the lower urinary tract, which is especially important for USD patients who cannot undergo multichannel urodynamic studies. This study also suggests that regional flow variations may be visible between healthy and diseased cohorts, with normalization to flow rate being a potentially viable characterization technique (if shown in a larger cohort study). 


