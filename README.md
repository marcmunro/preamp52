# Pre-Amp 52

This is a hifi pre-amp that takes 5 channel surround sound and mixes
it down to stereo.

It is designed to be part of a hifiberry and chromecast TV based sound
system that looks something like this:



    +------------+
    |            |           +-----------+
    | Chromecast |           | hifiberry |--------+
    |            |           +-----------+        |
    +------------+                     | Stereo   |
          |                            +------+   |
          | hdmi                              |   |Control
         \|/                                  |   |
    +----------------------+  5-channel audio |   |
    | hdmi to analog audio |------+           |   |
    +----------------------+      |     +-----+   |
          |                       |     |         |
          | hdmi                  |     |         |
         \|/                     \|/   \|/       \|/
    +-----------+            +-------------------------+     
    |           |            |                         |
    |    TV     |            |       Pre-Amp 52        |
    |           |            |                         |
    +-----------+            +-------------------------+
                                 |               |  
                                 |               |  
                                 |               |  
                                \|/             \|/
                +----------------------+  +----------------------+
                |   Stereo Amplifier   |  |  Headphone Amplifier |
                +----------------------+  +----------------------+


The reason for doing all this, is that a lot of programming has poor
stereo mixing.  Being able to separately control the mix amplitude for
the centre channel in particular can greatly improve the listening
experience (you may even be able to understand some of the dialog).

Note that the Rpi based hifiberry provides all of the control for the
pre-amp.  This includes switching from TV to hifi sources, managing
the relative volumes of the centre, front and rear channels, and
overall volume.

## Contents

The `kicad` directory contains kicad6 files for the circuit.  A PDF of
the schematic is provided as `schematic.pdf`.  Note that there are 2
pages.

Gerber files for pcb generation can be found in `kicad/pcb`.

## Circuit Overview

### Input Selection and 5 Channel to 2 Downmixer

Stereo input, in my case from a Rasperry Pi-based hifiberry is
provided from J1 and J7 (in my case these are hard-soldered as the
hifiberry is in the same case as the preamp).  All inputs are
AC-coupled via 1μF capacitors.  Depending on your sources it may be
possible to use DC coupling and eliminate thses capacitors.

5-channel input is provided by J2 to J6.  J4, the centre input feeds 2
operational amplifiers configured as non-inverting buffer amplifiers.
These are present to eliminate cross-talk in the passive mixer stage
comprsising U2, U3 and U4, which are audio quality stereo digital
potentiometers and mix the 5 channel input into two channel stereo
with the centre channel being fed to both stereo channels.

This mixer stage allows independent attenuation of the front, centre
and rear channels.

A latching relay switches between the stereo input and the downmixed 5
channel input.

### Buffer Amplifier Stage

The pair of unity-gain non-inverting buffer amplifiers formed from U5
take the selected input and feed it to the tape (or in my case,
headphone) output and to the master volume control stage.

Using buffer amps here eliminates any impedence matching issues
between the input sources and the digital potentiometer U6.  

The OPA2134 op-amps are high quality audio components and should add
no noticable noise or distortion to the signal path.

### Master Volume Control Stage

Buffered signal inputs feed U6, another audio quality digital
potentiometer which provides the master volume control.  Output from
this is fed through a final unity-gain non-inverting buffer amplifier,
U7, which provides a low impedence output to drive an amplifier.  As
with the input stages, it may be possible to eliminate the decoupling
capacitors, depending on your setup.

### Power Supply

The power supply takes a single 12V supply and provides plus and minus
7V supplies to drive the opamps, and +7V for the digital
potentiometers.  The +5V needed by the digital potentiometers is
provided from the Raspberry Pi.

U8 is a DC-DC converter, that provides an isolated 12V supply from a
12V input.  This isolated supply, is used to provide -12V to R38, D4,
Q4 and R42 forming a primitive but good enough voltage regulator for
the -7V supply.  C21 further smooths this supply rail.

R39, D3, Q3 and R41 form another voltage regulator, this time on the
positive side.  C20 smooths this supply.

### Relay Drivers

R37 and Q1, and R40 and Q2, each provide relay drivers to switch K1
between the 5-channel and stereo inputs.  These are driven from
Raspberry Pi GPIO pins.  Note that as the relay is a latching type,
only a short pulse is required to switch the relay one way or the
other.

### Digital Potentiometer Controls

The 4 digital potentiometers that control the 5-channel downmixer and
volume, are all controlled from the I2C bus input from the Raspberry
Pi.

Each potentiometer has its own address on the bus with: U4, controlling
the centre channel, having address 0; U3, controlling the rear
channels, address 1; U2 controlling the front channels, address 2; and 
U6, the master volume, address 3.



