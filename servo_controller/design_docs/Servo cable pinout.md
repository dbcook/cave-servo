# Cave Retrofit Servo Cable Pinout and Connectors

## Cable Plan

The output connectors and panel wiring have been designed such that all four
output channels can accommodate 6-wire STEP/DIR servos and 4-wire stepper
motors on each channel without having to change the panel connectors or
the internal wiring to the MaxPCB4 board.

The proper driver module must be installed for each channel on the MaxPCB4.
This will be a ShiftStick for servos, and any suitable StepStick for steppers.

For the 2 servo + 2 stepper configuration that I need, I made the panel
with 8-pin connectors for the two RA/Dec servo drives and 6-pin connectors for
the focuser/rotator stepper channels.  That way there is no possibility of connecting a
motor to an incorrect driver.

This table shows the capabilities of each channel:

| Name     | 8-wire servo | 6-wire servo | 4-wire stepper
| ---      | ---          | ---          | ---
| RA       | YES          | YES          | YES
| Dec      | YES          | YES          | YES
| Foc      | NO           | YES          | YES
| Rot      | NO           | YES          | YES

### Signal list for RA and DEC servo and stepper cables

The servo connectors are Amphenol 8-pin, while the stepper connectors are 6-pin.
Steppers only really need 4 connectors, but adding power and ground allows
servos to be used in place of the aux steppers, though without the alarm
contact closure pins.

The Amphenol 8-pin circular connector pin numbers are set so that there is 
uniform maximal distance between consecutively numbered pins, so that pins 1
and 2 are actually not adjacent in the circle.

| Pin  | Servo Signal   | Stepper Signal | Servo Cable  | Inside box    | Notes
| ---  | ---            | ---            | ---          | ---           | ---
|  1   | GND            | GND            | BLK 18awg    | BLK 18awg     | high servo current   
|  2   | +48VDC         | +12VDC         | RED 18awg    | RED 18awg     | high servo current 
|  3   | STEP+          | 1B             | WHT 22awg    | WHT 22awg
|  4   | STEP-          | 1A             | GRN 22awg    | GRN 22awg
|  5   | DIR+           | 2A             | BLU 22awg    | BLU 22awg
|  6   | DIR-           | 2B             | BWN 22awg    | BWN/ORG 22awg |
|  7   | ALM+           | nc             | RED 22awg    | YEL 22awg     | servo fault contact closure
|  8   | ALM-           | nc             | BLK 22awg    | YEL 22awg     | servo fault contact closure

### Servo Cable Design

*  RA 3m length (both the power and signal sub-cables)
*  Dec 4m length due to dec drive motion around RA axis
*  Servo power sub-cable
   *  2-conductor, shielded
   *  18 AWG stranded
   *  Cerrowire PN 225-1002J2, 18-2 alarm/security cable
   *  Source: Home Depot
   *  Conductor assignments
      * Black - GND
      * Red - +48VDC servo power
*  Digital signals sub-cable
   *  6-conductor, shielded
   *  22 AWG stranded 7x30
   *  Belden PN: 5504FE 008U500
   *  Source: Digi-key
   *  Cost: $137.44 for 500ft (shorter lengths not available)
   *  Conductor assignments:
      * BLK: ALM-
      * RED: ALM+
      * WHT: STEP+
      * GRN: STEP-
      * BLU: DIR+
      * BWN: DIR-
    * Connectors
       * Cable Mount connector at panel end
          * 8-cond Amphenol circular
          * Male
          * for 6-8mm cable diam
          * Amphenol PN C091 31H008 101 2
          * Source: Mouser
          * Cost: $19.53
       * Mating connector in panel:
          * 8-cond Amphenol circular
          * Female
          * rear mount (important)
          * Amphenol PN C091 31G008 100 2
          * Source: Mouser
          * Cost $12.65
       * Cable connectors at servo
          * 2-pin for power
          * 6-pin for digital signals
          * Source: Furnished by Stepperonline with servo
          * Cost: included with servos

### Aux Steppers for Focuser/Rotator

* 4m cable length - need extra length for motion loops
* Stepper drive cables (both identical)
   *  6-conductor, shielded - same wire as for servo cables
      *  Allows use of either a 4-wire stepper or a 6-wire servo
   *  22 AWG stranded 7x30
   *  Belden PN: 5504FE 008U500
   *  Source: Digi-key
   *  Cost: $137.44 for 500ft (shorter lengths not available)
   *  Conductor assignments:
      * BLK: GND
      * RED: +12VDC (or other as desired for a servo)
      * WHT: 1B stepper / STEP+ servo
      * GRN: 1A stepper / STEP- servo
      * BLU: 2A stepper / DIR+ servo
      * BWN: 2B stepper / DIR- servo
    * Connectors
       * Cable Mount connector at panel end
          * 6-cond Amphenol circular
          * Male
          * for 4-6mm cable diam
          * Amphenol PN C091 31H006 101 2
          * Source: Mouser
          * Cost: see BOM
       * Mating connector in panel:
          * 6-cond Amphenol circular
          * Female
          * rear mount (important)
          * Amphenol PN C091 31G006 100 2
          * Source: Mouser
          * Cost see BOM
       * Cable connectors at stepper
          * TBD


## Connector Build Order

For the hybrid servo cables with the 18AWG power wire, it's easiest to put the wires on the
Amphenol panel connectors in this order:

1.  GND and +48VDC (the 18AWG power cable)
1.  The two ALM contact closures.  Inside the panel, use discrete 22AWG yellow wires.
1.  The rest of the conductors in the 22-6 Belden wire

## Sleeving

In each servo cable, the two sub-cables are encased in PC modding style sleeving as follows:
* Mfr: MDPC-X (Germany)
* Type: Classic small cable sleeving, white
* Source: TitanRig on Ebay (no longer available from this vendor)
* Cost: $23.99 for 50 ft

## Additional Materials

* Small 4" cable ties to hold sub-cables together before sleeving
* 2 ea. 5" piece of OneWrap velcro to hold coiled cables
* 1/8" heatshrink for individual conductors on Amphenol connector
* 3/8" heatshrink to hold down the sleeving on the long external cables
* 5.8mm printable heatshrink tube for Brother labeling machine
* 6mm Brother lable tape to label connectors on board (designators missing from silkscreen)