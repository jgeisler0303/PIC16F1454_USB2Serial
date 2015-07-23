PIC 16F1454 USB to Serial Adapter Firmware
==========================================

This firmware turns a PIC 16F1454 into a USB to Serial adapter like e.g. the famous FTDI FT232RL.

## Advantages
* The PIC is less than half the price and available in a 14 pin DIL package.
* Like the FTDI it runs without an external oscillator.
* Unlike the FTDI the source code is available and you can know exactly how everything is working, or even change things like e.g. the blinking pattern.
* Unlike some [atmega8 solutions](http://www.recursion.jp/avrcdc/), it runs in full speed (USB 2.0) and is thus fully compliant with the USB standard.

## Origin
The code is mainly a copy of the [this](http://sky.geocities.jp/home_iwamoto/P16F145x/P16_L03.htm) project ([google english](http://translate.google.de/translate?hl=de&sl=ja&u=http://sky.geocities.jp/home_iwamoto/P16F145x/P16_L03.htm&prev=search)),
which is basically a stand-alone copy of Microchip's  CDC - Serial Emulator Demo code.

The following changes were made:
* The project was changed to compile for the 16F1454 instead of the 16F1455.
* The code was changed to break out the DTR signal (on pin RC3) which is needed for fully automatic flashing of Arduino-like atmegas.
* The status LEDs were relocated to pins RC0 and RC1

## Obtaining the firmware
For your convenience, the pre-compiled .hex-file is can be downloaded [here (right click "Save as...")](https://raw.githubusercontent.com/jgeisler0303/PIC16F1454_USB2Serial/master/PIC16F1454_USB2Serial.hex).

## Getting it to run
If you come from the Arduino/AVR world and don't have a programmer for PICs, you can try out my fork of [ardpicprog](https://github.com/jgeisler0303/ardpicprog) that enables you to program some PICs and especially the 16F1454 using a special sketch for your Arduino.
Read this [documentation](http://rweather.github.io/ardpicprog/) and further detail for the 16F1454 [here](https://github.com/jgeisler0303/ardpicprog#support-for-16f14545559-added-by-jgeisler0303) to learn how to use the programmer.

For a basic wiring instructions, please refer to the schematic on the [original project page](http://sky.geocities.jp/home_iwamoto/P16F145x/P16_L03.htm).
Generally, the following connections need to be made:
* USB D+ (green wire) to pin RA0
* USB D- (white wire) to pin RA1
* USB GND (black wire) to pin Vss (14) and to GND of the serial destination device
* RC5 (Rx) to Tx of serial destination device (optionally via a 220 Ohm resistor)
* RC4 (Tx) to Rx of serial destination device (optionally via a 220 Ohm resistor)
* A 0.1µF cap between Vdd (pin 1) and GND
* A 0.1 to 0.47µF cap between Vusb (pin 11) and GND

For a USB powered circuit (5V serial signals!)
* USB Vdd (red wire) to Vdd (and possibly to serial destination device). Optionally via a PTC self resetting fuse).

If your serial destination device runs on 3.3V, you either have to 
* power the PIC from the 3.3V supply of the destination device, 
* or you have to power the PIC (and possibly the destination device) with a separate voltage regulator (you should use an LDO, i.e. a low drop-out type if you want to power the regulator form USB Vdd),
* or you have to use level-shifters or voltage dividers on the Rx and TX lines.

Optionally:
* Status LEDs on RC0 and RC1 via current limiting resistors to ground
* DTR signal on RC3 to reset circuit of atmega with Arduino-bootloader (via level-shifter, if PIC and atmega don't run both on the same voltage)

## Making changes
If you are running Linux and have the [xc8 compiler](http://www.microchip.com/pagehandler/en_us/devtools/mplabxc/) installed, you should ba able to build the project yourself using make.
Otherwise you will have to install the [MPLAB.X IDE](http://www.microchip.com/pagehandler/en-us/family/mplabx/home.html) (the MPLAB.X directory is the IDE's project definition).

