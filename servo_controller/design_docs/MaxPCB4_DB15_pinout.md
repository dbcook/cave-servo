# MaxPCB4 DB15 Connector Pinout and GPS Interface

## Pinout List

Silkscreen signal names taken directly from the MaxPCB4 schematic; Teensy pin IDs from Teensy pinout card.

| Pin | MaxPCB4 Signal | Teensy Pin    | Description                    | Use for / notes
| --- | ---            | ---           | ---                            | ---
|  1  | GND            | GND (pin 25)  | DC ground                      | GPS: GND
|  2  | 5V             | VIN           | +5VDC from MaxPCB              | GPS: +5V
|  3  | EN1_B          | NA            | D6 from WeMOS wifi module      | encoder 1 input, routed to WiFi module
|  4  | EN1_A          | NA            | D5 from WeMOS wifi module      | encoder 1 input, routed to WiFi module
|  5  | EN2_B          | NA            | D2 from WeMOS wifi module      | encoder 2 input, routed to WiFi module
|  6  | EN2_A          | NA            | D1 from WeMOS wifi module      | encoder 2 input, routed to WiFi module
|  7  | AUX3           | D21 - A7 - RX5| Digital/analog GPIO            | GPS serial RX
|  8  | AUX4           | D22 - A8      | Digital/analog GPIO            | PADDLE: A8 - analog rate input
|  9  | TX1_AUX5       | D1 - TX1      | serial TX1                     | debug monitor serial TX
| 10  | RX1_AUX6       | D0 - RX1      | serial RX1                     | debug monitor serial RX
| 11  | SCL            | D19 - SCL     | i2c SCL clock line             |
| 12  | SDA            | D18 - SDA     | i2c SDA data line              | 
| 13  | PEC            | D2            | Digital GPIO                   | GPS: FIX input to Teensy
| 14  | AUX2           | D20 - A6 - TX5| Digital/analog/serial IO       | GPS serial TX
| 15  | 3V3            | 3V3           | 3.3V supply 250 mA from Teensy | _avoid using_

## The DB15 Connector on the MaxPCB4

The DB15 connector on the MaxPCB4 OnStepX card carries a grab bag of various signals that don't lend themselves to being put
on specific connectors, because there would have to be a lot of them.

There are 3.3 and 5V power outputs, two serial ports, the I2C interface (currently unused), a group of 4 encoder signals
that go directly to the WiFi module, and 4 other pins that are unassigned.

The DB15 connector is outdated and kind of big and clunky; I think it's on the MaxPCB4 as a way to bring those signals out of a
very small 3D printed enclosure.  For my MaxPCB4 builds I'm going to omit the DB15 and solder connections directly
to its footprint on the card and use inline XH connectors for device disconnect.

In my configuration I've been able to keep both serial ports available.  One gets used for the GPS interface and the
other remains as a spare on DB15 pins 7 and 14.

The vias on the MaxPCB4 are just big enough to take a 22AWG wire if you are careful to get all strands into the hole.
XH style connectors are used for disconnect from external devices (GPS and hand paddle).

## Discussion of Available Functions on the DB15

### GPIO

Some of the pins on the DB15 can potentially be used as discrete GPIOs from the Teensy or WdMOS D1 Mini.  In this case I'm using
Teensy pin D2 (formerly the PEC signal) for the GPS FIX input.

### Power 5V and 3.3Vd

Pins 1 (digital GND), 2 (+5V), and 15 (3.3V) provide limited amounts of power that can be used to supply other digital electronics.  The Teensy can only provide 250mA total, and some is in use on the MaxPCB4 already for pullups on the ST4 port and chip power on the StepStick driver sockets, so I advise against using pin 15 to power any offboard device.

The 5V regulator connected to pin 2 can supply 1000 mA and is in use on the MaxPCB4 to power the Teensy (100mA @ 600MHz) and the
WeMOS wifi module (68mA when scanning).  Thus you can safely consume ~500mA.  In my design there's a dedicated 12A 5V DIN rail power supply
to power multiple USB devices so we don't need to fan out this output.

### Serial Ports

