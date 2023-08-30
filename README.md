# cave-servo

This repo contains various design and implementation artifacts for my restoration and servo drive conversion
of a 1975 Cave Optical "Astrola 8 inch Model B Deluxe" telescope.  This is considered
a classic golden age Newtonian reflector, and the optical figure of the Cave primary mirrors was
often exceptional.

## References

### Project documentation

[Project thread in the Cloudy Nights Classic Telescopes forum](https://www.cloudynights.com/topic/742954-cave-astrola-8-f7-model-b-deluxe-restoration-upgrade)

[Detailed docs folder](servo_controller/design_docs/)

*  [Analog hand paddle design](servo_controller/design_docs/hand_paddle_design.md)

*  [GPS connection and the DB15 pins on the MaxPCB4](servo_controller/design_docs/MaxPCB4_DB15_pinout.md)

*  [SW dev setup for OnStepX](servo_controller/design_docs/onstepx_maxpcb4_servo_setup.md)

### External references

[MaxPCB4 homepage on OnStep wiki](https://onstep.groups.io/g/main/wiki/33523)

[MaxPCB4 schematic on EasyEDA](https://easyeda.com/editor#id=e2233fc0dbd54d4aaf792255a189c136) (free account required)

[Smart Hand Controller homepage on OnStep wiki](https://onstep.groups.io/g/main/wiki/7152)


## Resto-mod Build Thread on CloudyNights

There is a lot of information in [this thread](https://www.cloudynights.com/topic/742954-cave-astrola-8-f7-model-b-deluxe-restoration-upgrade),
covering all phases of the project including the optical tube restoration, refinishing, saddle/mount upgrades, conversion of AC synchronous motor drive to servo drive, and more.  Many photos of the work in progress are included.

## Optical Tube Assembly (OTA)

The OTA phase of the project is done.  This was more of a process than a design exercise, but mechanical reconfiguration to use dovetail bars instead of the orignal saddle caused noticeable changes in the tube balance.

Here's a summary of what was done.

* Refinish original Parks fiberglass (polyester resin) tube
  *  Sand out cracks in gelcoat
  *  Fiberglass (epoxy resin) patches to fill disused holes and old damage
  *  Paint inside of tube with very high absorption paint
  *  Add curved nut backers behind all screws in the tube
  *  Repaint with 2-part epoxy paint and primer, polish to high gloss
* Convert inaccurately made cast saddle to dovetail bars with machined interface
* Change all hardware to stainless steel
* Strip paint and polish the aluminum tube rotating rings
* Add dovetail bars for finder/aux scope
* Repair jammed focuser
* Refinish the finder rings and add non-mar tips to brass adjuster screws
* Polish aluminum tube end rings
* Repaint Takahashi 50mm finder tube
* Acquired 80mm secondary / guide scope with ADM mounting rings
* Recoat primary mirror and secondary flat
* Clean up rough castings on primary mirror cell, anodize
* Make new clips for primary mirror cell
* Machine solid backing block for secondary mirror
* New tube caps and a Bahtinov focusing mask
* Built a spreadsheet with accurate mass and Dec axis balance model

[Weight and balance spreadsheet on Google Sheets](https://docs.google.com/spreadsheets/d/1t2u4017FmPkpbuE6fjug7ya-knz-X_A-nPPB0cKtMMI/edit?usp=sharing)

## Equatorial Mount and Stand

Thus far:

* Refinished steel pier and cast aluminum legs
   * Leg curved mating flanges re-machined with fly cutter to have correct curvature
   * Leg casting flashing removed (they were very messy), repainted with epoxy primer and paint
   * Steel pier tube de-rusted and repainted with epoxy primer and paint
* Replaced both RA and dec shafts with Misumi 304 hardened stainless steel shafts
   * Dec shaft extended from 27" to 30" long to give increased counterweight moment
   * Lower end of dec shaft tapped for a counterweight retainer bolt/disc
   * Slop in the bearings reduced; original shafts were about .002" undersized
* Other refinishing
   * RA and dec castings repainted with epoxy primer and paint (FS dark gull gray, good match to original)
   * Pier top repainted with epoxy primer and VHT wrinkle black paint (matches original)
   * Main counterweight repainted with wrinkle black 
* Fixes and upgrades
   * Upgraded leveling feet and re-threaded legs to get much less wobble
   * Milled the pier top to fix casting defects and restore full latitude adjustment range
   * Main elevation pivot bolt replaced with stainless steel
   * Pier top attachment screws replaced with knob screws for tool-less disassembly
   * Hex key access holes added to RA casting to simplify installation/removal of the RA shaft setscrews

Todo:

* Make new base plates to mount servo+worm on both axes
* Revisit stacking order on dec axis...possibly move dec drive to lower end of casting
* Improve dec axis thrust bearing at upper end of casting?

## Control Electronics

Now using an OnStep MaxPCB4, after starting with the MaxPCB3.  The MaxPCB4 changes many components to surface mount, eliminates parts on the back, and removes the obsolete RTC module, replacing it with a coin cell holder for internal RTC backup on the Teensy.

The electronics are housed in a Seahorse SE540 (Pelican clone) weatherproof case.  The power supplies, terminal blocks, and MaxPCB4 card are all mounted on a DIN rail attached to the underside of the top panel.  It took a lot of design iterations to get everything to fit in the SE540 case.  I did not want to go to the next larger case in the series because it's *much* larger, to the point of being very ungainly.

All I/O connectors and controls are mounted in the top panel, so that it can be removed from the case as a unit.  There is only one cable connection to anything attached to the case itself - the vent fan.

Rack handles are installed at the outer edges of the panel to aid removal of the panel from the case, and to give a stable and level rest when the panel is inverted on the bench for wiring and testing.

### Cooling

The case cooling is a cross-flow system using a single ultra-quiet Noctua computer case fan.  The fan speed is controlled with a modified Noctua PWM speed controller and can be reduced down to as little as 500 rpm.  Below around 1000 rpm the fan is almost inaudible.  Max speed is around 2400 rpm, which moves quite a bit of air.

The fan is installed in a 3" hole in one end of the case, using a foam filter sandwiched between two aluminum mesh grilles, with a 3D printed louver on the outside to keep rain out.  The exhaust aperture on the other end is identical except there is no fan.  I didn't want to have case penetrations, but there was no other way to make everything fit.

### Power Supplies

There are 3 DIN rail mounted Mean Well power supplies:

*  48V 240W (servo power)
*  12V 60W  (steppers and accessory power)
*   5V 60W  (accessory port power and GPS)

Originally I had a 24V PSU in the design instead of the 5V unit.  That was intended per the original MaxPCB4 design to handle larger RA/Dec steppers that need more than 12 Volts.  Later I realized that since I only need to support smaller 12V steppers for focusers, I could just feed the MaxPCB4 the 12V supply and eliminate the 12V regulator from the board with a jumper.  That gave me the opportunity to have a more substantial 5V PSU to support USB devices from the accessory ports, which are all 3-wire connectors that can be wired for GND/5V/12V.

My plan right now is to build a ~3m accessory cable with a female USB-A connector on the end.  This would allow flexible power distribution:

* plug in an off-the-shelf USB "hydra" cable
* put a 4-6 channel USB hub on the optical tube

Things that might want 5V power could include

* Illuminated reticles
* Cameras*
* Guider
* Mirror cell fan

Note that if we want full USB data connectivity from a camera to a computer, we'd need a different USB cable going back to the computer, not just a power-only cable.

### GPS Integration

Adafruit Ultimate GPS is reported to work fine with the MaxPCB4.
See [this OnStep Forum post](https://onstep.groups.io/g/main/topic/91682216?p=Created,,,20,1,0,0::recentpostdate/sticky,,,20,0,20,91682216,previd=1654743214120117903,nextid=1655301750859438880)

Here are the GPS related settings for config.h:

*  time location source GPS
*  SERIAL_GPS Serial1
*  SERIAL_GPS_BAUD 9600
*  SERIAL_C_BAUD_DEFAULT OFF

Also go fix the OnStepX code to read the FIX signal on some aux port and get rid of the arbitrary 2-minute delay before setting the date/time in the software.

### MaxPCB4 Assembly

When building up the boards it's good to get a batch of 8-pin header sockets.  You need to install 10 of them and it's a PITA to have to cut that many down to length.  The sockets are tough to cut cleanly without losing a pin and ending up with a useless 7-pin socket strip.  You can also get pre-made 24-pin header sockets for the Teensy.

I've eliminated several of the connectors from the stock MaxPCB4 design, including the round DC power input (they fall out easily), the RJ12 for the ST4
(obsolescent and I need 7 pins anyway), the RJ45's for the servo/stepper connections (using terminal blocks instead), the DB15 (multiple issues), 
the Molex power switch connector (removed and hard jumpered), and the 3mm audio plug with AUX7 and AUX8 (don't need these and if I did I wouldn't use that connector).

One note on the boards is that the OKI78SR-5 5V voltage regulators are out of stock with 6-month lead times on Mouser. The 12V regulator (unused in my implementation) is
also out of stock with long lead time.  I was fortunate to have a few already that were intended for the MaxPCB3's. A fallback if you can't even get the 5V regulator 
would be to power the 5V section of the board from the external DIN rail 5V supply.

## Byers Drive Retrofit with Servo Motor Conversion

[Servo controller block diagrams](/servo_controller/design_docs/)

The original AC synchronous motor drives for RA tracking and Dec slow-motion are being replaced with DC servos from StepperOnline.  The control system is based on OnStepX, which can be controlled either by an app like SkySafari, or by a dedicated hardwired analog hand controller.

In addition, I'm replacing the original small, low-precision Cave worm gears with much larger (7.5" for dec and 9.1" for RA) vintage astronomical drives from the Edward R. Byers co.  These are now very hard to get and I was fortunate to be able to obtain them through contacts on an astronomy forum.

All this is motivated by several consideration:

* AC synchronous motors are no longer made.  Some can still be found on eBay, but they are getting scarce.
* The electrical system design in the original Cave mount had 120VAC running all the way out to the Dec hand paddle, and was completely ungrounded.  Given that telescope are frequently exposed to condensing humidity, the design is intrinsically hazardous.
* The AC wires had deteriorated from age to the point where bits of insulation were starting to fall off of wires that were only a few mm away from making the entire mount hot with 120V.
* The AC sync motors can be speed controlled to some extent by variable frequency drive, but they can't really speed up enough for effective slewing to give "go-to" operation.  I have an old "drive corrector" frequency control unit but it's no longer functional.
* The original Cave worm gear sets were of very small diameter and rather poor quality.  The dec drive has huge backlash and was always frustrating to operate.

So, based on experience with doing a servo based CNC conversion of a bench mill, I set out to make a servo conversion of the Cave drives.  This entails both creating a drive electronics package, and replacing the original Cave worm and worm gear with the Byers drives.

### The Byers Drives

The Edward R. Byers Company made precision astronomical drive worm gear sets for half a century, and are regarded as perhaps the best ever made.

The 9.1" drive matches, in almost every respect, some photos of 1990s era drives that remain on the Byers website.  It has a 1.5" shaft bore that fits on my 1.5" RA axis without modification, and a clutch system with an aluminum clutch plate and low friction plastic bearing pads.

The 7.5" drive is somewhat different.  It has a fiberboard (Masonite) clutch plate that has been reported on the forums as sometimes used in older Byers drives.  But there are differences in the hub/clutch design and fabrication that convince me that the hub and clutch plate were not made by Byers:

*  There is only one setscrew through the side of the hub to secure it on the shaft.  On all genuine Byers drives that I have seen, there are two setscrews in quadrature at 90˚ angles, which makes mechanical sense.
*  There are only 5 clutch tensioning bolts.  All photos of known Byers drives that I've seen show 6.  In addition, the spacing of the tapped holes in the hub is very uneven, with one of them a couple of mm out of place.  I don't think Ed Byers would have ever allowed that much slop.  Even if it had been made in the pre-CNC era, a drill template would have been used and there should have been no noticeable error.
*  The interface between the back of the worm wheel and the hub face is metal to metal with oil lubrication.  This is a total mismatch with the Byers design philosophy, which is to rely on dry lubricants and _not_ use oil, which collects dirt.  As a result, the worm wheel has some wear marks in the anodizing.  Fortunately they don't look serious.
* Measurements show that the hub boss thickness does not allow for a plastic bearing plate behind the worm wheel, showing that this hub was most likely not even copied from an actual Byers hub.
* The workmanship on the hub falls well short of the usual Byers standards, with poor surface finish.  It is not gold anodized (though apparently not all Byers hubs were), and the surface is not even level across the back of the hub.

My plan at this point is to make a new hub for the 7.5" drive that will match the design of the 9.1" hub/clutch, and have somewhat improved build quality over the existing one.

* 6 tensioner bolts
* 2 setscrews securing it to the shaft
* bored for 1.5" shaft so no bushing needed
* plastic low friction bearing plates on both sides of the worm wheel
* boss height corrected to allow for the rear bearing plate
* polished and gold anodized


### New Features vs Original Cave Mount and Drives

* Go-to operation 
* Computed polar alignment
* Slewing at 5˚/sec or more
* Greatly reduced drive backlash in both axes
* Level shifters to drive 5V signals to any step/dir servos
* Two spare stepper channels for focuster and field de-rotator
* Time and location from an Adafruit Ultimate GPS module with external active antenna
* WiFi connectivity with an external antenna
* V-Lock AC power cord that won't come unplugged if someone trips over it
* Four 5V/12V accessory connectors to support numerous USB and 12V accessories
* Power meter on the servos with "dark mode" display disable
* An all-analog ST4 style motion hand paddle with a rate control for vintage style operation
* CPU module programmable from the front panel
* Electronics housed in a Seahorse SE540 weatherproof case.  The system will have good moisture resistance even with the lid open, and excellent resistance with the lid closed.
* Near silent high end computer cooling fan with speed control

### Progress So Far

#### Electronics

* Control system design based on Howard Dutton's OnStepX software and MaxPCB4 board is complete.
* Five MaxPCB4 boards purchased; two boards fully built up
* MaxPCB4 connectors revised for greatly increased robustness
* Construction of the case and wiring of the electronics complete except for debugging.
* Servo cables built (difficult due to hybrid wire gauges)
* Firmware compiling capability on my computers verified
* WeMOS D1 Mini Pro wifi modules programmed with the OnStep SWS (smart web server)
   * They are flaky and often throw illegal instruction errors due to bad memory timing
   * Started design work on a replacement wifi shield using a more integrated Espressif 8266 module

#### Mechanical

* Acquired vintage Byers drives for both axes
   * 9.1" 359 tooth drive for RA axis, 1.5" axis bore.  Usable as is.
   * 7.1" 359 tooth drive for dec axis, 2.0" axis bore, with non-Byers hub/clutch.
* Detailed CAD model made of the Byers 9.1" drive spring loaded worm block.
* CAD model made of a prototype base plate for the 9.1" drive (needs revisions though)
* CAD model made of the 7.5" drive clutch and hub assembly
* Teflon clutch bearing pads added to the 7.5" clutch/hub, which appears to not be Byers-made
* 6061 blank acquired for new hub with correct shaft bore and allowance for clutch bearing pads
* Prototype NEMA 23 servo bracket fabricated

#### Awaiting Decisions

* Setting circles and pointer setup
* Mounting provisions for future Renishaw absolute encoders
* 3D printed covers for worm gear rings