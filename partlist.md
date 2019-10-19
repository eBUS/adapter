---
ref: partlist
lang: de
---
## Bauteilelisten

Die benötigten Bauteile für die Bestückung der beiden Platinen sind folgende:

| Anzahl | Bezeichnung                          | Wert/Typ | Basis | Erweiterung |
|-------:|--------------------------------------|---------:|:-----:|:-----------:|
|      1 | Basisplatine                         |          |     1 |           - |
|      1 | Erweiterungsplatine                  |          |     - |           1 |
|      1 | R1 Widerstand                        |      390 |     1 |           - |
|      2 | R2+R14 Widerstand                    |      220 |     2 |           - |
|      1 | R3 Widerstand                        |     100k |     1 |           - |
|      1 | R4 Widerstand                        |      22k |     1 |           - |
|      1 | R5+R6 Widerstand                     |      470 |     2 |           - |
|      2 | R7 Widerstand (+R2 auf Erweiterung)  |      18k |     1 |           1 |
|      1 | R8 Widerstand                        |      15k |     1 |           - |
|      1 | R9 Widerstand                        |     220k |     1 |           - |
|      1 | R11 Widerstand                       |      820 |     1 |           - |
|      1 | R12 Widerstand                       |      10k |     1 |           - |
|      1 | R13 Widerstand (+R1 auf Erweiterung) |     1,2k |     1 |           1 |
|      1 | R3 Widerstand (nur auf Erweiterung)  |     150k |     - |           1 |
|   n.B. | Drahtwiderstand 1W zum Testen        |      330 |  n.B. |           - |
|      4 | D1-4 Diode                           |   1N4007 |     4 |           - |
|      1 | D6 Zenerdiode 1,3W                   |     9,1V |     1 |           - |
|      2 | OK1-2 Optokoppler                    |  CNY17-4 |     2 |           - |
|      1 | IC1 Dual Komparator DIP-8            | TLC393CP |     1 |           - |
|      1 | IC2 Spannungsregler 5V 0,1A positiv  |    78L05 |     1 |           - |
|      1 | Q1 Transistor NPN                    | BC337-40 |     1 |           - |
|      1 | DC-DC Wandler RNM0512S/RO0509S       |          |     1 |           - |
|      1 | C1 Elektrolytkondensator             |     10uF |     1 |           - |
|      2 | C2 Keramikkondensator (+C1 auf Erw.) |    100nF |     1 |           1 |
|      2 | C3+4 Keramikkondensator              |    2,2uF |     2 |           - |
|      6 | LED1-3 LowCurrent (gelb, rot, grün)  |      2mA |     3 |           3 |
|      3 | LED Abstandshalter f. 3mm            |     10mm |     3 |           3 |
|      1 | Printklemme 2-fach Raster 5,08       |          |     1 |           - |
|    0-1 | UART nach Wahl (ELV, Mini, FTDI232)  |          |     1 |           - |
|      1 | 2-pol. Buchsenleiste                 |    2-pol |     - |           - |
|      1 | 3-pol. Buchsenleiste                 |    3-pol |     - |           - |
|      2 | 4-pol. Buchsenleiste                 |    4-pol |     - |           1 |
|      2 | 6-pol. Buchsenleiste                 |    6-pol |     1 |           1 |
|      1 | 13-pol. Buchsenleiste                |   13-pol |     - |           1 |
|      1 | 2-pol. Stiftleisten                  |    2-pol |     - |           - |
|      1 | 3-pol. Stiftleisten                  |    3-pol |     - |           - |
|      1 | 4-pol. Stiftleisten                  |    4-pol |     - |           - |
|      1 | 5-pol. Stiftleiste gewinkelt         |    5-pol |     - |           - |
|    0-1 | Stiftleiste gerade/gewinkelt         |   25-pol |     - |         0-1 |
|      4 | Distanzhülsen+Muttern                |          |     - |           - |
|      2 | Kurzhubtaster, h=4,3mm               |    6x6mm |     - |           2 |
|    0-1 | Reed-Kontakt                         |      2mm |     - |         0-1 |
|    0-1 | NTC-0,2 10K                          |      10K |     - |         0-1 |
|    1-2 | Wemos                                |  D1 Mini |     - |         1-2 |
|    0-1 | Dupont-Kabel 5-pol                   |          |     - |           - |
|    0-1 | USB Steckernetzteil 1-2A             |          |     - |         0-1 |

Hinweise:
* Für alle Widerstände sollte eine Metallfilm Variante 1/4 Watt verwendet werden.
* `n.B.` steht für `nach Bedarf`
* In der Spalte `Anzahl` ist die Menge für die Kombination aus Basisplatine und Erweiterungsplatine hinterlegt.
* In den Spalten `Basis` bzw. `Erweiterung` ist die Menge für die jeweilige Platine einzeln hinterlegt.
* Die Kondensator Werte können in den Warenkörben etwas abweichen (insbes. der 2,2uF). Das ist OK und liegt an entsprechender Verfügbarkeit.
