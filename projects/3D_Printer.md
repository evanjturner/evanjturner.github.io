---
layout: page
title: "Custom Engineered 3D Printer (Personal Project)"
---

### Project Overview
I was introduced to CAD and manufacturing in a high school engineering course, where after zipping ahead on the curricular work, I was given the opportunity to undertake my own project. Being fascinated with the CNC machines we had the privilege to use, I intended to design my very own CNC using inexpensive, easily sourced parts found at a hardware store. I quickly realized why this accessibility aspect was not feasible for a machine that moves a heavy, quickly spinning router bit, and instead switched gears to something more practical: depositing melted plastic with 3D Printing. 

In this page I will go through a selection of the countless 3D Printer designs I have iterated through since 2020 and share the motivation/design choices that brought about each.

### Leadscrew Printer

As mentioned, I originally aimed to build a machine from mostly parts that could be sourced at any generic hardware shop or 3D printed (from a friend or the school), both for ease of access and cost. I was lucky enough to be gifted some aluminum T-slot extrusion to start this project, so I had a rigid base to build on. 

However, this accessibility goal led me to a somewhat limited motion system. Before realizing that the standard of early hobbyist machines was v-slot bearings, I discovered that roller blade bearings actually fit perfectly into the slots of my 1" extrusions, and with some careful tolerancing, I was able to make smooth-rolling carriages. I wanted my machine to be driven entirely by leadscrews, but since my hardware shop didn't carry metric leadscrews and I was determined to keep things local/cheap, I opted to use ACME 3/8" threaded rods with a nut. The limitations of these two design choices would be addressed in future iterations. I sourced the electrical components for my machine mostly based on price and simplicity (and only longevity for my 32-bit mainboard), buying parts online such as the stepper motors, power supply, PTFE hotend, metal bowden extruder, build plate, rep-rap LCD, etc. 

<span class="image fit">
    <img src="{{ '/images/fulls/Printer_01_LS.png' | relative_url }}" alt="LS Printer" />
</span>

I unfortunately don't have any actual photos of this build so the CAD model will have to suffice, but I was able to construct the printer, set up the electronics, and build the firmware enough to see that the motion system did "work". However, I ran into a few mechanical issues that prevented me from even attempting to print: the "leadscrews". It was extremely difficult to align them as they weren't straight, and coping with these imperfections at certain ends of the travel distance required too much torque. Not to mention the horizontal axes had significant backlash that would also reduce print efficacy. 

After many revisions to try to provide more flexibility for leadscrew alignment, I reluctantly redesigned the printer to incorporate belts into the X and Y axes.
 
### Belt Printer
Driving the X and Y axes with belts made printer movement much smoother and easier on the machine. Alignment errors could be "absorbed" by the belt path and belt tension could be maintained manually to ensure the printer didn't miss steps. 

<div class="row">
    <div class="7u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_02_BD_CAD.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="5u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_02_BD_Pic1.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

With the motion system working adequately, I was able to test out toolhead/extrusion capabilities and complete my first print (partially)! As you see below on the left, the print stopped part-way through and continued to extrude a blob on the top layer. Once I sorted this out, I was able to print the following calibration cube: my first full print!

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_02_BD_P1.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_02_BD_P2.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> It's clear from the images that my machine was severely over-extruding, and it turns out this was because I actually doubled the extruder steps/mm value in firmware. I did not catch this until the next full redesign of the printer, which I worked on while I was away from home for a semester. Before this, however, I made a few modifications to the machine using laser cut parts to simplify the X and Z carriages (saving me printing time) while I troubleshooted the extrusion issues. A gallery of photos from this sub-iteration are shown below. </p>

<!-- Gallery -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css" />
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

<div class="swiper mySwiper" style="width: 100%; height: 400px; border-radius: 8px;">
  <div class="swiper-wrapper">
    <div class="swiper-slide"><img src="{{ '/images/fulls/Printer_02_G1.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Printer_02_G2.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Printer_02_G3.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Printer_02_G4.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Printer_02_G5.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
  </div>
  <div class="swiper-button-next"></div>
  <div class="swiper-button-prev"></div>
  <div class="swiper-pagination"></div>
</div>

<script>
  const swiper = new Swiper('.mySwiper', {
    loop: true,
    navigation: { nextEl: '.swiper-button-next', prevEl: '.swiper-button-prev' },
  });
