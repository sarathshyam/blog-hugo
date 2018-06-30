---
author: Sarath
title: "Self Diagnosis Flash Lights for Mitsubishi Lancer 4G15"
date: 2017-02-04T14:49:35+05:30
categories:
  - Automotive
tags:
  - OBD
  - Mitsubishi
  - Lancer
draft: false
---

My daily drive is a 2007 Mitsubishi Lancer(4G15). After playing with the OBDII port of my car and with the help from WWW, I could activate the self diagnosis mode of the Lancer 4G15 ECU.

By doing a simple routine, we can read the error codes recorded in the ECU. The error codes are interpreted from the pattern of flashing of the Engine Check Light (ECL) in the dash.

![OBD Port](/img/OBD2.jpg)

The above pic shows the schematic of the 16 Pin OBDII port. For the Mitsubishi lancer 4G15, only pins 1, 4, 5, 7, 9, 16 are available. The OBDII port of my car is located under the dash board on the driver side- straight under the steering. We have to lie down on the car floor to clearly see it though.

**Steps**

1. Turn ignition key to OFF position.
2. Short pin 1 and pin 4. We can use a metallic paper clip- inert one end of the clip to pin 1 and other to pin 4.
3. Turn ignition key to ON position.The Check engine light in the dash will blink in a pattern. If there are no error codes, the light will stay on for 0.5 seconds, then stay off for 1 second and this pattern will repeat. If there are error codes, then the error code can be interpreted from the pattern. A 0.5 seconds duration of illumination represents a unit of 10 and a 0.2 seconds illumination represent a unit of 1. In my car, there was a 0.5 second duration illumination followed by four 0.2 seconds duration illuminations. Which means the error code was 14 !.

**A table which shows the error codes and reason.**

11 Oxygen sensor  
12 Intake air flow sensor  
13 Intake air temperature sensor  
14 Throttle position sensor  
15 ISC motor position sensor  
21 Engine coolant temperature sensor  
22 Engine speed sensor  
23 TDC sensor  
24 Vehicle speed sensor  
25 Barometric pressure sensor  
31 Knock sensor  
41 Injector circuit  
42 Fuel pump relay  
43 EGR  
44 Ignition coil  
36 Ignition circuit  

To clear the error codes, disconnect the -ve terminal of the battery for more than 16sec.
[From my old blog  http://sarathshyam.blogspot.in/2010/07/ecu-error-code-self-diagnosis-for.html]