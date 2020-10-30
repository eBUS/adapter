---
ref: finalization
lang: de
---
## Fertigstellung und Test vor Auslieferung
Hier sind die Schritte zur Fertigstellung eines Adapter festgehalten samt der notwendigen Tests, um die korrekte
Funktion des Adapters festzustellen.

Ausgangslage ist ein vom Bestücker gelieferter Adapter mit allen aufgelöteten SMD Bauteilen, jedoch ohne
Stift-/Buchsenleisten, Jumper oder eBUS Buchse, und ohne Firmware im PIC.

[TODO Bild]

### Fertigstellung

#### Lötarbeiten
 * Kontrolle der SMD Lötstellen, insbesondere im Hinblick auf potenzielle Lötbrücken am PIC und CP2102
 * Kontrolle der Einbaurichtung der ICs
 * Einlöten der Stift-/Buchsenleisten, Jumper und eBUS Buchse, je nach Variante:
   * für alle Varianten:
     * Jumper J1, J4
     * Stiftleisten J11, J12
     * eBUS Buchse J13 bzw. J14
   * zusätzlich für Raspberry Pi:
     * Stiftleisten J3, J5, J7
     * verkürzte Stiftleiste J6 (nur Pins 1-4, bei Universaladapter jedoch volle 6 Pins!)
     * Raspberry Pi GPIO Buchsenleiste J8
   * zusätzlich für Wemos:
     * Stiftleisten J3, J5, J6, J7
     * Wemos Buchsenleiste J9
     * Stiftleiste am Wemos selbst
   * zusätzlich für USR-ES1:
     * USR-ES1 Buchsenleiste J10
     * Stiftleiste am USR-ES1 selbst
 * Kontrolle der neuen Lötstellen

[TODO Bilder]

#### Elektrische Prüfung
 * Jumper J1 auf USB
 * Jumper J4 auf USB
 * [TODO] vorweg noch Stromverbrauch über Masse an USB und +3,3 V an J4/Pin 2 messen?
 * USB-Buchse an eine Stromversorgung anschließen (USB Netzteil oder Host)
   * gelbe LED leuchtet permanent
   * grüne LED leuchtet permanent
   * rote LED leuchtet nicht
   * blaue LED leuchtet nicht
 * RX-Test:
   * Spannung an eBUS-Buchse mit 330 Ohm Widerstand anlegen (Labornetzteil) [TODO richtig so?]
   * eBUS Stromverbrauch geht auf auf maximal [TODO] mA
   * ab [TODO] 12 V geht grüne LED aus
 * TX-Test:
   * C1/TX am PIC auf High setzen [TODO Option in Bootloader oder Firmware einbauen "TX-Test"]
   * eBUS Stromverbrauch geht auf maximal [TODO] mA
   * Spannung an eBUS-Buchse geht runter auf maximal [TODO] V

#### Flashen der Firmware
 * PICKit mit J11 verbinden
 * mit MPLAB X IPE combined Firmware flashen inkl. CONFIG


### Test des Bootloaders
 * Jumper J1 auf USB
 * Jumper J4 auf USB
 * Jumper J11: Pins 3-4 verbinden (Bootloader Modus)
 * USB-Buchse an Host anschließen
   * USB serial wird vom Host erkannt und als COM Schnittstelle eingerichtet
   * gelbe LED leuchtet permanent
   * grüne LED leuchtet permanent
   * rote LED leuchtet nicht
   * blaue LED leuchtet sofort permanent (Bootloader Modus)
 * `picloader /dev/ttyUSB0` gibt ohne Fehlermeldung aus:
    ```text
    Device ID: 30b0 (PIC16F15356)
    Device revision: 0.1
    Bootloader version: 1
    Firmware version: 1
    MAC address: ae:80:53:**:**:**
    IP address: DHCP
    ```
 * Optional:
   * Flashen der Firmware mit `picloader -f offset.hex /dev/ttyUSB0`
 * Jumper an J11 entfernen
 * Adapter bootet nach Reset mit `picloader -r /dev/ttyUSB0`
   * blaue LED leuchtet wie dokumentiert: fade-up


