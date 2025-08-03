# Connecting Gizmos

Before a device powered by a Gizmo can be used on a field, it must be
bound to the FMS.  This process is extremely similar to the process of
binding a Gizmo to the Driver's Station in that it uses a USB cable to
the System Processor and a 2 second press on the System Processor's
BOOTSEL button to initiate binding.  The primary difference is that
when binding to the FMS you will be prompted to identify which team
the Gizmo being bound belongs to.

## `gizmo fms config-server`

The `gizmo` tool provides a command for rapidly binding multiple
Gizmos to the FMS.  For this reason, it is recommended that you
perform binding from the FMS workstation.

Launch the tool by running `sudo -u _gizmo -g dialout gizmo fms
config-server` which will initialize and then wait for a Gizmo to
request configuration.  Connect a USB cable from the FMS workstation
to the System Processor on the Gizmo to be bound, then press and hold
the BOOTSEL button for 2 seconds.


You will see output similar to the following:

```
2024-05-08T22:10:56.258-0500 [INFO]  config-server: Found a port!: port=/dev/ttyACM1
? Select configuration to bind to this Gizmo  [Use arrows to move, type to filter]
> team470
  team451
  team459
  team461
  team465
  team460
  team463
```

A list of all teams that the FMS is aware of will be presented.  You
can either arrow through this list or type the first few characters of
the team name you wish to select.  Pressing enter will confirm your
selection and upload the desired configuration to the connected Gizmo.

> [!TIP]
>
> You can call the config-server with `--oneshot` to have it exit
> after each one.  This may be more intuitive to some users and is
> also easier to avoid accidentally double flashing a Gizmo.
