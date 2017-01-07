# MIDI Accordion
Steps and code for building an Arduino-powered MIDI accordion (based off Dmitry Yegorenkov's AccordionMega project: https://github.com/accordion-mega/AccordionMega)



| Table of Contents      |
| ---------------------- |
| [Intro](#p0)           |
| [Overview](#p1)        |
| [Changelog](#p2)       |
| [Materials](#p3)       |
| [Software](#p4)        |
| [Quick Guide](#p5)     |
| [Links](#p6)           |

//TODO - reorganize all of this.  It should link to the detailed project page (and maybe have links to photos/videos), but this should only be a Quick Guide with the minimum amount needed to get the project working:

- demo videos
- brief intro
- changelog
- BoM
- software list
- schematics/code
- bare-boned instructions
- links

## <a name="p0"></a>Intro

The information and code on the AccordionMega project was invaluable to getting my MIDI accordion up and running, but as someone who has never worked on an electronics project before, the documentation was a bit bare-boned for my lack of experience and much of it was outdated (broken links, deprecated parts, etc.).  My hope with this project is to organize and break down each of the components necessary to build a MIDI Accordion so that any aspiring accordionist/musician/programmer/maker has all the information they need in one place to make it happen.

That being said, while it IS theoretically possible to just follow these instructions blindly, I highly recommend taking some time to research the ins and outs of each step of this project to gain a better understanding of how everything works in case you find yourself troubleshooting or fixing mistakes.  I've provided a set of links that I've used to help get my project up and running - if any of them are broken please let me know and I'll update the list accordingly.

## <a name="p1"></a>Overview

The goal of this project is to take a traditional piano accordion and turn it into a MIDI controller capable of playing music from your computer.  To do this requires solving a few different problems:

1. Sending a digital signal from the accordion keys and buttons to the computer
2. Converting that signal to MIDI
3. Playing that signal back with music software
4. Communicating that signal for each button on the accordion

The above goals are enough to have a functional MIDI instrument, but there are a few additional optional problems that can also be solved:

5. Communicating MIDI signals wirelessly (via Bluetooth)
6. Decoupling the accordion's power supply from the computer/wall
7. Adding dynamic expression using the accordion bellows (via barometric pressure sensor)

## <a name="p2"></a>Changelog

### v1.2.00 (1/2/2017)

- Added BMP180 support, allowing changes in pressure from moving bellows to change MIDI expression (i.e. volume)
   - Made the following changes from the original [AccordionMega](https://github.com/accordion-mega/AccordionMega) BMP085 code:
       - Updated the temperature and pressure reading code to use SparkFun's BMP180 library.
	   - Updated pressure<->velocity mapping algorithm according to various play tests.
       - Pressure is now read asynchronously from the Arduino - adds approximately 2ms delay when enabled (original added delay was about 3-5ms).

### v1.1.00 (12/28/2016)

- Added Bluetooth support (e.g. HC-05), allowing MIDI data to communicate wirelessly instead of over USB
    - Uses #DEFINE variable to toggle between using USB and additional serial port to reduce bandwidth usage

### v1.0.00 (12/09/2016)

- First working version of final project with all accordion keys mapped according to bvavra's PCB design.
- Made the following changes from the original [AccordionMega](https://github.com/accordion-mega/AccordionMega) code:
    - Updated files (and syntax) from .pde to .ino (new Arduino IDE)
    - Removed register shift code 
	    - Although it's a cool idea that could be realized on the accordion by adding some digital register buttons, I've decided to keep things simple and only transmit one register per button and control which registers sound via MIDI playback software (e.g. Logic, Reason, etc.)
    - Shaved off 2-3 ms of latency by reorganizing how the opto-interruptors are read.
    - Updated MIDI code to use Arduino MIDI Library
    - Added some extra comments to explain what some of the more obtuse lines are doing (port manipulation, bit operations, etc.)

## <a name="p3"></a>Materials

The final project requires the following materials and components:

//TODO - link to BoM (possibly embedded table?)

You will also need the following tools:

//TODO - link to or list tools used

## <a name="p4"></a>Software

### Project Source

The only files required for the final project are the Arduino files located in the root level [MIDI_Accordion directory](https://github.com/bvavra/MIDI_Accordion/tree/master/MIDI_Accordion).  Everything else in this repo is supplemental information to help research and build the project (e.g. Prototypes, Datasheets, Design docs, etc.).  Of course, you will need the [Arduino IDE](https://www.arduino.cc/en/main/software) to write and upload the project source code to your Arduino.

### Arduino Libraries

The following libraries are required to compile the MIDI_Accordion Arduino sketch:

- [Arduino MIDI Library](http://playground.arduino.cc/Main/MIDILibrary)
    - Used to simplify sending data via the MIDI protocol.
- [Sparkfun's Arduino BMP180 Library](https://github.com/sparkfun/BMP180_Breakout)
    - Used to interface with the BMP180 barometric pressure sensor.

### Third Party

To play your finished MIDI Accordion you'll need the following additional software:

- Serial-to-MIDI converter
    - Used to convert data coming from the Arduino from Serial to MIDI.  I recommend [Hairless MIDI <-> Serial Bridge](http://projectgus.github.io/hairless-midiserial/).
- Virtual MIDI port
    - Used to connect the MIDI data to playback software.  
	- [OSX configuration instructions](http://feelyoursound.com/setup-midi-os-x/)
	- [Linux (Debian/Ubuntu) config instructions](https://ubuntuforums.org/showthread.php?t=1445186)
	- Windows requires 3rd party software - I recommend [LoopMIDI](http://www.tobias-erichsen.de/software/loopmidi.html).
- MIDI controller/sequencer/player
    - Used to play back MIDI data triggered by the accordion.  It can be whatever your preferred MIDI playback software is (e.g. Logic, Reason, etc.), but if you don't have one I recommend using [Virtual MIDI Piano Keyboard (VMPK)](http://vmpk.sourceforge.net/), which is free and Windows/OSX/Linux compatible.
- (Optional) [ASIO4All](http://www.asio4all.com/)
    - Used to reduce latency when playing back MIDI on Windows machines without ASIO audio drivers.

## <a name="p5"></a>Quick Guide

This is meant to be a bare-bones guide to building your own MIDI accordion, providing only a general roadmap for those who already know what they're doing or for those who are curious about the process, but only want a tl;dr.  A complete guide is in the works and will be made available soon.

[MIDI Accordion Photo Gallery](http://imgur.com/a/U7L83)

//TODO - fill in outline steps

## <a name="p6"></a>Links

The following links were helpful in making this project happen.  If any of these links are broken, or if you have any additional links that you found helpful, please contact me and let me know so I can update this list accordingly.

### Accordion

- [Lots of accordion repair resources](http://www.accordionrevival.com/Home.php)
- [Taking apart the Stradella bass system](http://paisanoaccordions.blogspot.com/2009/09/not-for-faint-of-heart.html)
- [Taking apart the piano keyboard](http://www.instructables.com/id/How-to-fix-an-accordion-keyboard/)
- [Accordion repair videos](http://www.ehow.com/videos-on_2531_apart-accordion.html)
- [Accoridon repair tools](http://accordionplus.com/tools.html)

### Arduino

- [Internal pullup resistor tutorial](https://www.arduino.cc/en/Tutorial/InputPullupSerial)
- [Info about switches](http://www.gammon.com.au/switches)
- [Arduino port manipulation](https://www.arduino.cc/en/Reference/PortManipulation)
- [Port manipulation tutorial](http://www.instructables.com/id/Arduino-is-Slow-and-how-to-fix-it/?ALLSTEPS)
- [Another port manipulation tutorial](http://www.instructables.com/id/Fast-digitalRead-digitalWrite-for-Arduino/?ALLSTEPS)
- [Organizing Arduino code with tabs](https://liudr.wordpress.com/2011/02/16/using-tabs-in-arduino-ide/)
- [Arduino baud rates](http://arduino.stackexchange.com/questions/296/how-high-of-a-baud-rate-can-i-go-without-errors)

### MIDI

- [MIDI Protocol essentials](https://ccrma.stanford.edu/~craig/articles/linuxmidi/misc/essenmidi.html)
- [Arduino MIDI Library docs](http://arduinomidilib.fortyseveneffects.com/index.html)
- [When does audio latency matter and not matter?](http://music.stackexchange.com/questions/30323/when-does-audio-latency-matter-and-not-matter)
- [MIDI CC List](http://nickfever.com/music/midi-cc-list)
- [Send and receive MIDI with Arduino](http://www.instructables.com/id/Send-and-Receive-MIDI-with-Arduino/?ALLSTEPS)
- [Arduino MIDI Expression Pedal](https://www.codeproject.com/articles/38203/arduino-based-midi-expression-pedal)
- [Latency problem using MS GS Wavetable Synth](https://answers.microsoft.com/en-us/windows/forum/windows_7-pictures/latency-problem-using-ms-gs-wavetable-synth/1e85704a-803c-438f-b472-fb0cb5211be4)
- [Reducing latency on MIDI-over-USB keyboard](http://sound.stackexchange.com/questions/27583/reducing-latency-on-midi-over-usb-keyboard)
- [Difference between MIDI velocity and Expression](http://www.sweetwater.com/insync/what-difference-between-midi-volume-expression/)
- ["Unexpected data byte" error when sending CC data over Hairless MIDI](https://github.com/projectgus/hairless-midiserial/issues/16)

### Opto-Interruptors

- [ITR-9608 Datasheet](http://www.electrodragon.com/w/images/6/60/ITR9608.pdf)
- [How To Set Up A Photo Interrupter (or Slotted Optical) Switch On The Arduino](http://www.utopiamechanicus.com/article/arduino-photo-interruptor-slotted-optical-switch/)
- [Connecting a photo interrupter/optoisolator to an Arduino](http://www.martyncurrey.com/connecting-an-photo-interrupter-to-an-arduino/)

### Bluetooth

- [Cheap 2-Way Bluetooth Connection Between Arduino and PC](http://www.instructables.com/id/Cheap-2-Way-Bluetooth-Connection-Between-Arduino-a/?ALLSTEPS)
- [Modify The HC-05 Bluetooth Module Defaults Using AT Commands](http://www.instructables.com/id/Modify-The-HC-05-Bluetooth-Module-Defaults-Using-A/?ALLSTEPS)
- [Arduino with HC-05 Bluetooth module - AT MODE](http://www.martyncurrey.com/arduino-with-hc-05-bluetooth-module-at-mode/)

### Barometric Pressure Sensor

- [BMP180 Barometric Pressure Sensor Hookup](https://learn.sparkfun.com/tutorials/bmp180-barometric-pressure-sensor-hookup-)

### Power

- [Feeding power to Arduino: the ultimate guide](http://www.open-electronics.org/the-power-of-arduino-this-unknown/)
- [Arduino power sharing](http://electronics.stackexchange.com/questions/36947/arduino-power-sharing)
- [9V is not a good power source](http://cybergibbons.com/uncategorized/arduino-misconceptions-6-a-9v-battery-is-a-good-power-source/)
- [Arduino power switch](http://www.instructables.com/id/Powering-Arduino-with-a-Battery/)
- [Is it bad practice to provide power through the VIN Pin?](https://www.reddit.com/r/arduino/comments/2jdyba/is_it_bad_practice_to_provide_power_through_the/)
- [Powering an Arduino Mega with external USB Power Bank?](http://arduino.stackexchange.com/questions/9069/powering-an-arduino-mega-with-external-usb-power-bank)

### Wiring

- [Soldering 101 (adafruit)](https://learn.adafruit.com/lets-put-leds-in-things/soldering)
- [How to solder electronics (wikiHow)](http://www.wikihow.com/Solder-Electronics)
- [How to solder (instructables)](http://www.instructables.com/id/How-to-solder/?ALLSTEPS)
- [Basic de-soldering guide](http://www.epemag.wimborne.co.uk/desolderpix.htm)
- [Custom Cables & Guide to Crimping Dupont PCB Interconnect Cables](https://www.youtube.com/watch?v=GkbOJSvhCgU)

### Electronics

- [How to read a schematic](https://learn.sparkfun.com/tutorials/how-to-read-a-schematic)
- [Understanding the functions of a multimeter](https://learn.adafruit.com/multimeters/)
- [Light-Emitting Diodes (LEDs)](https://learn.sparkfun.com/tutorials/light-emitting-diodes-leds)
- [How to test diodes using a digital multimeter](http://en-us.fluke.com/training/training-library/test-tools/digital-multimeters/how-to-test-diodes-using-a-digital-multimeter.html)
- [Why exactly can't a single resistor be used for many parallel LEDs?](http://electronics.stackexchange.com/questions/22291/why-exactly-cant-a-single-resistor-be-used-for-many-parallel-leds)
- [8 Tips On How To Pick A Resistor For An Arduino (Circuit)](http://www.utopiamechanicus.com/article/arduino-resistor-selection/)
- [Voltage Dividers tutorial](https://learn.sparkfun.com/tutorials/voltage-dividers)
- [Transistors tutorial](https://learn.sparkfun.com/tutorials/transistors)
- [How transistors work](http://www.build-electronic-circuits.com/how-transistors-work/)