### Test der Kommunikation
Diese Tests werden ohne Anschluss am eBUS durchgeführt.

#### USB Variante
 * Jumper J1 auf USB
 * Jumper J4 auf USB
 * Jumper J11+J12 leer
 * USB-Buchse an Host anschließen
 * weiter bei [Prüfen Kommunikation](#check) mit /dev/ttyUSB0 (o.ä.) als DEVICE

#### Raspberry Pi Variante
 * Jumper J1 auf RPI
 * Jumper J4 auf RPI
 * Jumper J11+J12 leer
 * weiter bei [Prüfen Kommunikation](#check) mit /dev/ttyAM0 als DEVICE

#### WLAN Variante
 * Jumper J1 auf RPI
 * Jumper J4 leer
 * Jumper J11+J12 leer
 * Wemos mit ebusd-esp Firmware aufstecken und konfigurieren (in Adapter 3 Modus)
 * Stromversorgung über USB-Buchse am Wemos
 * weiter bei [Prüfen Kommunikation](#check) mit ebusd IP/Port laut Wemos Konfigurationsseite als DEVICE

#### Ethernet Variante
 * Jumper J1 auf RPI
 * Jumper J4 auf USB
 * Jumper J11 leer
 * Jumper J12: Pins 5-6 verbinden (Ethernet Modus)
 * USR-ES1 aufstecken und mit LAN verbinden
 * weiter bei [Prüfen Kommunikation](#check) mit zugewiesener IP-Adresse und Port 8880 [TODO 9999] als DEVICE
 * optional fixe IP-Adresse konfigurieren:
   * Jumper J1 auf USB
   * Setzen der IP Konfiguration mit `picloader -i 192.168.1.10 -m 24 /dev/ttyUSB0`
   * Wiederholen ab [Prüfen Kommunikation](#check) mit eingestellter IP-Adresse und Port 8880 [TODO 9999] als DEVICE

#### Universal-Variante
 * alles von oben durchgehen

#### Prüfen Kommunikation
{:id="check"}
 * nach Stromversorgung:
   * gelbe LED leuchtet permanent
   * grüne LED leuchtet permanent
   * rote LED leuchtet nicht
   * blaue LED:
     * fade-up auf volle Helligkeit (enhanced Modus)  
     * zusätzlich bei Ethernet:
       * Blinken bis USR-ES1 antwortet, Link aufgebaut ist, IP-Adresse zugewiesen wurde 
 * ebusd starten mit `ebusd -f -s --lograwdata -a ff -d enh:DEVICE`
   * Verbindung zu Device steht (keine device errors)
   * no signal
 * eBUS anschließen:
   * grüne LED flackert regelmäßig ohne eBUS Aktivität bzw. unregelmäßig mit
   * ebusd meldet signal acquired
   * Messages trudeln ein
   * keine ungewöhnlichen Fehlermeldungen
 * Scan starten, bspw. mit `ebusctl scan 08`:
   * rote LED flackert beim Schreiben auf eBUS
   * Slave antwortet
   * keine Arbitrierungsfehler in ebusd


### Optionale Tests
#### non-enhanced Modus
Optional können sämtliche Test mit eBUS zusätzlich oder alternativ mit non-enhanced ebusd protocol durchgeführt werden.

#### Reset Jumper
Beim Verbinden von Pin 7-8 am Jumper J12 führt der PIC einen Reset durch.

In der WIFI Variante wird der Wemos dabei auch resettet, ebenso wie der USR-ES1 in der Ethernet Variante.


### Test von Sensoren/Display
In den Varianten Wemos und Raspberry Pi können Sensoren und/oder Displays an J3, J5, J6 und J7 angeschlossen werden.

#### Sensoren mit Wemos
Am einfachsten mit ESPEasy.
tbc

#### Sensoren mit Raspberry Pi
Am einfachsten mit gpio und/oder i2c Tool.
tbc
