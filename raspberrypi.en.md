---
ref: raspberrypi
lang: en
navorder: 3
navtitle: Raspberry Pi
---
## Raspberry Pi Variant
[<img src="img/smd-3drpi.png" width="200" alt="RPI" title="RPI">](img/smd-3drpi.jpg)  
In the Raspberry Pi variant, the adapter is plugged directly onto the [Raspberry Pi](https://www.raspberrypi.org/) via the GPIO socket.

Attention! Depending on the adapter version, the socket can also be shorter, but
it must always align to pin 1 at the "lower end" of the PCB.

The adapter is supplied with power directly from the Raspberry Pi and communication takes place via the following pins
on the [Raspberry Pi GPIO](https://www.raspberrypi.org/documentation/usage/gpio/):
* TxD: Pin 8 (GPIO 14)
* RxD: Pin 10 (GPIO 15)

These pins are assigned to the physical device "serial0".
The physical device is usually assigned to the logical device ttyAMA0, but depending on the Rasperry Pi version and the Raspbian / Rasperry Pi OS version different assignments may take place. The correct logical device can be found by
executing:
* `ls -l /dev` and then search for the mapping of serial0.

This is usually either ttyAMA0 or ttyS0. In the following examples, **ttyAMA0** is always used, please replace with **ttyS0** if necessary.

The ebusd device configuration is therefore:

 `-d enh:/dev/ttyAMA0 --latency=50`

If there are no other reasons not to do so, it is strongly recommended to use the enhanced protocol ("enh:"), as this is the only way to ensure compliance with the correct timings on the eBUS. Nevertheless, "latency=50" should be specified so that the ebusd will not run into a timeout caused by some UART drivers.

### Configuration with Raspberry Pi OS
By default, the pins required for the eBUS adapter are used by the Raspberry Pi OS for a serial login shell.

In order to be able to run the eBUS adapter via this interface instead, this login shell must be disabled.

Attention! If this shell is not completely disabled, it may send uncontrolled data to the eBUS, which may lead to a disruption of the entire bus system.

To disable the serial login shell, the following steps must be carried out
(note that the text below may slightly vary depending on the Raspberry Pi OS):
* run `sudo raspi-config`
* select `Interface Options`
* select `Serial Port`
* answer `Would you like a login shell to be accessible over serial?` with "No"
* answer `Would you like the serial port hardware to be enabled?` with "Yes"
* exit "raspi-config" and reboot

#### Special UART settings
In some Raspberry Pi versions and/or Raspbian/Raspberry Pi OS versions, no UART is active by default.
Therefore it should be checked whether in `/boot/config.txt` the entry

`enable_uart=1`

is included. Add if necessary and then reboot.

Attention! If the ttyebus driver is still present, it must be completely uninstalled. This driver is not suitable for Enhanced Mode.

The enhanced mode of the ebusd works perfectly on both the ttyAMA0 (PL011) and the ttyS0 (MiniUART).

If you still want to swap the UARTs, e.g. to continue operating Bluetooth, you can add the following entry in `/boot/config.txt`
```ini
[all]
dtoverlay=disable-bt
```

However, if you want to shutdown Bluetooth completely, this can be done by executing:
```shell
sudo systemctl disable hciuart
```

To verify that the settings are correct, you can run `dmesg|grep ttyAMA0`, which should print a line like this, that should contain at least` PL011`:
```
[    1.138083] 20201000.serial: ttyAMA0 at MMIO 0x20201000 (irq = 81, base_baud = 0) is a PL011 rev2
```

