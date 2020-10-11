---
ref: wemosebus
lang: de
navorder: 6
navtitle: Wemos
---
## Wemos D1 Mini Firmware für eBUS

Aufgrund viel zu hoher Latenzzeiten lässt sich leider ESPEasy oder eine andere Firmware nicht für einen stabilen Zugriff
auf den eBUS mit ebusd nutzen.
Deshalb gibt es dafür eine eigene [Low-Latency Firmware hier](http://github.com/john30/ebusd-esp).

Die Konfiguration ist hier noch einmal in Deutsch beschrieben.


### Warum ebusd-esp?

ebusd-esp wurde von John extra für dieses Projekt programmiert und diese Firmware ermöglicht sehr kleine Latenzzeiten von 5 Millisekunden.

ESPEasy und ESP-Link haben zu große Latenzzeiten und können im derzeitigen Entwicklungsstand leider nicht für eBUS eingesetzt werden.

Der Empfang würde zwar meist funktionieren, aber das Senden an den eBUS funktioniert nicht bzw. nur schlecht und funkt sozusagen ständig den anderen Teilnehmern am Bus dazwischen und stört deren Kommunikation.


### Software Konfiguration Wemos ebusd-esp

Der Wemos muss [laut Anleitung geflasht](http://github.com/john30/ebusd-esp) und dann mit einer der folgenden beiden Methoden konfiguriert werden:
* [mit serieller Konsole](#konfiguration-mit-serieller-konsole) (z.B.: Arduino serieller Monitor)
* [über WLAN](#konfiguration-mit-wlan)


#### Konfiguration mit serieller Konsole

Nach Reset des Wemos muss innerhalb von 5 Sekunden eine Taste gedrückt werden (im Arduino einfach "Senden" drücken), um in die Konfiguration zu gelangen.  

Hier sollte dann über die Menüpunkte am besten eine statische IP eingestellt werden same Netzwerk Maske und Gateway.

Unter Punkt 6 wird gewählt ob RX und TX geswappt werden soll, hier im Beispiel wird direkt gewählt.

Nachdem die Änderungen eingetragen sind, muss noch "speichern" aufgerufen werden.

```
Welcome to eBUS adapter 2.0, build 20171230
ebusd device string: 10.0.0.161:8889
Press any key within 5 seconds to change the configuration...
Entering configuration mode.
Chip ID: ********
CPU frequency: 80
Free heap: 37648
Hostname: ebus-******

Configuration:
 1. WIFI SSID: SSID des eigenen WLAN
 2. WIFI secret: Passwort
 3. WIFI IP address: 10.0.0.161/24, gateway: 10.0.0.254
 4. ebusd TCP/UDP mode: TCP
 5. ebusd TCP port: 8889
 6. ebusd RX+TX PINs: direct RX+TX (GPIO3+1)
 7. Management TCP port: 9999
 8. LED PINs: RX:disabled, TX:disabled
 9. Initial PIN direction: D0:L, D4:L

 d. Set current PIN direction: D0:L, D4:L
 t. Toggle current output PIN
 e. Dump EEPROM content
 f. Load factory settings
 F. Factory reset (i.e. erase EEPROM)
 r. Reboot (without saving)
 E. Start with temporary echo (for debugging only!)
 0. Start

Enter your choice:
```


#### Konfiguration mit WLAN

Nachdem einer neuer Wemos (oder ein alter, der einen Factory Reset via serieller Konsole bekommen hat) mit der ebusd-esp Firmware geflasht wurde, aktiviert dieser einen WLAN Access Point mit SSID "EBUS".

[<img src="img/wemosebus-wlan.jpg" width="200" alt="WLAN" title="WLAN">](img/wemosebus-wlan.jpg)  
Hat man sich mit diesem WLAN verbunden (kein Passwort notwendig), lässt sich der Wemos im Web-Browser unter der Adresse [http://192.168.4.1](http://192.168.4.1) aufrufen und konfigurieren.

Dabei werden neu eingegebene Werte/Parameter zunächst mit "Check & Update" geprüft und anschließend mit "Save & Reset" gespeichert und der Wemos neu gestartet.
[<img src="img/wemosebus-webcfg.png" width="300" alt="Web configuration" title="Web configuration">](img/wemosebus-webcfg.png)


### ebusd Konfiguration

Die in diesem Beispiel verwendete IP und Portnummer 8889 sollte in der /etc/default/ebusd dann so aussehen:

`EBUSD_OPTS="-d 10.0.0.161:8889 -l /var/log/ebusd.log --scanconfig --latency=20000 --address=01"`

Hier wurde auch mit "--address=..." eine neue Adresse vergeben, damit dieser Wlan Adapter nicht mit einem 2. Adapter kollidiert. Die default Adresse (keine Angabe) ist die "31" (hex).

Alternativ: Wenn der der Einsatz von mehr als einem Adpater geplant ist, dann muss die Master-Adresse der zweiten ebusd Instanz geändert werden, sonst kollidieren die default Adressen miteinander.


### Logauszug Rawdata

```
2017-11-08 11:04:10.134 [bus notice] <aa
2017-11-08 11:04:10.178 [bus notice] <aa
2017-11-08 11:04:10.221 [main notice] starting initial broadcast scan
2017-11-08 11:04:10.225 [bus notice] <aa     Syn vom eBus, Client dürfen den eBus belegen
2017-11-08 11:04:10.228 [bus notice] >01     eigene Adresse wird an den eBus gesendet um eine Anfrage zu initiieren
2017-11-08 11:04:10.233 [bus notice] <01     Bestätigung vom eBus, der Client darf nun senden
2017-11-08 11:04:10.237 [bus notice] >fe     nun setzt der Konverter seine Abfragen auf und baut  diese Byteweise zusammen
2017-11-08 11:04:10.242 [bus notice] <fe     jedes Byte wird vom eBus bestätigt
2017-11-08 11:04:10.246 [bus notice] >07     usw.
2017-11-08 11:04:10.251 [bus notice] <07
2017-11-08 11:04:10.252 [bus notice] >04
2017-11-08 11:04:10.258 [bus notice] <04
2017-11-08 11:04:10.259 [bus notice] >00
2017-11-08 11:04:10.264 [bus notice] <00
2017-11-08 11:04:10.266 [bus notice] >14
2017-11-08 11:04:10.272 [bus notice] <14    die Anfrage ist durch, nun muss etwas gewartet werden, 
2017-11-08 11:04:10.275 [bus notice] >aa    es folgen SYN und andere Client dürfen wieder senden
2017-11-08 11:04:10.280 [bus notice] <aa
2017-11-08 11:04:10.327 [bus notice] <aa
2017-11-08 11:04:10.372 [bus notice] <aa
2017-11-08 11:04:10.418 [bus notice] <aa
2017-11-08 11:04:10.462 [bus notice] <aa
2017-11-08 11:04:10.506 [bus notice] <aa
2017-11-08 11:04:10.552 [bus notice] <aa
```
