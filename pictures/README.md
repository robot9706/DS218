# Pictures

## The full PCB:
![PCB](pcb.jpg)

## CPU - RealTek RTD1296
![RTD1296](cpu.jpg)

## Samsung DDR4 RAM
![k4a4g165we-bcrc](ram.jpg)

## Debug edge connector
![edge connector](edge_connector.jpg)

J2 is a debug connector. Looking from the front the pinout is:

|5 - ???|3 - ???|1 - 3.3V|
|---|---|---|
|6 - **RX**|4 - **TX**|2 - **GND**|

The serial is 115200 8N1.

## PCB and components

![PCB with labels](pcb_label.jpg)

* 1: Realtek RTD1296 CPU.
* 2: 4x Samsung K4A4G165WE DDR4
* * x16 bus
* * 4Gb / chip
* * 512MB / chip
* * Total of 2GB RAM.
* 3: PIC board management CPU
* 4: Debug connector
* 5: Boot SPI flash
* * MIXC mx25l6433f