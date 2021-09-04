---
layout: project
title: Real life virtual buttons using Azure Kinect.
description: 
weight: 102
usemathjax: true
showInFrontPage: true
displayImage: /assets/Images/VirtualButtons/VirtualButtons700x300.png
pageintro: For a project at HITLab @ Howest, I had been given the task to create a game to help visually impaired children train their vision.
---

While playing around with depth cameras, I wanted to see if it was possible to create buttons with no physical shape.  
After doing some research we arrived at a working prototype using a projection beamer and the Microsoft Azure kinect.  
During the calibration phase we calculate the bounding box for each button and sampling the initial depth values.  
By comparing the current depth values with the initial ones, we can keep track of the hands and save them to a black and white texture.   
For each button we calculate a % of how many pixels are filled in the bounding box.  
By applying a threshold on this value we can determine whether a player sufficiently covers the virtual button and is at the correct height to "press the button".

<div class="row">
        <div class="col-lg-6">
            <img class="img-fluid rounded mb-4" src="/assets/Images/VirtualButtons/buttons1900x1080.webp" alt="">
        </div>
    </div>