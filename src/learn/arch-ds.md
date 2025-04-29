# Driver's Station

The Driver's Station is a component that provides the termination
point of the USB data stream from the gamepad and is the point at
which control messages are inserted to the UDP data stream.  The
driver's station allows the platform to be self contained by providing
all of the services that the full fledged FMS does when not connected.

The Driver's Station is implemented as a command that is part of the
Gizmo tool.  Because the Driver's Station is highly specific to the
underlying Linux APIs that interact with the network stack, it is only
compiled for the Linux architecture.  This is why if you download the
`gizmo` tool on macOS or Windows, you won't have an option to run the
`ds` commands.

Internal to the driver's station system image, there are several
processes that are running.  We'll look at each one in detail.

## `hostapd`

This process manages the wireless radio.  Its considered a Swiss Army
Knife of radio management, and is what powers most commercial access
points under the hood.  On the driver's station, it takes care of
managing the point-to-point network that gets created when not using a
field management system.  Its managed and supervised by other parts of
the system image and is stopped when the FMS is connected, which has
the side effect of shutting down the local radio in the driver's
station.  This is important to reduce interference by having as few
transmitting radios powered on as possible.

## `gizmo-ds`

This is the main driver's station process.  It is the result of the
`gizmo ds run` command.  It takes care of polling the gamepad,
connecting to the Field Management System (FMS) and providing
localized field services when the FMS is not available, such as during
a practice scenario or during development of your machine.

## `gizmo-link`

The `gizmo ds linkmon` process runs to supervise the network.  On the
driver's station, the wireless and wired interfaces are part of a
bridge device.  You can learn more about bridge devices in the [Kernel
Wiki](https://wiki.linuxfoundation.org/networking/bridge).  The
practical upshot of this is that from the perspective of software on
the driver's station the connection medium (Ethernet or WiFi) doesn't
actually matter, only the address matters and there is exactly one of
those since its held by the bridge.

The `linkmon` process also watches what's going on with the Ethernet
interface and if it sees the link status change, it restarts
`gizmo-ds` so that any control streams are forcibly re-established.

## `gizmo-config`

The `gizmo ds config-server` process provides a serial configuration
server that is part of the pairing process with the Gizmo.  This
enables the driver's station to provide its `gsscfg.json` to any
connected Gizmo that's in binding mode.  The `config-server` checks
every 5 seconds to see if a USB device was connected that identifies
itself as being a Raspberry Pi Pico and is pretending to be a serial
port.  If its a serial port, the `config-server` opens the port and
waits to see if it receives the string `GIZMO_REQUEST_CONFIG`.  Once
this string is received, the configuration data is sent back across
the serial link and the port is closed.

> [!NOTE]
>
> Actually before the port is closed, it has to be "drained".  This is
> a limitation of the way that serial ports and specifically UARTs
> work, which means that we need to write data into the port and then
> wait until the port says its written the same number of bytes that
> were dumped into it, meaning that all data has been sent.

## `gizmo-logmon`

The `gizmo-logmon` process is responsible for copying the log file to
the HDMI port, so that you can plug in a monitor to see the log scroll
in real-time.  This is an advanced debugging feature, but may be
useful for some users.

## `console`

The `console` service is an advanced debugging feature that allows
access to the running system on the driver's station.  To access it,
connect a monitor and keyboard and press `ctrl + alt + f2`.  Keep in
mind that the Gizmo DS boots from read-only storage, so changes you
make while in the console won't be saved.

## `dnsmasq`

While the Gizmo and the Driver's Station have statically defined
addresses, not all devicse do.  The `dnsmasq` service provides DHCP to
devices connected to the Driver's Station's Ethernet port.  This is
useful to plug in a laptop and review the metrics data from the
Driver's Station when not using the FMS.

# Boot Time Configuration

All of the services we've talked about so far run as long-lived
processes once the embedded operating system on the Driver's Station
finishes initialization.  During late-boot, however, there is a `gizmo
ds configure` process that runs as a one-shot process which performs
initialization and exits.

The `configure` process is responsible for several things.  Primarily,
it sets up all the services that are mentioned above, and it
configures the network devices.  These steps happen once per boot, and
are what allows the driver's station system image to be completely
identical for all machines with the only distinction being the
`gsscfg.json` file.  This file is read from the boot partition to
setup all the other services.

The `gsscfg.json` file can be statically provisioned to the `/boot`
directory, however this is not recommended.  Just before `gizmo ds
configure` runs, the autoconfiguration phase will occur.  If the
volume label is of the form `GIZMOXXXX` where `XXXX` is a number, the
config file will be automatically generated using `XXXX` as the team
number.  This allows the images and contents of the micro SD cards to
be completely identical, with the only distinction being the volume
label, which makes mass-imaging cards more feasible.
