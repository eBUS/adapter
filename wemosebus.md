# Wemos D1 Mini-Firmware für eBUS Betrieb

Aufgrund viel zu hoher Latenzzeiten lässt sich leider ESPEasy oder eine andere Firmware nicht für einen stabilen Zugriff
auf den eBUS mit ebusd nutzen.
Deshalb gibt es dafür eine eigene [Low-Latency Firmware hier](http://github.com/john30/ebusd-esp).

Die Konfiguration ist hier noch einmal in Deutsch beschrieben.


## Software Konfiguration Wemos ebusd-esp

Den Wemos [laut Anleitung flashen](http://github.com/john30/ebusd-esp) und eine serielle Konsole (zB: Arduino serieller Monitor) starten.
Innerhalb von 5 Sekunden muss nun eine Taste gedrückt werden (im Arduino einfach senden drücken) um in die Konfiguration zu gelangen.
Es ist eine statische IP einzustellen, mit Angabe von Netmask und Gateway.
Unter Punkt 6 wird gewählt ob Rx und Tx geswappt werden soll, hier im Beispiel wird direkt gewählt.
Nach Änderungen „speichern“ nicht vergessen.

```
ebusd device string: 10.0.0.161:8889
Press any key within 5 seconds to change the configuration...
Entering configuration mode.
Chip ID: 00538AA0
CPU frequency: 80
Free heap: 41816

Configuration:
 1. WIFI SSID: SSID des eigenen Wlan
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
 f. Factory reset
 r. Reboot (without saving)
 0. Start

Enter your choice:
```


## Default Konfig mit Wlan

Die in diesem Beispiel verwendete IP und Portnummer 8889 sollte in der /etc/default/ebusd dann so aussehen:

`EBUSD_OPTS="-d 10.0.0.161:8889 -l /var/log/ebusd.log --scanconfig --latency=20000 --address=01"`

Hier wurde auch mit „address“ eine neue Adresse vergeben, damit dieser Wlan Adapter nicht mit einem 2. Lan Adapter kollidiert.


## Warum ebusd-esp?

Ebusd-esp wurde von John für dieses Projekt programmiert und es ermöglicht sehr schnelle Latenzzeiten von 5 msec.
ESPEasy und ESP-Link haben zu große Latenzzeiten und können im derzeitigen Entwicklungsstand leider nicht eingesetzt werden.
Der Empfang würde funktionieren, aber eine Sendeanfrage an den eBUS funktioniert nicht.


## Logauszug Rawdata

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
