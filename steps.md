---
ref: steps
lang: de
navorder: 5
navtitle: Erste Schritte
---
## Erste Schritte zur Inbetriebnahme des Adapters
Abhängig von der Variante sind ein paar Schritte notwendig, um den Adapter in Betrieb zu nehmen:

1. [Adapter konfigurieren](#adaptercfg)
2. [ebusd installieren](#ebusdinst)
3. [ebusd konfigurieren](#ebusdcfg)
4. [eBUS anschließen](#ebusconn)
5. [eBUS Nachrichten empfangen/versenden](#messages)

### Adapter konfigurieren
{:id="adaptercfg"}
In jeder Variante sollten die Jumper J1 und J4 kontrolliert werden, auch wenn diese normalerweise schon von uns richtig
gesetzt wurden, siehe dazu die [Details der Variante](index#variants).

Ebenso sollte geprüft werden, ob die Jumper an J12 stimmen, insbesondere für die Varianten-Einstellung (Pins 4-6) und
den enhanced mode (Pins 1-2).

Danach kann man den Adapter entsprechend der Variante verbinden, also via USB-Anschluss J2 bei USB und Ethernet, über
den Wemos direkt bei der WIFI Variante oder aufgesteckt auf den Raspberry Pi.

Bei der WIFI Variante muss der Wemos auf das eigene WLAN konfiguriert werden und auf die Verwendung des Adapters.
Das geht am einfachsten über den vom Wemos eingerichteten WIFI Access Point mit SSID "EBUS", ohne Passwort und über die
IP Addresse "192.168.4.1", [Details dazu hier](wemosebus#ebusd-esp)

Bei der Ethernet Variante kann der Adapter auf eine feste IP-Adresse konfiguriert werden, [Details dazu hier](picfirmware#ethernetconfig).

### ebusd installieren
{:id="ebusdinst"}
Sofern die Kommunikation via ebusd ablaufen soll, muss dieser installiert werden.
Das geht entweder direkt vom [Debian Package aus dem letzten Release](https://github.com/john30/ebusd/releases/latest),
über das [Kompilieren der Sourcen](https://github.com/john30/ebusd/wiki/1.-Build-and-install)
oder als [Docker Container](https://hub.docker.com/repository/docker/john30/ebusd/).

### ebusd konfigurieren
{:id="ebusdcfg"}
Abhängig von der Variante muss der Device String gesetzt werden. Zudem ist bei einem frisch installierten ebusd im
Device String noch nicht der enhanced mode eingestellt.
Wie dieser Device String genau aussehen muss, findet sich wieder bei den [Details der Variante](index#variants).

Beim Docker Container wird dieser im Compose File oder im docker run als Argument an ebusd übergeben und bei einer
Installation unter `/etc/default/ebusd` eingetragen. Dort stehen standardmäßig folgende Einstellungen:

`EBUSD_OPTS="--scanconfig"`

Ohne weitere Angabe verwendet ebusd als Device `/dev/ttyUSB0`. Wegen des enhanced mode und auch wenn man den Adapter
nicht via USB angeschlossen hat, muss der Device String angegeben werden. Für USB sähe das dann so aus:

`EBUSD_OPTS="--scanconfig -d enh:/dev/ttyUSB0"`

Und für WIFI könnte das z.B. so aussehen:

`EBUSD_OPTS="--scanconfig -d 192.168.0.50:9999"`

Wird nun ebusd gestartet, sollten im Logfile (`/var/log/ebusd.log` bzw. Docker Logging) die folgenden Zeilen auftauchen
(Werte in spitzen Klammern hängen von der jeweiligen Konfiguration ab):
```
<DATE TIME> [main notice] ebusd <VERSION>> started with auto scan on enhanced device <DEVICE>
...
<DATE TIME> [bus notice] device status: resetting
...
<DATE TIME> [bus notice] device status: reset
```

### eBUS anschließen
{:id="ebusconn"}
Jetzt kann die eBUS Leitung mit der Buchse des Adapters verbunden werden. Auf die Polung braucht dabei keine Rücksicht
genommen zu werden.
Im Logging sollten jetzt auch Nachrichten von anderen Teilnehmern am Bus erscheinen, bspw. so:
```
<DATE TIME> [update notice] received unknown MS cmd: 1050b505072b000100000000 / 00
...
<DATE TIME> [update notice] received read ehp Status QQ=10: 18.94;1.540;2.340;off;00
```

### eBUS Nachrichten empfangen/versenden
{:id="messages"}
Bevor man sich um weitere [Integrationen](https://github.com/john30/ebusd/wiki/7.-Integrations) kümmert, sollte zuerst
die Kommunikation via `ebusctl` geprüft werden. Dazu am besten einfach mal schauen, welche Nachrichten durch den
automatischen Scanvorgang zur Verfügung stehen mit `ebusctl find`.  

Die ausgegebene Liste enthält die Nachrichten-ID (circuit gefolgt von name) und hinter dem Gleichheitszeichen die
zuletzt (innerhalb der letzten 5 Minuten) empfangenen Daten. Ist in der Liste bspw. folgender Eintrag zu finden:

`hwc Mode = no data stored`

dann lässt sich dieser mit dem Kommando `ebusctl read hwc Mode` aktiv auslesen, was bspw. folgendes Ergebnis liefert:

`hwc Mode = 53;auto;disabled;hwc;00;day`
