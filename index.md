---
ref: index
lang: de
---
## Willkommen zum eBUS Adapter 3!

Dies ist die Dokumentation des eBUS Adapters, mit dessen Hilfe man mit einer eBUS-fähigen Heizungs-, Lüftungs- oder
Solaranlage kommunizieren kann.

### Einführung

Version 3 des eBUS Adapters erfüllt erstmals die von der eBUS Spezifikation geforderten Zeiten bei der Arbitrierung.

Dies wird durch Einsatz eines PIC ermöglicht, der u.a. folgende Vorteile mit sich bringt:

 * minimale Zeitverzögerung durch Hardware-nahe Programmierung
 * flexible, konfigurierbare Varianten zur Verbindung mit dem Host:
   * USB serial mit CP2102
   * Raspberry Pi über GPIO/ttyAMA0
   * WIFI durch Wemos D1 mini mit ebusd-esp
   * Ethernet durch W5500
 * aktualisierbare Firmware mittels seriellem Bootloader

Um all diese Optionen auf einer 5cm x 5cm großen Platine realisieren zu können, wird fast nur in SMD bestückt:

[<img src="img/smd-layout.png" width="200" alt="schema" title="Layout">](img/smd-layout.png)

Die SMD Technik bietet ebenfalls einige Vorteile:
* hohe Packdichte
* alle Varianten auf einer Platine
* kein CP2102 Aufsteckmodul mehr notwendig
* günstige Bestückung

Die einzelnen Varianten bieten zum Teil Optionen zum Anschluss von Sensoren und/oder Displays.

### Varianten
In allen Varianten ist die Unterstützung für USB fest verbaut, da der CP2102 immer direkt auf der Platine bestückt ist.
Dies kann durch Jumper konfiguriert werden.

#### USB
Zur Nutzung des Adapters über den USB-Anschluss J2 müssen die Jumper wie folgt gesetzt werden:
* J1: 2-3
* J4: 2-3

Die Stromversorgung erfolgt direkt über den USB-Anschluss J2 am Adapter.

#### Raspberry Pi
Durch Einsatz einer 2x13 poligen Buchsenleiste an J8 lässt sich der Adapter auf den Raspberry Pi aufstecken.
Die Jumper müssen dazu wie folgt gesetzt werden:
* J1: 1-2
* J4: 1-2

Die Stromversorgung erfolgt direkt über die Raspberry Pi Buchsenleiste J8.

#### WIFI
Wird ein Wemos D1 mini auf J9 gesteckt, dann lässt sicher der Adapter via WLAN ansprechen.
Die Jumper müssen dazu wie folgt gesetzt werden:
* J1: 1-2
* J4: offen

Die Stromversorgung erfolgt direkt über den USB-Anschluss am Wemos.

#### Ethernet
Wird ein W5500 auf J10 gesteckt, dann lässt sicher der Adapter via LAN nutzen.
Die Jumper müssen dazu wie folgt gesetzt werden:
* J1: 1-2
* J4: 2-3

Die Stromversorgung erfolgt direkt über den USB-Anschluss J2 am Adapter.

Die Ethernet Konfiguration (IP Adresse, Netzmaske, Gateway) wird durch den Bootloader im PIC ermöglicht und über den
USB-Anschluss J2 vorgenommen, [siehe Ethernet Konfiguration](picfirmware#ethernet-konfiguration).

### Anschluss von Sensoren, Aktoren oder Displays
In den Varianten Wemos und Raspberry Pi stehen folgende Pin Header für den Anschluss weiterer Komponenten zur Verfügung:
* J3: Gassensor oder Schalter
* J5: I2C, z.B. OLED SSD1306 oder Nextion 
* J6:
  * bei Raspberry Pi: I2C wie oben (Pins 1-4)
  * bei Wemos: I2C (Pins 1-4) und zusätzlich noch Wemos D0 (Pin 5) und A0 (Pin 6)
* J7: 1wire, z.B. für Temperatursensor DS18B20


### weitere Anschlüsse

#### PIC Programmieranschluss J11
Am J11 lässt sich die Firmaware des PIC mit einem entsprechenden Programmiergerät austauschen inkl. des Bootloaders.
Das sollte nur in den seltensten Fällen notwendig sein, da der PIC vor Auslieferung bereits programmiert wird.

#### PIC Anschluss J12
Dieser Anschluss führt Leitungen des PIC und deren Belegung und Nutzungsmöglichkeiten hängen ausschließlich von der
PIC Firmware ab. Details dazu siehe [PIC Firmware](picfirmware).

**Wichtiger Hinweis:** Die Pins am J12 dürfen nie mit irgendeinem Pin der anderen Jumper/Stecker-/Buchsenleisten in
Verbindung gebracht werden, da hier getrennte Stromquellen zum Einsatz kommen. Jegliche Verbindung gefährdet den Adapter
und Geräte am eBUS!


### Verwendung

Neben dem Adapter wird eine Software benötigt, die den eBUS Verkehr interpretiert und auswertet. Das übernimmt bspw.
[ebusd](https://github.com/john30/ebusd/), der ebenfalls auf einen Rasperry PI installiert werden kann.

#### Verbindungen

Hier ist eine Übersicht der einzelnen Komponenten mit ihren Verbindungen:
[<img src="img/smd-schema.png" width="600" alt="schema" title="Verbindungsschema">](img/smd-schema.png)

* Heizung  
  wird mit dem Adapter über eine 2-Drahtleitung verbunden.
* Adapter  
  wird mit ebusd entweder über USB (UART), über GPIO (UART) des Raspberry Pi, über WLAN ([Wemos ebusd-esp](v2/wemosebus)) TODO überarbeiten  
  oder LAN verbunden.
* ebusd  
  stellt TCP Client, MQTT und HTTP für FHEM, Node-Red, ioBroker und weitere zur Verfügung.

#### Gleichzeitige Verwendung von USB für ebusd und Wemos für Sensoren:
TODO Testen:  
Die Jumper müssen dazu wie folgt gesetzt werden:
* J1: 2-3
* J4: 2-3

Die Stromversorgung erfolgt direkt über den USB-Anschluss J2 am Adapter und RX/TX des Wemos samt seines USB serial sind
nicht nutzbar (auf RX kommt ebus Traffic an).

**Achtung:** immer nur eine Stromversorgung verbinden, also maximal einen Anschluss von:
* USB-Anschluss J2 des Adapters
* Raspberry Pi Buchsenleiste J8 des Adapters
* USB Anschluss am Wemos 


### Weiterführende Links

Hier einige Links, die zum Thema beitragen, bzw. Basisinformationen und Grundlagen enthalten:

* [eBUS Spezifikation (physikalische Schicht OSI 1)](Spec_Prot_12_V1_3_1.pdf)
* [Wiki über Platine V 1.6](https://wiki.fhem.de/wiki/eBUS)
* [Dokumentation Adapter V 2.0-2.2](https://ebus.github.io/adapter/v2/)
* [Reichelt Warenkorb Stift-/Buchsenleisten](https://www.reichelt.de/my/1758624)
* [ebusd-esp Firmware für Wemos D1](https://github.com/john30/ebusd-esp)
* [ebusd Wiki](https://github.com/john30/ebusd/wiki)
