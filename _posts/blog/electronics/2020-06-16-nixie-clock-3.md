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

Two alternatives were considered for the USB controller circuit - the [TUSB3210](https://www.ti.com/lit/ds/slls466g/slls466g.pdf?ts=1595438237932&ref_url=https%253A%252F%252Fwww.google.com%252F) and the [FT232R](https://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT232R.pdf) chip. Our initial preference leaned towards the the FT232 from the desire for greater simplicity. Some problems with timing that could not be precisely controlled lead to us picking the TUSB3210 RAM based micro-controller. The details of these problems could be quite relevant and are available in our [project report](https://youtu.be/MN-FbMPmbiw). To use the TUSB3210 controller in the USB connection circuit, some additional circuitry like its own power supply and some resetting circuitry were also needed. The details of these elements are also present in our project report.     

An interesting question to be answered is what part of the processing should occur on the embedded controller and what parts should be done by the host computer. We pushed all the time keeping and computation onto the host computer leaving the USB controller with only the burden of two things. (1) Translating the digits of time being streamed from the host to control signals that the display front end can understand and (2) Using the control signals to multiplex between the various tubes in such a way that only a single tube is turned on at a time. This is required since the power available from the USB power source is, in principle, enough to light up only a single tube as discussed in [part 1]({{site.baseurl}}/blog/nixieClock). 

#### **Optocoupler isolation and Control Signals**
The GPIO pins on the microcontroller are responsible for the control signals that display the time on the Nixie Tubes. The design is configured in a manner that there are 10 control signals. The breakdown being - 6 signals for selection among the 6 tubes and 4 signals for digit selection among the digits 0-9 using a Binary Coded Decimal (BCD) code. In a more optimized design, the 6 tube selection signals could be replaced by 3 control signals and a decoder chip.

While designing the USB controller circuit described in the previous section, we thought that the 10mA or so of current required to drive the internal LED of the opto-couplers on would be available from the GPIO pins of the controller itself. This assumption was based on a faulty reading of the somewhat unclear data sheet of the TUSB3210 controller. Since sufficient current could not be sourced from the controller directly, there had to be a current amplification stage sitting between the opto-coupler input port (which is essentially an LED) and the signals being received from the GPIO pins.  

#### Current Amplifier
<div style="align: left; text-align:center;">
    <img src="{{site.baseurl}}/assets/images/GPIO_ckt.jpg" height="300px"/>
    <div class="caption"> Configuration for a single signal </div>
</div>
<br>
The current amplifier stage is essentially BJTs configured as switches which turn on when there is a small current going into the base of the transistors. The base current requirement is small enough that the GPIO pins from the TUSB controller can provide it. The complete arrangement for a single control signal is described in the above image. When the MCU GPIO signal is high, the BC547 transistor turns on which in turn switches on the IR LED at the input port of the optocoupler. Once the input LED is on, it excites the photo-sensitive base of the output photo-transistor which turns the transistor on. In the configuration available to us only the collector terminal of the output transistor is available to us. 

As shown in the above circuit diagram, this would lead to the output signal being 5V high when the photo-transistor if off, and low when the photo-transistor is on. This is a problem since the photo-transistor needs to be turned on really hard for the output taken at the collector terminal of the transistor to be low enough for the subsequent electronic stage (which is the display front end) to recognize it as off. This forced us to pick a very low resistance value for the opto-coupler input LED. As seen in the above diagram this value is 30 &#x2126;. With such a low series resistance, the current pushed through the LED is high at about 30mA. This also reduces the life of the input LED significantly. We found ourselves replacing the optocoupler ICs quite often during testing. The only possible solutions to this problem seem to be either to look for an optocoupler IC that uses a very sensitive photo-transistor or to somehow change the sensitivity of the electronics following the output of the optocoupler to have a very high threshold for the on state, close to the high logic level of 5V DC.  

***
*This project was completed in partial fulfilment of the requirements for EE 344: Electronic Design Lab at IITB*<br>
*A more detailed description of the project along with schematics can be found in our [project report]({{site.baseurl}}/assets/docs/DD08_Design_Lab_report.pdf)*<br>
*A demonstration of our project can be [found here](https://youtu.be/MN-FbMPmbiw)*
