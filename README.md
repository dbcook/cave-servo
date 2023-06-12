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
