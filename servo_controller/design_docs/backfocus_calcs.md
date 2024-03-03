# Back-Focus Calculations

## Key Dimensions

*  CellOff = Axial dist from centerline of cell mount bolt holes to front edge of mirror:  20mm
*  FL = Focal length of mirror (given as 56.25" inscribed by Cave): 1428.75mm
*  Drad = Radial dist from center of spider to edge of tube:  123mm
*  DTubeF = Axial dist from front surface of mirror to center of secondary (using mirror cell mount position): 46.43" = 1179mm
*  DTubeR = Axial dist from front surface of mirror to center of secondary (factory original cell position): 1179 + 44 = 1223mm
*  DSec = Minor axis of secondary mirror = 44mm

*  WOBPHt = WO focuser baseplate height above tube: 4mm
*  WOMinHt = WO focuser minimum height above bottom of base plate: ***TBD***

* BPBPht = Baader focuser baseplate height above tube: 2mm
* BPMinHtStd = Baader focuser minimum height above bottom of base plate with std comp ring: 62mm (spec and measured)
* BPMinHtM48 = Baader focuser mininum height above base plate with M48 thread adapter: 51mm (spec)

* FPPosD800 = Focal plane offset for Nikon D800 behind face of the F mount ring: 46.67mm  (Nikon spec)
* FPOffT2 = Focal dist consumed by Nikon F mount to 48mm T2 adapter: 8.33mm (spec = 55 - 46.67 = 8.33mm)

## TS-Optics Newtonian Focuser Configurations

In the descriptions for their line of Newtonians, [TS-Optics](https://www.teleskop-express.de/en/telescopes-4/newtonian-telescopes-315/unc-ontc-high-end-newtonians-from-germany-312)
gives recommended focus position heights as follows:

| Application                 | FP Dist Above 2" Focuser Tube  | Remarks
| ----                        | ----                           | -----
| Visual Only                 | 35mm                           | At this position you don't have enough height to get a DSLR into focus
| Photo with Std Coma Corrs   | 65mm                           | TS corrector, Baader MPCC.  "For visual use, you need to add the 2" extension tube"
| Photo with ASA reduce/corr  | 90mm                           | For ASA2KORRR.

## Eyepiece Focal Plane (Effective Field Stop) Locations

In eyepieces the effective field stop (position of the focal plane) is ususally within a few mm of the 2" or 1.25"
shoulder location.  

A table of data for TeleVue eyepieces can be found [here](https://www.televue.com/engine/TV3b_page.asp?id=214).
The field stop location is the "F" dimension in this table.  For most TeleVue eyepieces the FS location 
is +0.25" or 6mm below (inwards toward the primary) the shoulder face.  For the longest EFL wide-field eyepieces
(Ethos, Delos, Nagler) the FS location shifts outward to around -0.38" or 9.7mm (outwards) relative to the shoulder.

### Configuration Analysis - Visual

Looking at the Baader focuser with its minimum drawtube height of 64mm above the main tube, putting the focal
plane at 75mm should (barely) enable the use of all TeleVue eyepieces for a person of normal vision with the
focuser position all the way in.  However, setting up the mirror position this way will preclude almost
any photographic work.


### Configuration Analysis - Photographic

The first thing you need to know for photographic back focus calculations is that for DSLRs (and other cameras)
is an industry standard 55mm distance from the front of a T-ring to the sensor plane.  This has the benefit
of making all cameras parfocal when used with T-adapters, and also means that we don't have to be concerned
about what camera is in use when setting up the telescope focus situation.

Dedicated astronomy cameras need a similar amount of focus path.  ZWO, the dominant vendor of such cameras
these days, suggests always allowing 55 mm of focus path for the camera stack, which is conveniently the
same as for general purpose cameras.  In ZWO cameras, the in-camera focal path is usually only ~6-15 mm,
but the imaging stack also very often contains an electronic filter wheel (EFW) and sometimes an off-axis
guider (OAG).  These take up focal path and in some setups there may be only a few mm left to reach 55.
Another reason for standardizing on one path length is that if a flattener/compressor lens is used, the
distance from that lens to the sensor plane matters optically and has to be quite exact.  So by convention
the flattener systems are always designed for 55mm back focus distance to the sensor, and M48 spacer rings
are available in a variety of thicknesses to allow this distance to be set precisely.

To set the focal plane height above the main telescope tube, you need to consider the minimum height of the focuser
plus the needed FP distance above the top of the focuser tube (at the bottom of travel) plus the height of the
focuser base plate above the telescope tube, plus the path length of any shoulder on your 2" adapter snout.

The Baader focuser has a special M48 thread adapter that provides a reduced minimum height of 51mm.  It takes some
effort to change over from the visual, but improves the situation with DSLRs by taking 11mm out of the needed
back focus distance.

You can get some additional help by using the right 2" T2 adapter snout.  The Televue adapter has a 9mm shoulder,
which is a lot and really is pretty bad as it doesn't need to have that much shoulder.  A much better choice is
the Baader 2" T-adapter which has a 3mm shoulder *that is removeable* so you can end up with zero path, where
the face of the T-ring itself contacts the top of the focuser.

For any standard camera T-ring setup, you need an *absolute minimum* of 55mm above the top of the focuser tube.
In reality you need a few millimeters more to allow for fine focusing and errors in measurement.
In the table above TS-Optics suggests 65mm above the focuser tube, which gives 10mm of margin.
The focuser minimum height is 51mm and the baseplate height is 2mm, so you need the FP distance above
the telescope main tube for this case to be at least 51+55+2 = 108mm.  If you take the TS-Optics value with its margin,
it's 51+65+2 = 118mm.

## Calculations

* FPTubeF = Focal plane dist above tube for front bolt holes = FL - DTubeF + CellOff - Drad = 1429 - 1246 + 20 - 125 = 78mm
* FPTubeR = Focal plane dist above tube for rear  bolt holes = FL - DTubeR + CellOff - Drad = 1429 - 1290 + 20 - 125 = 34mm

### Dicussion of Calculations

The rear mirror cell bolt hole location was the factory original from Cave, set up for the Carle helical focuser.