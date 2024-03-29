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
[ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md)
benötigt, mit dem sich auch eine feste IP-Adresse für den W5500 des USR-ES1 Moduls setzen lässt,
oder die [Bootloader Host Application](https://www.microchip.com/promo/8-bit-bootloader) von Microchip.

Um die Firmware zu aktualisieren, wird der Adapter in die USB oder Raspberry Pi Variante versetzt: Dazu werden
der Wemos D1 mini bzw. das USR-ES1 Modul entfernt und die Jumper J1 und J4 richtig eingestellt, [siehe hier](index#varianten).

Um den Bootloader zu aktivieren, werden am Jumper J11 die Pins 3 und 4 verbunden und erst dann die Stromversorgung
hergestellt.

Wenn der Bootloader aktiv ist, leuchtet die blaue LED sofort sehr hell (heller als alle anderen), [siehe hier](#led).

Die Firmware wird dann über eines der o.g. Tools über die serielle Schnittstelle mit 115200 Baud ab Wort-Adresse 0x400 geflasht.

Mit dem
[ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md)
geht das bspw. so (wobei `/dev/ttyUSB0` evtl. durch das richtige USB serial device ersetzt werden muss):
`ebuspicloader -f firmware.hex /dev/ttyUSB0`

### Firmware Versionen
{:id="versions"}
Aktuelle [Version 20221009](firmware/20221009-offset.hex):  
* Firmware Version laut [ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md): `1 [87a3]`  
* Features:
  * ebusd enhanced protocol V1 inkl. high-speed Modus
  * Ethernet mit DHCP oder fester IP
  * extra Infos
  * konfigurierbares Arbitrierungs-Delay
  * Schaltplan v3.0 und v3.1
* Änderungen: siehe [Changelog](firmware/ChangeLog)  
* Minimale ebusd Version: 
  * [21.1](https://github.com/john30/ebusd/releases/tag/v21.1) (enhanced protocol, normal-speed)  
  * [22.4](https://github.com/john30/ebusd/releases/tag/v22.4) (enhanced protocol, high-speed)
* Anschlussbelegung J12:  
  * Pin 1: Vdd
  * Pin 2: ebusd protocol:
    * offen (oder verbunden mit Pin 1): enhanced protocol (ebusd)
    * gegen GND (Pin 3): standard protocol (eBUS direkt)
  * Pin 3: GND
  * Pin 4: (WIFI-Check, nur benötigt für Schaltplan v3.0)
  * Pin 5: Variante:
    * offen: RPi oder USB
    * gegen WIFI-Check (Pin 4): WIFI (nur benötigt für Schaltplan v3.0)
    * gegen GND (Pin 6): Ethernet
  * Pin 6: GND
  * Pin 7:
    * offen: normal-speed serial
    * gegen GND (Pin 6): high-speed serial (nur für enhanced protocol, ab Version 20220731)
  * Pin 8: Reset: kurz gegen LOW (Pin 7) oder GND

Frühere Versionen: siehe [Changelog](firmware/ChangeLog)

**Wichtiger Hinweis:** Die Pins am J12 dürfen nie mit irgendeinem Pin der anderen Jumper/Stecker-/Buchsenleisten in
Verbindung gebracht werden, da hier getrennte Stromquellen zum Einsatz kommen. Jegliche Verbindung gefährdet den Adapter
und Geräte am eBUS!

### Ethernet Konfiguration
{:id="ethernetconfig"}
Wenn für den Einsatz mit USR-ES1 Modul eine feste IP-Adresse eingestellt werden soll, anstatt diese via DHCP zu beziehen,
dann kann das mit dem
[ebus PIC Loader](https://github.com/john30/ebusd/blob/master/src/tools/README.md)
über den USB-Anschluss oder den Raspberry Pi GPIO/ttyAMA0 vorgenommen werden.

Um bspw. die IP-Adresse 192.168.10.20 mit einer Netzmaske von 255.255.255.0 (=Länge 24) einzustellen, wird der ebus PIC
Loader wie folgt aufgerufen (wobei `/dev/ttyUSB0` evt. durch das richtige USB serial device ersetzt werden muss):
`ebuspicloader -i 192.168.10.20 -m 24 /dev/ttyUSB0`

### LED
Die blaue LED wird zur Signalisierung von Zuständen aus der PIC Firmware genutzt. Hier muss grundsätzlich zwischen
Bootloader und normalem Modus unterschieden werden.

#### LED im Bootloader
{:id="ledboot"}
Wenn die LED nach Anschließen der Stromversorgung sofort permanent sehr hell leuchtet (heller als alle anderen), dann
ist der PIC im Bootloader-Modus und wartet auf Kommandos auf der seriellen Schnittstelle.

#### LED bei normalem Betrieb (ohne Ethernet)
{:id="ledregular"}
Bei normalem Betrieb ist die blaue LED nach Anschließen der Stromversorgung zunächst aus.
* Beim Betrieb mit WIFI blinkt die LED dann langsam, bis der Wemos seine Bereitschaft signalisiert
  (dazu muss der Jumper auf WIFI-Check konfiguriert sein).
* Bleibt die LED mit sehr geringer Helligkeit an, dann ist das enhanced ebusd protocol deaktiviert
  ([siehe Jumper](index#jumper)).
* Ansonsten wird sie bis zur normalen Helligkeit hochgedimmt.
* Nach jeder Initialisierung im enhanced protocol leuchtet sie für 2 Sekunden sehr hell (heller als alle anderen).
* Als sign-of-life macht sie alle 4 Sekunden einen kurzen Ping
* Im Falle eines Protokollfehlers auf eBUS oder Host Seite leuchtet sie für 5 Sekunden sehr hell (heller als alle anderen).

#### LED bei Betrieb mit Ethernet
{:id="ledethernet"}
Ist das Ethernet Modul aufgesteckt und der Jumper an J12 entsprechend gesetzt, dann hat die blaue LED noch weitere
Bedeutungen zusätzlich zu den oben beschriebenen, die in folgenden Stufen durchlaufen werden:
1. Bis der W5500 des USR-ES1 Moduls nach dem Reset antwortet, blinkt die LED (2x pro Sekunde).
2. Bis ein Link auf der Leitung gefunden wurde, blinkt die LED sehr schnell (20x pro Sekunde).
3. Im Fall von DHCP blinkt die LED schnell (4x pro Sekunde), bis eine IP-Adresse per DHCP ausgehandelt wurde. 
