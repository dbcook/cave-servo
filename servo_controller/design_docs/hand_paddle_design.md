# Hand Paddle Design

## Hand Controllers for the Cave Telescope

My design goal is to be able to operate the telescope in a
"classic" finder-assisted pointing mode using a rough polar alignment and just the hand paddle 
to move the telescope around.

In the spirit of the Cave era, I'd like to retain an all-analog hand paddle, even if it is talking
to the digital OnStep board.   An analog hand controller can be used at the eyepiece
without having to worry about light from a display.

The fully digital OnStep "Smart Hand Controller" (SHC) is more capable but requires
a processor and a display screen.  The OnStep site gives plans and the schematic for the Smart
Hand Controller.  It utilizes an ESP32
processor with a 3D printed custom case and talks to OnStep via Wifi.  It can perform almost all OnStep
operations without requiring the Android app or a computer.  But it's not very Cave-like and relies
on a display.

See https://onstep.groups.io/g/main/wiki/7152

## ST4 Interface

The advantage of using the simple ST4 interface is that with it you can operate a telescope manually
using only an analog hand paddle wired to the OnStep controller.

The ST4 6-wire 4-direction interface has been used for SBIG style autoguiding as well as manual
telescope guiding and motion for a few decades.  It is very simple and robust but in its base form
it doesn't provide for an on-paddle rate/speed adjustment.  

Electrically the ST4 interface consists of +5VDC and GND plus four
discrete wires that can be given a contact-closure to GND via normally open switches.

The MaxPCB4 ST4 implementation has an optional +5VDC line that is enabled by a jumper.  This allows
the remote ST4 device to be powered by the 5V line.  The jumper is there to prevent applying
the 5V to a device that could be damaged by unexpected voltage on that pin.

_For this hand controller design, the +5 jumper must be
enabled so that the analog rate control will work_.  The jumper may need to be removed for
some autoguiders.

Historically the connector for ST4 was a six-conductor RJ12 (often incorrectly called RJ11, but
RJ11 is only 4-conductor), but with the demise of commercial PBX's, the RJ12 connector is nearly
obsolete from the telecom industry.  In addition, the RJ series connectors are not weatherproof,
use very small gauge wire, have poor resistance to having the wires yanked out of the connector,
and are intended for indoor use only.

To improve the connector situation, I am using the much more modern and robust Amphenol Tuchel
circular connectors on my control box and hand paddle.  This connector type is rated IP67 for
water/dust resistance, accepts wire sizes up to 20AWG, and has excellent retention and strain
relief.  Of course they are more expensive.  Part numbers for the Amphenol Tuchel connectors
are given below.  An adapter cable has to be made to enable use of guide cameras with
traditional RJ12 output jacks.

### Enclosure

For the paddle housing I settled on Bud Industries PN AN-1302-AB black aluminum IP68 rated
box with external size 4.55 x 2.58 x 1.18" and internal dimensions of 4.18 x 2.15 x 0.98".
This is an ideal width for one-hand operation, comparable to mobile device width, and the
depth is enough to allow an Amphenol 8-pin circular connector to be mounted in the end.
It has a gasket seal and is a much better unit than the original Cave hand controller box.

My new design at least resembles the original 1960s-1970s Cave dec controller, which was a
similarly sized flat sheet metal aluminum box in black wrinkle paint with two very small buttons.
Internally the original Cave controller carried 120VAC with two small motor-start capacitors.  It had no
provision for grounding and used wire with inferior insulation, making it a hazardous design
by modern standards.

