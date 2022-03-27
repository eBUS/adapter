---
ref: picfirmware
lang: en
navorder: 2
navtitle: PIC Firmware
---
## PIC Firmware

### Firmware Update
On the PIC, there is a bootloader that allows to update the firmware via the USB port or the Raspberry Pi GPIO/ttyAMA0.

Either the
[ebus PIC Loader] (https://github.com/john30/ebusd/blob/master/src/tools/README.md)
is needed which also allows setting a fixed IP address for the W5500 of the USR-ES1 module,
or the [Bootloader Host Application] (https://www.microchip.com/promo/8-bit-bootloader) from Microchip.

To update the firmware, the adapter has to be configured to USB or Raspberry Pi variant:
the Wemos D1 mini / USR-ES1 module have to be removed and the jumpers J1 and J4 set correctly, [see here](index.en#variants).

To activate the bootloader, pins 3 and 4 on jumper J11 have to be short cut before supplying power.

When the bootloader is active, the blue LED lights up immediately very brightly (more than all others), [see here](#led).

The firmware is then flashed by using one of the above tools via the serial interface with 115200 baud from word address 0x400.

With the
[ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md)
it works like this (where `/ dev / ttyUSB0` may have to be replaced by the actual USB serial device):
`ebuspicloader -f firmware.hex /dev/ttyUSB0`

### Firmware Versions
{:id="versions"}
* [Version 20220327](firmware/20220327-offset.hex):  
  Firmware version as reported by [ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md): `1 [63aa]`  
  Features: ebusd enhanced protocol V1, Ethernet with DHCP or fix IP, extra infos, configurable arbitration delay  
  Changes: see [Changelog](firmware/ChangeLog)  
  Minimal ebusd version: [21.1](https://github.com/john30/ebusd/releases/tag/v21.1) (enhanced protocol)  
  Pins on J12:  
  * Pin 1: Vdd
  * Pin 2: ebusd protocol:
    * open (or connected to pin 1): enhanced protocol (ebusd)
    * to GND (pin 3): standard protocol (eBUS direct)
  * Pin 3: GND
  * Pin 4: (WIFI-check)
  * Pin 5: Variant:
    * open: RPi or USB
    * to WIFI-check (Pin 4): WIFI
    * to GND (Pin 6): Ethernet
  * Pin 6: GND
  * Pin 7: (LOW)
  * Pin 8: Reset: shortly to LOW (Pin 7) or GND

**Important note:** The pins on J12 must never be connected to any pin on the other jumpers/pin headers/sockets as
separate power sources are used here. Any connection endangers the adapter and devices on the eBUS!

### Ethernet Configuration
{:id="ethernetconfig"}
If a fixed IP address is supposed to be used with the USR-ES1 module instead of obtaining it via DHCP,
this can be done with the
[ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md)
via the USB connection or the Raspberry Pi GPIO/ttyAMA0.

For example, to set the IP address 192.168.10.20 with a network mask of 255.255.255.0 (=length 24), the ebus PIC
Loader is called as follows (where `/dev/ttyUSB0` may have to be replaced by the actual USB serial device):
`ebuspicloader -i 192.168.10.20 -m 24 /dev/ttyUSB0`

### LED
The blue LED is used to signal states from the PIC firmware. In general, the bootloader and normal modes are quite different.

#### LED in Bootloader
{:id="ledboot"}
If the LED lights up permanently very brightly (more than all others) immediately after supplying power, the PIC is in
bootloader mode and is waiting for commands on the serial interface.

#### LED in normal operation (without Ethernet)
{:id="ledregular"}
In normal operation, the blue LED is initially turned off after supplying power.
* When operating with WIFI, the LED flashes slowly with low brightness until the Wemos signals its readiness  
  (the jumper must be configured to WIFI-check).
* If the LED stays on with very low brightness, the enhanced ebusd protocol is deactivated ([see jumpers](index.en#jumper)).
* Otherwise, it is fading up to normal brightness.
* After each initialization in the enhanced protocol, it lights up very brightly (more than all others) for 2 seconds.
* In case of a protocol error on eBUS or host, it lights up very brightly (more than all others) for 5 seconds.

#### LED in Ethernet operation
{:id="ledethernet"}
If the Ethernet module is plugged in and the jumper J12 is set accordingly, the blue LED has other meanings in addition
to the ones described above, which are run through in the following stages:
1. Until the W5500 of the USR-ES1 module responds after the reset, the LED flashes (2x per second).
2. Until a link is found on the line, the LED flashes very quickly (20x per second).
3. In case of DHCP, the LED flashes quickly (4x per second) until an IP address has been negotiated via DHCP.
