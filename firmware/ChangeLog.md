---
ref: changelog
lang: en
---
## Firmware change history

### [Version 20220220](20220220-offset.hex), reported as `1 [9939]`:
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
