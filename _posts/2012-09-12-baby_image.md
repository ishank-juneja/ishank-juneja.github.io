---
layout: page
date: 2017-09-24 
permalink: /projects/baby-image/
---
# Baby Image Segmentation
This project was through an intership with a start-up called [Cradlewise](http://cradlewise.com/about/). Their flagship product is an all inclusive smart cradle. One of the features which I contributed to, is an unsafe position detector.  

When an infant rolls over on its side, a dangerous position may arise where the baby may suffocate. The goal was to flag down such dangerous positions so that an alram can be raised.

To classify safe and unsafe positions, I explored image segmentation based approaches using the openCV library in python. For example the *watershed algorithm* -- which assigns the same segment to regions of low gradient.

The challenge in this project was integrating together information available in two forms: A Depth Image (zFrame) and an Intensity Image (iFrane)

- Looking for circles in the intensity Frame (iFrame)

Anoter non segmentation based method was implemented which followed the sequence: Look for circles corresponding to the Head, Torso and Legs --> Check the alignment between the centers of the three circles --> If mis-aligned beyond a certain threshold, declare unsafe.
<p align="center">  
  <img src="{{site.baseurl}}/images/circles_baby.png" alt="Depth Frame" style="width:184px;height:160px;" />
</p>

- A rendered version of a typical depth frame (zFrame) 
<p align="center">  
  <img src="{{site.baseurl}}/images/rendered_depth_frame.png" alt="Depth Frame" style="width:184px;height:160px;" />
</p>
