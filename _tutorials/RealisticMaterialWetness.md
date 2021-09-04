---
layout: tutorial
title: Realistic Material Wetness
description:
weight: 100
usemathjax: true
showInFrontPage: true
image: /assets/Images/Tutorials/RealisticMaterialWetness/RealisticMaterialWetness700x300.webp
height: 700
width: 300
pageintro: ""
github: Shaderic/Realistic-Material-Wetness
---

<br/>

  <div class="row">
    <!-- <div class="col-lg-6">
       <img class="img-fluid rounded mb-4" src="http://placehold.it/750x450" alt="">
    </div>-->
    <div class="col-lg-6">
      <h1></h1>
      Since physically based rendering (PBR) is now a standard in most games we try to simulate better and more lighting models that cover more material types. With the classic lighting model, everybody chose to darken the diffuse term and boost the specular term. With the method explained below you can provide a way to simulate wet surfaces while keeping your material physically accurate.<br/>
<br/>
We can easily distinguish between a dry and wet surface. The best visual cue is that wet surfaces look darker, have a higher specular value, and exhibit subtle changes in saturation.<br/>
This behavior is made by rough/porous materials (brick, clay, concrete, plaster...), Absorbent materials (cotton, fabric), and organic materials like hair.
Materials that are smooth don't change however since they are already reflecting a lot of light.
    </div>
    <div class="col-lg-6">
<h1 class="my-4"></h1>
        <img class="img-fluid rounded mb-4" src="/assets/Images/Tutorials/RealisticMaterialWetness/RealisticMaterialWetness700x300.webp" alt="">
     </div>
  </div>
  
<br/>
  
---  

### Darkening the albedo

When looking at all the visual cues of wet materials, we can see that the surface looks darker. This depends on how porous the material is and how much light is absorbed. Another thing we can see is that brighter diffuse values stay bright but everything else darkens.
At first, it seems reasonable to multiply to diffuse factor with a scalar value depending on the wetness. The problem with this method is that bright diffuse colors lose more color than supposed to and the materials appear rather black instead of darker.

A better way to darken the diffuse is to square it. This makes the brighter AND the darker colors lose less value while others at around 50% will be adjusted more accurately. Below you can see a graph that shows the difference between a squared diffuse and a diffuse multiplied with a low value (0.35 here).

<div class="row">
    <!-- <div class="col-lg-6">
       <img class="img-fluid rounded mb-4" src="http://placehold.it/750x450" alt="">
    </div>-->
    <div class="col-lg-5">
    <img class="img-fluid rounded mb-4" src="/assets/Images/Tutorials/RealisticMaterialWetness/darkerDiffuseGraph.png" alt="">
    </div>
    <div class="col-lg-6">
        <img class="img-fluid rounded mb-4" src="/assets/Images/Tutorials/RealisticMaterialWetness/DarkeningAlbedo700x300.png" alt="">
        <footer class="blockquote-footer">The difference between the normal diffuse (left), the diffuse multiplied with a low value (middle), and the squared diffuse (right)</footer>
     </div>
  </div>


---  


  
### Input properties
To keep the input intuitive for artists, we use as few material properties as possible.
To control the wetness on the material itself we use a single greyscale texture that the artists can paint to keep visual consistency, physical correctness and still keep things intuitive.

We also use a single wetness float to scale the mask so we can dynamically change it via gameplay.
Below you can find a visualization of multiple wetness masks.

<br/>  

<img class="img-fluid rounded mb-4" src="/assets/Images/Tutorials/RealisticMaterialWetness/WetnessTexture700x300.png" alt="">

<br/>  

| Property        | Type           | Description  |
| ------------- |-------------| -----|
| Wetness      | Float | This drives the transition between the different phases and also acts as a scalar value of the wetness mask |
| Wetness Mask     | Greyscale Texture |   This controls the material wetness  |

<br/>  

<div class="alert alert-info" role="alert">
  Since we want porous materials to darken when they're wet, we could give this as a second property to the artist. However, since the porosity is based on the materials roughness value we can use the roughness map as input for this.
</div>

<div class="alert alert-info" role="alert">
  An easy way to create a wetness map is to start with an inverted height map and apply some contrast to it!
</div>

<br/>
  
---  


### The Shader code
Here's the shader code. The artists have control of the **wetness** parameter whereas the **porosity** parameter is the material's roughness value. Wetness drives the transition and the porosity darkens the base color. To darken the material we lerp to a squared version of the base color. This is a reasonable approximation of the darkening and more accurate if we would multiply the base color with a low grayscale value.

We also force the vertex normal to point straight up of the wetness value gets above 98%. This convinces the player that the surface of the pool is flat like water.

<script src="https://gist.github.com/Shaderic/0bbecff9e134537c051775a8d29dadfd.js"></script>
<script src="https://gist.github.com/Shaderic/1ede66fb72c66a4a7c28cefc8d9b882e.js"></script>
  
<br/>
  
---  
 

### References
[The Technical Art of Uncharted 4](https://advances.realtimerendering.com/other/2016/naughty_dog/NaughtyDog_TechArt_Final.pdf){:target="_blank"}  
[Physically based wet surfaces](https://seblagarde.wordpress.com/2013/03/19/water-drop-3a-physically-based-wet-surfaces/){:target="_blank"}

Texture Assets from:  
[Textures.com](https://www.textures.com/){:target="_blank"}  
[TextureHaven](https://texturehaven.com/){:target="_blank"}

