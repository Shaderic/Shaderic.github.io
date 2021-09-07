---
layout: project
title: Subsurface Scattering
description:
weight: 99
usemathjax: true
showInFrontPage: true
image: "/assets/Images/subsurfacescattering/SubsurfaceScattering700x300.webp"
pageintro: "Almost every material in the real world is slightly translucent, this means that light enters the material and is scattered around and then exits the surface. I have updated the Unity’s standard shader based on Dice’s implementation which presents a fast and scalable approximation of translucency for a convincing subsurface scattering look. "
---

Almost every material in the real world is slightly translucent, this means that light enters the material and is scattered around and then exits the surface.
In computer graphics, this is mostly done with ray tracing. This, however, is a very expensive computation to use in real-time so other methods have to be used. I have updated the Unity’s standard shader based on Dice’s implementation which presents a fast and scalable approximation of translucency for a convincing subsurface scattering look. 

<div class="align-content-center">
    <div class="card">
        <img class="card-img-top" src="/assets/Images/subsurfacescattering/SubsurfaceScattering1400x800.webp">
    </div>
</div>

***

### Technique Details

To simulate the basic light transport inside an object we begin by using **distance-attenuated regular diffuse lighting, combined with the distance-attenuated dot product of the view vector and an inverted light vector.** However, this does not take the thickness into account. To properly compute the light transport inside the shape we could use a local thickness map, since they allow us to compute the distance travelled by the light from the light source through the shape and to the pixel. This map uses dark values where the object is opaque and bright values for translucent. 

<script src="https://gist.github.com/Shaderic/f3720c0cb74275c3fd0d26e766e5de29.js"></script>

<br>

### Computing Local Thickness maps

The workflow of creating a local thickness map relies on a normal inverted computation of Ambient Occlusion (AO). Since AO determines how much light arrives at a surface point, we can use this information for the inside of the shape. Simply inverting the AO map gives different results than the workflow explained below. 

- Flip the normals and make sure your mesh is properly UV-unwrapped.
- Set the **Dark** color to the maximum value you want for translucency (white - max, black - min).
- Set the **Bright** color to the minimum value you want for translucency.
- The **Max distance** is the distance traveled by the rays. If you have a big object, make the distance larger. The opposite for a small object. 

<div class="align-content-center">
    <div class="card">
        <img class="card-img-top" src="/assets/Images/subsurfacescattering/AOvsIAOvsThickness_1050x600.webp">
        <div class="card-body">
            <div class="card-text">The difference between thickness map (left), ambient occlusion (middle) and the
                inverted ambient occlusion (right)
            </div>
        </div>
    </div>
</div>

<br>

### References

[Approximating Translucency for a Fast, Cheap and Convincing Subsurface Scattering Look](https://colinbarrebrisebois.com/2011/03/07/gdc-2011-approximating-translucency-for-a-fast-cheap-and-convincing-subsurface-scattering-look/){:target="_blank"}   
[Creating Thickness maps in 3ds max](https://colinbarrebrisebois.com/2011/04/04/approximating-translucency-part-ii-addendum-to-gdc-2011-talk-gpu-pro-2-article/){:target="_blank"}