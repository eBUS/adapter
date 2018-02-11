---
ref: diagnostics
lang: en
---
## Diagnostics


### Diagnostics with multimeter

The [Measurement Plan of the base board](base.en#measuring-plan) contains all important measurement points with the necessary voltages.

The measurement described here takes place only on the base board without the extension board plugged onto it:

1. Remove eBUS.
2. Remove UART.
3. Insert TX LED2.
4. Connect base board to a power supply using the 330 ohm 5 Watt resistor.
5. The voltage at eBUS terminal should be 17 volts.
6. Compare the voltages with the measurement plan.


[<img src="images/optocoupler.png" width="200" alt="optocoupler" title="OK2">](images/optocoupler.png)  
In order to switch to TX mode, pin 4-5 must be short-circuit on optocoupler 2.
This can be achieved by using a small screwdriver hold in between the pins.

[<img src="images/base-measuring.jpg" width="200" alt="multimeter" title="Measurement with multimeter">](images/base-measuring.jpg)

In case it is nnecessary to solder out an optocoupler, the pins can be cut with a cutter and afterwards clean the holes with desoldering wire.

### Diagnostics with oscilloscope

If you have an oscilloscope you can simplify troubleshooting a lot since more information can be gathered.
Here are some examples of erroneous signals:

[<img src="images/base-measure-oszi-bad.jpg" width="200" alt="bad TX" title="bad TX">](images/base-measure-oszi-bad.jpg)

Here, the second channel (bottom) was triggered by the TX signal of the UART.
The upper channel shows a signal coming from an improper optocoupler measured at the OK2 pin 4.
This was an experiment with an optocoupler having a Darlington stage.
As can be clearly seen, the edge rises in a curve form and the eBUS interprets this signal incorrectly.
Ideally, the upper signal should be equal to the lower one, the steeper the flank, the better.

[<img src="images/base-measure-wave.png" width="200" alt="bad RX" title="bad RX">](images/base-measure-wave.png)

Likewise, the signal shown here (incidentally, this was measured by a Raspberry on the RX) would also lead to no success.
The maximum allowed 50 microseconds were far exceeded due to the waveform.

[<img src="images/base-measure-oszi-bad2.jpg" width="200" alt="bad TX 2" title="bad RX">](images/base-measure-oszi-bad2.jpg)

This is a signal that is drawn too low on ground (GND is the lowest first grid block).
There is an error in the GND connection on the board, GND was connected to GND2.
This error can also be found with a multimeter (see above).