</script>

<p style="margin-top: 2em;"> </p>

### Next Belt Printer
At this point, I began to shift the objective of this project from using accessible materials to optimizing for better quality while still being affordable to a college student. I knew I wanted to reduce the footprint and make the machine more rigid, which I accomplished by lifting and shifting back the Y axis, allowing me to close the front of the frame and more securely fasten the Y extrusions. I also added T8x8 metric leadscrews for the Z axis to fully replace the ACME rods, reducing the wobbling experienced during upward travel and seen as layer lines on the prints. I also added a stronger part cooling fan to try to improve part surface quality, however, it was during this time that I discovered my mis-calculated extrusion steps value, and this turned out to be the main issue. 

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_03_CAD.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_03.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> With these improvements, I had printer that I could be proud of! Below I show the comparison between my first 3DBenchy print on the left and the same model after tuning/etc on the right. </p>

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_03_B1.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_03_B2.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> I was happy with this configuration for a while but came to recognize that steel bearings in an aluminum extrusion slot was not a fantastic solution for this machine's motion long term. It wore on the aluminum extrusions, causing aluminum shavings to fall off, and produced a somewhat annoying sound, especially during fast travel movements. I started to investigate what my next major motion system upgrade to this machine should entail. </p>

### Linear Rail Printer
I decided to pursue linear rails for the motion system for this next printer design for multiple reasons. While linear rods would be slightly less expensive and easier for maintenance, linear rails would provide more design flexibility and future proofing. I felt my printer would be more original and distinguishable from the Prusa line and widespread clones, while also being more rigid.

My first design intended to use 5 linear rail assemblies (shown in the CAD model below on the left), with modular components that bolt together to form the Z carriage. However, I started with upgrading only the X and Y axis to linear rails as the Z travels much less (and slower) and I wanted to make sure things functioned properly before getting the last set of linear rails. This mid-design iteration is shown in the image below on the right.  

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_04_CAD1.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_04_P1.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> The linear rails for X and Y made the toolhead and bed feel even more stable and smooth, while providing a more appealing look (to me). I noticed that the bearings on aluminum extrusion weren’t nearly as destructive on the Z axes, as expected, and this intermediate step allowed me the chance to see aspects of the X carriage that I would like to improve, specifically with the X belt path. I wanted to clean up how the X and E motors were located within the layout and make it so I could more easily adjust the belt tension, and additionally I could improve the mounting of the leadscrew nut to the Z carriage. Instead of moving forward with the rest of the CAD design I show above, I decided to pivot slightly and more directly address these limitations while also finishing up the linear rail transition entirely. </p>

For this, I reimagined the Y axis to use only a single linear rail, redesigned the Z carriages to be a single piece with attachment for the leadscrew nuts integrated into them, and moved the X axis belt path directly under the X extrusion. With adjustable brackets to hold the X stepper motor and idler pulley on their respective sides of the machine, the X carriage became more compact and super easy to modify belt tension. I later realized that this design could be improved even further by transitioning the X axis to front-mounted linear rails, so I did this before I got any pictures of the top-mounted rail setup (notice how the CAD model on the left differs from the front-mounted rail setup you see in the actual picture on the right). 

<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_05_CAD.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/fulls/Printer_05_FM.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> This change drastically improved the rigidity of the nozzle relative to the X axis. </p>

You might also notice the raspberry pi mounted to the back of the frame in the photo on the right. This was because I also made the switch to a Klipper firmware system, which gave me remote control of the printer, eliminating the need for the LCD, and also opened the doors to many more advanced features that were at this point standard on all current commercial printers. Klipper offloads the heavier computation tasks such as kinematic calculations from the 3D printer's mainboard to a single board computer (raspberry pi), leaving the mainboard to focus solely on the basic control tasks. This allows the machine to run much faster and use advanced control schemes such as input shaping, which performs vibration cancelling on the motion commands to improve print quality at higher speeds/accelerations. Having the firmware attached to the raspberry pi also allows for online connectivity with the machine, making uploading prints and configuration changes much easier.  

### Current Printer
The main driver behind my recent design choices has become print speed. The 3D Printing space has exploded in recent years, with cheap bedslingers and CoreXY machines now able to print literally 10x faster than what the industry standard was when I first started building my printer. I want my machine to be at least on a comparable playing field with the bedslinger printers that are currently in the space.

