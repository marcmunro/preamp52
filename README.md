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

Note that the Rpi hased hifiberry provides all of the control for the
pre-amp.  This includes switching from TV to hifi sources, managing
the relative volumes of the centre, front and rear channels, and
overall volume.

