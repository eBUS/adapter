---
ref: wemos
lang: de
navorder: 4
navtitle: Wemos
---
## Wemos D1 mini für eBUS

Die Kommunikation am eBUS ist heikel in Bezug auf Latenzzeiten, also die Verzögerung von gesendeten oder empfangenen
Bytes. So muss bspw. ein Folgebyte einer Nachricht zwingend innerhalb von nur 50 ms gesendet werden, da die Nachricht
andernfalls verworfen wird.

Werden eBUS Nachrichten via WLAN über einen Wemos transportiert, dann muss die Wemos Firmware also dafür sorgen, dass
das mit kleinstmöglicher Verzögerungszeit geschieht.

Aus diesem Grund wurde die [ebusd-esp Firmware](https://github.com/john30/ebusd-esp) entwickelt, mit der die
Verzögerung unter normalen Bedingungen unter 20 ms bleibt.

Mit Einführung des enhanced ebusd protocol seit [ebusd Version 21.1](https://github.com/john30/ebusd/releases/tag/v21.1)
wird der besonders kritische Teil der Arbitrierung am eBUS in die Hardware verlagert (PIC Controller).
Dadurch kann jetzt auch Wemos Firmware eingesetzt werden, die ein Port-Forwarding vom Netzwerk an die serielle
Schnittstelle erlaubt und bspw. durch Ausführung anderer Tasks eine höhere Latenzzeit ausweist.

Bis dato sind folgende Wemos Firmwares mit ebusd enhanced protocol als funktionstüchtig getestet worden:
 
* [ebusd-esp](#ebusd-esp)
* [ESPEasy](#espeasy) [TODO Testen]
* [ESPHome](#esphome) mit [TODO] plugin [TODO Testen]
* [ESP-Link](#esplink) [TODO Testen]


### Konfiguration ebusd-esp
{:id="ebusd-esp"}
Der Wemos muss [laut Anleitung geflasht](https://github.com/john30/ebusd-esp) und dann mit einer der folgenden Methoden konfiguriert werden:
* [mit serieller Konsole](#konfiguration-mit-serieller-konsole) (z.B.: Arduino serieller Monitor)
  Achtung: Das geht nur, während der Wemos **nicht** auf dem Adapter steckt.
* [über WLAN](#konfiguration-mit-wlan)


#### Konfiguration mit serieller Konsole

Nach Reset des Wemos muss innerhalb von 5 Sekunden eine Taste gedrückt werden (im Arduino einfach "Senden" drücken), um in die Konfiguration zu gelangen.  

Hier sollte dann über die Menüpunkte am besten eine statische IP eingestellt werden samt Netzwerk Maske und Gateway.

Nachdem die Änderungen eingetragen sind, muss noch "speichern" aufgerufen werden.
```
Welcome to eBUS adapter 3, build 20201122
ebusd device string: 10.0.0.161:9999
Press any key within 5 seconds to change the configuration...
Entering configuration mode.
Chip ID: ********
Hostname: ebus-******

Configuration:
 1. WIFI SSID: <SSID des eigenen WLAN>
 2. WIFI secret: <WLAN Passwort>
 3. WIFI IP address: 10.0.0.161/24, gateway: 10.0.0.254
 w. WIFI power: normal
 4. WIFI hostname: ebus-******
 5. eBUS RX+TX PINs: Adapter 3 RX+TX (GPIO3+1)
 6. ebusd connection: enhanced on port 9999
 7. HTTP TCP port: 80
 8. LED PINs: RX:disabled, TX:disabled
 9. Initial PINs: D4:H

 p. Set current PINs: D4:H
 t. Toggle current output PIN
 s. Scan & read sensors
 c. Connect WIFI
 e. Dump EEPROM content
 f. Load factory settings
 F. Factory reset (i.e. erase EEPROM)
 o. OTA enabled: waiting
 r. Reboot (without saving)
 E. Start with temporary echo (for debugging only!)
 0. Start

Enter your choice:
```


#### Konfiguration mit WLAN

Nachdem einer neuer Wemos (oder ein alter, der einen Factory Reset via serieller Konsole bekommen hat) mit der ebusd-esp Firmware geflasht wurde, aktiviert dieser einen WLAN Access Point mit SSID "EBUS".

[<img src="v2/img/wemosebus-wlan.jpg" width="200" alt="WLAN" title="WLAN">](v2/img/wemosebus-wlan.jpg)  
Hat man sich mit diesem WLAN verbunden (kein Passwort notwendig), lässt sich der Wemos im Web-Browser unter der Adresse [http://192.168.4.1](http://192.168.4.1) aufrufen und konfigurieren.

Dabei werden neu eingegebene Werte/Parameter zunächst mit "Check & Update" geprüft und anschließend mit "Save & Reset" gespeichert und der Wemos neu gestartet.
[<img src="v2/img/wemosebus-webcfg.png" width="300" alt="Web configuration" title="Web configuration">](v2/img/wemosebus-webcfg.png)


### ESPEasy Konfiguration
{:id="espeasy"}
[TODO]

### ESPHome Konfiguration
{:id="esphome"}
[TODO]

### ESP-Link Konfiguration
{:id="esplink"}
[TODO]
