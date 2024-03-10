# Firmware Installation

Before you can use the Gizmo system with a gamepad and drive a robot
around, it is necessary to install firmware, referred to as the Gizmo
System Software (GSS).

While there is a lot of information on this page, the entire process
should take you about 15 minutes given reasonable internet speed and a
modern computer.

## What is Firmware?

Firmware is a form of software.  Historically, firmware would have to
be installed at a factory or during final assembly of a system.  This
often involved high voltage programmers, powerful ultraviolet lights,
or even advanced photolithography techniques to indelibly etch the
program into the a physical structure on a devices memory chips.  As
you can imagine, having to have the firmware "burned in" made it a
high stakes operation since there was no possibility to update or fix
bugs after devices left the manufacturer's facilities.

As time has gone on, the term firmware has changed to mean any
software that is installed at the most basic layers of a device that
provides its core functionality.  Firmware in this sense is no
different than any other program, merely becoming yet another software
program installed onto a device.

The Gizmo is no different, and the System Processor requires firmware
to operate.  This firmware is developed by the Gizmo Team and is
written in C++ using the Arduino framework.  You can inspect this code
[here](https://github.com/gizmo-platform/firmware).  The firmware that
runs on the System Processor is responsible for interacting with the
wireless data link as well as providing control signals to the color
changing indicators, driving the safety watchdog, and supplying
information about gamepad and field state to the User Processor.

# Automatic Installation

Firmware can be installed automatically if you have an up to date
driver's console.  This workflow is still in development, and this
section will be updated once the automated experience is finalized.

# Manaul Installation

Installation of the firwmare is comprised of 3 steps.  First we must
configure the firmware, then we must compile it, finally we must
install it.  The first two steps are managed by the `gizmo` tool.

Note that this process is only required if you choose not to use the
automatic firmware installation process described above.

## Configuration

Configuring the firmware generates a configuration file that is used
to tell the Gizmo what team it belongs to, which is how the Gizmo
board knows what data streams to subscribe to from the field
controller.  We can configure the Gizmo firmware using the `gizmo` tool.

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

### `? Use the driver's console`

The driver's console is a dedicated hardware device which removes the
requirement to use a seperate computer to interact with the Gizmo.
When using the driver's console, almost all setup is automatic, so all
other questions will be skipped.

### `? Use Avahi (mDNS)`

Avahi, also known as Bonjour, ZeroConf, and Multicast DNS is a
technology that allows you to use DNS names to refer to components of
a network without needing a DNS server.  Selecting yes to this prompt
will suppress questions about IP addressing in later prompts.

Note that Avahi is usually disabled on large scale networks, so this
may not work correctly when using a non-dedicated network controller.

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

This question will only be shown if you selected not to use Avahi
(mDNS).  When not using mDNS, you will need to provide an IP address
that the Gizmo can communicate with for control information.  It is
recommended that this be a static address, but this is not strictly
required (you will, however, have to update your Gizmo if this address
is changed which is why Avahi is recommended).

By default, the address of the machine you are running the `gizmo`
tool on will be pre-populated for you.  If you wish to use another
address, simply type the one you want.

## Build the Firmware

Once you have completed the configuration steps, its time to build the
firmware image.  A firmware image is a file that contains the program
code in a format that can be installed onto the System Processor.
This file is the result of compiling the source code combined with the
configuration values you entered during the configuration phase above.

The Gizmo tool automated this process for you, though you must have
completed the Arduino IDE or Arduino CLI installation as detailed in
the [prerequisites](prereq.md#installing-the-arduino-ide).

To build the firmware, execute the following command:

```
$ .\gizmo.exe firmware build
```

This command can take a while depending on the speed of your computer,
but you should see output that looks similar to this:

```
2024-03-10T18:26:13.954-0500 [INFO]  firmware: Log level: level=info
2024-03-10T18:26:13.954-0500 [INFO]  firmware.factory: Performing Step: team=1234 step=Unpack
2024-03-10T18:26:13.954-0500 [INFO]  firmware.factory: Performing Step: team=1234 step=Configure
2024-03-10T18:26:13.954-0500 [INFO]  firmware.factory: Performing Step: team=1234 step=Compile
2024-03-10T18:26:32.956-0500 [INFO]  firmware.factory: Performing Step: team=1234 step=Export
2024-03-10T18:26:32.957-0500 [INFO]  firmware.factory: Performing Step: team=1234 step=Cleanup
```

When the command completes, you'll have a `uf2` file in the directory
where you ran it.  This file will be called `gss_<number>.uf2` where
`<number>` is the team number you entered during configuration.

## Installing the Firmware

Installation is easy.  Connect the small end of a USB B-Micro cable to
the System Processor.  The System Processor located to the top left
edge of the Gizmo when viewed with the indicator lights on the left
and the connector blocks on the right.

Locate and hold down the bootsel button.  If you are using a Gizmo
without a case, you can use this graphic from the Raspberry Pi
Foundation:

![Location of a Bootsel Button](https://projects-static.raspberrypi.org/projects/getting-started-with-the-pico/5ebf38b3cdb484ab2185f695b89dd81d190516d1/en/images/Pico-bootsel.png)

If you are using a Gizmo inside a case, the button will be located in
the same place, but guarded by the case.  Depending on which version
of the various case designs you have selected to use, you may need an
unfolded paperclip to reach the button.

Once you have located the button, press and hold it while plugging the
other end of the USB cable into your computer.  Your computer will
detect the Gizmo in mass-storage (thumb drive) mode, and it will be
attached as a removable drive.  Simply drag-and-drop the `uf2` file
from above onto the drive.  After a few seconds, the drive will
disconnect and the light on the System Processor will begin to glow
then flash.

As soon as the drive disappears, you may remove the USB cable, the
firmware has been installed.
