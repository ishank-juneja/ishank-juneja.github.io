---
layout: post
date: 2020-07-18
permalink: /blog/nixieClockController
comments: true
title: Clock Controller Design
mathjax: true
---

***
<p align="center">
  <br>
  <img src="{{site.baseurl}}/assets/images/controller_annotated.png" alt="Controller Board Annotated Image" height='350'/>
</p>

This post is Part 3 in a three part series on our design of a USB powered Nixie Tube Clock display. <br>
[Part 1]({{site.baseurl}}/blog/nixieClock) discusses the characterization of Nixie tubes. <br>
[Part 2]({{site.baseurl}}/blog/nixieClockPower) discusses the design of the power supply that steps up 5V to 170V.

***

#### **Controller Board Overview**
The controller board a.k.a the USB bridge circuit is responsible for taking in timing information via USB from the host computer and displaying it, through the means of electrically isolated control signals, onto the front end display module consisting of 6 Nixie Tubes.   
<div style="align: left; text-align:center;">
    <img src="{{site.baseurl}}/assets/images/block_diagram.png" height="400px"/>
    <div class="caption"> Complete Block Diagram </div>
</div>
<br>
The flow of information and power is illustrated in the above block diagram. From the above block diagram, Nixie Tube Driver Module, Micro-controller and the Nixie Tubes are all part of the controller board. The details of these and some of their sub-components are illustrated next.

#### **USB Controller Circuit**
The most interesting component of the controller board is the USB micro-controller circuit. The host computer, which could be a single board computer for portability and compactness, is connected to the USB controller. After which the host side USB interface program first uploads the firmware onto the controller and then streams time information data to it. The firmware and host side program software was written with help from our advisor Prof. [Mukul Chandorkar](https://www.ee.iitb.ac.in/~mukul/) and is [available here](https://github.com/ishank-juneja/TUSB3210-Controller).

Two alternatives were considered for the USB controller circuit - the TUSB3210 and the FT232R chip. Our initial preference leaned towards the the FT232 from the desire for greater simplicity. Some problems with timing that could not be precisely controlled lead to us picking the TUSB3210 RAM based micro-controller. The details of these problems could be quite relevant and are available in our [project report](https://youtu.be/MN-FbMPmbiw). To use the TUSB3210 controller in the USB connection circuit, some additional circuitry like its own power supply and some resetting circuitry were also needed. The details of these elements are also present in our project report.     

An interesting question to be answered is what part of the processing should occur on the embedded controller and what parts should be done by the host computer. We pushed all the time keeping and computation onto the host computer leaving the USB controller with only the burden of two things. (1) Translating the digits of time being streamed from the host to control signals that the display front end can understand and (2) Using the control signals to multiplex between the various tubes in such a way that only a single tube is turned on at a time. This is required since the power available from the USB power source is, in principle, enough to light up only a single tube as discussed in [part 1]({{site.baseurl}}/blog/nixieClock). 

#### **Optocoupler isolation and Control Signals**


***
*This project was completed in partial fulfilment of the requirements for EE 344: Electronic Design Lab at IITB*<br>
*A more detailed description of the project along with schematics can be found in our [project report]({{site.baseurl}}/assets/docs/DD08_Design_Lab_report.pdf)*<br>
*A demonstration of our project can be [found here](https://youtu.be/MN-FbMPmbiw)*
