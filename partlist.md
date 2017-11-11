# Bauteilelisten für die Basis- und Erweiterungsplatine

Die benötigten Bauteile für die Bestückung der beiden Platinen sind folgende:

| Anzahl | Bezeichnung                          | Wert/Typ | Basis | Erweiterung |
|-------:|--------------------------------------|---------:|:-----:|:-----------:|
|      1 | R1 Widerstand                        |      820 |     1 |           - |
|      1 | R2 Widerstand                        |      220 |     1 |           - |
|      1 | R3 Widerstand                        |     100k |     1 |           - |
|      1 | R4 Widerstand                        |      39k |     1 |           - |
|      1 | R5 Widerstand                        |      470 |     1 |           - |
|      1 | R6 Widerstand                        |       1k |     1 |           - |
|      1 | R7 Widerstand                        |      18k |     1 |           1 | ??
|      1 | R8 Widerstand                        |      15k |     1 |           - |
|      1 | R9 Widerstand                        |     220k |     1 |           - |
|      1 | R10 Widerstand                       |      100 |     1 |           - |
|      1 | R11 Widerstand                       |      2k2 |     1 |           - |
|      1 | R12 Widerstand                       |      10k |     1 |           - |
|      1 | R13 Widerstand                       |      1k2 |     - |           1 | !!
|      1 | R14 Widerstand                       |      220 |     - |           - |
|   n.B. | Drahtwiderstand 5 W zum Testen       |      330 |     X |           - |
|      5 | D1-5 Diode                           |   1N4007 |     5 |           - |
|      1 | D6 Zenerdiode 1,3W                   |     9,1V |     1 |           - |
|      2 | OK1-2 Optokoppler                    |  CNY17-4 |     2 |           - |
|      1 | IC1 Dual Komparator DIP-8            | TLC393CP |     1 |           - |
|      1 | IC2 Spannungsregler 8V 0,1A positiv  |    78L08 |     1 |           - |
|      1 | Q1 Transistor NPN                    | BC337-40 |     1 |           - |
|      2 | C1-2 Elektrolytkondensator           |     10uF |     2 |           - |
|      1 | C4 Keramik Kondensator               |    100nF |     1 |           1 |
|      3 | LED1-3 LowCurrent (gelb, rot, grün)  |      2mA |     3 |           3 |
|      3 | LED Abstandshalter f. 3mm            |     15mm |     3 |           3 |
|      1 | Printklemme 2-fach Raster 5,08       |          |     1 |           - |
|    0-1 | UART nach Wahl (ELV, Mini, FTDI232)  |          |     1 |           - |
|   n.B. | div. Buchsenleiste                   |          |     X |           X |
|   n.B. | div. Stiftleisten                    |          |     X |           X |
|   n.B. | div. Distanzhülsen+Muttern           |          |     X |           - |
|      1 | Kurzhubtaster, h=4,3mm               |    6x6mm |     - |           1 |
|      2 | SMD-Kurzhubtaster, h=1,9mm           |4,2x2,8mm |     - |           2 |
|   n.B. | Reed-Kontakt                         |      2mm |     - |         0-1 |
|    1-2 | Wemos                                |  D1 Mini |     - |           X |
|    0-1 | USB Steckernetzteil 1-2A             |          |     - |           X |

Hinweise:
* Für alle Widerstände sollte eine Metallfilm Variante verwendet werden.
* `n.B.` steht für `nach Bedarf`
* In der Spalte `Anzahl` ist die Menge für die Kombination aus Basisplatine und Erweiterungsplatine hinterlegt.
* In den Spalten `Basis` bzw. `Erweiterung` ist die Menge für die jeweilige Platine einzeln hinterlegt.
