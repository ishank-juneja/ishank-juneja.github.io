---
layout: post
date: 2020-06-30
permalink: /blog/nixieClockPower
comments: true
title: Power Supply Design
mathjax: true
---

***
<p align="center">
  <br>
  <img src="{{site.baseurl}}/assets/images/power_supply.jpg" alt="Power Supply" height='300'/>
</p>

This post is Part 2 in a three part series on our design of a USB powered Nixie Tube Clock display. <br>
[Part 1]({{site.baseurl}}/blog/nixieClock) discusses the characterization of Nixie tubes. <br>
[Part 3]({{site.baseurl}}/blog/nixieClockController) describes the design of a circuit which controls the display using isolated control signals.<br>
This work on the design of the power supply was spearheaded by Anantha Hegde and Yojit Srivastava.
***

#### **Introduction**
This page is about the design of the power supply module from our USB powered Nixie Tube clock display project. Since the USB source voltage is 5V DC and the operating voltage of the Nixie Tube display units is close to 180V DC, we needed to design a power supply capable of achieving this voltage step up. DC-DC power conversion and a large difference between input and output voltage difference mandated a switched mode (SMPS) isolated power supply. 

Despite the high output voltage level required, the convertor still operates in the low power regime (about 2.5 W as explained in [part 1]({{site.baseurl}}/blog/nixieClock)). This made the Flyback convertor topology ideal for our purpose.

#### **Initial Design Process**
<div style="align: left; text-align:center;">
    <img src="{{site.baseurl}}/assets/images/boost.svg" width="400px"/>
    <div class="caption"> A non isolated Boost convertor </div>
</div>
<br>
Rather than the choice of a Flyback convertor, a non-isolated Boost convertor with a voltage boost from 5V to 180V could have been used. However, since the power and data source is a single USB port on a computer, it is crucial that we keep the low voltage (LV) and high voltage (HV) sides electrically isolated from each other. The only way to do this would be to choose an isolated convertor.<br>
Further, an isolated convertor like the Flyback has the advantage of being able to utilise the turns ratio of its transformer as the voltage step up component. The problem of a high step up of $\frac{180}{5} = 36$ times using a non-isolated Boost convertor can be illustrated by comparing the input to output transfer functions of the Boost and the Flyback convertors.

The Boost convertor has the input $V_i$ to output $V_o$ transfer function of 
\\[V_o = \frac{V_i}{1-D}.\\]
Where $D$ is the duty cycle (or the fraction time spent on). So for a step up of $36$ times an unsustainable $D$ of $\frac{35}{36}$ will be required. However, now we can consider the Flyback convertor topology used. 
<div style="align: left; text-align:center;">
    <img src="{{site.baseurl}}/assets/images/simple_flyback.png" width="400px"/>
    <div class="caption"> An isolated Flyback convertor </div>
</div>
<br>

Due to the transformer present in the circuit, the transfer function is given by,
\\[V_o = \frac{N_2}{N_1} \frac{D}{1-D} V_i.\\]
Where $\frac{N_2}{N_1}$ is the turns ratio of the transformer in question. With a reasonably large turns ratio, little of the step up burden falls on the $D$ term thereby making for a reasonable duty cycle.

***
*This project was completed in partial fulfilment of the requirements for EE 344: Electronic Design Lab at IITB*<br>
*A more detailed description of the project along with schematics can be found in our [project report]({{site.baseurl}}/assets/docs/DD08_Design_Lab_report.pdf)*<br>
*A demonstration of our project can be [found here](https://youtu.be/MN-FbMPmbiw)*
