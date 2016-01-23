# Waveshare 7" HDMI Display linux touchscreen driver

A pretty simple patch to add single touch support for usb device 0eef:0005 to linux kernel.

In theory the device supports multitouch, but I don't feel like writing a driver for it :)

## Installation

Patch is currently only tested with kernel 4.1.17 (raspberry pi).

Remember to blacklist this device from usbhid, eg. add 
```usbhid.quirks=0xeef:0x5:0x4``` to kernel commandline. Otherwise
this driver cannot find the touchscreen and things wont work.

To run with xorg, use evdev driver. Some configuration is also needed,
please see below.

/etc/X11/xorg.conf.d/99-calibration.conf
```
Section "InputClass"
        Driver "evdev"
        Identifier      "touchscreen"
        Option "Device" "/dev/input/event0"
        Option "Calibration"   "4 806 -13 488"
EndSection

Section "InputDevice"
    Identifier "dummy"
    Driver "void"
    Option "Device" "/dev/input/mice"
EndSection
```

The first section configures the touch area for the touchscreen, and the second
section disabled the normal mouse (if necessary). Second section is optional.

If this configuration does not work for you, use xinput_calibrate tool to set
up calibration coordinates yourself, eg:

```DISPLAY=:0 xinput_calibrate```

That's it, enjoy!

## License

GPLv2. Please see LICENSE for details.

