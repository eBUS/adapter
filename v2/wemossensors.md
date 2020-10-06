---
ref: wemossensors
lang: de
---
## Betrieb von zusätzlichen Sensoren auf der Erweiterungsplatine

Um zusätzliche Sensoren mit der Erweiterungsplatine nutzen zu können, muss derzeit noch ein eigener Wemos D1 verwendet werden,
der wegen zu hoher Latenzzeiten nicht gleichzeitig für eBUS genutzt werden kann.


### ESPEasy auf der Erweiterungsplatine

Wer weitere Sensoren auf der Erweiterungsplatine platzieren will ist mit [ESPEasy](https://www.letscontrolit.com/wiki/index.php/ESPEasy) bestens bedient.

Hier ein Beispiel eines Luftdrucksensors/Feuchte/Temperatur BME280 an der Platine aufgesteckt:

[<img src="images/exten-final-v21.jpg" width="200" alt="extension+wemos" title="Erweiterungsplatine mit Wemos und Sensor">](images/exten-final-v21.jpg)

In einem Gehäuse eingebaut sollte der Sensor allerdings dann mit Kabel herausgeführt werden, da die Luftfeuchtigkeit in dem Gehäuse kaum jemand interessiert.

### ESPEasy flashen

Wenn sich jemand die Firmware neu flasht sollte beim ersten einloggen das Passwort „configesp“ verwenden. 
Da es offensichtlich immer wieder bei einzelnen Usern zu Problemen mit der Konfiguration führt (Login geht nicht), hier eine kleine Anleitung wie ihr mit der seriellen Konsole den Wemos konfigurieren könnt.  
Ihr könnt aber vorher noch versuchen mit dem Handy eine Verbindung aufzubauen. Eventuell hilft es auch zu kontrollieren, ob ihr am Laptop bei der Verbindung unter Eigenschaften auf "privates Netzwerk" eingestellt habt.

Ich verwende für die serielle Verbindung die Arduino Umgebung ( ich habe 1.8.3 in Verwendung )  mit der eingebauten seriellen Konsole (Werkzeuge / Serieller Monitor), vorher aber bitte die COM Schnittstelle definieren. Baudrate in der seriellen Konsole ist 115200! Zuerst Wemos anschließen und Konsole starten und dann nochmals beim Wemos "Reset" drücken.

**Die Befehlsfolge lautet in Kurzform wie folgt:**

**Settings**        = listet die Konfiguration  
**WifiSSID Liwest** = setzt die SSID (eure SSID eingeben)  
**WifiKey geheim**  = setzt das Passwort (euer Passwort eingeben)  
**ip 10.0.0.167**   = setzt die IP Adresse wenn statisch gewünscht wird (eure IP eingeben)  
**WifiConnect**     = Verbindet sich mit diesen Einstellungen mit dem Wlan  
**save**            = sichert die Konfiguration  
**reboot**          = startet den Wemos neu  

[<img src="images/espeasy-serial.png" width="200" alt="Assembly" title="Bestückung">](images/espeasy-serial.png)

### ESPEasy mit BME280

Unter Controllers zunächst das FHEM Protokoll aktivieren:

[<img src="images/espeasy-config1.png" width="300" alt="ESPEasy config" title="ESPEasy Konfiguration">](images/espeasy-config1.png)

Die Konfiguration des Sensors in ESPEasy ist denkbar einfach:

[<img src="images/espeasy-config2.png" width="600" alt="ESPEasy config sensor" title="ESPEasy Konfiguration Sensor">](images/espeasy-config2.png)

Auf diese Art und Weise können verschiedene Sensoren an die Platine angeschlossen werden.


### BME280 in FHEM konfigurieren

[<img src="images/espeasy-fhem.png" width="400" alt="FHEM sensors" title="Sensoren in FHEM">](images/espeasy-fhem.png)

In Fhem muss die ESPbridge eingerichtet werden und der Rest wird dann von der Bridge automatisch erledigt:

```
############### ESPEasy ###############
define espBridge ESPEasy bridge 8383
attr espBridge room ESPEasy
```

Die Konfiguration des Sensors sieht dann so aus:

```
define ESPEasy_ESP_Easy_BME280 ESPEasy 10.0.0.166 80 espBridge ESP_Easy_BME280
attr ESPEasy_ESP_Easy_BME280 IODev espBridge
attr ESPEasy_ESP_Easy_BME280 Interval 600
attr ESPEasy_ESP_Easy_BME280 group ESPEasy Device
attr ESPEasy_ESP_Easy_BME280 presenceCheck 1
attr ESPEasy_ESP_Easy_BME280 readingSwitchText 1
attr ESPEasy_ESP_Easy_BME280 room ESPEasy
attr ESPEasy_ESP_Easy_BME280 setState 3
```

Die statische Vergabe der IP spielt für solche Zwecke eine wichtige Rolle.

Logfile:

```
define SVG_FileLog_ESPEasy_ESP_Easy_BME280 SVG myDbLog:SVG_FileLog_ESPEasy_ESP_Easy_BME280:HISTORY
attr SVG_FileLog_ESPEasy_ESP_Easy_BME280 room ESPEasy
```
