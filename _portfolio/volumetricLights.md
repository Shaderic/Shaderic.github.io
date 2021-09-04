---
layout: project
title: Volumetric Lighting
description:
weight: 100
usemathjax: true
showInFrontPage: true
displayImage: /assets/Images/VolumetricLights/VolumetricLight700x300.webp
pageintro: "Volumetric light effects are caused by light being scattered in a humid or dusty environment. It's a beautiful looking natural phenomenon also known as 'god rays'.
To challenge myself, I tried implementing this effect in real-time in the Unity game engine."
---

Volumetric light effects are caused by light being scattered in a humid or dusty environment. It's a beautiful looking natural phenomenon also known as 'god rays'.
To challenge myself, I tried implementing this effect in real-time in the Unity game engine based on the same technique used in Killzone: Shadow Fall from Guerrilla Games.

##### Technique overview

- For each light, create a custom render target (half or quarter resolution for better performance).
- Render a shape that represents the light's volume. For point lights we render a sphere and for spot lights a cone. Directional lights are rendered with a fullscreen quad as the volume covers the entire scene.
- At every pixel, calculate a line segment of the view ray that intersects with the light's volume.
- Raymarch along this line segment and sample multiple light contributions.
  - A dither effect is used to offset the ray origin.
- In the post processing stage, we add the volumetric light render target to the light buffer.


<div class="row">
        <div class="col-lg-6">
            <div style='position:relative; padding-bottom:calc(56.25%); margin-bottom:1.5rem;rounded mb-4'>
                <iframe src='https://gfycat.com/ifr/SlushyBreakableDunlin' frameborder='0' scrolling='yes' width='100%'
                        height='100%' style='position:absolute;top:0;left:0;' allowfullscreen></iframe>
            </div>
        </div>
        <div class="col-lg-6">
        </div>
        <div class="col-lg-6">
            <img class="img-fluid rounded mb-4" src="/assets/Images/VolumetricLights/VolumetricLight1900x1080.webp" alt="">
        </div>
        <div class="col-lg-6">
            <img class="img-fluid rounded mb-4" src="/assets/Images/VolumetricLights/VolumetricLight1900x1080_2.webp" alt="">
        </div>
        <div class="col-lg-6">
            <img class="img-fluid rounded mb-4" src="/assets/Images/VolumetricLights/VolumetricLight1900x1080_3.webp" alt="">
        </div>
        <div class="col-lg-6">
            <img class="img-fluid rounded mb-4" src="/assets/Images/VolumetricLights/VolumetricLight1900x1080_4.webp" alt="">
        </div>
    </div>

### References
[GPU Pro 5](https://www.amazon.com/GPU-Pro-Advanced-Rendering-Techniques/dp/1482208636){:target="_blank"} - Volumetric Light Effects in Killzone: Shadow Fall by Nathas Vos
