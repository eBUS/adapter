---
ref: raspberrypi
lang: de
navorder: 3
navtitle: Raspberry Pi
---
## Raspberry Pi Variante
[<img src="img/smd-3drpi.png" width="200" alt="RPI" title="RPI">](img/smd-3drpi.jpg)  
In der Raspberry Pi Variante wird der Adapter über die GPIO Buchse direkt auf den [Raspberry Pi](https://www.raspberrypi.org/) aufgesteckt.

Dabei erfolgt die Stromversorgung des Adapters direkt vom Raspberry Pi und die Kommunikation geht über die folgenden
Pins am [Raspberry Pi GPIO](https://www.raspberrypi.org/documentation/usage/gpio/):
* TxD: Pin 8 (GPIO 14)
* RxD: Pin 10 (GPIO 15)

Die ebusd device Konfiguration lautet: `-d enh:/dev/ttyAMA0 --latency=50`


### Konfiguration mit Raspberry Pi OS
Standardmäßig werden die für den eBUS Adapter benötigten Pins vom Raspberry Pi OS für eine serielle Login Shell verwendet.

Um nun stattdessen den eBUS Adapter über diese Schnittstelle laufen lassen zu können, muss das OS umkonfiguriert werden.
Das geht je nach verwendeter Hardware Version des Raspberry Pi unterschiedlich, siehe unten.

Details dazu finden sich auch in der [Raspberry Pi OS Dokumentation](https://www.raspberrypi.org/documentation/configuration/uart.md).

#### Alle Raspberry Pi Versionen
Um die serielle Login Shell zu deaktivieren, den UART aber zu behalten, müssen folgende Schritte durchgeführt werden:
* `sudo raspi-config` ausführen
* "5 Interfacing Options" auswählen
* "P6 Serial" auswählen
* "Would you like a login shell to be accessible over serial?" mit "Nein"/"No" beantworten
* "Would you like the serial port hardware to be enabled?" mit "Ja"/"Yes" beantworten
* "raspi-config" beenden und **noch nicht** rebooten

#### Raspberry Pi Version 2
Nach Ausführen von `raspi-config` wie oben beschrieben, sollte in `/boot/config.txt` bereits die Einstellung
`enable_uart=1` enthalten sein.

Weitere Einstellungen sind nicht notwendig und nach einem Reboot ist der Raspberry Pi dann einsatzbereit.

Um zu verifizieren, dass die Einstellungen richtig sind, kann man `dmesg|grep ttyAMA0` ausführen, was eine Zeile wie
folgt ausgeben sollte, die zumindest `PL011` enthalten muss:
```
[    1.138083] 20201000.serial: ttyAMA0 at MMIO 0x20201000 (irq = 81, base_baud = 0) is a PL011 rev2
```

#### Raspberry Pi Version 3, 4 und Zero W
Um den PL011 UART auf den Pins für den Adapter zu legen, muss dieser aktiviert und vom Bluetooth Device abgekoppelt werden.

Nach Ausführen von `raspi-config` wie oben beschrieben, sollte in der Sektion `[all]` von `/boot/config.txt` bereits
die Einstellung `enable_uart=1` enthalten sein.

Für die Deaktivierung des Bluetooth Device muss dann noch `dtoverlay=disable-bt` in `/boot/config.txt` hinzugefügt werden.

Am Ende muss die Sektion `[all]` von `/boot/config.txt` also (mindestens) folgende Einstellungen enthalten:
```ini
[all]
enable_uart=1
dtoverlay=disable-bt
```

Durch die Deaktivierung des Bluetooth Device muss zusätzlich noch der HCI Service inaktiv gemacht werden mit:
```shell
sudo systemctl disable hciuart
```
Nach einem Reboot ist der Raspberry Pi dann einsatzbereit.

Um zu verifizieren, dass die Einstellungen richtig sind, kann man `dmesg|grep ttyAMA0` ausführen, was eine Zeile wie
folgt ausgeben sollte, die zumindest `PL011` enthalten muss:
```
[    1.166081] fe201000.serial: ttyAMA0 at MMIO 0xfe201000 (irq = 29, base_baud = 0) is a PL011 rev2
```

**Hinweis:** Falls Bluetooth benötigt wird, kann alternativ auch der mini UART an das Bluetooth Device zugewiesen werden.
Dazu muss in obigen Anpassungen einfach `miniuart-bt` anstelle von `disable-bt` verwendet werden.
Allerdings wird dadurch die Performance des Systems beeinträchtigt, da der GPU Takt auf 250Mhz reduziert wird.
