---
layout: page
title: "Lithophane Generator (Personal Project)"
---

### Project Overview
Lithophanes are thin translucent models that show a detailed image when light is shined through them. While there are multiple sites on the internet that will import a 2D image to generate a 3D lithophane model, I wanted to build my own local python script to accomplish this. 

Below is an example of a lithophane I generated using this script, specifically made to slot into an illumination stand I designed. On the left is the raw image, in the middle is the 3D printed model, and on the right is the illuminated lithophane. 

<div class="row">
    <div class="4u 12u$(small)">
        <img src="{{ '/images/fulls/LithoImage.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="4u 12u$(small)">
        <img src="{{ '/images/fulls/LithoDark.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
    <div class="4u$ 12u$(small)">
        <img src="{{ '/images/fulls/LithoLight.jpg' | relative_url }}" alt="Project Image" style="width: 100%; border-radius: 4px;" />
    </div>
</div>

<p style="margin-top: 2em;"> </p>
### Code Pipeline
1. Parse in physical parameters of the model
2. Resize/resample the image and add a frame
3. Convert brightness to depth
4. Generate watertight triangular mesh 
5. Export as STL

While simple, this was a nice python exercise that allows me to quickly generate consistent lithophanes that could fit into a uniform stand. Here are a few of the lithophanes that I've printed:

<p style="margin-top: 2em;"> </p>

<span class="image fit">
    <img src="{{ '/images/fulls/LithoAll.png' | relative_url }}" alt="Lithophane Lit" />
</span>

### Code
```pyhton
# Custom Lithopane Generator

import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from skimage.color import rgb2gray
import trimesh
import os
import argparse

# Parse in Parameters
parser = argparse.ArgumentParser('LithoGen')
parser.add_argument("filename")
parser.add_argument('-r','--resolution',type=float, default=0.1,help="Resolution (mm/pixel)")
parser.add_argument('-ht','--height', type=float, default=82.0,help="Height (mm)")
parser.add_argument('-w', '--width',type=float, default=82.0,help="Width (mm)")
parser.add_argument('-d', '--depth',type=float, default=3.0,help="Depth (mm)")
parser.add_argument('-md', '--minDepth',type=float, default=0.6,help="Minimum Depth (mm)")
parser.add_argument('-ft', '--frameThickness',type=float, default=1.0,help="Frame Thickness (mm)")
args = parser.parse_args()

# Calculate Image Dimensions based on Resolution
resamp_img_dim = round((args.width-2*args.frameThickness)/args.resolution + 1) , round((args.height-2*args.frameThickness)/args.resolution + 1)

# Open Image in Grayscale Enhance and Resize
img = Image.open(args.filename)
img_resized = rgb2gray(np.asarray(img.resize(resamp_img_dim,Image.Resampling.LANCZOS)))

# Add Frame to Image
frame_width = round(args.frameThickness/args.resolution)
img_wFrame = np.pad(img_resized,pad_width=frame_width, mode='constant',constant_values=0)
img_wFrame = img_wFrame[::-1, :]
# plt.figure()
# plt.imshow(img_wFrame,cmap='gray')

# Calculate Dimensions of New Image
output_dim = np.shape(img_wFrame)
N = output_dim[0]*output_dim[1]

# Calculate all Coordinates
Z = np.round((1-img_wFrame)*(args.depth-args.minDepth) + args.minDepth,decimals=3)
x = np.arange(output_dim[1])*args.resolution
y = np.arange(output_dim[0])*args.resolution
X,Y = np.meshgrid(x,y)
# plt.figure()
# ax = plt.subplot(projection='3d')
# ax.plot_surface(X,Y,Z,cmap='gray')
# plt.axis('equal')
# ax.view_init(elev=90, azim=90)

########## STL GENERATION ##########
# Point List
coord_front = np.stack([X, Y, Z], axis=-1).reshape(-1, 3)
ind = np.arange(N).reshape(output_dim[0],output_dim[1])

# Extract Perimeter (CW)
top = ind[0, :]
right = ind[1:, -1]
bottom = ind[-1, :-1][::-1]
left = ind[1:-1, 0][::-1]
perim_ind = np.concatenate([top,right,bottom,left])
N_PERIM = len(perim_ind)

# Generate Back Coords and Combine All
coord_back = coord_front[perim_ind].copy()
coord_back[:,2] = 0
coords = np.vstack([coord_front, coord_back, ])

# Generate Front Faces
tl = ind[:-1, :-1].flatten()    # Pick indexes of each point on triangle
tr = ind[:-1, 1:].flatten()
bl = ind[1:, :-1].flatten()
br = ind[1:,1:].flatten()
faces_front = np.vstack([ np.stack([tl,bl,tr],axis=-1), np.stack([tr,bl,br],axis=-1) ])

# Generate Side Faces
u_front = perim_ind             # Pick indexes of each point on triangle
v_front = np.roll(u_front,-1)
u_back = np.arange(N, N+N_PERIM)
v_back = np.roll(u_back, -1)
faces_sides = np.vstack([ np.stack([u_front, v_front, v_back],axis=-1), np.stack([u_front, v_back, u_back],axis=-1)])

# Generate Back Face
back_anch = np.full(N_PERIM -2, N)      # Pick Indexes of each point on triangle
back_p1 = np.arange(N+1, N+N_PERIM-1)
back_p2 = np.arange(N+2, N+N_PERIM)
# Grab triangles composing each quad
faces_back = np.stack([back_anch,back_p1,back_p2],axis=-1)

# Assemble all faces
all_faces = np.vstack([faces_front, faces_sides, faces_back])
mesh = trimesh.Trimesh(vertices=coords, faces=all_faces)
mesh.fix_normals()

# Determine Filename and Export
fname,ext = os.path.splitext(args.filename)
mesh.export(fname+".stl")
print("STL Exported!")
```