An OnShape CAD design with the paddle panel layout and hole cutouts is here:
[paddle layout](https://cad.onshape.com/documents/c8a15f66c30f7eddd0eac00d/w/c4d6828416a260d3a80b3b59/e/4e94d409d0708a2baee341bf)
To cut the panel openings I exported a STEP file from OnStep, uploaded it into Fusion 360,
and generated the gcode from there using settings for my CNC milling machine and 
a 1/4" carbide end mill.  Most any CNC router could probably be used, though you might need
a smaller end mill.

The racetrack hole layout for the 8-pin Amphenol connector is here: 
[paddle end layout](https://cad.onshape.com/documents/fff41f718c8e6f1f2d31a363/w/a46d183233d9051d0535ca37/e/bb40830c74602f4e0938cff0)
Positioning of this hole and preparation of the end wall of the box
are crucial.  I first used a 1/4" end mill to machine off (manual mode) the inside ribs on the side wall and
make it vertical and a bit thinner to allow enough thread engagement on the retaining ring.
There is barely enough room along the end wall for this; you need to flatten as much of it as you can.
The center point of the racetrack pattern was located 13mm down from the top of the
end wall (not the little ledge at the very outside that is already couple of mm down).  Be
careful with this measurement, you probably have < 0.5mm of room for error.

### Buttons and Potentiometer

I found some nice 12mm IP-65 rated tactile buttons on Amazon:
[Starelo 12mm buttons](https://www.amazon.com/dp/B09BKXT1J1).
They are on the small side like the original buttons, but not quite so flimsy and tiny, and a lot more weather-resistant.

A [Vishay weatherproof 10K potentiometer](https://www.mouser.com/ProductDetail/72-P16NP-10K) was chosen for the rate control knob.

### Cable and Connectors

For this application we need a minimum 7-conductor 22AWG cable.  It does not have to be shielded since the
signals are very low frequency.  Inside the main control box I've handled the wiring with discrete
22AWG wires on an XH style connector pair for disconnect from the MaxPCB4 card, leading to an 8-pin
Amphenol panel connector.  The paddle will be attached to the main panel connector via an 8-conductor unshielded
cable with Amphenol 8-pin cable mount male connectors on either end.

Belden 8-conductor low voltage cable is about $1/ft unshielded and the smallest quantity available is 100 ft.
I found some less expensive unshielded 22-8 cable on Amazon for about $0.56/ft; I'm trying that first.

Connectors: Amphenol Tuchel C091D series DIN 8-pin, solder terminals, female rear-mount receptacle, male cable plug
for 4-6mm cable.  Using rear-mount receptables is crucial to permit wiring the connector without them being
installed into the panel and paddle box.

| Mount    |  Subtype | Gender  |  Mfr PN (Mouser link)
| ----     |  ----    | ----    |  -----
| Panel    |  Rear Mt | Female  |  [C091 31G008 100 2](https://www.mouser.com/ProductDetail/523-C09131G0081002)
| Cable    |          | Male    |  [C091 31H008 100 2](https://www.mouser.com/ProductDetail/523-C09131H0081012)

### Rate Control

You need a rate control on the hand paddle to allow convenient slewing.
This in turn requires an additional analog rate signal on the processor.
The rate control can then be done from the center tap of a 10k pot, routed
to a re-purposed pin on the Teensy with analog capability.

The AN_RATE signal will be connected to the AUX4 signal on the MaxPCB4, which is A8 / D22 on the Teensy.
This pin was used for PEC in earlier MaxPCB versions, but PEC support on the MaxPCB4 has been
eliminated and AUX4 is now free.
AUX4 is routed to the DB15 pads on the board.  I've removed the physical DB15 connector, so
a wire is just soldered into the via intended for AUX4 on the connector.

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

## ASCOM Interfaces

_For reference only, the hand paddle does not implement ASCOM_

ASCOM provides a software protocol for guiding commands that can be sent over serial and USB.
This requires that your computer, guide camera, hand controller, and mount firmware all understand
this protocol and have a comms interface.

The guide rate is also set in software, allowing full flexibility.

The biggest downside of ASCOM interfaces is that standalone operation without a computer becomes problematic.

OnStep has ASCOM drivers for Telescope, Focuser, Rotator, Switch, and Observing Conditions;
they are available at http://www.stellarjourney.com/index.php?r=site/software_telescope
They do support OnStepX, but are only implemented for Windows OS.
