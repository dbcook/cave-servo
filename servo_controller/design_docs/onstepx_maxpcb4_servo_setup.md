# OnStepX Development Configuration for MaxPCB4 board with Stepperonline Servos

## Tensyduino IDE

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

## vscode IDE

I've gotten both OnStepX and the SWS web server image to compile and load from within vscode using the
Arduino plugin.  The compile is somewhat slower than in the Teensyduino IDE, and there is no feedback
during the downloads to the hardware, but the code browsing is *drastically* better.

You should install the "teensy" applet from the Teensy website and have it running when you load code.
This enables vscode to load the hex files into the processor without having to push the button on the
Teensy module.  In other words, this enables code updating through the front panel USB port.


## Gremlins

*  To be able to see files in the Arduino/Teensyduino IDE that reside in subfolders, you have to create
   an empty .ino file inside that subdirectory, and then open that .ino in the IDE, which spins up a
   whole new instance of the IDE.  A whole world of awful...that's why I want to edit in vscode
*  Outside of things included in the Arduino15 library plugin, you have to teach vscode some include
   paths in the preferences.  There's no way to export them from the Teensyduino IDE.

## TODO List

*  [DONE] Figure out how to make the firmware use the onboard emulated EEPROM in the Teensy.  
   ANSWER: This is the default on Teensy 4.1
*  [DONE] Can we compile Teensy code in vscode?  I found the Arduino15 library plugin after writing
   the config procedure.
   ANSWER: This works.  Needed to update the Arduino base lib and the TinyGPS lib (my own fork to fix a serious warning)