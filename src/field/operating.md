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
