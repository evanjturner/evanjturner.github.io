---
layout: page
title: "Virtual Surgery Modeling"
---

### Project Overview
The Fontan procedure is the third surgery in a series that is used to treat a congenital heart defect where the child has only one working ventricle. The first surgery, often the Norwood procedure, occurs within days of birth and reconstructs the underdeveloped aorta to ensure oxygenated blood reaches the body. A pathway between atria is also opened to allow oxygenated blood to make its way into the single working ventricle, where it can be pumped to the rest of the body. The next surgery, the Glenn procedure, occurs within 3-6 months and connects the superior vena cava (carrying deoxygenated blood from upper body) directly to the pulmonary arteries (going to lungs), relieving the burden on the single working ventricle. The Fontan procedure is then performed at 2-5 years of age, connecting the inferior vena cava (carrying deoxygenated blood from lower body) to the pulmonary arteries and completing the separation of oxygenated and deoxygenated blood. Assuming all goes well, the patients are expected to live a somewhat normal life with a reduced cardiac capacity and standard monitoring.  

The Norwood and Glenn procedures primarily use native tissues for reconstruction, which generally can grow with the patient as they age. The Fontan procedure, however, is typically performed with a synthetic conduit, and therefore cannot grow with the patient as they age. 

**Aim:** Perform patient-specific virtual surgery to determine whether this individual would benefit from upsizing the Fontan conduit in regard to improving hepatic flow distribution.

### Input Data
This patient had undergone MRI and CT imaging sessions, along with a catheter lab, which provided all the data necessary to be able to closely approximate the fluid dynamics within the anatomy. Specifically, CT images were used to generate the 3D geometry of the Fontan junction, which includes the pulmonary artery and Glenn vessel (as labeled in the schematic on the left below). An interactive view of the simplified 3D geometry is shown on the right (oriented just like the schematic), where you can see that this junction is not symmetrical.

<div class="row" markdown="0">
    <div class="6u 12u$(small)">
        <img src="{{ '/images/fulls/FontanFig.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="6u$ 12u$(small)">
        <div id="three-container" style="width: 100%; height: 400px; background: #ffffff; border-radius: 4px;"></div>
    </div>
    <script type="importmap">
    {
      "imports": {
        "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
        "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
      }
    }
    </script>
    <script type="module">
        import * as THREE from 'three';
        import { STLLoader } from 'three/addons/loaders/STLLoader.js';
        import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
        const container = document.getElementById('three-container');
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0xffffff);
        const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 2000);
        scene.add(camera);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(renderer.domElement);
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.screenSpacePanning = true;
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
        scene.add(ambientLight);
        const cameraLight = new THREE.DirectionalLight(0xffffff, 0.8);
        cameraLight.position.set(0, 0, 1);
        camera.add(cameraLight);
        const loader = new STLLoader();
        loader.load(
            "{{ '/assets/models/FontanModel.stl' | relative_url }}",
            (geometry) => {
                geometry.computeBoundingBox();
                geometry.center();
                geometry.rotateX(-Math.PI / 2);
                const box = geometry.boundingBox;
                const maxDim = Math.max(box.max.x - box.min.x, box.max.y - box.min.y, box.max.z - box.min.z);
                const fov = camera.fov * (Math.PI / 180);
                let cameraZ = Math.abs(maxDim / 2 / Math.tan(fov / 2)) * 1.2;
                // Position camera along front Z axis (equator view)
                camera.position.set(0, 0, cameraZ);
                camera.near = cameraZ / 100;
                camera.far = cameraZ * 100;
                camera.updateProjectionMatrix();
                controls.target.set(0, 0, 0);
                controls.update();
                const material = new THREE.MeshPhongMaterial({ color: 0xA9A9A9, specular: 0x222222, shininess: 30 });
                const mesh = new THREE.Mesh(geometry, material);
                scene.add(mesh);
            },
            undefined,
            (error) => {
                console.error("Error loading STL model:", error);
            }
        );
        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }
        animate();
        window.addEventListener('resize', () => {
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        });
    </script>
</div>

4D Flow MRI is a technique that estimates the transient flow within an anatomy averaged across many cardiac cycles. For this case, results from a 4D flow scan were used as reference for the flow within each of the branches of the anatomy, providing the inlet boundary conditions for fluid dynamics simulations as well as targets to tune the outlet against. Based on the data from the 4D flow, and since the Fontan and Glenn are so far distal in the vasculature, a steady flow approximation was deemed adequate for this work. 

