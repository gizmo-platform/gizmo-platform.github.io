# Configuration

Before the FMS can be initialized, the system must be configured.
This step must be completed after the FMS system software has been
installed, since it must be completed from the FMS Workstation.

Prior to completing this step, obtain a CSV containing your team's
information.  The information should be in the following format:

```csv
Number,Name,Hub,Table
```

The headers should be as shown above.  The FMS will parse this file to
construct a record for each team.  To perform initial configuration,
run the configuration wizard by invoking `gizmo fms setup wizard`.
You will be asked a series of questions.

### `? Specify teams CSV file`

This question prompts for the CSV containing your team information as
listed above.  Either type the path to this file or press tab to show
files in the current directory.

### `? Loaded 21 teams, does this look right?`

The wizard will parse the file you provided and tell you how many
teams were in it.  If the number looks okay, answer yes, otherwise
answer no to exit the wizard.  If you need to exit the wizard, check
that the file is not malformed.

### `? Select the number of fields present`

Select between 1, 2, or 3 fields.  If you need to run more than 3
fields concurrently, contact the Gizmo team to receive further
instructions on how to configure this larger number of fields and
recommended hardware for hosting very large events.

### `? Input the MAC address for ether1 for field 1 (label on the bottom)`

> [!NOTE]
>
> This question will be repeated for each field that you've told the
> system you have.

Locate the Field Box that you will use for each field prompted.  On
the bottom of the device there is a label containing various
information.  Locate the line beginning with `E01:` which will be
followed by a MAC address.  Input the sequence of 12 hexadecimal
characters exactly as printed, including the colons.  This field is
case-insensitive.

### `? Read-only user password (username: gizmo-ro)`

In the unlikely event you need to log into the interface on the field
network directly this username will be available to you on the Scoring
Box and all Field Boxes.  The generated password is a good mix of
security and ease of use, so unless you have a need to change it,
accept the default.

### `? Make infrastructure network visible`

The infrastructure network is the network that operates on the 5Ghz
radio and is available for you to connect tablets, projectors, and
other assorted devices to.  It operates independent of any robot
channels.  In general making the network will improve ease of use, and
making it invisible does not improve security meaningfully.

The primary advantage of making the network invisible is to discourage
people from asking for access to it, and to prevent clutter from
showing up in WiFi menus.

### `? Infrastructure network SSID gizmo`

This is the SSID of the infrastructure network described above.  You
can freely change this to any value you wish as it is not used
internally by any of the FMS components.

### `? Infrastructure network PSK`

A password will be automatically generated for the infrastructure
network, though you can change it to any value you desire.  Note that
the password will not be obscured in this interface.

### `? MAC Address of the FMS`

The FMS has a pinned address pre-allocated for it by the Scoring Box
DHCP server.  In order to pin this address, the MAC address must be
known.  This option will be pre-populated with the correct value when
running the wizard from the FMS Workstation.  Do not change this value
unless you are using another machine to perform this configuration and
will subsequently transfer files to the Workstation (very advanced use
case).

### `? Configure really advanced network features`

Answer no to this question unless you have received specific
instructions otherwise from a member of the Gizmo development team.