Pins 9 and 10 carry the TX and RX for Serial port 1 on the Teensy.  This serial port is *not* shared
with the Teensy USB adapter.  If you've enabled debug output, you could receive it on this port,
though it's better to just use the Teensy native USB interface for that.

Pins 7 and 14 on the DB15 (Teensy D21/D20) are RX5/TX5 and can be used for the GPS serial port.

### I2C

The external I2C devices (EEPROM and RTClock) that appear on some OnStep boards
aren't necessary with a Teensy 4.1 and are not present on the MaxPCB4. I don't think I2C
will ever be needed in a Teensy+ESP32 design, so it may be possible to reclaim pins 11 and 12 for GPIO,
though I don't know if OnStepX supports turning off the Teensy I2C via configuration.

### PEC

Digital pin 2 on the Teensy appears on pin 13 on the DB15, and was originally
designated for use with PEC (periodic error correction).
There are differing opinions on the usefulness of PEC in the era of autoguiders.
It is predictive and theoretically could have lower latency, but it still has to
be processed by the controller, and you have to get the correction aligned in time
with where you are on the RA worm rotation. PEC is totally unnecessary for
visual use, so as for now I'm deciding I will not support PEC, and rely instead on
autoguide for camera use.

### PPS is no more on MaxPCB4

Pin 14 on the DB15 connects to digital pin 20 on the Teensy.  In MaxPCB3 this was
designated for PPS and was also routed to the external RTC module, which is gone from MaxPCB4.
DB15 pin 14 is now labeled as AUX2 and is part of serial port 5, which is needed
for the external GPS anyway.  

### WiFi Module Encoder Input Pins

Four pins from the WiFi module are hooked up directly to DB15 pins 3 through 6.
These signals are intended to bring encoder inputs into the WiFi module and are *not*
routed to the Teensy. This supports a "relay" function where the WiFi module makes the
encoder readings available to the Teensy with low enough latency for servo operation.

## GPS Connection and Fix Determination

The GPS module uses an external Pulse GPSGB1330 active antenna mounted on top of the panel, since the module's built-in antenna
is not of much use with a solid metal panel between it and the sky.  It turns out that
when running the electronics on the bench, the GPS will achieve lock if the panel is right side
up, and will lose lock if flipped over - very handy for testing.

*  Pins 1 and 2 provide GND and +5V to the GPS module
*  Pins 7 and 14 are used for serial RX5 and TX5 (Teensy serial port 5) for GPS NMEA input.
*  Pin 13 (Teensy D2) receive the FIX output from the GPS, providing a GPS-lock indication.

OnStepX currently has a 2-minute fixed delay before declaring the GPS OK.
That is way too long if the GPS has good ephemeris and is near the last known location, and likely not long enough
if there's no backup battery or it has trouble acquiring enough satellites.

However, the GPS FIX signal is a little hard to use directly.  On the Ultimate GPS, it's the same
as the LED fix indicator output, and blinks.  You have to decode the timing of the pulses to
tell whether the GPS is locked.

It's actually easier to look at the GPS serial messages and wait for the HDOP value (horizontal
dilution of precision) to drop below some threshold.  I've implemented this in my fork of OnStepX
and it seems to work fine.

Overall there are 5 wires going to the GPS module from the DB15: +5V, GND, TX, RX and FIX

### GPS Signals Not Used

*   PPS: MaxPCB4 does not support external PPS.

*   VBAT: Not needed, we're using the GPS module's onboard coin cell holder

*   ENA: Only needed for ultra low power mode.  We have plenty of power.


## References

[Pulse GPS active antenna datasheet]( https://www.mouser.com/datasheet/2/447/GPSGBXXXX-2903608.pdf )

[Adafruit Ultimate GPS Module](https://www.adafruit.com/product/746)

[MaxPCB4 homepage on OnStep wiki](https://onstep.groups.io/g/main/wiki/33523)

[MaxPCB4 schematic on EasyEDA](https://easyeda.com/editor#id=e2233fc0dbd54d4aaf792255a189c136) (free account required)

[Smart Hand Controller homepage on OnStep wiki](https://onstep.groups.io/g/main/wiki/7152)
