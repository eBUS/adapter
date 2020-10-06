---
ref: picfirmware
lang: de
---
## eBUS Adapter 3 - PIC Firmware

### Firmware Update
Auf dem PIC ist ein Bootloader enthalten, womit die Firmware über den USB-Anschluss oder den Raspberry Pi GPIO/ttyAMA0
aktualisiert werden kann.

Dazu wird entweder das
[Bootloader Tool](https://github.com/john30/ebus3/tree/master/tools/bootloadercheck) TODO finale URL
benötigt, mit dem sich auch eine feste IP-Adresse für den W5500 setzen lässt,
oder die [Bootloader Host Application](https://www.microchip.com/promo/8-bit-bootloader) von Microchip.

Um die Firmware zu aktualisieren, muss der Adapter in die USB oder Raspberry Pi Variante versetzt werden (also
Wemos oder W5500 entfernen und Jumper J1 und J4 richtig einstellen).

Die Firmware wird dann über eines der o.g. Tools über die serielle Schnittstelle mit 9600 Baud ab Adresse 0x400 geflasht.

### Firmware Versionen
* Version 1.0:  
  Released: 01.11.2020  
  Features: ebusd enhanced protocol V1, Ethernet  
  Minimale ebusd Version: 4.0  
  Anschlussbelegung J12:  
  * Pin 1: Vdd
  * Pin 2: ebusd enhanced protocol: offen=aktiv, GND=inaktiv
  * Pin 3: GND
  * Pin 4: -
  * Pin 5: Ethernet mit W5500: offen=inaktiv, GND=aktiv TODO
  * Pin 6: GND
  * Pin 7: -
  * Pin 8: -

**Wichtiger Hinweis:** Die Pins am J12 dürfen nie mit irgendeinem Pin der anderen Jumper/Stecker-/Buchsenleisten in
Verbindung gebracht werden, da hier getrennte Stromquellen zum Einsatz kommen. Jegliche Verbindung gefährdet den Adapter
und Geräte am eBUS!

### Ethernet Konfiguration
Wenn für den Einsatz mit W5500 eine feste IP Adresse eingestellt werden soll, dann kann das mit dem
[Bootloader Tool](https://github.com/john30/ebus3/tree/master/tools/bootloadercheck) TODO finale URL
über den USB-Anschluss oder den Raspberry Pi GPIO/ttyAMA0 vorgenommen werden.

TODO Details