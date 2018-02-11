---
ref: extension
lang: de
---
## Erweiterungsplatine

Die Erweiterungsplatine ist so konzipiert, dass sie auf die Basisplatine aufgesteckt werden kann.
Sie ermöglicht die Nutzung verschiedener Sensoren wie Luftdruck, Feuchte und Temperatur, sowie Display, Gas-Zähler etc..
Ebenso ist ein Buzzer für eine Alarmausgabe vorgesehen.

Wer experimentieren möchte, kann die Erweiterungsplatine auch "Solo" (autark) benutzen, d.h. ohne eBUS Funktionen.
Damit können an den Wemos bequem Sensoren oder Displays angeschlossen werden.


### Allgemeiner Aufbau
[<img src="images/exten-top.jpg" width="200" alt="extension" title="Erweiterungsplatine">](images/exten-top.jpg)

Die Anzahl der Bauelemente ist wesentlich geringer als bei der Basisplatine und besteht vor allem aus Buchsen- und Steckleisten zur Aufnahme von Wemos, Displays oder Sensoren.

Die Spannungsversorgung erfolgt über ein USB Stecker Netzteil direkt am C-Stecker des Wemos.


### Schaltplan

[Hier ist der Schaltplan der Erweiterungsplatine.](images/exten-circuit.png)


### Bauteileliste

Die benötigten [Bauteile sind hier aufgeführt](partlist).


### Bestückung

Die Platine ist nicht bleifrei gefertigt (HASL), daher kann mit normalem bleihaltigem Lötzinn gelötet werden.
Der Gesetzgeber lässt dies im privaten und teilweise im kommerziellen Bereich noch zu.

Die Bestückung der Erweiterungsplatine erfolgt analog zur Basisplatine nach den üblichen Regeln, d.h. die niedrigen Bauteile zuerst, dann schichtweise die nächst höheren, also wie folgt:

1. SMD Taster
2. Widerstände
3. abgewinkelte Steckerleisten
4. Kondensatoren
5. Taster
7. Buchsenleisten
8. LEDs
9. Stiftleisten auf Unterseite zum Aufstecken auf die Basisplatine
10. Wemos D1 mit Stiftleisten aufstecken

Vor dem Einlöten der Widerstände sollte man unbedingt den Widerstandswert mit einem Ohm-Meter kontrollieren, um Verwechslungen zu vermeiden.

**Wichtige Hinweise:**
- Die beiden SMD Taster sind etwas kniffelig und es genügt, nur S3 zu bestücken, da S2 ja auch vom Taster S1 übernommen wird:
  - Den SMD Taster mit einem Finger platzieren und leicht andrücken (oder mit etwas anderem beschweren).
  - Dann mit der anderen Hand die bereits verzinnte Lötfläche erhitzen und verlöten.
  - Erst wenn der Taster hält, kann der zweite Pin regulär verlötet werden, und anschließend kann man in Ruhe den ersten Pin noch einmal frisch verlöten, sofern notwendig.
- Die LEDs sitzen auf Abstandshaltern, die gleichzeitig einen Knickschutz darstellen.  
  Bei Verwendung eines Gehäuses mit Klarsichtdeckel sollte dann auch der Abstand passen, so dass die LEDs etwas über dem Wemos hervorragen.
- Die UART Buchsenleisten kann man sich in der Regel komplett sparen, da meist ein Wemos eingesetzt wird.


#### Bestückungsvarianten

