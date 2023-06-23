# Cave Retrofit Servo Cable Pinout and Connectors

## Cable Plan

### Signal list for RA and DEC servo cables

The Amphenol circular connector pin numbers are set so that there is 
uniform maximal distance between consecutively numbered pins, so that pins 1
and 2 are actually not adjacent in the circle.

| Pin  | Signal   | Servo Cable  | Inside box    | Notes
| ---  | ---      | ---          | ---           | ---
|  1   | GND      | BLK 18awg    | BLK 18awg     | center pin, high current   
|  2   | +48VDC   | RED 18awg    | RED 18awg     | high current 
|  3   | STEP+    | WHT 22awg    | WHT 22awg
|  4   | STEP-    | GRN 22awg    | GRN 22awg
|  5   | DIR+     | BLU 22awg    | BLU 22awg
|  6   | DIR-     | BWN 22awg    | BRO/ORG 22awg | check hookup wire colors
|  7   | ALM+     | RED 22awg    | YEL 22awg     | contact closure
|  8   | ALM-     | BLK 22awg    | YEL 22awg     | contact closure (center pin of connector)

### RA servo

*  3m cable length (both sub-cables)
*  Servo power sub-cable
   *  2-conductor, shielded
   *  18 AWG stranded
   *  Cerrowire PN 225-1002J2, 18-2 alarm/security cable
   *  Source: Home Depot
   *  Conductor assignments
      * Black - GND
      * Red - +48VDC servo power
      * Shield - tied to GND at panel end
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

### DEC Servo
  * Same as RA except 4m long - extra length for the motion loop

### Aux Steppers for Focuser/Rotator

* 4m cable length - need extra length for motion loops
* Stepper drive cables (both identical)
   *  6-conductor, shielded - same wire as for servo cables
   *  22 AWG stranded 7x30
   *  Belden PN: 5504FE 008U500
   *  Source: Digi-key
   *  Cost: $137.44 for 500ft (shorter lengths not available)
   *  Conductor assignments:
      * BLK:
      * RED:
      * WHT: STEP+
      * GRN: STEP-
      * BLU: DIR+
      * BWN: DIR-
    * Connectors
       * Cable Mount connector at panel end
          * 4-cond Amphenol circular
          * Male
          * for 4-6mm cable diam
          * Amphenol PN TBD
          * Source: Mouser
          * Cost: TBD
       * Mating connector in panel:
          * 4-cond Amphenol circular
          * Female
          * rear mount (important)
          * Amphenol PN C091 31G004 100 2
          * Source: Mouser
          * Cost TBD
       * Cable connectors at servo
          * TBD


## Sleeving

In each servo cable, the two sub-cables are encased in PC modder style sleeving as follows:
* Mfr: MDPC-X
* Type: Classic small cable sleeving, white
* Source: TitanRig on Ebay
* Cost: $23.99 for 50 ft

## Additional Materials

* Small 4" cable ties to hold sub-cables together before sleeving
* 2 ea. 5" piece of OneWrap velcro to hold coiled cables
* 1/8" heatshrink for individual conductors on Amphenol connector