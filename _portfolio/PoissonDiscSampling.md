---
layout: project
title: Natural object placement with Poisson Disk Sampling
description:
weight: 98
usemathjax: true
showInFrontPage: true
image: /assets/Images/poissonDiskSampling/Trees_700x300.webp
pageintro: "While looking for a way to place objects in a scene in a natural, evenly distributed way I discovered Poisson Disk Sampling.."
---

I recently came across a tweet from Pico Tanks on where they create regions, defined by a shape, where players can hide. What stands out to me the most, is how the trees in those regions never seem to overlap. After looking more into this, I found an algorithm called **Poisson disk sampling**. 


 <div class="row">
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <blockquote class="twitter-tweet" data-theme="light"><p lang="en" dir="ltr">I got inspired by <a href="https://twitter.com/PicoTanks?ref_src=twsrc%5Etfw">@PicoTanks</a> Spline based object placement so I decided to try and create my own. <a href="https://twitter.com/hashtag/gamedev?src=hash&amp;ref_src=twsrc%5Etfw">#gamedev</a> <a href="https://twitter.com/hashtag/madewithunity?src=hash&amp;ref_src=twsrc%5Etfw">#madewithunity</a> <a href="https://twitter.com/hashtag/screenshotsaturday?src=hash&amp;ref_src=twsrc%5Etfw">#screenshotsaturday</a> <a href="https://twitter.com/hashtag/indiedev?src=hash&amp;ref_src=twsrc%5Etfw">#indiedev</a> <a href="https://t.co/fdj9jxysFT">pic.twitter.com/fdj9jxysFT</a></p>&mdash; Cédric Van Huffelen (@Shaderic_) <a href="https://twitter.com/Shaderic_/status/1051125026261483520?ref_src=twsrc%5Etfw">October 13, 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 
            <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
        </div>
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <div class="card h-100">
                <a href="#">
                    <img class="card-img-top" src="/assets/Images/poissonDiskSampling/Trees_700x300.webp"></a>
                <div class="card-body">
                    <div class="card-text">Trees placed using Poisson Disk Sampling.
                    </div>
                </div>

            </div>
        </div>
    </div>

***

### Poisson Disk sampling

The reason why Poisson disk sampling produces a natural pattern is the way the points are generated. Instead of generating a random X and Y coordinate, each point is generated between a minimum and maximum distance between each other. 

<div class="row">
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <div class="card h-100">
                <a href="#">
                    <img class="card-img-top" src="/assets/Images/poissonDiskSampling/RandomXY_700x400.webp"></a>
                <div class="card-body">
                    <div class="card-text">Points generated with random X and Y coordinates.</div>
                </div>
            </div>
        </div>
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <div class="card h-100">
                <a href="#">
                    <img class="card-img-top" src="/assets/Images/poissonDiskSampling/PoissonDiscSampling_700x400.webp"></a>
                <div class="card-body">
                    <div class="card-text">Points generated with Poisson Disk Sampling. All of the points are
                        distributed evenly with at least 2 units apart from each other.
                    </div>
                </div>

            </div>
        </div>
    </div>

***

### How it works

<div class="row">
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <p>
                Before we start generating the points we have to set up all the needed inputs. The algorithm itself only
                takes
                in
                <b>r</b> and <b>k</b> as input. <b>r</b> is the minimum distance between the samples and <b>k</b> is a
                constant
                which is the maximum samples before rejection. First, we initialize a 2D grid for storing the
                samples.<br> The
                cell
                size of this grid is bounded by <b>r/√n</b> where <b>n</b> is the number of dimensions (2 in this
                case) so that
                each cell will contain at most 1 sample.
            </p>
            <p>
                In step 1 we generate the initial sample that is randomly chosen from the domain. This sample is
                inserted into
                the grid and added to the active list, this active list holds the current samples which we generate new
                samples
                from.
            </p>
            <p>
                After we have our initial sample, while the active list is not empty, we choose a random index from that
                list
                and generate up to <b>k</b> points around it. These points lay between <b>r</b> and <b>2*r</b> distance
                from the
                current index. For each point, we check if it is between <b>r</b> distance from any of the samples saved
                in the
                grid and since the points are generated between <b>r</b> and <b>2*r</b>, we can skip unnecessary checks
                by only
                checking the neighboring cells of the current index. If we found a point that is adequately far from
                existing
                samples, we save it to the grid and the active list. If after <b>k</b> samples, no such points are
                found, we
                remove the current index from the active list.
            </p>
        </div>
        <div class="col-lg-6 col-sm-6 portfolio-item">
            <div style='position:relative; padding-bottom:calc(56.25% + 44px)'>
                <iframe src='https://gfycat.com/ifr/DenseMistyBarnswallow' frameborder='0' scrolling='no' width='100%'
                        height='100%' style='position:absolute;top:0;left:0;' allowfullscreen>
                </iframe>
            </div>
        </div>
        <div>

        </div>

    </div>

***

### Checking which points are inside the shape

To know which points are inside the shape, I found a very fast and short algorithm on the Unity3D wiki.  
The basic idea is to run a semi-infinite ray horizontally out from the test point and count how many edges it crosses.   
At each crossing, a boolean is switched between true and false. At the end of the ray, this boolean is returned. 


<script src="https://gist.github.com/Shaderic/3bb596d38f39cff3d621f7864f4f9fc4.js?ts=2"></script>


***

### References

[Poisson Disk Sampling in Arbitrary Dimensions](https://www.cct.lsu.edu/~fharhad/ganbatte/siggraph2007/CD2/content/sketches/0250.pdf){:target="_blank"}    
[Poly contains point from Wiki.Unity3d](http://wiki.unity3d.com/index.php/PolyContainsPoint){:target="_blank"}