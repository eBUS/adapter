---
ref: diagnostics
lang: de
---
## Fehlersuche


### Fehlersuche mit Multimeter

Der [Messplan der Basisplatine](base#messplan) enthält alle wichtigen Messpunkte mit den notwendigen Spannungen.

Die hier beschriebene Messung erfolgt nur auf der Basisplatine ohne aufgesteckte Erweiterungsplatine:

1. eBUS abklemmen.
2. UART abziehen.
3. TX LED2 einsetzen.
4. Platine über den mitgelieferten 330 Ohm 5 Watt Widerstand an ein Netzteil anschließen.
5. Die Spannung an eBUS Klemme sollte 17 Volt betragen.
6. Die Spannungen mit dem Messplan vergleichen.


[<img src="images/optocoupler.png" width="200" alt="optocoupler" title="OK2">](images/optocoupler.png)  
Um in den Sendemodus wechseln zu können, sind Pin 4-5 am Optokoppler 2 zu verbinden.
Dies kann mit einem kleinen Schraubendreher erfolgen indem man in dazwischen hält.

[<img src="images/base-measuring.jpg" width="200" alt="multimeter" title="Messung mit Multimeter">](images/base-measuring.jpg)

Sollte es wirklich einmal notwendig sein, einen Optokoppler auslöten zu müssen, dann kann man mit einem Elektronikseitenschneider
die Pins durchtrennen und anschließend mit einer Pinzette und Entlötsauglitze die Löcher säubern.


### Fehlersuche mit Oszilloskop

Wer über ein Oszilloskop verfügt kann sich die Fehlersuche vereinfachen, da hier vieles besser sichtbar ist.
Hier als Beispiel einige fehlerhaften Signale:

[<img src="images/base-measure-oszi-bad.jpg" width="200" alt="bad TX" title="schlechtes TX">](images/base-measure-oszi-bad.jpg)

Bei dieser Darstellung wurde mit dem 2.Kanal (unten) auf das TX Signal des UART getriggert.
Der obere Kanal zeigt ein Signal, das von einem nicht geeigneten Optokoppler gemessen am OK2 Pin 4 stammt.
Es wurde hier mit einem Optokoppler mit Darlingtonstufe experimentiert.
Wie deutlich zu sehen ist, steigt die Flanke in einer Kurvenform an und der eBUS interpretiert dieses Signal falsch.
Im Idealfall sollte das obere Signal dem unteren gleichen, je steiler die Flanke, je besser.

[<img src="images/base-measure-wave.png" width="200" alt="bad RX" title="schlechtes RX">](images/base-measure-wave.png)

Ebenso wird hier ein Signal dargestellt (das stammt übrigens von einem Raspberry am RX gemessen), welches ebenfalls zu keinem Erfolg führen würde.
Die maximal erlaubten 50 Mikrosekunden wurden wegen der Kurvenform bei weitem überschritten.

[<img src="images/base-measure-oszi-bad2.jpg" width="200" alt="bad TX 2" title="schlechtes RX">](images/base-measure-oszi-bad2.jpg)

Hier ist ein Signal, das viel zu wenig auf Ground gezogen wird (GND ist der unterste erste Rasterblock).
Hier liegt ein Fehler in der GND Verbindung an der Platine, GND war mit GND2 verbunden.
Dieser Fehler kann auch mit einem Multimeter gefunden werden (siehe oben).
