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

[Hier ist der Schaltplan der Erweiterungsplatine v2.1 (neueste Version)](images/exten-circuit-v21.png)  
[Hier ist der Schaltplan der Erweiterungsplatine v2.0](images/exten-circuit-v20.png)


### Bauteileliste

Die benötigten [Bauteile sind hier aufgeführt](partlist).


### Bestückung

Die Platine ist nicht bleifrei gefertigt (HASL), daher kann mit normalem bleihaltigem Lötzinn gelötet werden.
Der Gesetzgeber lässt dies im privaten und teilweise im kommerziellen Bereich noch zu.

Die Bestückung der Erweiterungsplatine erfolgt analog zur Basisplatine nach den üblichen Regeln, d.h. die niedrigen Bauteile zuerst, dann schichtweise die nächst höheren, also wie folgt:

1. SMD Taster (nur für v2.0)
2. Widerstände
3. abgewinkelte Steckerleisten
4. Kondensatoren
5. Taster
7. Buchsenleisten
8. LEDs
9. Stiftleisten auf Unterseite zum Aufstecken auf die Basisplatine
10. Wemos D1 mit Stiftleisten aufstecken

Vor dem Einlöten der Widerstände sollte man unbedingt den Widerstandswert mit einem Ohm-Meter kontrollieren, um Verwechslungen zu vermeiden.

**Wichtige Hinweise zu Version 2.0**
- Die beiden SMD Taster sind etwas kniffelig und es genügt, nur S3 zu bestücken, da S2 ja auch vom Taster S1 übernommen wird:
  - Den SMD Taster mit einem Finger platzieren und leicht andrücken (oder mit etwas anderem beschweren).
  - Dann mit der anderen Hand die bereits verzinnte Lötfläche erhitzen und verlöten.
  - Erst wenn der Taster hält, kann der zweite Pin regulär verlötet werden, und anschließend kann man in Ruhe den ersten Pin noch einmal frisch verlöten, sofern notwendig.

**Wichtige Hinweise zu allen Versionen**
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
Bestückte und verlötete Platine (v2.0).

[<img src="images/exten-2.jpg" width="200" alt="Final" title="Bestueckung der Platine hinten">](images/exten-2.jpg)  
Ansicht von hinten (v2.0) mit Jumpern und Stiftleisten für die Verbindung zur Basisplatine.


#### Fertig aufgebaute Erweiterungsplatine

[<img src="images/exten-final-v21.jpg" width="200" alt="Final" title="Fertig aufgebaute Erweiterungsplatine">](images/exten-final-v21.jpg)  
So sieht die fertige Erweiterungsplatine (v2.1) aufgesteckt auf die Basisplatine aus inkl. Wemos mit ESPEasy und direkt aufgestecktem BME280.  
Links sieht man das Dupont-Kabel für die Verbindung zum Wemos mit ebusd-esp.


### Funktionsmatrix Jumper

[<img src="images/exten-jumper-v21.png" width="200" alt="Jumper" title="Jumper v2.1">](images/exten-jumper-v21.png) [<img src="images/exten-jumper-v20.jpg" width="200" alt="Jumper" title="Jumper v2.0">](images/exten-jumper-v20.jpg)

Auf der Rückseite der Erweiterungsplatine befinden sich einige Jumper (Lötbrücken), die je nach gewünschten Sensoren bzw. eingesetzter Wemos Firmware unterschiedliche Belegungen erlauben.

Je nachdem für welchen Zweck die Erweiterungsplatine eingesetzt wird, ist hier auf die Jumper zu achten, da diese Sensoren sonst funktionslos bleiben.

**Wichtig:** Der Wemos mit ESPEasy darf nicht an RX und TX gejumpert werden, da ansonsten die Signale vom gleichzeitig angeschlossenen Wemos mit ebusd-esp verschliffen würden!  
Solche Fehler können unter Umständen augenblicklich nichts ausmachen und zeigen sich meist erst wenn mehrere Geräte am eBUS angeschlossen sind.


#### Jumper für Version 2.1

**Achtung:** hier sind SJ8+SJ4 vom Layout her nicht richtig, denn der eigentlich mittlere Pin ist fälschlicherweise ganz rechts!

| Sensor  | Wemos PINs |  SJ8  |  SJ4  | SJ7 | SJ3 | SJ6 | SJ2 |   SJ5   |   SJ1   | Bemerkung                                                     |
|---------|------------|-------|-------|-----|-----|-----|-----|---------|---------|---------------------------------------------------------------|
|ebusd-esp| RX/TX      |       |       |     |     | 2-3 | 2-3 |   2-3   |   2-3   | nur für direkte Verwendung von ebusd-esp auf der Erweiterung! |
| ESPEasy |            |       |       |     |     |     |     |**offen**|**offen**|                                                               |
| BME280  | D1/D2      |  3-1  |  3-1  |     |     |     |     |         |         |                                                               |
| BME280  | **D3/D4**  |**2-1**|**2-1**|     |     |     |     |         |         |                                                               |
| Buzzer  | **D0**     |       |       |     |     |     |     |         |         |                                                               |
| OLED    | D1/D2      |  3-1  |  3-1  |     |     |     |     |         |         |                                                               |
| OLED    | **D3/D4**  |**2-1**|**2-1**|     |     |     |     |         |         |                                                               |
| Nextion | RX/TX      |       |       | 2-1 | 2-1 |     |     |         |         |                                                               |