To do this, I needed to focus first on the extrusion system, as the "upgraded" Creality hotend and extruder I originally purchased simply cannot keep up with the flow rates needed to push 100+ mm/s speeds. I went with a TZ4-E3 clone hotend and a BMG clone extruder, which together can reach 25+ mm<sup>3</sup>/s flow rates without problem. I also added double shear support on the X and Y stepper motors and idler pulleys which should remain stiff and allow me to maintain higher belt tension without harming the components. I shifted the Z stepper motors inward and dropped them down, allowing me to mount the Z linear rails on the inside of the frame.  


<div class="row">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/Printer_07_CAD.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <img src="{{ '/images/thumbs/Printer_Current.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>


<p style="margin-top: 2em;"> I now run this machine at 200 mm/s speed and 3500 mm/s<sup>2</sup> acceleration, which feels respectable enough for a personally engineered machine vs other bedslingers that can go closer to 300 mm/s at 5k mm/s<sup>2</sup> or more, but I will always want to make this faster. My extrusion system can certainly handle faster speeds; however, the main limiting factor is the mass of the Y axis. With a low resonant frequency of 34.8 Hz, I need to make it lighter and maintain stiffness in order to push higher accelerations that warrant any faster print speeds. </p>


<script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.4.0/model-viewer.min.js"></script>
<!-- Trigger Button -->
<button class="button" onclick="document.getElementById('modelDialog').showModal()">View 3D Model</button>

<!-- Built-in Browser Dialog -->
<dialog id="modelDialog" style="border: none; border-radius: 8px; padding: 20px; max-width: 650px; width: 90%;">
  <form method="dialog" style="text-align: right; margin-bottom: 10px;">
    <button style="padding: 0 10px; line-height: 1;">&times;</button>
  </form>
  <model-viewer
    src="{{ '/assets/models/Printer2.glb' | relative_url }}"
    alt="Project 3D Model"
    camera-controls
    auto-rotate
    shadow-intensity="1"
    style="width: 100%; aspect-ratio: 1 / 1; height: auto; border-radius: 4px; background-color: #ffffff;">
  </model-viewer>
</dialog>


Here are some close up examples of parts I've printed: 

<!-- Gallery -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css" />
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>
<style>
  .mySwiper { width: 100%; height: 400px; border-radius: 8px; }
  .swiper-slide { position: relative; }
  .swiper-slide img { width: 100%; height: 100%; object-fit: contain; }
  .slide-caption {
    position: absolute; bottom: 0; left: 0; width: 100%;
    background: rgba(0, 0, 0, 0.65); color: #ffffff;
    text-align: center; padding: 10px 0; font-size: 14px; z-index: 5;
  }
</style>
<div class="swiper mySwiper">
  <div class="swiper-wrapper">
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG1.png' | relative_url }}"><div class="slide-caption">3D Benchy</div></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG2.png' | relative_url }}"><div class="slide-caption">3D Benchy</div></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG3.png' | relative_url }}"><div class="slide-caption">Max Flow Test: Pressure build up in the nozzle leads to overextrusion at the end of each layer at high flow without pressure compensation. Ringing is also very visible. This reached 300mm/s without any sign of underextrusion </div></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG4.png' | relative_url }}"><div class="slide-caption">Y Stepper Mounting Bracket: You can see ringing next to the mounting holes (this print came before I implimented input shaping) </div></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG5.png' | relative_url }}"><div class="slide-caption">Car Water Bottle Holder: Surface quality isn't great but that can be tuned </div></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/PG6.png' | relative_url }}"><div class="slide-caption">Z Carriage Brackets for an Intermediate Design</div></div>
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

<p style="margin-top: 2em;"></p>
Check out my [Lithophane Project](/projects/LithoGen.html) for a cool example of things I print.

### Future 
I've also dreamed up a custom CoreXY printer that I could build with most of the same parts from this machine, but it still needs some work before I would consider tearing down my current machine to give it a completely different look. This CoreXY machine employs a different kinematic system where the bed moves up and down while the X and Y axis remain stationary. This reduces the moving mass significantly versus that of a bedslinger, as the X and Y axis both can handle aggressive acceleration as they share the same lightweight toolhead as their moving mass. 

<span class="image fit">
    <img src="{{ '/images/fulls/Printer_CoreXY.png' | relative_url }}" alt="CoreXY Printer" />
</span>