<div class="row">
    <div class="6u 12u$(small)">
        <p> A visualization of this 4D flow data is shown on the right, where total outflow is labeled for each pulmonary artery and hepatic flow distribution is labeled under Fontan. A roughly 50/50 split for the hepatic flow distribution would be ideal, however, our data suggests this is not the case. It should also be noted that while 4D Flow MRI is a powerful imaging tool, it does not inherently abide by physical laws such as conservation of mass nor have an incredibly high spatial resolution, which is why computational fluid dynamics (CFD) has been used in past studies to compliment it.</p>
    </div>
    <div class="6u$ 12u$(small)">
        <p> </p>
        <img src="{{ '/images/fulls/4DFlow.png' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

Catheter data, specifically pressure measurements using a pressure catheter within each of the regions of the anatomy, made up the last of the data necessary to simulate this patient's flow. Using the flow through each vessel and the pressure within, a reduced-order model can be tuned for each of the outlets to represent the downstream effects that would impact flow in the simulated region. For this case, I used 3rd order Windkessel (RCR) models, which include 2 *resistors* and 1 *capacitor* to model the transient flow effects. Tuning the models meant that I systematically adjusted the resistance and capacitance values of these elements until I found the values that yielded the same flow distribution and pressures measured from our input data (4D flow MRI and catheter data).

### Study Structure
<p style="margin-bottom: 0.2rem;">The methodology was as follows:</p>
<ol style="margin-top: 0;">
  <li>Extract anatomical geometry/inputs from CT/MRI</li>
  <li>Generate parametric model from the geometry to allow conduit upsizing</li>
  <li>Tune RCR parameters and simulate the patient's current state</li>
  <li>Simulate virtual surgery option 1 of upsizing diameter by 10%</li>
  <li>Simulate virtual surgery option 2 of upsizing diameter by 25%</li>
  <li>Compare hepatic flow distribution for all options, present results to the cardiac surgeons</li>
</ol>

### Brief Methods
Image segmentation, where the geometry was extracted, was performed semi-automatically in Materialise Mimics and a parametric model was traced from the generated 3D object in SolidWorks. 4D Flow MRI data was analyzed with MATLAB and Paraview, where the inflow/outflow for all the vessels was calculated and hepatic flow distributions could be determined. A mesh refinement assessment using the inflow boundary conditions and zero-resistance outlet was then conducted to determine the proper grid size that balanced accuracy with computational demand. After a proper grid size was selected, simulations of the patient's current state were performed in SimVascular (an open-source vascular CFD package), using the total flow through each pulmonary artery and catheter pressure to tune RCR models. 

### Results
Once properly tuned, the hepatic flow distribution from the current state could be analyzed and compared to the 4D flow hepatic distribution result as a sanity check. Here we see that the simulated flow distribution matches the result seen in 4D flow well. The Fontan conduit was then upsized to simulate the virtual surgical outcome, where CFD was performed with the same RCR parameters as used in the original case. Upsizing of 10% and 25% by diameter was performed, with the results shown below. 

<!-- Gallery -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css" />
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>

<div class="swiper mySwiper" style="width: 100%; height: 400px; border-radius: 8px;">
  <div class="swiper-wrapper">
    <div class="swiper-slide"><img src="{{ '/images/fulls/Fontan_R_Orig.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Fontan_R_10p.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
    <div class="swiper-slide"><img src="{{ '/images/fulls/Fontan_R_25p.png' | relative_url }}" style="width:100%; height:100%; object-fit:contain;"></div>
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

<p style="margin-top: 2em;"> As you can see, these results suggest the upsizing itself does not provide much of an impact on the hepatic flow split, which makes sense as our virtual surgery does not significantly alter the geometry of the Fontan junction. While upsizing the conduit does reduce the power loss (a higher power loss indicates a less efficient junction), our work suggests that the pure upsizing without altering the conduit direction into the junction would have minimal impact on equalizing hepatic flow distribution between the left and right pulmonary arteries in this specific patient. Based on this work and consultation with a research group at another hospital, the surgeons decided not to upsize the Fontain conduit for this patient. </p> 