* Die empfohlenen Einstellungen sind **fett**.
* `1-2` und `3-2` bedeuten: verbinde den linken Lötpunkt mit dem mittleren (Achtung unterschiedliche Reihenfolgen z.B. SJ7 im Vergleich zu SJ6).
* `2-3` und `2-1` bedeuten: verbinde den mittleren Lötpunkt mit dem rechten (Achtung unterschiedliche Reihenfolgen z.B. SJ7 im Vergleich zu SJ6).
* `3-1` bedeutet: verbinde den linken Lötpunkt mit dem rechten (nur bei SJ8+SJ4).

**Beispiel 1:** soll ein Wemos mit ESPEasy und einem BME280 verwendet werden, dann müssen bei SJ8 und SJ4 jeweils 3 mit 1 verbunden werden und SJ5 und SJ1 bleiben offen.

**Beispiel 2:** soll ein Wemos mit ebusd-esp direkt auf der Erweiterungsplatine verwendet werden, dann müssen SJ6/SJ2 und SJ5/SJ1 entsprechend der Tabelle gesetzt werden.


#### Jumper für Version 2.0

**Achtung:** hier ist die Beschriftung bei SJ2+SJ1 verkehrt rum, also SDA+SCL ist oben!

| Sensor  | Wemos PINs |  SJ2  |  SJ1  | SJ3 | SJ4 | SJ5 | SJ6 |   SJ8   |   SJ7   | Bemerkung                                                     |
|---------|------------|-------|-------|-----|-----|-----|-----|---------|---------|---------------------------------------------------------------|
|ebusd-esp| RX/TX      |       |       |     |     | 1-2 | 1-2 |   1-2   |   2-3   | nur für direkte Verwendung von ebusd-esp auf der Erweiterung! |
| ESPEasy |            |       |       |     |     |     |     |**offen**|**offen**|                                                               |
| BME280  | **D1/D2**  |**1-2**|**1-2**|     |     |     |     |         |         |                                                               |
| Buzzer  | **D0**     |       |       |     |     |     |     |         |         |                                                               |
| OLED    | **D1/D2**  |**1-2**|**1-2**|     |     |     |     |         |         |                                                               |
| Nextion | RX/TX      |       |       | 1-2 | 1-2 |     |     |         |         |                                                               |

* Die empfohlenen Einstellungen sind **fett**.
* `1-2` bedeutet: verbinde den linken bzw. oberen Lötpunkt mit dem mittleren (also 1 mit 2 in obigem Bild).
* `2-3` bedeutet: verbinde den mittleren Lötpunkt mit dem rechten bzw. unteren (also 2 mit 3 in obigem Bild).

**Beispiel 1:** soll ein Wemos mit ESPEasy und einem BME280 verwendet werden, dann müssen bei SJ2 und SJ1 jeweils 1 mit 2 verbunden werden und SJ8 und SJ7 bleiben offen.

**Beispiel 2:** soll ein Wemos mit ebusd-esp direkt auf der Erweiterungsplatine verwendet werden, dann müssen SJ5/6 und SJ8/7 entsprechend der Tabelle gesetzt werden.


### Jumper bei Vollausbau

[<img src="images/exten-full.jpg" width="200" alt="Assembly" title="Bestückung">](images/exten-full.jpg)

Hier abgebildet ist eine Variante mit Vollausbau, Basisplatine mit Wemos ebusd-esp und Erweiterungsplatine mit ESPEasy und einem Temp-Feuchte-Druck-Sensor BME280.

Bei dieser Variante sind für den Wemos mit ebusd-esp keine Jumper zu setzen, da dieser am Konnektor JP8 angesteckt ist und hier fix das RX + TX Signal zugeführt wird.

Der Wemos mit ESPEasy sitzt auf der Erweiterungsplatine und nutzt somit alle vorgesehenen Stiftleisten.
Sitzt wie hier ESPEasy auf der Erweiterungsplatine und ist mit dem Sensor BME280 verbunden, müssen die Jumper SJ8/SJ4 (v2.1) bzw. SJ2/SJ1 (v2.0) gesetzt werden.

#### Jumper Vollausbau Version 2.1

| Sensor  | Wemos PINs |  SJ8  |  SJ4  | SJ7 | SJ3 | SJ6 | SJ2 | SJ5 | SJ1 |
|---------|------------|-------|-------|-----|-----|-----|-----|-----|-----|
| BME280  | **D3/D4**  |**2-1**|**2-1**|     |     |     |     |     |     |
| OLED    | **D3/D4**  |**2-1**|**2-1**|     |     |     |     |     |     |

**Wichtig:** Für diese Jumper bei v2.1 sind beim ESPEasy die I2C Einstellungen auf D3/D4 umzustellen.

#### Jumper Vollausbau für Version 2.0

| Sensor  | Wemos PINs |  SJ2  |  SJ1  | SJ3 | SJ4 | SJ5 | SJ6 | SJ8 | SJ7 |
|---------|------------|-------|-------|-----|-----|-----|-----|-----|-----|
| BME280  | **D1/D2**  |**1-2**|**1-2**|     |     |     |     |     |     |
| OLED    | **D1/D2**  |**1-2**|**1-2**|     |     |     |     |     |     |

Für diese Jumper bei v2.0 können beim ESPEasy die I2C Einstellungen auf den Standardwerten bleiben (D1/D2).
