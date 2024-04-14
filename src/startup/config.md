# Configuration

Certain parts of the Gizmo system depend on information that cannot be
inferred.  To provide this information to the system, you must
generate a configuration file that contains the relevant settings.
The `gizmo` tool provides a guided walkthrough to generate these
settings.

The file that this process results in will be named `gsscfg.json`
which you may see referred to throughout this documentation as the
Gizmo System Software configuration file.  This file is used to
specify certain critical parameters on both the System Processor and
the Driver Station.

## Generating the Config File

In a terminal window, execute the following command:

```
$ .\gizmo.exe firmware configure
```

You will be asked a series of questions.  Each question is described
in detail below.  Note that depending on your answers to some
questions, later questions may be skipped when they are not required.

### `? Team Number`

What team number should be associated with this Gizmo?  The Gizmo uses
the team number in various places to pair it with a driver's console,
to identify itself to a field, and to seperate metrics out when
multiple Gizmo devices are operating in concert such as a scrimmage or
a competition.  Your number should be an integer value below 9999.

### `? Use the driver's station`

The driver's station is a dedicated hardware device which removes the
requirement to use a seperate computer to interact with the Gizmo.
When using the driver's station, almost all setup is automatic, so all
other questions will be skipped.


### `? Use external network controller`

When using the driver's console, a secure point to point connection
will be made between the driver's console and the Gizmo.  This
connection is suitable to practice with and has the same performance
characteristics of the full field radio and communications system,
however it is not as powerful as the field radio.  If you want to use
an external radio, answer yes to this question.

Be aware that using an external network controller will involve
configuring an 802.11 switched network with appropriate security
controls and is an advanced topic.  We do not recommend this option
for most users and its existence is primarily to support particularly
advanced users or cases where the Gizmo team is performing more
advanced diagnostics than can be performed using the self-contained
wireless link.

> [!WARNING]
>
> The Gizmo makes use of a real-time communications protocol to ensure
> latency free and safe operation of any systems connected to it.
> Since disruption of the connection between the controller and the
> Gizmo may represent an out-of-control machine, its important to
> ensure that the entire control path is dedicated to this purpose.
>
> Do not connect the Gizmo to a school or workplace WiFi network!
>
> Prior to initializing your own network controller, reach out to your
> IT and explain to them what its for.  In short your IT will want to
> know the following:
>
>   * The network uses a non-broadcasting SSID.
>   * The network uses a scrambled 32-character ESSID and PSK.
>   * The network will be an island (not connected to any other wired
>     or wireless uplinks).
>   * The network will only be used intermittently while controlling a
>     Gizmo, and will be powered down unless in use.

### `? Network SSID` and `? Network PSK`

These questions will only be shown if you selected to use an external
network controller.  Enter your SSID and PSK in these fields.  To
generate these values, we recommend using the password generator from
[this
site](https://www.random.org/passwords/?num=2&len=32&format=html&rnd=new).
Use one value as the SSID and one value as the WPA2-PSK.  Remember to
configure your network as a non-broadcasting network, and do not join
any devices beyond the Gizmo and the computer hosting the `gizmo` tool
which will be used to drive.

### `? Address of the field server`

It is recommended that this be a static address, but this is not
strictly required (you will, however, have to update your Gizmo if
this address is changed).  If you network supports
Avahi/mDNS/ZeroConf, you may enter `gizmo<team>.local` here and the
Gizmo will find your controller using mDNS.

By default, the address of the machine you are running the `gizmo`
tool on will be pre-populated for you.  If you wish to use another
address, simply type the one you want.
