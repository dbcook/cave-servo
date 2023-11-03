# WiFi Modules for the MaxPCB4

## Original MaxPCB4 Design with Wemos D1 Mini Pro Module

This table shows the wiring of the socket for the D1 Mini Pro and the Teensy 4.1 on the MaxPCB4.

| D1 Mini Pro Module Pin | ESP8266EX Pin  | Teensy Pin | Teensy Function |  Explanation
| ----                   | -----          | ----       |  ------         |  ------
| D7                     | IO13 / MTCK    | 8          |  TX2            |  alternate ESP serial U0RXD (see ESP8266EX data sheet)
| D8                     | IO15 / MTD0    | 7          |  RX2            |  alternate serial U0TXD from ESP8266EX

## Quality Problems

The D1 Mini Pro module has been cloned by various manufacturers, and many of them are low quality, often exhibiting problems with memory access.
This is because the ESP8266EX relies on eternal flash chips, and apparently some of the modules are using
flash with improper memory timing specs.  I have a few such generic modules from one supplier, and so far
none of the ones I've tested work properly.  They will only boot up once in a while and often throw
illegal instruction errors, symptomatic of memory access failures.

## Solution

In order to get more modern modules of assured quality, I'm switching to the official Espressif
ESP32-WROOM-32U dev module.  This has a larger and different footprint than the D1 Mini Pro, necessitating
an adapter board that plugs into the D1 Mini Pro sockets on the MaxPCB boards.

The benefits are many:

* All flash is on the ESP32 itself, so no chance of external flash problems
* Made by Espressif, board quality is visibly better.
* Adds Bluetooth capability
* Faster CPU (240MHz) and more flash (4MB).
* A lot more GPIOs if you want them

## Needed Pin Mappings for the ESP32-WROOM-32U Dev Module

I've made a small, passive pin remapping board using an AdaFruit half-size "perma-proto" board,
cut down with a tile saw and with jumper wires soldered on.

The board is cut to be just long enough for both boards to sit end-to-end, and the
power bus rows on both sides have been cut off.

The adapter board hangs out over the Teensy, so you need to use two stacked sets of 8-pin
headers when plugging it into the MaxPCB4 to get enough vertical clearance.

In addition to the connections in this table, be sure to hook up +5V and GND.

| ESP32 Module Pin   | ESP Function  | Teensy Pin  | Teensy Function | D1 Footprint Name
| -----              | ----          | ----        | ----            | ----
| GPIO16             | U2RXD         | 8           | TX2             | D7
| GPIO17             | U2TXD         | 7           | RX2             | D8

The following connections exist between the Mini D1 Pro socket and the DB15 connector
on the MaxPCB4. They are intended as encoder inputs that can optionally be
used by the SWS (Smart Web Server) software on the ESP module.  If you don't
need those inputs, you don't need to hook them up.

| DB15 pin           | D1 Module Pin  | Description   |  ESP32 Module Pin
| ----               | ----           | ------        |  ------
| EN1_A              | D5             | Encoder 1A in |  TBD - check pinmaps in SWS code
| EN1_B              | D6             | Encoder 1B in |  TBD
| EN2_A              | D1             | Encoder 2A in |  TBD
| EN2_B              | D2             | Encoder 2B in |  TBD

I hope someday to run my mount with Renishaw absolute encoders, but that's a
story for a different day.