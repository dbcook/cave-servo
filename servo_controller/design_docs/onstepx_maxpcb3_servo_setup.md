# OnStepX Configuration for MaxPCB3 board with Stepperonline Servos

1.  Install the [Teensyduino IDE](https://www.pjrc.com/teensy/td_download.html)
1.  Install optional additional libraries to ~/Library/Arduino15
    1.  [Arduino PID library](https://github.com/hjd1964/Arduino-PID-Library)
       This is derived from a fork of (https://github.com/br3ttb/Arduino-PID-Library) which appears
       in the Arduino IDE library manager as "PID" version 1.2.0 (though the repo now says 1.2.1) by Brett Beauregard.
       Only needed if you have bringing in the encoder inputs and having OnStep do the low level servo control.
       You don't need this library if you set up the Stepperonline in Position mode, and configure OnStepX axis mode to GENERIC.
1.  Set "use external editor" in the Teensyduino IDE preferences.  This just disallows editing inside the IDE, which doesn't handle
       shared edits.
1.  Set the important include paths in vscode preferences.  Do it for "workspace", not "user".  You need the /** to make them recursive.
    ```
    /Users/yourname/Library/Arduino15/libraries/**
    /Users/yourname/Library/Arduino15/packages/arduino/hardware/**
    /Users/dbcook/Library/Arduino15/packages/arduino/tools/avr-gcc/7.3.0-atmel3.6.1-arduino7/avr/include/**
    ```

## Gremlins

*  You have to teach vscode a lot of include paths in the preferences.  There's no way to export them from the Teensyduino IDE.
*  Not sure if vscode processes #ifdef to figure out what headers are actually included
*  Arduino libraries all have their .h files in their individual library directory.  Not a fundamental problem but it means in
   practice that you have to put all of them on the vscode include path.

## TBDs

*  Figure out how to make the firmware use the onboard emulated EEPROM in the Teensy.  
   ANSWER: Looks like this is the built-in default when board is Teensy, nice!
*  Get time support working in the Teensy (selected it in config.h already) and check on how to hook up the backup battery
   and de-populate the RTC module on the board.  
   ANSWER: hook up 3V coin cell to VBAT (near the micro SD card).  The TIME library included with teensyduino should do the rest.  No reason to rely on the cheesy and obsolete external I2C RTC/EEPROM modules.  Teensy 3.6 has weird limitations on EEPROM use vs clock speed that interfere with I2C anyway, better not to use I2C when not essential.