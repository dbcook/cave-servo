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

## Servo Drive Conversion

[Servo controller block diagrams](/servo_controller/design_docs/)

