## Willkommen zum eBUS Adapter 2.0!

Dies ist eine Anleitung zum Herstellen eines Adapters, mit dessen Hilfe man mit einer eBUS-fähigen Heizungs-, Lüftungs- oder Solaranlage kommunizieren kann.

Der Adapter besteht aus einer [Basisplatine](base.md) und kann zusätzlich mit einer optionalen [Erweiterungsplatine](extension.md) genutzt werden, die in Verbindung mit einem Wemos D1 den Zugang zum eBUS über WLAN erlaubt.


## Basisplatine

[<img src="base-3d-left.png" width="200" alt="base" title="Basisplatine">](base.md)

[Die Infos zur Herstellung der Basisplatine finden sich hier.](base.md)


## Erweiterungsplatine

[<img src="exten-assemble1.png" width="200" alt="extension" title="Erweiterungsplatine">](extension.md)

[Die Infos zur Herstellung der Erweiterungsplatine finden sich hier.](extension.md)


## Raspberry PI

[<img src="base-rpi.jpg" width="200" alt="rpi-base" title="Basisplatine auf Raspberry PI">](raspberrypi.md)

[Basisplatine huckepack auf dem Raspberry PI](raspberrypi.md)


## Wemos Software

Für den Einsatz des Wemos D1 zur Kommunikation mit eBUS wird eine spezielle Firmware benötigt, [siehe hier](wemosebus.md).

Um zusätzliche Sensoren mit der Erweiterungsplatine nutzen zu können, muss derzeit noch ein eigener Wemos D1 verwendet werden,
der wegen zu hoher Latenzzeiten nicht gleichzeitig als eBUS Worker genutzt werden kann, [siehe hier](wemossensors.md).


## Bauteile

[Hier finden sich die Bauteilelisten für die Basis- und Erweiterungsplatine.](partlist.md)


## Fehlersuche

Tipps zur Fehlersuche auf der Basisplatine für den Fall, dass diesenach Zusammenabu nicht wie erwartet funktioniert,
[finden sich hier](diagnostics.md).


## Weiterführende Links

Einige Links die zum Thema beitragen, bzw. Basisinformationen und Grundlagen enthalten.

* [eBUS Spezifikation (physikalische Schicht OSI 1, Verbindungsschicht OSI 2)](Spec_Prot_12_V1_3_1.pdf)
* [Wiki über Platine V 1.6](https://wiki.fhem.de/wiki/EBUS)
* [Thread über Platine 2.0](https://forum.fhem.de/index.php/topic,75878.0.html)
* [Reichelt Warenkorb Basisplatine](https://www.reichelt.de/my/1381342)
* [Reichelt Warenkorb Erweiterungsplatine](https://www.reichelt.de/my/1389121)
* [Mini Firmware von John für Wemos D1 auf Erweiterungsplatine](https://github.com/john30/ebusd-esp)
* [ebusd Wiki](https://github.com/john30/ebusd/wiki)
* [eBUS background in ebusd Wiki](https://github.com/john30/ebusd/wiki/eBUS-background)
* [eBus Wiki](http://ebus.wiki.org)
