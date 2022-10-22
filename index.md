---
ref: index
lang: de
ishome: true
navorder: 1
navtitle: Willkommen
---
## Willkommen zum eBUS Adapter 3!

Dies ist die Dokumentation des eBUS Adapters, mit dessen Hilfe man mit einer eBUS-fähigen Heizungs-, Lüftungs- oder
Solaranlage kommunizieren kann.

Einen solchen [Adapter kann man hier reservieren](https://ebusd.de/meinadapter/)
und dieser wird dann der Reihe nach versendet, sobald die nächste Charge verfügbar ist.

### Einführung

Version 3 des eBUS Adapters erfüllt erstmals die von der eBUS Spezifikation geforderten Zeiten bei der Arbitrierung.

Dies wird durch Einsatz eines PIC ermöglicht, der u.a. folgende Vorteile mit sich bringt:

 * minimale Zeitverzögerung durch Hardware-nahe Programmierung
 * flexible, konfigurierbare Varianten zur Verbindung mit dem Host:
   * [USB](#usb){:.usb} serial über CP2102 (onboard)
   * [Raspberry Pi](#raspberry-pi){:.rpi} über GPIO/ttyAMA0
   * [WIFI](#wifi){:.wifi} über LOLIN/Wemos D1 mini mit ESP-8266
   * [Ethernet](#ethernet){:.ethernet} über USR-ES1 Modul mit W5500
 * volle Unterstützung für ebusd enhanced protocol sowie standard protocol
 * aktualisierbare [Firmware](picfirmware) mittels seriellem Bootloader

Um all diese Optionen auf einer 5cm x 5cm großen Platine realisieren zu können, wird fast nur mit SMD bestückt:

[<img src="img/smd-layout.png" width="200" alt="schema" title="Layout">](img/smd-layout.png)

Die SMD Technik bietet ebenfalls einige Vorteile:
* hohe Packdichte
* alle Varianten auf einer Platine
* kein CP2102 Aufsteckmodul mehr notwendig
* günstige Bestückung

Zwei der Varianten bieten auch die Option zum Anschluss von Sensoren und/oder Displays.

### Verbindungen

Hier ist eine Übersicht der einzelnen Komponenten mit ihren Verbindungen:
[<img src="img/smd-schema.png" width="600" alt="schema" title="Verbindungsschema">](img/smd-schema.png)

* Heizung  
  wird mit dem Adapter über eine 2-Drahtleitung verbunden.
* Adapter  
  wird mit ebusd verbunden über
  * USB (UART),
  * GPIO (UART) des Raspberry Pi,
  * WIFI ([Wemos](wemosebus)) oder
  * LAN (USR-ES1 Modul mit W5500).
* ebusd  
  interpretiert das eBUS Protokoll und macht die Daten bidirektional via TCP, HTTP, MQTT und KNX
  für FHEM, Home-Assistant, Node-Red und weitere verfügbar.

### Varianten
{:id="variants"}
In allen Varianten ist die Unterstützung für USB fest verbaut, da der CP2102 immer direkt auf der Platine bestückt ist.
Diese ist notwendig, um bspw. die PIC Firmware zu aktualisieren oder eine spezielle Ethernet Konfiguration vorzunehmen.

Über Jumper wird die entsprechende Variante konfiguriert.

Als Protokoll zwischen ebusd und dem Adapter kann sowohl direkt das eBUS Protokoll ("standard protocol"), als auch das
ebusd "enhanced protocol" verwendet werden. Das enhanced protocol nutzt alle Vorteile des Adapters, indem die eBUS
Arbitrierung direkt durch die PIC Firmware übernommen wird.

**Hinweis zu high-speed enhanced Modus:**  
{:id="ens"}  
Für das enhanced protocol gibt es bei serieller Verbindung (USB, RPI und WIFI) zusätzlich die Variante **high-speed**, womit
unnötige Verzögerungszeiten durch den Transfer der Daten von/zu ebusd (oder ESP) vermieden werden.
Das wird durch Setzen eines [Jumpers an J12](picfirmware) aktiviert.  
Die ebusd device Konfiguration muss dann vom Präfix `enh:` auf den Präfix `ens:` gesetzt werden, sofern eine direkte
serielle Verbindung vorliegt (d.h. bei USR oder RPI).  
Für WIFI muss die ESP Firmware entsprechend konfiguriert werden und die ebusd device Konfiguration bleibt bei `enh:`.

#### USB
{:.usb}
[<img src="img/smd-3dusb.png" width="200" alt="USB" title="USB">](img/smd-3dusb.jpg)  
Zur Nutzung des Adapters über den USB-Anschluss J2 müssen die Jumper wie folgt gesetzt werden:
* [J1: USB](img/smd-usb.jpg)
* [J4: USB](img/smd-usb.jpg)

Die Stromversorgung erfolgt direkt über den USB-Anschluss J2 am Adapter.

Die ebusd device Konfiguration lautet z.B. `-d enh:/dev/ttyUSB0`, wobei `ttyUSB0` bei mehreren angeschlossenen
USB serial Adaptern anders lauten kann.  
Für den high-speed enhanced Modus, [siehe Hinweis oben](#ens).

#### Raspberry Pi
{:.rpi id="rpi"}
[<img src="img/smd-3drpi.png" width="200" alt="RPI" title="RPI">](img/smd-3drpi.jpg)  
Durch Einsatz einer 2x13 poligen Buchsenleiste an J8 lässt sich der Adapter auf den
[Raspberry Pi](https://www.raspberrypi.org/) aufstecken.
Die Jumper müssen dazu wie folgt gesetzt werden:
* [J1: RPI](img/smd-rpi.jpg)
* [J4: RPI](img/smd-rpi.jpg)

Die Stromversorgung erfolgt direkt über die Raspberry Pi Buchsenleiste J8.

Die ebusd device Konfiguration lautet: `-d enh:/dev/ttyAMA0 --latency=50`  
Für den high-speed enhanced Modus, [siehe Hinweis oben](#ens).

[Details zur Einrichtung des Raspberry Pi finden sich hier](raspberrypi).

#### WIFI
{:.wifi}
[<img src="img/smd-3dwifi.png" width="200" alt="WIFI" title="WIFI">](img/smd-3dwifi.jpg)  
Wird ein [LOLIN/Wemos D1 mini mit ESP-8266](https://docs.wemos.cc/en/latest/d1/d1_mini.html) auf J9 gesteckt,
dann lässt sicher der Adapter via WIFI verwenden.
Die Jumper müssen dazu wie folgt gesetzt werden:
* [J1: RPI](img/smd-wifi.jpg)
* [J4: offen (oder auf RPI)](img/smd-wifi.jpg)
* [J12: Pins 4-5 verbinden (WIFI-Check)](img/smd-wifi.jpg)

Die Stromversorgung erfolgt direkt über den USB-Anschluss am Wemos.

**Achtung:** Die Wemos sind unglaublich heikel in Bezug auf die Stromversorgung!
Da kommen die unterschiedlichsten Effekte zum Vorschein, wenn die Stromversorgung ungenügend ist,
wie z.B. reboots oder gar keine bzw. nur schlechte und instabile Verbindung zum Access Point.

Der Wemos muss mit einer passenden Firmware geflasht werden, z.B. [ebusd-esp](https://github.com/john30/ebusd-esp)
(ab [Version 22.11.2020](https://github.com/john30/ebusd-esp/blob/master/Changelog.md#build-20201122)).
ebusd-esp muss dann auf "Adapter 3 RX+TX" eingestellt werden.

Die ebusd device Konfiguration lautet z.B. `-d enh:192.168.178.2:9999`, wobei `192.168.178.2` durch die richtige
IP-Adresse ersetzt werden muss (ebusd enhanced high-speed Modus ist nicht für Netzwerk-basierte Übertragung gültig).  
Für den enhanced high-speed Modus als Transport zwischen Adapter und ebusd-esp, [siehe Hinweis oben](#ens).

#### Ethernet
{:.ethernet}
[<img src="img/smd-3dethernet.png" width="200" alt="Ethernet" title="Ethernet">](img/smd-3dethernet.jpg)  
Wird ein [USR-ES1 Modul mit W5500](https://www.pusr.com/download/ES1/USR-ES1-EN-V1.0.pdf) auf J10 gesteckt,
dann lässt sich der Adapter via LAN verwenden.
Die Jumper müssen dazu wie folgt gesetzt werden:
* [J1: RPI](img/smd-ethernet.jpg)
* [J4: USB](img/smd-ethernet.jpg)
* [J12: Pins 5-6 verbinden (Ethernet Modus)](img/smd-ethernet.jpg)

Die Stromversorgung erfolgt direkt über den USB-Anschluss J2 am Adapter.

Die Ethernet Konfiguration (IP-Adresse, Netzmaske, Gateway) wird durch den Bootloader im PIC ermöglicht und über den
USB-Anschluss J2 vorgenommen, [siehe Ethernet Konfiguration](picfirmware#ethernetconfig).

Die ebusd device Konfiguration lautet z.B. `-d enh:192.168.178.2:9999`, wobei `192.168.178.2` durch die richtige
IP-Adresse ersetzt werden muss (ebusd enhanced high-speed Modus ist nicht für Netzwerk-basierte Übertragung gültig).

Durch die PIC Firmware wird die MAC Adresse des Adapters im LAN auf AE:B0:53:XX:XX:XX gesetzt, wobei die XX von der ID
des PIC abhängen (`AEB053` steht für "Adapter eBUS 3").

{% if site.withcircuit %}
### Schaltplan

{% endif %}
### Anschluss von Sensoren, Aktoren oder Displays
In den Varianten mit Wemos und Raspberry Pi stehen folgende Pin Header für den Anschluss weiterer Komponenten zur
Verfügung:
* J3: Gassensor oder Schalter
* J5: I2C, z.B. OLED SSD1306 oder Nextion
* J6:
  * bei Raspberry Pi: I2C wie oben (Pins 1-4)
  * bei Wemos: I2C (Pins 1-4) und zusätzlich noch Wemos D0 (Pin 5) und A0 (Pin 6)
* J7: 1wire, z.B. für Temperatursensor DS18B20


### weitere Anschlüsse

#### PIC Programmieranschluss J11
Am Programmieranschluss J11 lässt sich die [Firmware](picfirmware) des PIC mit einem entsprechenden Programmiergerät
austauschen inkl. des Bootloaders.
Das sollte nur in den seltensten Fällen notwendig sein, da der PIC vor Auslieferung bereits programmiert wurde und
der Adapter somit sofort einsetzbar ist.
Details dazu unter [PIC Firmware](picfirmware#firmware-update).

#### PIC Anschluss J12
Dieser Anschluss führt Leitungen des PIC und deren Belegung und Nutzungsmöglichkeiten hängen ausschließlich von der
PIC Firmware ab, siehe unter [PIC Firmware](picfirmware#versions).

**Wichtiger Hinweis:** Die Pins am J12 dürfen mit keinem Pin der anderen Jumper/Stecker-/Buchsenleisten in
Verbindung gebracht werden, da hier verschiedene isolierte Stromquellen zum Einsatz kommen.
Jegliche Verbindung gefährdet den Adapter und potentiell auch Geräte am eBUS!

Hier ist ein Bild, das die beiden isolierten Hälften der Platine darstellt: rot für eBUS und grün für USB etc.:  
<img src="img/smd-2power.png" width="200" alt="2 Stromkreise" title="2 Stromkreise">

### Verwendung

Neben dem Adapter wird eine Software benötigt, die den eBUS Verkehr interpretiert und auswertet. Das übernimmt bspw.
[ebusd](https://github.com/john30/ebusd/), der auch auf einen Raspberry Pi installiert werden kann.

### Überblick Jumper/Pinleisten, Funktionen
{:id="jumper"}

| **Anschluss** | Funktion               | [USB](img/smd-usb.jpg)    | [Raspberry Pi](img/smd-rpi.jpg) | [WIFI](img/smd-wifi.jpg)       | [Ethernet](img/smd-ethernet.jpg) |
|:-------------:|------------------------|---------------------------|---------------------------------|--------------------------------|----------------------------------|
|    **J1**     | Jumper TX              | [USB](img/smd-usb.jpg)    | [RPI](img/smd-rpi.jpg)          | [RPI](img/smd-wifi.jpg)        | [RPI](img/smd-ethernet.jpg)      |
|    **J2**     | USB-Anschluss          | USB-Anschluss             | -                               | -                              | Strom-Anschluss                  |
|    **J3**     | Gassensor              | -                         | Gassensor                       | Gassensor                      | -                                |
|    **J4**     | Jumper POWER           | [USB](img/smd-usb.jpg)    | [RPI](img/smd-rpi.jpg)          | [(RPI)](img/smd-wifi.jpg)      | [USB](img/smd-ethernet.jpg)      |
|    **J5**     | I2C                    | -                         | I2C                             | (I2C)*                         | -                                |
|    **J6**     | I2C                    | -                         | I2C                             | (I2C)*+ext                     | -                                |
|    **J7**     | 1wire Sensor           | -                         | 1wire Sensor                    | 1wire Sensor                   | -                                |
|    **J8**     | Buchsenleiste RPi GPIO | -                         | Raspberry Pi                    | -                              | -                                |
|    **J9**     | Buchsenleiste Wemos    | -                         | -                               | Wemos D1 mini                  | -                                |
|    **J10**    | Buchsenleiste USR-ES1  | -                         | -                               | -                              | USR-ES1 W5500                    |
|    **J11**    | PIC PROG               | -                         | -                               | -                              | -                                |
|    **J12**    | PIC AUX                | [PIC Jumper](picfirmware) | [PIC Jumper](picfirmware)       | [PIC Jumper](picfirmware): 4-5 | [PIC Jumper](picfirmware): 5-6   |
|  **J13/J14**  | eBUS-Anschluss         | eBUS                      | eBUS                            | eBUS                           | eBUS                             |

\* Zu den Punkten in Klammern:
  * I2C wird derzeit noch nicht von der [ebusd-esp](https://github.com/john30/ebusd-esp) Firmware unterstützt.
  * bei WIFI kann J4 weggelassen oder auf RPI gesteckt werden.

[<img src="img/smd-jumper.png" height="400" alt="Jumper" title="Jumper">](img/smd-jumper.png)


### LEDs
Der Adapter verfügt über 4 LEDs mit folgender Zuordnung:
* gelb: Stromversorgung PIC
* blau: Signale vom PIC
* grün: eBUS Empfangen
* rot: eBUS Senden

Nur wenn die gelbe LED leuchtet, ist der PIC mit Strom versorgt und kann überhaupt arbeiten.
Die grüne und rote LED leuchten beim entsprechenden eBUS Traffic, wobei die grüne permanent leuchtet, wenn die eBUS
Leitung noch nicht angeschlossen ist oder wenn auf der Leitung zu wenig Spannung vorgefunden wird.
Die blaue LED wird von der PIC Firmware gesteuert, was [hier beschrieben ist](picfirmware#led).


### Weiterführende Links

Hier einige Links, die zum Thema beitragen, bzw. Basisinformationen und Grundlagen enthalten:

* [eBUS Spezifikation (physikalische Schicht OSI 1)](Spec_Prot_12_V1_3_1.pdf)
* [Wiki über Platine V 1.6](https://wiki.fhem.de/wiki/eBUS)
* [Dokumentation Adapter V 2.0-2.2](v2/)
* [Reichelt Warenkorb Stift-/Buchsenleisten](https://www.reichelt.de/my/1774398):
  * die kurzen Stiftleisten und Jumper sind für J1, J4, J11 und J12 (1x6 in 2 Stück 1x3 teilen, 1x14 in 1x8 + 1x6 teilen)
  * die 1x6 Buchsenleisten sind für J10
  * die langen Stiftleisten sind für J3, J5, J6 und J7
* [ebusd-esp Firmware für Wemos D1](https://github.com/john30/ebusd-esp)
* [ebusd Wiki](https://github.com/john30/ebusd/wiki)
