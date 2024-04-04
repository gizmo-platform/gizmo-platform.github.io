# Firmware Installation

Before you can use the Gizmo system with a gamepad and drive a robot
around, it is necessary to install firmware, referred to as the Gizmo
System Software (GSS).

While there is a lot of information on this page, the entire process
should take you about 15 minutes given reasonable internet speed and a
modern computer.

## What is Firmware?

Firmware is a form of software.  Historically, firmware would have to
be installed at a factory or during final assembly of a system.  This
often involved high voltage programmers, powerful ultraviolet lights,
or even advanced photolithography techniques to indelibly etch the
program into the a physical structure on a devices memory chips.  As
you can imagine, having to have the firmware "burned in" made it a
high stakes operation since there was no possibility to update or fix
bugs after devices left the manufacturer's facilities.

As time has gone on, the term firmware has changed to mean any
software that is installed at the most basic layers of a device that
provides its core functionality.  Firmware in this sense is no
different than any other program, merely becoming yet another software
program installed onto a device.

The Gizmo is no different, and the System Processor requires firmware
to operate.  This firmware is developed by the Gizmo Team and is
written in C++ using the Arduino framework.  You can inspect this code
[here](https://github.com/gizmo-platform/firmware).  The firmware that
runs on the System Processor is responsible for interacting with the
wireless data link as well as providing control signals to the color
changing indicators, driving the safety watchdog, and supplying
information about gamepad and field state to the User Processor.

## Installing the Firmware

Installation is easy.  Connect the small end of a USB B-Micro cable to
the System Processor.  The System Processor located to the top left
edge of the Gizmo when viewed with the indicator lights on the left
and the connector blocks on the right.

Locate and hold down the bootsel button.  If you are using a Gizmo
without a case, you can use this graphic from the Raspberry Pi
Foundation:

![Location of a Bootsel Button](https://projects-static.raspberrypi.org/projects/getting-started-with-the-pico/5ebf38b3cdb484ab2185f695b89dd81d190516d1/en/images/Pico-bootsel.png)

If you are using a Gizmo inside a case, the button will be located in
the same place, but guarded by the case.  Depending on which version
of the various case designs you have selected to use, you may need an
unfolded paperclip to reach the button.

Once you have located the button, press and hold it while plugging the
other end of the USB cable into your computer.  Your computer will
detect the Gizmo in mass-storage (thumb drive) mode, and it will be
attached as a removable drive.

Obtain the latest firmware release from
[here](https://github.com/gizmo-platform/firmware/releases/).  You
need the `uf2` file from the release artifacts located at the bottom
of the card.

Simply drag-and-drop the `uf2` file from above onto the drive.  After
a few seconds, the drive will disconnect and the light on the System
Processor will begin to glow then flash.

## Bind the Gizmo to a Driver's Station

When you install firmware for the very first time, the Gizmo won't
know what driver's station its supposed to talk to.  The process of
connecting a Gizmo to a driver's station is referred to as binding,
and is a fully automated process.

With the driver's station powered on, connect the system processor to
any free USB port on the driver's station, and power on the Gizmo.  If
the Gizmo has never been bound before, the network and position status
lights will rapidly blink.  If the Gizmo has been paired before, you
can re-enter pairing mode by pressing the bootsel button for 2 seconds
after all the lights on the board have turned on.  The configuration
will be synchronized between the driver's station and the Gizmo, and
the Gizmo will reboot.

> [!NOTE]
>
> If you leave your finger on the bootsel button too long it will
> still be held down when the Gizmo reboots.  If this happens, the
> pattern of lights will stop changing in the status indicators.  Just
> unplug the USB cable and turn the Gizmo off and back on again to
> complete the reboot process.

After a few seconds, the Gizmo will connect to the Driver's Station
and begin communicating.
