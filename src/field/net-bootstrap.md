# Bootstrap Network

After installing software on all of your Scoring and Field boxes, its
time to connect them all together and perform network initialization.
This process should take about 15 minutes.

You will need:

  * Provisioned Scoring Box.
  * One or more provisioned Field Boxes - 1 per field.
  * Scoring Box power supply.
  * Ethernet cable to go between FMS Workstation and Scoring Box.
  * Ethernet cables to go between the Scoring Box and each field.

Start with the FMS Workstation connected to port 2 of the Scoring Box.
You may connect your WAN connection to port 1 if you have it
available, but it is not required at this time.  Ensure that the FMS
Workstation has a WiFi connection with internet available.

Connect power to the Scoring box and wait approximately 2 minutes for
it to finish initialization.  Do not connect any Field Boxes yet.
Begin the bootstrap procedure with the following command:

```
$ gizmo fms net bootstrap
```

You will be presented with a message summarizing most of the above
information:

```
You are about to complete out of box provisioning for your field.
Prior to this point, you should have used the flash-device command to
install the most recent qualified system image to your scoring box and
field box or boxes.  Begin the process with all devices powered off.

Connect the scoring table box's second port (the FMS port) directly to
the FMS workstation (this computer).  Connect no other cables or
devices.

Power on the scoring table box and wait approximately 30 seconds for
it to boot.  Once the device has booted (pattern of lights has
stabilized), confirm this dialog and the scoring table box will be
programmed.  You will receive more instructions on when to connect
field boxes after the main scoring box provisioning completes.
? Acknowledge and Proceed (y/N)
```

You will see a large amount of text scroll by that provides detailed
information on which resources have been provisioned and which
resoures are being actively configured.  After some time, the
provisioning workflow will pause and prompt you to connect the cables
to your Field Boxes:

```
The scoring box has been successfully programmed for your event.
Connect your field boxes to ports 3-5 on the scoring box at this time.
If you are not using a PoE enabled scoring box, connect power to your
field boxes at this time.

Once connected, wait approximately 30 seconds for your field boxes to
finish booting (pattern of lights has stabilized) and then confirm
this dialog.  You will see some error messages printed as the initial
configuration is programmed, this is normal.

This process can take up to 10 minutes to complete.

? Acknowledge and Proceed (y/N)
```

If you have the PoE version of the Scoring Box, connecting only the
network cables will be sufficient.  Cables should be installed between
port 1 of each Field box to ports 3-5 of the Scoring Box.  Power will
be supplied over the network cables and you should see the lights come
on with each Field Box.  If you are not using the PoE version of the
Scoring Box, you will need to additionally connect the Field Box power
cables.

After connecting cables, wait approximately 2 minutes for the devices
to power and and complete initialization before proceeding.  Once you
proceed the process will continue automatically until the Field boxes
are fully provisioned.

After all provisioning has completed, you will receive the following
message:

```
$ 2024-05-08T16:44:54.077-0500 [INFO]  bootstrap-net: Provisioning Complete
```

The final step is to run the following command which will resolve a
handful of items that are set to different values during
bootstrapping:

```
$ gizmo fms net reconcile
```

The command will run for approximately 2 minutes and validate your
entire network configuration.  If at any time you think your network
configuration may have become corrupt, you can re-run the `reconcile`
command to validate the entire configuration end-to-end.
