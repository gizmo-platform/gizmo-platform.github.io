# Hardware Preparation

Having completed the initial configuration wizard, you are now ready
to install the network device operating software.  This is an
automated process which takes about 5 minutes per device you need to
install.  At minimum, you will need to install the software on one
Scoring Box and at least one Field Box.  If you have more than one
field, you'll need to repeat this process for each Field Box.

Before you begin, make sure you have the following:

  * The Scoring and Field boxes.
  * A paperclip or other suitable instrument.
  * A network cable to connect the FMS Workstation to the device being
    provisioned.
  * The power supply for the Scoring Box - this PSU is interchangeable
    with all other boxes.

Conventionally, start by installing the software on the Scoring Box.
Do this by connecting port 1 of the Scoring Box to the FMS workstation
directly using an Ethernet cable.  Do not connect power to the Scoring
Box.  Once you have done this, launch the `flash-device` utility.  You
will see the following message:

```
$ sudo -u _gizmo gizmo fms setup flash-device
Welcome to the Device Flash utility.

This process will guide you through the process of installing the most
recently confirmed working firmware on your field device.

Before you begin, you should ensure that you have the field device, an
unfolded paperclip or similar instrument, and a cable to connect port
1 of the field device (says 'Internet'), and the FMS workstation (this
computer).
? Select the type of device you are flashing Scoring Table Box
? After you confirm this message, hold down the reset button using the
paperclip and connect power.  You may remove the paperclip after you
see a message containing the phrase 'client'.

Ready to proceed (y/N)
```

Notice that the first prompt asks which type of box is being
provisioned, and in the above example the `Scoring Table Box` option
was selected.

After you press `Y` and confirm your selection, press and hold the
reset button on the Scoring Box and then connect power.  The `USR`
light will blink approximately 12 times and then go out.  Once the
light goes out you may release the reset button and you should see the
software installation process start.  In the unlikely event that the
software installation process fails for any reason, it is safe to
restart.

The installation process will look similar to this:

```
2025-08-03T16:25:37.549-0500 [INFO]  flash-device: Writing configuration to file: path=/tmp/3206896280.rsc
2025-08-03T16:25:37.567-0500 [INFO]  flash-device: Version: 7.17(2025-01-16 09:26:27)
2025-08-03T16:25:37.570-0500 [INFO]  flash-device: Will reset to default config
2025-08-03T16:25:37.570-0500 [INFO]  flash-device: Will install devices only once
2025-08-03T16:25:37.584-0500 [INFO]  flash-device: Using interface eth0
2025-08-03T16:25:37.584-0500 [INFO]  flash-device: Using interface eth0
2025-08-03T16:25:37.584-0500 [INFO]  flash-device: Waiting for Link-UP on eth0
2025-08-03T16:26:18.591-0500 [INFO]  flash-device: Waiting for RouterBOARD...
2025-08-03T16:26:18.740-0500 [INFO]  flash-device: Assigned 192.168.88.1 to 78:9A:18:D3:24:C4
2025-08-03T16:26:20.385-0500 [INFO]  flash-device: Booting device 78:9A:18:D3:24:C4 into setup mode
2025-08-03T16:27:05.789-0500 [INFO]  flash-device: Formatting device 78:9A:18:D3:24:C4
2025-08-03T16:27:31.788-0500 [INFO]  flash-device: Sending packages to device 78:9A:18:D3:24:C4
2025-08-03T16:27:35.844-0500 [INFO]  flash-device: Packages and configuration script sent to device 78:9A:18:D3:24:C4
2025-08-03T16:27:35.899-0500 [INFO]  flash-device: Rebooting device 78:9A:18:D3:24:C4
2025-08-03T16:27:35.899-0500 [INFO]  flash-device: Successfully finished installing the device with MAC address 78:9A:
2025-08-03T16:27:35.900-0500 [INFO]  flash-device: Flashing complete, you may now disconnect cables.
```

To install the software on Field Boxes, repeat the process as
described above, with the only change being the selection of the box
type.
