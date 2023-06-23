# MaxPCB4 DB15 Pinout

## Pinout List

Signal names taken directly from hdutton's schematic

| Pin | Silk Signal Name | Teensy Pin   | Description                 | Use for
| --- | ---              | ---          | ---                         | ---
|  1  | GND              | GND (pin 25) | DC ground                   | GPS: GND
|  2  | 5V               | VIN          | +5VDC from regulator        | GPS: +5V
|  3  | EN1_B            | NA           | D6 from WeMOS wifi module   | 
|  4  | EN1_A            | NA           | D5 from WeMOS wifi module   | 
|  5  | EN2_B            | NA           | D2 from WeMOS wifi module   |
|  6  | EN2_A            | NA           | D1 from WeMOS wifi module   | 
|  7  | AUX3             | dig pin 21   | Dig 21/A7/RX5               | 
|  8  | AUX4             | dig pin 22   | Dig 22/A8                   | PADDLE: A8 - paddle analog rate input
|  9  | TX1_AUX5         | dig pin 1    | ser TX1                     | GPS serial RX
| 10  | RX1_AUX6         | dig pin 0    | ser RX1                     | GPS serial TX
| 11  | SCL              | dig pin 19   | i2c SCL                     |
| 12  | SDA              | dig pin 18   | i2c SDA                     | 
| 13  | PEC              | dig pin 2    | Dig 2                       | GPS: FIX
| 14  | AUX2             | dig pin 20   | Dig 20/A6, ser TX5          | 
| 15  | 3V3              | 3V           | 3.3V supply 250 mA from Teensy | 

## Summary of Functions

### GPIO

Some of the pins on the DB15 can potentially be used as discrete GPIOs from the Teensy or WdMOS D1 Mini.  DB15 pins 7, 8, and 14 have no other function and are always available as Teensy IOs.  Pin 13 is also available if PEC can be fully disabled in the OnStep code.

### Power 5V and 3.3Vd

Pins 1 (digital GND), 2 (+5V), and 15 (3.3V) provide limited amounts of power that can be used to supply other digital electronics.  Be aware that the Teensy can only provide 250mA total, and some is in use on the MaxPCB4 already for pullups on the ST4 port and chip power on the StepStick driver sockets.  

The 5V regulator can supply 1 Amp and is in use on the board to power the Teensy (100mA @ 600MHz) and the WeMOS wifi module (68mA when scanning).  Thus you can safely consume ~500mA.

### Serial and GPS

Pins 9 and 10 have TX and RX for Serial port 1 on the Teensy.  If you don't need the external serial port, they can be set for general purpose digital I/O.

I'm planning to hook up an Adafruit Ultimate GPS module with a Pulse GPSGB1330 active antenna.

The GPS serial comms will consume RX1/TX1 from the DB15.

I'm also going to route the FIX output from the GPS to digital pin 2 (taking over PEC) so I can have a gps-lock indication.

The Adafruit ultimate also provide a PPS signal which could be hooked up to the PPS network to provide an accurate 1 pps to the Teensy.
However, according to the OnStep documentation, the Teensy has a very good internal clock already and we don't need the PPS.

Finally, the Ultimate needs 5V power, so we will hook up DB15 pins 1 and 2 to provide pwr/gnd.

The GPS also has a VBAT input for an external backup battery, but there's already a CR1220 coin cell
holder on the GPS module, so we don't need that.

The GPS ENA signal is not needed except for ultra low power mode, so we won't hook up that either.  

Ultimately there will be total of 5 wires going to the GPS module from the DB15: +5V, GND, TX, RX and FIX

Pulse GPS active antenna datasheet: https://www.mouser.com/datasheet/2/447/GPSGBXXXX-2903608.pdf


### I2C

At this point I don't see any reason to use I2C, so it might be possible to reclaim these pins for GPIO, though I don't know if OnStep supports turning off the I2C via configuration.

### PEC

Digital pin 2 on the Teensy connects to pin 13 on the DB15, and is designated for use with PEC (periodic error correction).
There are differing opinions on the usefulness of PEC in the era of autoguiders.
It is predictive and theoretically could have lower latency, but it still has to be processed by the
controller, and you have to get the correction aligned in time with where you are on the RA worm rotation. PEC is totally unnecessary for
visual use, so as for now I'm deciding I will not support PEC, and rely instead on autoguide for camera use.

I am planning to not configure OnStepX for PEC, and will take over DIG2 to read the GPS FIX signal into the Teensy.

### PPS is no more on MaxPCB4

Pin 14 on the DB15 connects to digital pin 20 on the Teensy.  In MaxPCB3 this was designated for PPS and was also routed to
the external RTC module, which is gone from MaxPCB4.  DB15 pin 14 is now just labeled as AUX2 and is available for either
digital or analog I/O.  

### WeMOS module wifi pins

Four pins from the WeMOS D1 Mini are hooked up to DB15 pins 3 through 6.  I need to check but these should be digital I/O from
the ESP processor on the wifi module.  These signals are *not* routed to the Teensy.  This can allow discrete events to be triggered
directly from the wifi processor, or read directly by the ESP firmwware.  Could be a nice way to reset the Teensy if it was borked
but AFAICT the MaxPCB4 doesn't implement any graceful external-reset feature.

