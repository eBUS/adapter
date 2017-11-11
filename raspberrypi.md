# Raspberry Möglichkeiten

Um die Platine mit dem [Raspberry Pi](https://www.raspberrypi.org/) zu nutzen, gibt es derzeit nur die Variante mit eigenem
UART, also ohne Verwendung des RPi GPIO UART (dessen Latenz ist für eBUS nicht zu gebrauchen).


## Platine im Raspberry Pi

Eine mögliche Variante die Platine in oder auf einem Raspberry zu platzieren soll dieses Foto zeigen:

[<img src="base-rpi.jpg" width="200" alt="rpi-base" title="Basisplatine auf Raspberry PI">](base-rpi.jpg)

Hier wurde ein kleines 3mm Loch zwischen den USB Buchsen gebohrt und ein USB-Kabel durchgeführt.
Als Kabel wurde ein altes USB Kabel mit C-Stecker verwendet und etwa 6 cm abgezwickt.
Hinten an der Platine des Raspberry ist das Kabel aufgelötet und somit ist nichts nach außen geführt und es gibt keine
störenden Kabel aus oder in das Gehäuse:

[<img src="base-rpi-bottom.jpg" width="200" alt="rpi-base-bottom" title="Rückseite">](base-rpi-bottom.jpg)

Selbst mit IC Sockel ist hier noch ausreichend Bauhöhe vorhanden:

[<img src="base-rpi-side.jpg" width="200" alt="rpi-base-side" title="Seitenansicht">](base-rpi-side.jpg)

Der UART kann auch direkt mit der Stiftleiste eingelötet werden, dann wird es noch flacher.
