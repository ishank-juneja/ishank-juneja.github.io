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
[Part 3]({{site.baseurl}}/blog/nixieClockController) describes the design of a circuit which controls the display using isolated control signals.

***

#### **Introduction**
This page is about the design of the power supply module from our USB powered Nixie Tube clock display project. Since the USB source voltage is 5V DC and the operating voltage of the Nixie Tube display units is close to 180V DC, we needed to design a power supply capable of achieving this voltage step up. DC-DC power conversion and a large difference between input and output voltage difference mandated a switched mode (SMPS) isolated power supply. 

Despite the high output voltage level required, the convertor still operates in the low power regime (about 2.5 W as explained in [part 1]({{site.baseurl}}/blog/nixieClock))

***
*This project was completed in partial fulfilment of the requirements for EE 344: Electronic Design Lab at IITB*<br>
*A more detailed description of the project along with schematics can be found in our [project report]({{site.baseurl}}/assets/docs/DD08_Design_Lab_report.pdf)*<br>
*A demonstration of our project can be [found here](https://youtu.be/MN-FbMPmbiw)*
