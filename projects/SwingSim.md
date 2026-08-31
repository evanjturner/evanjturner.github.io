---
layout: page
title: "Miscellaneous Computational Modeling"
---

### Golf Ball Flight Simulation
I developed a model of golf ball flight that accounts for air resistance and spin in a MATLAB GUI. Experimental constants were determined mathematically to mimic average benchmarks of a PGA tour player. Future work would be to provide more adjustability on initial conditions, allowing user to input club speed, direction, etc.

<span class="image fit">
    <img src="{{ '/images/fulls/GolfSim.png' | relative_url }}" alt="Golf Sim" />
</span>

<hr>

### CFD Lite
Freshman year, I worked on a project with CJ Koerber aimed to show competency in MATLAB by creating a GUI. Our goal was to create a script that generated a NACA 4-digit airfoil and ran computational fluid dynamics calculations to determine relevant characteristics of the design. We had no prior fluid mechanics or simulation design experience. This tested our capacity for grasping new concepts from literature. Most of our time was spent investigating how CFD works and methods to simplify the problem.

<!-- Gallery -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css" />
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

<div class="swiper mySwiper" style="width: 100%; height: 400px; border-radius: 8px;">
  <div class="swiper-wrapper">
    <div class="swiper-slide"><img src="{{ '/images/fulls/CFDLite1.jpg' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/CFDLite2.jpg' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
  </div>
  <div class="swiper-button-next"></div>
  <div class="swiper-button-prev"></div>
  <div class="swiper-pagination"></div>
</div>

<script>
  const swiper = new Swiper('.mySwiper', {
    loop: true,
    pagination: { el: '.swiper-pagination', clickable: true },
    navigation: { nextEl: '.swiper-button-next', prevEl: '.swiper-button-prev' },
  });
</script>

<p style="margin-top: 2em;"> </p>

#### Brief Technical Overview
- Assume: incompressible flow, constant density, and no gravity 
- Determine boundary conditions: freestream velocity along all edges of grid, zero velocity for each element within the airfoil.
- Create model: finite volume solution governed by conservation of mass and momentum, using a derived pressure equation to advance steps. Use of staggered grid decouples velocity and pressure to reduce instability.
- Arrange & solve: discretize equations to create linear system and solve for all elements in the grid with simple explicit Euler integration. Implement into MATLAB GUI.

<span class="image fit">
    <img src="{{ '/images/fulls/CFDLite3.jpg' | relative_url }}" alt="Vel Field" />
</span>

#### Results
The code was incredibly inefficient but did ‘work’. The simulation could not run for an adequate number of iterations to be remotely accurate – note the use of only 5 iterations from the setup screen. Small velocity changes around the boundary of the airfoil are visible and the coefficients of lift and drag are potentially valid values. 
<hr>
 
### Lid Driven Cavity Problem
I later decided to develop a working CFD simulation on my own with a smaller/more feasible task using similar methodology. I built a script for a lid driven cavity that is much more efficient and accurate than the CFD Lite tool (solving a much simpler problem). This example uses a 40x40 element mesh and upwards of 1500 iterations to achieve a stable result. I hope to eventually adapt this for duct and freestream flow, then attack airfoil simulation.

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/CFD_LDC_Vel.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/CFD_LDC_KE.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

