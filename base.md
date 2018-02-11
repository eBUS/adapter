---
ref: base
lang: de
---
## Basisplatine

Die Basisplatine dient als Grundlage und stellt den eigentlichen Konverter dar.
Alleine mit ihr ist die Kommunikation via UART z.B. an einen Raspberry Pi oder mittels WLAN direkt an den eBUS Dämon (ebusd) möglich.


### Allgemeiner Aufbau

[<img src="images/base-top.jpg" width="200" alt="base" title="Basisplatine">](images/base-top.jpg)

Die Basisplatine beinhaltet den Komparator zur Detektierung der RX Signale, die Optokoppler zur galvanischen Trennung und den Sendeteil an den eBUS.

Sie kann drei verschiedene UARTs zur seriellen Kommunikation aufnehmen oder über eine Stiftleiste mit einem Wemos D1 verbunden werden.

Die Stromversorgung der Platine erfolgt direkt aus dem Netz des eBUS.
Die Gesamtstromaufnahme sollte laut eBUS Spezifikation jedoch nicht mehr als 18 mA betragen.
Der Adapter hat eine galvanische Trennung zwischen eBUS und dem Kommunikationsteil zum eBUS Dämon.

Die Stromversorgung des UART oder des Wemos erfolgt über ein handelsübliches USB-Stecker Netzteil mit ausreichender Leistung (1A-2A).


### Schaltplan

[Hier ist der Schaltplan der Basisplatine.](images/base-circuit.png)


### Bauteileliste

Die benötigten [Bauteile sind hier aufgeführt](partlist).


### Bestückung

Die Platine ist nicht bleifrei gefertigt (HASL), daher kann mit normalem bleihaltigem Lötzinn gelötet werden.
Der Gesetzgeber lässt dies im privaten und teilweise im kommerziellen Bereich noch zu.

Die Bestückung der Basisplatine erfolgt nach den üblichen Regeln, d.h. die niedrigen Bauteile zuerst, dann schichtweise die nächst höheren, also wie folgt:

