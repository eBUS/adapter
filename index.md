---
ref: index
lang: de
---
## Willkommen zum eBUS Adapter 2.0!

Dies ist eine Anleitung zur Herstellung und zur Inbetriebnahme eines eBUS Adapters, mit dessen Hilfe man mit einer eBUS-fähigen Heizungs-, Lüftungs- oder Solaranlage kommunizieren kann.


### Einführung

Die Version 2 der eBUS Platine wurde hauptsächlich entworfen, um die Abstimmung des Potis zu vermeiden.
Viele Anwender hatten mit der Justierung Probleme, vor allem auch dadurch dass die Bauteile einer gewissen Toleranz unterliegen und die Feinabstimmung etwas kniffelig ist.
Eine weitere Erschwernis war die Art und der Typ des UART, zum Beispiel wie dieser nun angeschlossen werden muss (TX an TX oder TX an RX).

All diese Probleme sollten nun der Vergangenheit angehören und es können die unterschiedlichsten Typen von UARTs angeschlossen werden.

Nebenbei wurde der Funktionsumfang noch wesentlich erweitert:

* Mit Hilfe der Erweiterungsplatine kann ein Wemos mit ESPEasy aufgesteckt werden und Sensoren via I2C, ein Gaszähler oder ein Buzzer für die Alarmausgabe angeschlossen werden.  
  Anschlüsse für Displays (OLED SSD1306 bzw. Nextion) zur lokalen Ausgabe von Alarmen oder Messwerten sind ebenfalls vorhanden.
* Für die Serial-Ethernet Kommunikation wurde extra eine Firmware für den Wemos entwickelt und somit kann die Platine über WLAN mit dem eBUS Dämon kommunizieren.
* Durch das Stecksystem ergeben sich viele Kombinationsmöglichkeiten und jeder Anwender kann sich das auf seine Bedürfnisse anpassen.


### Aufbau

Der Adapter besteht aus einer [Basisplatine](base) und kann optional mit einer zusätzlichen [Erweiterungsplatine](extension) genutzt werden, die in Verbindung mit einem Wemos D1 den Zugang zum eBUS über WLAN erlaubt.

In den folgenden Seiten wird der Aufbau, die Bestückung und das Prüfen der gelöteten Platine beschrieben.  
Die dafür benötigten [Bauteile sind hier aufgeführt](partlist).

* [Basisplatine](base)  
  [<img src="images/base-final.jpg" width="200" alt="base" title="Basisplatine">](base)  
  [Hier finden sich Tipps zur Fehlersuche](diagnostics) für den Fall, dass die Basisplatine nach Zusammenbau nicht wie erwartet funktioniert.

* [Erweiterungsplatine](extension)  
  [<img src="images/exten-final.jpg" width="200" alt="extension" title="Erweiterungsplatine">](extension)

* [Raspberry Pi](raspberrypi)  
  [<img src="images/base-rpi.jpg" width="200" alt="rpi-base" title="Basisplatine auf Raspberry Pi">](raspberrypi)


### Verwendung

Die fertig aufgebaute Basis- und/oder Erweiterungsplatine kann in verschiedenen Varianten mit Wemos und/oder UART verwendet werden:

* Basisplatine mit Erweiterung:  
  * 2 Wemos:  
    - ebusd-esp als WLAN Brücke für ebusd
    - ESPEasy für Sensorik, Display etc.
  * UART und Wemos:  
    - UART für ebusd
    - ESPEasy für Sensorik, Display etc.
  * 1 Wemos:  
    - ebusd-esp als WLAN Brücke für ebusd (ohne Sensorik, Display etc.)

* Basisplatine solo (ohne Erweiterung):  
  * Wemos:  
    - ebusd-esp als WLAN Brücke für ebusd
  * UART:  
    - UART für ebusd

* Erweiterungsplatine solo
  * 1 Wemos:  
    - ESPEasy für Sensorik, Display etc.

Zusätzlich wird noch ein Rechner (bspw. Rasperry PI) mit ebusd benötigt, da der Wemos mit ebusd-esp diesen nicht ersetzt, sondern lediglich die serielle Schnittstelle im WLAN bereitstellt.

#### Verbindungen

Hier ist eine Übersicht der einzelnen Komponenten mit ihren Verbindungen:
[<img src="images/schema.png" width="600" alt="schema" title="Verbindungsschema">](images/schema.png)

* Heizung  
  wird mit dem Adapter über eine 2-Drahtleitung verbunden.
* Adapter  
  wird mit ebusd über USB (UART) oder WLAN (Wemos ebusd-esp) verbunden.
* ebusd  
  stellt TCP client, MQTT und HTTP für FHEM, Node-Red und weitere zur Verfügung.


#### Wemos Firmware

Für den Einsatz des Wemos D1 zur Kommunikation mit eBUS wird eine [spezielle Firmware benötigt](wemosebus).

Um zusätzliche Sensoren mit der Erweiterungsplatine nutzen zu können, muss derzeit noch ein [eigener Wemos D1 verwendet werden](wemossensors),
der wegen zu hoher Latenzzeiten nicht gleichzeitig als eBUS Worker genutzt werden kann.


### Weiterführende Links

Hier einige Links, die zum Thema beitragen, bzw. Basisinformationen und Grundlagen enthalten:

* [eBUS Spezifikation (physikalische Schicht OSI 1, Verbindungsschicht OSI 2)](Spec_Prot_12_V1_3_1.pdf)
* [Wiki über Platine V 1.6](https://wiki.fhem.de/wiki/eBUS)
* [Thread über Platine 2.0](https://forum.fhem.de/index.php/topic,75878.0.html)
* [Reichelt Warenkorb Basisplatine](https://www.reichelt.de/my/1381342)
* [Reichelt Warenkorb Erweiterungsplatine](https://www.reichelt.de/my/1389121)
* [Mini Firmware von John für Wemos D1 auf Erweiterungsplatine](https://github.com/john30/ebusd-esp)
* [ebusd Wiki](https://github.com/john30/ebusd/wiki)
* [eBUS background in ebusd Wiki](https://github.com/john30/ebusd/wiki/eBUS-background)
* [eBUS Wiki](http://ebus-wiki.org)
