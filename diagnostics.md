# Fehlersuche


## Fehlersuche mit Multimeter

Unten findet sich ein [Messplan](#Messplan), welcher alle wichtigen Messpunkte mit den notwendigen Spannungen angibt.

Es wird hier zwischen den Spannungen bei Rx (<span style="color:green">grün</span>) und Tx (<span style="color:red">rot</span>) unterschieden.

Die hier beschriebene Messung erfolgt nur auf der Basisplatine ohne aufgesteckte Erweiterungsplatine:

1. eBUS abklemmen.
2. UART abziehen.
3. Platine über den mitgelieferten 330 Ohm 5 Watt Widerstand an ein Netzteil anschließen.
4. Die Spannung an der Platine (eBus Klemme) sollte 17 Volt betragen.
5. Die Spannungen mit dem Messplan vergleichen.
6. Die Sende-LED 2 muss unbedingt eingesetzt sein.


[<img src="images/optocoupler.png" width="200" alt="optocoupler" title="OK2">](images/optocoupler.png)  
Um in den Sendemodus wechseln zu können, sind Pin 4-5 am Optokoppler 2 zu verbinden.
Dies kann mit einem kleinen Schraubendreher erfolgen indem man in dazwischen hält.

[<img src="images/base-measuring.jpg" width="200" alt="multimeter" title="Messung mit Multimeter">](images/base-measuring.jpg)

Sollte es wirklich einmal notwendig sein, einen Optokoppler auslöten zu müssen, dann kann man mit einem Elektronikseitenschneider
die Pins durchtrennen und anschließend mit einer Pinzette und Entlötsauglitze die Löcher säubern.


## Fehlersuche mit Oszilloskop

Wer über ein Oszilloskop verfügt kann sich die Fehlersuche vereinfachen, da hier vieles besser sichtbar ist.
Hier als Beispiel einige fehlerhaften Signale:

[<img src="images/base-measure-oszi-bad.png" width="200" alt="bad TX" title="schlechtes TX">](images/base-measure-oszi-bad.png)

Bei dieser Darstellung wurde mit dem 2.Kanal (unten) auf das Tx Signal des Uarts getriggert.
Der obere Kanal zeigt ein Signal, das von einem nicht geeigneten Optokoppler gemessen am OK2 Pin4 stammt.
Es wurde hier mit einem Optokoppler mit Darlingtonstufe experimentiert.
Wie deutlich zu sehen ist, steigt die Flanke in einer Kurvenform an und der eBus interpretiert dieses Signal falsch.
Im Idealfall sollte das obere Signal dem unteren gleichen, je steiler die Flanke, je besser.

[<img src="images/base-measure-wave.png" width="200" alt="bad RX" title="schlechtes RX">](images/base-measure-wave.png)

Ebenso wird hier ein Signal dargestellt (das stammt übrigens von einem Raspberry am RxD gemessen), welches ebenfalls zu keinem Erfolg führen würde.
Die maximal erlaubten 50 ysec wurden wegen der Kurvenform bei weitem überschritten.

[<img src="images/base-measure-oszi-bad2.jpg" width="200" alt="bad TX 2" title="schlechtes RX">](images/base-measure-oszi-bad2.jpg)

Hier ist ein Signal, das viel zu wenig auf Ground gezogen wird (GND ist der unterste erste Rasterblock).
Hier liegt ein Fehler in der GND Verbindung an der Platine, GND war mit GND2 verbunden.
Dieser Fehler kann auch mit einem Multimeter gefunden werden (siehe oben).


## Messplan

In diesem Messplan sind alle wichtigen Messpunkte mit den notwendigen Spannungen angegeben:

[<img src="images/base-measure-values.png" width="800" alt="measure plan" title="Messplan">](images/base-measure-values.png)

Bei den Spannungen wird farblich zwischen Rx (<span style="color:green">grün</span>) und Tx (<span style="color:red">rot</span>) unterschieden.