1. Widerstände
2. Dioden
3. abgewinkelte Steckerleisten
4. ICs
5. Kondensatoren
6. Buchsenleisten
7. Transistoren
8. Printklemme
9. LEDs (nur ohne Erweiterungsplatine)
10. UART mit Stiftleiste aufstecken oder [Wemos mit Dupontkabel an JP8 anstecken](#wemos-d1)

Vor dem Einlöten der Widerstände sollte man unbedingt den Widerstandswert mit einem Ohm-Meter kontrollieren, um Verwechslungen zu vermeiden.

**Wichtige Hinweise:**
- Der Kondensator C1 entfällt!
- Der Widerstand R10 wird mit einer Drahtbrücke oder 1 Ohm Widerstand bestückt.
- Die Position der Zenerdiode und der 1N4007 nicht verwechseln.
- Bei den ICs (OK1, OK2, IC1) unbedingt darauf achten, dass diese richtig herum eingesetzt werden.  
  Es befindet sich links unten jeweils ein Punkt, das ist Pin 1 und ist so zu platzieren, dass dieser beim Aufdruck der Aussparung ist.


#### Bestückung der LEDs

[<img src="images/led.jpg" width="200" alt="Final" title="LED Polung">](images/led.jpg)  
Bei der Bestückung der Leds ist auf die Polung zu achten.

Hält man die LED gegen das Licht, so ist die Kathode "-" als Querbalken erkennbar bzw. als deutlich größere Fläche.
Außerdem ist die Anode "+" an den Anschlußdrähten immer ein paar Millimeter länger.

**Achtung:** LED3 ist im Vergleich zu den anderen LEDs um 180° gedreht einzusetzen (Pluspol/Minuspol vertauscht).

**Wichtig:** Die 3 LEDs sind mit folgenden Fraben zu bestücken (nicht anders, da jede Farbe eine andere Spannung hat!)
* LED1: grün, RX
* LED2: rot, TX
* LED3: gelb, eBUS PWR


#### Verlöten der Basisplatine

[<img src="images/base-1.jpg" width="200" alt="Assembly" title="Bestueckung der Platine vorne">](images/base-1.jpg)  
**Achtung:** Der Elektrolytkondensator C1 entfällt bei dieser Variante. Dieser war bei der 8V Variante vorgesehen, die jedoch wegen anderer Nachteile nicht zur Auslieferung gekommen ist.

[<img src="images/base-2.jpg" width="200" alt="Assembly" title="Bestueckung der Platine hinten gesteckt">](images/base-2.jpg)  
Ansicht von hinten, die Drahtenden werden leicht aufgebogen damit die Bauteile beim Umdrehen nicht herausfallen.
So kann dann alles gemeinsam bequem verlötet werden.

[<img src="images/base-3.jpg" width="200" alt="Assembly" title="Bestueckung der Platine hinten verloetet">](images/base-3.jpg)  
Fertig verlötete Platine von hinten, die Drähte werden nach der Verlötung alle mit einem Elektronik Seitenschneider abgelängt.


#### Fertig aufgebaute Basisplatine

[<img src="images/base-final.jpg" width="200" alt="Final" title="Fertig aufgebaute Basisplatine">](images/base-final.jpg) 

Die Buchsenleisten für nicht vorhandene UARTs kann man sich in der Regel sparen und müssen nicht bestückt werden.


### Messplan

Um die fertig gelötete Platine hinsichtlich der korrekten Funktion zu prüfen, sollte der Messplan durchlaufen werden.

Dabei wird der eBUS mit dem mitgelieferten 330 Ohm Widerstand an ein Netzteil angeschlossen (nicht an die Heizung!) und weder Erweiterungsplatine noch UART mit der Platine verbunden.

Hier sind die wichtigsten Messpunkte auf der Basisplatine von oben gesehen abgebildet:

[<img src="images/base-measure.jpg" width="300" alt="measure" title="Messpunkte">](images/base-measure.jpg)  
Bei den Spannungen wird farblich zwischen RX (<span style="color:green">grün</span>) und TX (<span style="color:red">rot</span>) unterschieden.  
Der erste Durchlauf ist für den Empfang (RX) und der zweite für das Senden (TX) mit Brücke am OK2.

Im [Schaltplan mit Messwerten](images/base-measure-values.png) sind auch noch einmal die Spannungen für diese Punkte angegeben.


### UARTs

[<img src="images/uarts.jpg" width="200" alt="UARTs" title="UARTs">](images/uarts.jpg)

Diese 3 UARTs wurden mit der Basisplatine getestet.

Der ELV UART ist der einzige der keine eigenen LEDs auf der Platine hat, die beiden anderen sind jedoch damit bestückt und dienen der zusätzlichen Kontrolle.

Beim FTDI UART ist darauf zu achten, dass hier eine gerade Stiftleiste eingesetzt werden muss, da die gewinkelte (wie im Bild oben) nicht aufsteckbar wäre.


### Wemos D1

Über eine einfache Verdrahtung kann der Wemos D1 auch direkt mit der Basisplatine betrieben werden, im Gegensatz [zum Einsatz mit Erweiterungsplatine](extension.md#bestückung).

[<img src="images/wemos-wiring.jpg" width="200" alt="Wemos D1 wiring" title="Wemos D1 Verdrahtung">](images/wemos-wiring.jpg)  
**Wemos D1 Verdrahtung**

Soll auf der Basisplatine ein Wemos zum Einsatz kommen, muss dieser erst vorbereitet werden und es müssen an RX, TX, VCC
und GND Kabel aufgelötet werden.
Auf eine Stiftleiste kann in diesem Fall verzichtet werden.

[<img src="images/wemos-wired.jpg" width="200" alt="Wemos D1 wired" title="Wemos D1 an Basisplatine">](images/wemos-wired.jpg)  
**Wemos D1 an Basisplatine**

Der fertig aufgelötete Wemos wird dann direkt an JP8 angesteckt.
In diesem Fall empfiehlt sich daher an JP8 eine gewinkelte Stiftleiste zu verwenden.
Der Wemos soll dann so in einem Gehäuse platziert werden, dass er keine metallischen Teile in der Nähe hat.

Die Spannungsversorgung des Wemos erfolgt direkt über ein USB-Steckernetzteil.
