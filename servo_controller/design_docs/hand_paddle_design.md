# Hand Paddle Design

## ST4 Interface

The ST4 6-wire 4-direction interface has been used for SBIG style autoguiding as well as manual
telescope guiding and motion for a few decades.  It is very simple and robust but in its base form
it doesn't provide for an on-paddle rate/speed adjustment.  

Electrically the ST4 interface consists of +5VDC and GND plus four
discrete wires that can be given a contact-closure to GND via normally open switches.
Many autoguiders are set up with this interface.

The MaxPCB4 ST4 interface has an optional +5VDC line that is enabled by a jumper.  This allows
the remote ST4 device to be powered by the 5V line.  The jumper is there to prevent applying
the 5V to a device that doesn't use it or that could be damaged
by unexpected voltage on that pin.  For this hand controller design, the +5 jumper must be
enabled so that the analog rate control will work.  The jumper may need to be removed for
some augoguiders.

Historically the connector for this was an six-conductor RJ12 (often incorrectly called RJ11, but
that was only 4-conductor), but RJ12's are nearly
obsolete from the telecom industry.  I am using the much more modern and robust Amphenol
circular connectors on my control box and hand paddle.  An adapter cable will need
to be made for guide cameras with traditional RJ12 output jacks.

The advantage of using the simple ST4 interface is that with it you can operate a telescope manually
using only a hand paddle and the OnStep controller.

## Hand Controller for the Cave Telescope

My design goal is to be able to operate the telescope in a
"classic" finder-assisted pointing mode using a rough polar alignment and just the hand paddle 
to move the telescope around.

In the spirit of the Cave era, I'd like to retain an all-analog hand paddle, even if it is talking
to the digital OnStep board.  The OnStep "Smart Hand Controller" (SHC) is more capable but requires
a processor and a display screen.   This can be done at the eyepiece without having to worry about light from a display.

The OnStep site gives plans and the schematic for the Smart Hand Controller.  It utilizes an ESP32
processor with a 3D printed custom case and talks to OnStep via Wifi.  It can perform almost all OnStep
operations without requiring the Android app or a computer.  But it's not very Cave-like.

See https://onstep.groups.io/g/main/wiki/7152

### Enclosure

For making a custom hand paddle, Bud Industries PN AN-1302-AB is a black aluminum IP67 rated
box with external size 4.55 x 2.58 x 1.18" and internal dimensions of 4.18 x 2.15 x 0.98".
This is an ideal width for one-hand operation, comparable to mobile device width, and the
depth is enough to allow an Amphenol 8-pin circular connector to be mounted in the end.
It has a gasket seal and is a much better unit than the original Cave hand controller box.

It at least resembles the original 1960s-1970s Cave dec controller, which was a flat sheet
metal aluminum box in black wrinkle paint with two very small buttons.  Internally it
carried 120VAC with two small motor-start capacitors.  The Cave original controller had no
provision for grounding, making it a hazardous design by modern standards.

### Buttons and Potentiometer

I found some nice 12mm IP-65 rated tactile buttons on Amazon: [buttons]().  They are on the small side like the
original buttons, but not quite so flimsy and tiny, and a lot more weather-resistant.

Currently looking for an IP65 or better weatherproof 10K potentiometer.

### Cable

Belden 8-conductor low voltage cable is about $1/ft unshielded and the smallest quantity available is 100 ft.
I found some less expensive unshielded 22-8 cable on Amazon for about $0.56/ft, trying that first.

Connectors: Amphenol C091D series DIN 8-pin, solder terminals, female rear-mount receptacle, male cable plug
for 4-6mm cable.

Female PN  C091 31G008 100 2
Male PN    C091 31H008 100 2

### Rate Control for ST4

You need a rate control on the hand paddle to allow convenient slewing.
This in turn requires an additional analog rate signal on the processor.
The rate control can then be done from the center tap of a 10k pot, routed
to a re-purposed pin on the Teensy with analog capability.  

The AN_RATE signal will be connector to the "AUX4" signal on the MaxPCB4, which is A8 / D22 on the Teensy.
This pin was previously used for PEC in earlier MaxPCB versions, but on the MaxPCB4 it's now free.
That pin is routed to the DB15 on the board, but since the DB15 is now only supporting the external
GPS module, I'm going to attach AN_RATE to the underside of the board, soldering it to the pin 8 pad
and epoxying the wire down for strain relief.

###  Pinout

The pinout on the 8-pin DIN connectors is:

| Pin  | Signal   | Color   | Notes
| ---  | ---      | ---     | ---
|  1   | GND      | BLK     | DC Ground
|  2   | 5VDC     | RED     | Regulated +5VDC
|  3   | RA+      | WHT     | RA increase 
|  4   | RA-      | GRN     | RA decrease 
|  5   | DEC+     | BWN     | DEC increase
|  6   | DEC-     | BLU     | DEC decrease
|  7   | AN_RATE  | YEL     | Analog rate, 10K pot across 5V.  0V => fine track, 5V => Max slew
|  8   | nc       | ORG     | NO CONNECT / SPARE

For this we need a minimum 7-conductor 22AWG cable.  It does not have to be shielded since the
signals are very low frequency.  Inside the box I've done this with discrete
22AWG wires on an XH style connector pair for disconnect from the MaxPCB4 card, leading to an 8-pin
Amphenol panel connector.  The paddle will be attached to the panel via an 8-conductor unshielded
cable with Amphenol 8-pin cable mount connectors on either end.

## ASCOM Interface

ASCOM provides a software protocol for guiding commands that can be sent over serial and USB.
This requires that your computer, guide camera, hand controller, and mount firmware all understand
this protocol and have a comms interface.

The guide rate is also set in software, allowing full flexibility.

The biggest downside of ASCOM interfaces is that standalone operation without a computer becomes problematic.

OnStep has ASCOM drivers for Telescope, Focuser, Rotator, Switch, and Observing Conditions;
they are available at http://www.stellarjourney.com/index.php?r=site/software_telescope
They do support OnStepX, but are only implemented for Windows OS.