Die möglichen [Einsatzvarianten sind hier beschrieben](index.md#verwendung).

Je nach gewünschtem Einsatz kommen viele Bestückungsvarianten in Frage.
Insbesondere sollte man sich vor dem Löten überlegen, ob und wie bspw. BME280, OLED, Nextion, Gaszähler, NTC, Buzzer etc. angeschlossen werden.

Wenn bspw. ein BME280 zur Messung von Temperatur etc. außerhalb des Gehäuses genutzt werden soll, wird dafür ein 4-adriges Kabel benötigt, das über Stecker, Buchse oder auch direkt mit der Erweiterungsplatine verbunden wird.
Davon abhängig sind dann die entsprechenden Buchsen und/oder Stiftleisten zu wählen und zu verlöten.

Auch die [Jumper-Belegung hängt letztlich von der genutzten Variante ab](#funktionsmatrix-jumper).


#### Bestückung der LEDs

[Siehe Basisplatine.](base.md#bestückung-der-leds)


#### Verlöten der Erweiterungsplatine

[<img src="images/exten-1.jpg" width="200" alt="Assembly" title="Bestueckung der Platine vorne">](images/exten-1.jpg)  
Bestückte und verlötete Platine.

[<img src="images/exten-2.jpg" width="200" alt="Final" title="Bestueckung der Platine hinten">](images/exten-2.jpg)  
Ansicht von hinten mit Jumpern und Stiftleisten für die Verbindung zur Basisplatine.


#### Fertig aufgebaute Erweiterungsplatine

[<img src="images/exten-final.jpg" width="200" alt="Final" title="Fertig aufgebaute Erweiterungsplatine">](images/exten-final.jpg)  
So sieht die fertige Erweiterungsplatine aufgesteckt auf die Basisplatine aus inkl. Wemos mit ESPEasy und direkt aufgestecktem BME280.  
Links sieht man das Dupont-Kabel für die Verbindung zum Wemos mit ebusd-esp.


### Funktionsmatrix Jumper

[<img src="images/exten-jumper.jpg" width="200" alt="Jumper" title="Jumper">](images/exten-jumper.jpg)

Je nachdem für welchen Zweck die Erweiterungsplatine eingesetzt wird, ist hier auf die Jumper (Lötbrücken) auf der Rückseite zu achten da diese Sensoren sonst funktionslos bleiben.

**Achtung:** hier ist die Beschriftung bei SJ1+SJ2 falsch und verkehrt rum, also SDA+SCL ist oben!

Auf der Rückseite der Erweiterungsplatine befinden sich einige Jumper, die je nach gewünschten Sensoren bzw. eingesetzter
Wemos Firmware unterschiedliche Belegungen erlauben.

| Sensor  | SJ1 | SJ2 | SJ3 | SJ4 | SJ5 | SJ6 | SJ7 | SJ8 |
|---------|-----|-----|-----|-----|-----|-----|-----|-----|
|ebusd-esp|     |     |     |     | 1-2 | 1-2 | 2-3 | 1-2 |
| ESPEasy |     |     |     |     |     |     |offen|offen|
| BME280  | 1-2 | 1-2 |     |     |     |     |     |     |
| Buzzer  |     |     |     |     |     |     |     |     |
| OLED    | 1-2 | 1-2 |     |     |     |     |     |     |
| Nextion |     |     | 1-2 | 1-2 |     |     |     |     |

* `1-2` bedeutet den linken bzw. oberen Lötpunkt mit dem mittleren verbinden (also 1 mit 2 in obigem Bild).
* `2-3` bedeutet den mittleren Lötpunkt mit dem rechten bzw. unteren verbinden (also 2 mit 3 in obigem Bild).

**Beispiel 1:** soll ein Wemos mit ESPEasy und einem BME280 verwendet werden, dann müssen bei SJ1 und SJ2 jeweils 1 mit 2 verbunden werden und SJ7 und SJ8 bleiben offen.

**Beispiel 2:** soll ein Wemos mit ebusd-esp direkt auf der Erweiterungsplatine verwendet werden, dann müssen SJ5/6 und SJ7/8 entsprechend der Tabelle gesetzt werden.


### Jumper bei Vollausbau

[<img src="images/exten-full.jpg" width="200" alt="Assembly" title="Bestückung">](images/exten-full.jpg)

Hier abgebildet eine Variante mit Vollausbau, Basisplatine mit Wemos ebusd-esp und Erweiterungsplatine mit ESPEasy und einem Temp-Feuchte-Druck-Sensor BME280.

Bei dieser Variante sind für den Wemos mit ebusd-esp keine Jumper zu setzen, da dieser am Zusatz Konnektor JP8 angesteckt ist und dieser fix das RX + TX Signal zugeführt hat.

Der Wemos mit ESPEasy sitzt auf der Erweiterungsplatine und nutzt somit alle vorgesehenen Stiftleisten. Sitzt wie hier ESPEasy auf der Erweiterungsplatine und ist mit dem Sensor BME280 verbunden, müssen die Jumper SJ1 und SJ2 gesetzt werden.

| Sensor  | SJ1 | SJ2 | SJ3 | SJ4 | SJ5 | SJ6 | SJ7 | SJ8 |
|---------|-----|-----|-----|-----|-----|-----|-----|-----|
| BME280  | 1-2 | 1-2 |     |     |     |     |     |     |


**Wichtig:** Der Wemos mit ESPEasy darf nicht an RX und TX gejumpert werden, da ansonsten die Signale vom gleichzeitig angeschlossenen Wemos mit ebusd-esp verschliffen würden!  
Solche Fehler können unter Umständen augenblicklich nichts ausmachen und zeigen sich meist erst wenn mehrere Geräte am eBUS angeschlossen sind.
