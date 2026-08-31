---
layout: page
title: "Intradiscal Injection (B.S. Thesis)"
---

### Project Overview
Intervertebral discs (IVD’s) separate each vertebrae in the spine. The lack of blood supply to the IVD combined with its primary function of shock absorption and rotational mobility leads to natural deterioration with age. Intradiscal injections are the primary non-invasive means to introduce therapeutics to the damaged tissue, but there is no well-defined clinical protocol for the technique. 

**Aim:** Validate a through-puncture model for ex-vivo assessment of intradiscal injection leakage and characterize the impact of post-injection movement on the injectate retention.

### Through-Puncture Model

<div class="row">
    <div class="6u 12u$(small)">
        <p>The through-puncture model was designed to accurately mimic the behavior of pressurized injectate following needle retraction during an intradiscal injection procedure while providing the ability to measure injectate pressure in real time. <br><br> As shown in the schematic, a needle is inserted through the entirety of the IVD then retracted to the center, after which, injection can be performed and leakage observed out the other side.</p>
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/ThroughPuncture.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

A study injecting fluorescent dye into a caudal bovine disc was performed to test the hypothesis that injectate would not escape around the needle through its defect pathway but would rather exit the IVD in the through puncture "leakage" pathway. Results from this study (below) showed substantial deposition of dye on the leakage side and minimal staining on the insertion side, therefore suggesting that this model can accurately mimic the mechanics of needle retraction while keeping the needle inside the IVD for real-time pressure monitoring. 

<span class="image fit">
    <img src="{{ '/images/fulls/ThruPuncValidation.png' | relative_url }}" alt="Through Puncture Validation" />
</span>

### Forward Flexion
Forward flexion of the spine is the motion required for a patient to sit up following an intradiscal injection procedure and would place the posterior (injection side) of the IVD in tension, thus logically "opening up" the puncture path for potential injectate leakage. To test this hypothesis and characterize flexion degree associated with potential leakage risk, I designed a custom fixture and conducted an ex-vivo experiment.

<div class="row">
    <div class="7u 12u$(small)">
        <p style="margin-bottom: 0.2rem;">Design specifications:</p>
        <ul style="margin-top: 0;">
            <li>Rapid and secure fastening of specimen</li>
            <li>~15 degrees of flexion</li>
            <li>Record flexion angle directly into existing niDAQ </li>
            <li>Axial compression of specimen with adjustable loading</li>
            <li>Anterolateral access for needle insertion</li>
            <li>Low cost and quickly buildable</li>
        </ul>
        <p> Specimens were clamped to two 3D printed specimen mounting cups that were free to pivot along the transverse. Mounting cups were connected by a leadscrew, the rotation of which flexed the specimen. The flexion angle was captured by a calibrated potentiometer positioned between the cups. A weight on the top pivot arm was adjusted to provide constant resting stress on the IVD. </p>
    </div>
    <div class="5u$ 12u$(small)">
        <p > <br> </p>
        <img src="{{ '/images/fulls/FixtureSchematic.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p >I designed the fixture in SolidWorks, then sourced the hardware and manufactured the custom specimen mounting cups. On the left is the CAD model of the fixture, while on the right is the finished instrumentation for the study (including the previously designed injector). A scale model of a specimen is held within the fixture. </p>

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/FixtureCAD.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/FlexionFixture.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> The through puncture method was sensitive to forward flexion. Of nine specimens, two experienced a sudden drop in pressure during the flexion phase indicating leakage (at  6.0° and 6.7°). </p> 

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/NoLeak.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Leak.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> Although larger studies are needed before any claims can be drawn, this work provides a foundation for assessing intradiscal injection leakage ex-vivo that allows for screening of potential risk factors. </p>

For more information on this project feel free to check out our [paper](https://doi.org/10.1007/s00586-022-07140-y) published into the European Spine Journal!

