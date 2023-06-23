# cave-servo

This repo contains various design and implementation artifacts for my restoration and servo drive conversion of a 1975 Cave Optical "Astrola 8 inch Model B Deluxe" telescope.  This is considered
a classic golden age Newtonian reflector, and the optical figure of the Cave primary mirrors was
often exceptional.

## Resto-mod Build Thread on CloudyNights

[thread in the Classic Telescopes forum](https://www.cloudynights.com/topic/742954-cave-astrola-8-f7-model-b-deluxe-restoration-upgrade)

There is a lot of information in the thread linked above, covering all phases of the project including the optical tube restoration, refinishing, saddle/mount upgrades, conversion of AC synchronous motor drive to servo drive, and more.

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

[Weight and balance spreadsheet on Google Sheets](https://docs.google.com/spreadsheets/d/1t2u4017FmPkpbuE6fjug7ya-knz-X_A-nPPB0cKtMMI/edit?usp=sharing)

## Equatorial Mount and Stand

Thus far:

* Refinish steel pier and cast aluminum legs
* Upgrade leveling feet and re-thread legs to get much less wobble
* Replace both RA and dec shafts with Misumi hardened stainless steel shafts
* Acquire vintage Byers drives for both axes
* Refinish the major EQ head castings and the counterweights
* Add counterweight retainer to lower end of dec shaft
* Drill access holes for RA bearing setscrews

Todo:

* Mill the pier top to fix casting defects and restore full latitude adjustment range

## Control Electronics

Now using an OnStep MaxPCB4, after starting with the MaxPCB3.  The MaxPCB4 changes many components to surface mount, eliminates parts on the back, and removes the obsolete RTC module, replacing it with a coin cell holder for internal RTC backup on the Teensy.

The electronics are housed in a Seahorse SE540 (Pelican clone) weatherproof case.  The power supplies, terminal blocks, and MaxPCB4 card are all mounted on a DIN rail attached to the underside of the top panel.  It took a lot of design iterations to get everything to fit in the SE540 case.  I did not want to go to the next larger case in the series because it's *much* larger, to the point of being very ungainly.

All I/O connectors and controls are mounted in the top panel, so that it can be removed from the case as a unit.  There is only one cable connection to anything attached to the case itself - the vent fan.

Rack handles are installed at the outer edges of the panel to aid removal of the panel from the case, and to give a stable and level rest when the panel is inverted on the bench for wiring and testing.

### Cooling

The case cooling is done with a cross-flow system using a single ultra-quiet Noctua computer case fan.  The fan speed is controlled with a modified Noctua PWM speed controller and can be reduced down to as little as 500 rpm.  Below around 1000 rpm the fan is almost inaudible.  Max speed is around 2400 rpm, which moves quite a bit of air.

The fan is installed in a 3" hole in one end of the case, using a foam filter sandwiched between two aluminum mesh grilles, with a 3D printed louver on the outside to keep rain out.  The exhaust aperture on the other end is identical except there is no fan.  I didn't want to have case penetrations, but there was no other way to make everything fit.

### Power Supplies

There are 3 DIN rail mounted Mean Well power supplies:

*  48V 240W (servo power)
*  12V 60W  (steppers and accessory power)
*   5V 60W  (accessory port power and GPS)

Originally I had a 24V PSU in the design instead of the 5V unit.  That was intended per the original MaxPCB4 design to handle larger RA/Dec steppers that need more than 12 Volts.  Later I realized that since I only need to support smaller 12V steppers for focusers, I could just feed the MaxPCB4 the 12V supply and eliminate the 12V regulator from the board with a jumper.  That gave me the opportunity to have a more substantial 5V PSU to support USB devices from the accessory ports, which are all 3-wire connectors that can be wired for GND/5V/12V.

My plan right now is to build a ~3m accessory cable with a female USB-A connector on the end and then use a widely available "hydra" cable to support plugging in multiple USB powered devices.

### GPS Integration

Adafruit Ultimate GPS is reported to work fine with the MaxPCB4.
See [this OnStep Forum post](https://onstep.groups.io/g/main/topic/91682216?p=Created,,,20,1,0,0::recentpostdate/sticky,,,20,0,20,91682216,previd=1654743214120117903,nextid=1655301750859438880)

settings for config.h:

*  time location source GPS
*  SERIAL_GPS Serial1
*  SERIAL_GPS_BAUD 9600
*  SERIAL_C_BAUD_DEFAULT OFF

Also go fix the code to read the FIX signal on some aux port and get rid of the arbitrary 2-minute delay before setting the date/time in the software.

### MaxPCB4 Assembly

When building up the boards it's good to get a batch of 8-pin header sockets.  You need to install 10 of them and it's a PITA to have to cut that many down to length.  The sockets are tough to cut cleanly without losing a pin and ending up with a useless 7-pin socket strip.

One note on the boards is that the OKI78SR-5 5V voltage regulators are out of stock with 6-month lead times on Mouser. The 12V regulator (unused in my implementation) is
also out of stock with long lead time.  I was fortunate to have a few already that were intended for the MaxPCB3's. A fallback if you can't even get the 5V regulator 
would be to power the 5V section of the board from the external DIN rail 5V supply.

## Servo Drive Conversion

[Servo controller block diagrams](/servo_controller/design_docs/)

The original AC synchronous motor drives for RA tracking and Dec slow-motion are being replaced with DC servos from StepperOnline.  The control system is based on OnStepX, which can be controlled either by an app like SkySafari, or by a dedicated hardwired analog hand controller.

In addition, I'm replacing the original small, low-precision Cave worm gears with much larger (7.5" for dec and 9" for RA) vintage astronomical drives from the Edward R. Byers co.

All this is motivated by several consideration:

* AC synchronous motors are no longer made.  Some can still be found on eBay, but they are getting scarce.
* The electrical system design in the original Cave mount had 120VAC running all the way out to the Dec hand paddle, and was completely ungrounded.  Given that telescope are frequently exposed to condensing humidity, the design is intrinsically hazardous.
* The AC wires had deteriorated from age to the point where bits of insulation were starting to fall off of wires that were only a few mm away from making the entire mount hot with 120V.
* The AC sync motors can be speed controlled to some extent by variable frequency drive, but they can't really speed up enough for effective slewing to give "go-to" operation.  I have an old "drive corrector" frequency control unit but it's no longer functional.
* The Cave worm gear sets themselves were of very small diameter and  rather poor quality.

So, based on experience with doing a servo based CNC conversion of a bench mill, I set out to make a servo conversion of the Cave drives.  This entails both creating a drive electronics package, and replacing the original cave worm and worm gear with the Byers drives.

This new design adds a great many features to the telescope:

* Computed polar alignment
* Slewing at 5˚/sec or more and go-to operation
* Level shifters to drive 5V signals to any step/dir servos
* Two spare stepper channels for focuster and field de-rotator
* Time and location from an Adafruit Ultimate GPS module
* WiFi connectivity with an external antenna
* V-Lock AC power cord that won't come unplugged if someone trips over it
* Multiple 12V accessory connectors
* Power meter on the servos with "dark mode" display disable
* An all-analog ST4 style motion hand paddle with a rate control for vintage style operation
* CPU and wifi module programmable from the front panel
* Electronics housed in a Seahorse SE540 weatherproof case.  The system will have good moisture resistance even with the lid open, and excellent resistance with the lid closed.
* Near silent cooling fan with speed control

Progress thus far:

* Control system design based on Howard Dutton's OnStepX software and MaxPCB4 board is complete.
* Nearly all parts acquired
* Detailed CAD model made of the Byers 9" drive spring loaded worm block.
* Construction of the case and wiring of the electronics is well along.
* Servo cables built (a bit difficult due to hybrid wire gauges)
* Firmware compiling capability on my computers verified
