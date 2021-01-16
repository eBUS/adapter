---
ref: v2raspberrypi
lang: en
navorder: 4
navtitle: Raspberry Pi
---
## Raspberry Pi Board

This Variant of the extended [Base Board](base.en) is directly placed on top of a [Raspberry Pi](https://www.raspberrypi.org/).

For construction, assembly, and part list, see the corresponding chapters of the [Base Board](base.en).


### Circuit Diagram

[<img src="img/rpi-circuit-v22.png" width="600" alt="Circuit Raspberry Pi Board" title="Circuit Raspberry Pi Board">](img/rpi-circuit-v22.png)


### Measuring Plan

To test the soldered board for proper function, the measurement plan should be walked through.

For doing so, the eBUS socket is connected to a power supply (not to the heater!) with the supplied 330 ohm resistor.
Starting with version 2.1 with DC-DC converter, an additional 5V supply is needed for measurement, e.g. at JP1.

Here are the main measuring points on the board seen from top:

[<img src="img/rpi-measure-v22.jpg" width="300" alt="measure" title="Measuring v2.2">](img/rpi-measure-v22.jpg)

For the voltages, RX (<span style="color:green">green</span>) and TX (<span style="color:red">red</span>) are distinguished.  
The first pass is for reception (RX) and the second for transmission (TX) with the short-circuit at OK2.
