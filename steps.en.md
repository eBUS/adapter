---
ref: steps
lang: en
navorder: 5
navtitle: First Steps
---
## First steps to start using the adapter
Depending on the variant, a few steps are necessary to put the adapter into operation:

1. [Configure adapter](#adaptercfg)
2. [Install ebusd](#ebusdinst)
3. [Configure ebusd](#ebusdcfg)
4. [Connect eBUS](#ebusconn)
5. [Receive / send eBUS messages](#messages)

### Configure adapter
{:id="adaptercfg"}
In each variant, the jumpers J1 and J4 should be checked, even if they have usually already been set correctly by us,
see the [details of the variant](index.en#variants).

You should also check whether the jumpers at J12 are correct, especially for the variant setting (pins 4-6) and enhanced
mode (pins 1-2).

Then you can connect the adapter according to the variant, i.e. via USB connection J2 with USB and Ethernet, via the
Wemos directly with the WIFI variant or plugged onto the Raspberry Pi.

With the WIFI variant, the Wemos needs to be configured to your own WLAN and to using the adapter.
The easiest way to do this is via the WIFI access point set up by Wemos with SSID "EBUS", without a password and via the
IP address "192.168.4.1", [see details here](wemosebus#ebusd-esp).

With the Ethernet variant, the adapter can be configured to a fixed IP address,
[see details here](picfirmware.en#ethernetconfig).

### Install ebusd
{:id="ebusdinst"}
If communication is to be taken over by ebusd, it has to be installed.
This can be done either directly from the [Debian package of the last release](https://github.com/john30/ebusd/releases/latest),
by [compiling the sources](https://github.com/john30/ebusd/wiki/1.-Build-and-install),
or as [Docker container](https://hub.docker.com/repository/docker/john30/ebusd/).

### Configure ebusd
{:id="ebusdcfg"}
Depending on the variant, the device string must be set. In addition, the enhanced mode is not yet set in the device
string of a freshly installed ebusd.
How this device string must look exactly can be found in the [details of the variant](index.en#variants).

With the Docker container, this is passed as an argument to ebusd in the compose file or in the docker run line and
in case of an installation entered under `/etc/default/ebusd`. The following settings are available there by default:

`EBUSD_OPTS="--scanconfig"`

Without further information, ebusd uses `/dev/ttyUSB0` as the device. Because of the enhanced mode and if the adapter
is not connected via USB, the device string must be set. For USB it would look like this:

`EBUSD_OPTS="--scanconfig -d enh:/dev/ttyUSB0"`

And for WIFI it could e.g. look like this:

`EBUSD_OPTS="--scanconfig -d 192.168.0.50:9999"`

If ebusd is started now, the following lines should appear in the log file (`/var/log/ebusd.log` or Docker Logging,
values in brackets depend on the respective configuration):
```
<DATE TIME> [main notice] ebusd <VERSION>> started with auto scan on enhanced device <DEVICE>
...
<DATE TIME> [bus notice] device status: resetting
...
<DATE TIME> [bus notice] device status: reset
```

### Connect eBUS
{:id="ebusconn"}
Now the eBUS line can be connected to the adapter. The polarity does not need to be taken into account.
Messages from other participants on the bus should then appear in the logging, for example as follows:
```
<DATE TIME> [update notice] received unknown MS cmd: 1050b505072b000100000000 / 00
...
<DATE TIME> [update notice] received read ehp Status QQ=10: 18.94;1.540;2.340;off;00
```

### Receive / send eBUS messages
{:id="messages"}
Before you take care of further [integrations](https://github.com/john30/ebusd/wiki/7.-Integrations), the communication
via `ebusctl` should be checked first. The best way to do this is to see which messages are available through the
automatic scan process with `ebusctl find`.

The output list contains the message ID (circuit followed by name) and, after an equal sign, the data last received
(within the last 5 minutes). For example, if the following entry is part of the list:

`hwc Mode = no data stored`

then this can be actively read out with the command `ebusctl read hwc Mode`, which might yield in the following result:

`hwc Mode = 53;auto;disabled;hwc;00;day`
