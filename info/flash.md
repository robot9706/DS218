# SPI flash layout

The 8MB flash has 5 partitions:

|FIS entry location|Name|Location in flash|Memory location|Size|
|---|---|---|---|---|
|0x007FF000|RedBoot|0x00000000|0x00000000|0x00100000|
|0x007FF100|zImage|0x00100000|0x00100000|0x002F0000|
|0x007FF200|rd.gz|0x003F0000|0x003F0000|0x003FF000|
|0x007FF300|vendor|0x007EF000|0x007EF000|0x0x00010000|
|0x007FF400|FIS directory|0x007FF000|0x007FF000|0x00001000|

Notes:
* FIS means "RedBoot - FLASH image directory layout".
* Each entry is 0x100 bytes.
* "FIS entry location" is the location of the entry in SPI flash.
* "Location in flash" means the location of actual data of that partition.


## Links

[RedBoot FIS Header](https://github.com/robacklin/redboot/blob/master/ecos/packages/redboot/current/include/fis.h)