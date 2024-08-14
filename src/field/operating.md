# Operating the FMS

The field can be operated in of two ways:

  * Fully Manual - Team mapping is managed by a human operator who
    keys in each match when its time to change the field setup.

  * Remote Managed - The FMS is called by a remote system which
    maintains the schedule and inserts matches as required.

In either case, you first needs to start the FMS.

## Starting the FMS

Launch the FMS by running the following command, which will print out
some information on startup and then notify that its ready to serve.

```
$ gizmo fms run
2024-05-08T21:10:42.131-0500 [INFO]  fms: Log level: level=info
2024-05-08T21:10:42.138-0500 [INFO]  fms.mqtt: MQTT is starting
2024-05-08T21:10:42.138-0500 [INFO]  fms.web: HTTP is starting
2024-05-08T21:10:42.158-0500 [INFO]  fms.mqtt: Ready for connections
2024-05-08T21:10:42.159-0500 [INFO]  fms.tlm: Connected to broker
2024-05-08T21:10:42.159-0500 [INFO]  fms.metrics: Connected to broker
2024-05-08T21:10:42.165-0500 [INFO]  fms.metrics: Subscribed to topics
2024-05-08T21:10:42.166-0500 [INFO]  fms: Startup Complete!
```

## Manual Operation

To manually map a match, use the `gizmo fms remap` command to
configure a match.  The command will prompt sequentially for each
quadrant of each field.  If you wish to skip a field, enter a dash
(`-`).  If a team is currently on the field, that team will be offered
to you as a default that you can press enter to accept.

The remapping workflow will look similar to this:

```
$ gizmo fms remap
Enter new mapping
? field1:red 450
? field1:blue -
? field1:green -
? field1:yellow 472
```

This will place team #450 on the Red quadrant of field 1 and team #472
on the Yello quadrant of field 1.

At any time you can remap the teams that are active, but doing so will
disrupt communications to other Gizmos on the same field.  As long as
an active match is not in progress and no automation is remapping it
is safe to remap a match at any time, even if using automatic control.

## Automatic Operation

Automatic operation is available for some external software on an
as-requested basis.  If you have software you'd like to integrate in,
please reach out to the Gizmo team.

For any automation to interact with the FMS it must originate traffic
from either the FMS workstation itself, which is implicitly trusted,
or from a machine connected to the trusted field network (having an
address in `100.64.0.0/24`).  You can access this network by either
using an unmanaged workgroup switch connected to the FMS port, or
connecting to any unused field port on the scoring box.

> [!WARNING]
>
> Be careful what you plug into the FMS network.  The system ensures
> that traffic is coming from only expected locations and cannot hop
> networks, but you must still observe basic security measures such as
> not connecting untrusted devices to your scoring network.  Rule of
> thumb: if it doesn't have a good reason to be on the network it
> shouldn't be!

### BEST Robotics PCSM

To enable automatic operation of the FMS by PCSM, you will need to set
a system level environment variable that defines the location that the
PCSM application will use to contact the FMS.  In an administrative
PowerShell window (Run as Administrator), key the following command:


```shell
[System.Environment]::SetEnvironmentVariable('FieldManagementUrl', 'http://100.64.0.2:8080', 'Machine')
```

After setting this variable, you must reboot.  After rebooting, you
may confirm the variable has been persisted by running the following
in a non-administrative PowerShell:

```shell
[System.Environment]::GetEnvironmentVariable('FieldManagementUrl', 'Machine')
```

### BEST Robotics PiSM

The PiSM is similar to PCSM, but runs as a headless process.  When
running the process on Windows, use the method above to set the
environment variable.  In all other cases, export the variable
`FieldManagementUrl` in the shell where PiSM is running.
