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
$ gizmo fms setup flash-device
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
2024-05-08T13:39:44.624-0500 [INFO]  flash-device: Writing configuration to file: path=/tmp/2069178931.rsc
2024-05-08T13:39:44.681-0500 [INFO]  flash-device: Version: 7.14.2(2024-03-27 08:33:46)
2024-05-08T13:40:05.432-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:05.623-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:05.823-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:06.023-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:06.223-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:06.423-0500 [INFO]  flash-device: client: 78:9A:18:C7:F8:16
2024-05-08T13:40:06.432-0500 [INFO]  flash-device: Sending and starting Netinstall boot image ...
2024-05-08T13:40:14.033-0500 [INFO]  flash-device: Installed branding package detected
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Will reset to default config
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Interface Mask: 255.255.255.0
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Using Client IP: 192.168.88.1
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Using Server IP: 192.168.88.2
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Starting PXE server
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Waiting for RouterBOARD...
2024-05-08T13:41:36.511-0500 [INFO]  flash-device: Discovered RouterBOARD...
2024-05-08T13:41:36.512-0500 [INFO]  flash-device: Formatting...
2024-05-08T13:41:36.512-0500 [INFO]  flash-device: Sending package routeros-7.14.2-mipsbe.npk ...
2024-05-08T13:41:36.512-0500 [INFO]  flash-device: Sending autorun script 2069178931.rsc ...
2024-05-08T13:41:36.512-0500 [INFO]  flash-device: Ready for reboot...
2024-05-08T13:41:36.512-0500 [INFO]  flash-device: Sent reboot command
2024-05-08T13:41:36.516-0500 [INFO]  flash-device: Flashing complete, you may now disconnect cables.
```

To install the software on Field Boxes, repeat the process as
described above, with the only change being the selection of the box
type.
