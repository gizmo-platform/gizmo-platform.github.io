# Configuration

> [!WARNING]
>
> The information on this page is largely historical.  The GSS file
> has been auto-generated for several driver's station releases now,
> and running without the use of the driver's station is not a
> supported workflow.

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
$ .\gizmo.exe configure
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
