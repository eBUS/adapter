---
ref: changelog
lang: en
---
## Firmware change history

### [Version 20220831](20220831-offset.hex), reported as `1 [2ded]`:
* feat: add bootloader version to extra info, requires ebusd version >=[22.4](https://github.com/john30/ebusd/releases/latest)
* feat: add extra info with boot cause (reported as unknown for bootloader v1), requires ebusd version >=[22.4](https://github.com/john30/ebusd/releases/latest)
* feat: do not write to eBUS as soon as start bit was received
* fix: potential bytes skipped in transfer to Ethernet host

### Version 20220731, reported as `1 [54eb]`:
* feat: add high speed serial mode, requires ebusd version >=[22.4](https://github.com/john30/ebusd/releases/latest)
* feat: support v3.1 schematic

### Version 20220327, reported as `1 [63aa]`:
* feat: signal alive on blue LED as ping every 4 seconds
* feat: delay sending errors to host
* fix: remove TX buffers to avoid potential stuck condition after Wemos reboot

### Version 20220313, reported as `1 [dc99]`:
* feat: wait for Wemos to signal readiness after it rebooted autonomously

### Version 20220220, reported as `1 [9939]`:
* fix: for potential irregular W5500 disconnected state
* feat: add firmware checksum and jumpers to extra info 0x00

### Version 20211128, reported as `1 [554f]`:
* feat: more accurate arbitration timing

### Version 20211120, reported as `1 [4dea]`:
* feat: extra infos, requires ebusd version >=[21.1](https://github.com/john30/ebusd/releases/tag/v21.1)
* feat: configurable arbitration delay

### Version 20210106, reported as `1 [a220]`:
* feat: determine TCP remote hang-up, timeout TCP connection after 6 seconds

### Version 20201227, reported as `1 [32f6]`:
* feat: dim blue LED and use max level for 5 seconds error signalling
* fix: DHCP backoff and renew/rebind

### Version 20201219, reported as `1 [c5e7]`:
* feat: ebusd enhanced protocol V1, requires ebusd version >=3.5
* feat: WiFi with wait for readiness
* feat: Ethernet with DHCP or fix IP


## Bootloader change history

### Bootloader version 20201025, reported as `2 [4646]`
* feat: save reset infos and restart count for firmware

### Bootloader version 20201025, reported as `1 [0a6c]`
* feat: first version
