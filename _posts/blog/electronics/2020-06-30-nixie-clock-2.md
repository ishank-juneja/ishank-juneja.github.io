---
layout: post
date: 2020-07-13
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

#### **Related Existing Solutions - Survey**
Before we describe the solution we ended up implementing to reach the described end goal, let us look at some of the other solutions we considered prior to it. On scouring the web we found a few solutions that matched our requirement closely. Most of these solutions had an input operating voltage of 9V-12V. Perhaps this restriction of a slightly higher than available input voltage has something to do with most batteries (Lead Acid for example) having a terminal voltage in that range.

#### **Single IC Based solutions**

##### **TNY-278**
Our first attempt at a Power Supply was with the [TNY-278](https://www.power.com/sites/default/files/product-docs/tny274-280.pdf) IC by Power Integrations. This was our starting point since this chip was available in our project advisors lab. The TNY-278 is a switching IC which has a typical application of a step down convertor in a flyback like type topology (can be seen in linked [data sheet](https://www.power.com/sites/default/files/product-docs/tny274-280.pdf)). Our intention was to reverse the input and output ports of its typical application circuit to use the chip in a step up convertor. However, we soon realized that the usage of such a topology with a 5V input voltage would not be possible since the internal circuitry of the TNY-278 makes any voltage below 5.85V unusable. 

##### **LM3488**
Quite close to the start of our work we came across [this project](https://github.com/esynr3z/nixie-ps). The linked project describes the design of a Flyback convertor that steps up 5-12V to 180V. On the surface of it, this seemed like the end to our search. In fact we even went ahead and got a circuit board printed for this circuit.
<div style="align: left; text-align:center;">
    <img src="{{site.baseurl}}/assets/images/LM3488.png" width="600px"/>
    <div class="caption"> A non isolated Boost convertor </div>
</div>
<br>
The circuit is essentially a Flyback topology built around the [LM 3488](https://www.ti.com/lit/ds/snvs089o/snvs089o.pdf?ts=1594663885165&ref_url=https%253A%252F%252Fwww.google.com%252F) IC. The LM 3488 is the complete package as far as a switched power supply (SMPS) is concerned since it has voltage feedback from the output (FB pin), MOSFET gate drive (DR) and the switching frequency set (FA/SYNC/SD) all built into a single package. However, the design in the linked project destroyed the isolation obtained using the transformer of the Flyback convertor by sending in a direct voltage feedback from the HV side to the LV side without some sort of optocoupler isolation in between. Since this was a critical design flaw for our application(need for isolation described in the section on the design process) we decided to look at our own solutions. So, next began the phase of designing switched mode convertors from scratch using discrete components rather than packaged IC solutions.

#### **Discrete Components Based Solutions**


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
