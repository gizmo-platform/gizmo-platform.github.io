# Operating the FMS

The field can be operated in of two ways:

  * Fully Manual - Team mapping is managed by a human operator who
    keys in each match when its time to change the field setup.

  * Remote Managed - The FMS is called by a remote system which
    maintains the schedule and inserts matches as required.

In either case, you first needs to start the FMS.

## Checking the FMS is Running

The FMS runs as a system process.  You can check that it is running by
executing the following command:

```
$ sudo sv status gizmo-fms
```

If the FMS is running, you should see the following output:

```
$ sudo sv status gizmo-fms
run: gizmo-fms: (pid 3709) 191s
```

The PID and number of seconds displayed will be different, the
important item is the `run` state from the start of the line.  If you
believe the FMS is not operating correctly, you can restart it with
the following command, which will have the output shown below:

```
$ sudo sv restart gizmo-fms
ok: run: gizmo-fms: (pid 3805) 1s
```

## Manual Operation

To manually map a match, use the `gizmo fms remap` command to
configure a match.  The command will prompt sequentially for each
quadrant of each field.  If you wish to skip a field, enter a dash
(`-`).  If a team is currently on the field, that team will be offered
to you as a default that you can press enter to accept.

Since this is an authenticated administrative operation on the FMS,
you'll need to authenticate.  Gizmo CLI tools expect to retrieve this
information from the environment variables `GIZMO_FMS_USER` and
`GIZMO_FMS_PASS`.  Set these variables with the following commands:

```
$ export GIZMO_FMS_USER=<username here>
$ export GIZMO_FMS_PASS=<password here>
```

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

Automatic operations are available via various
[integrations](../integrations/).
