---
ref: picfirmware
lang: de
navorder: 2
navtitle: PIC Firmware
---
## PIC Firmware

### Firmware Update
Auf dem PIC ist ein Bootloader enthalten, womit die Firmware über den USB-Anschluss oder den Raspberry Pi GPIO/ttyAMA0
aktualisiert werden kann.

Dazu wird entweder der
[ebus PIC Loader](https://github.com/john30/ebusd/blob/enhanced_device/src/tools/ebuspicloader.cpp)
benötigt, mit dem sich auch eine feste IP-Adresse für den W5500 des USR-ES1 Moduls setzen lässt,
oder die [Bootloader Host Application](https://www.microchip.com/promo/8-bit-bootloader) von Microchip.

Um die Firmware zu aktualisieren, wird der Adapter in die USB oder Raspberry Pi Variante versetzt: Dazu werden
der Wemos D1 mini bzw. das USR-ES1 Modul entfernt und die Jumper J1 und J4 richtig eingestellt, [siehe hier](index#varianten).

Um den Bootloader zu aktivieren, werden am Jumper J11 die Pins 3 und 4 verbunden und erst dann die Stromversorgung
hergestellt.

Wenn der Bootloader aktiv ist, leuchtet die blaue LED sofort in voller Helligkeit, [siehe hier](#led).

Die Firmware wird dann über eines der o.g. Tools über die serielle Schnittstelle mit 9600 Baud ab Wort-Adresse 0x400 geflasht.

Mit dem
[ebus PIC Loader](https://github.com/john30/ebusd/blob/enhanced_device/src/tools/ebuspicloader.cpp)
geht das bspw. so (wobei `/dev/ttyUSB0` evtl. durch das richtige USB serial device ersetzt werden muss):
`ebuspicloader -f firmware.hex /dev/ttyUSB0`

### Firmware Versionen
{:id="versions"}
* [Version 20201129](firmware/20201129-offset.hex):  
  Features: ebusd enhanced protocol V1, Ethernet  
  Minimale ebusd Version: [3.5]([ebus PIC Loader](https://github.com/john30/ebusd/blob/enhanced_device) (enhanced protocol) [TODO finale URL]
  Anschlussbelegung J12:  
  * Pin 1: Vdd
  * Pin 2: ebusd protocol:
    * offen: enhanced protocol (ebusd)
    * gegen GND: standard protocol (eBUS direkt)
  * Pin 3: GND
  * Pin 4: (WIFI-Check)
  * Pin 5: Variante:
    * offen: RPi oder USB
    * gegen WIFI-Check (Pin 4): WIFI
    * gegen GND (Pin 6): Ethernet
  * Pin 6: GND
  * Pin 7: (LOW)
  * Pin 8: Reset: kurz gegen LOW (Pin 7) oder GND

**Wichtiger Hinweis:** Die Pins am J12 dürfen nie mit irgendeinem Pin der anderen Jumper/Stecker-/Buchsenleisten in
Verbindung gebracht werden, da hier getrennte Stromquellen zum Einsatz kommen. Jegliche Verbindung gefährdet den Adapter
und Geräte am eBUS!

### Ethernet Konfiguration
Wenn für den Einsatz mit USR-ES1 Modul eine feste IP-Adresse eingestellt werden soll, anstelle diese via DHCP zu beziehen,
dann kann das mit dem
[ebus PIC Loader](https://github.com/john30/ebusd/blob/enhanced_device/src/tools/ebuspicloader.cpp)
über den USB-Anschluss oder den Raspberry Pi GPIO/ttyAMA0 vorgenommen werden.

Um bspw. die IP-Adresse 192.168.10.20 mit einer Netzmaske von 255.255.255.0 (=Länge 24) einzustellen, wird der ebus PIC
Loader wie folgt aufgerufen (wobei `/dev/ttyUSB0` evt. durch das richtige USB serial device ersetzt werden muss):
`ebuspicloader -i 192.168.10.20 -m 24 /dev/ttyUSB0`

### LED
Die blaue LED wird zur Signalisierung von Zuständen aus der PIC Firmware genutzt. Hier muss grundsätzlich zwischen
Bootloader und normalem Modus unterschieden werden.

#### LED im Bootloader
Wenn die LED nach Anschließen der Stromversorgung sofort permanent in voller Helligkeit leuchtet, dann ist der PIC im
Bootloader-Modus und wartet auf Daten an der seriellen Schnittstelle.

#### LED bei normalem Betrieb (ohne Ethernet)
Bei normalem Betrieb ist die blaue LED nach Anschließen der Stromversorgung zunächst aus.
* Beim Betrieb mit WIFI blinkt die LED dann langsam mit geringer Helligkeit, bis der Wemos seine Bereitschaft
  signalisiert (dazu muss der Jumper auf WIFI-Check konfiguriert sein).
* Bleibt die LED mit sehr geringer Helligkeit an, dann ist das enhanced ebusd Protokoll deaktiviert
  ([siehe Jumper](index#jumper)). 
* Ansonsten wird die Helligkeit bis zum Maximum in mehreren Schritten erhöht und bleibt dann an.

#### LED bei Betrieb mit Ethernet
Ist das Ethernet Modul aufgesteckt und der Jumper an J12 entsprechend gesetzt, dann hat die blaue LED noch weitere
Bedeutungen zusätzlich zu den oben beschriebenen, die in folgenden Stufen durchlaufen werden:
1. Bis der W5500 des USR-ES1 Moduls nach dem Reset antwortet, blinkt die LED (2x pro Sekunde).
2. Bis ein Link auf der Leitung gefunden wurde, blinkt die LED sehr schnell LED (20x pro Sekunde).
3. Im Fall von DHCP blinkt die LED schnell LED (4x pro Sekunde), bis eine IP-Adresse per DHCP ausgehandelt wurde. 
