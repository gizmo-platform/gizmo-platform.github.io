# Architecture

The Gizmo platform is comprised of several different components.  In
this section we'll look at the architecture of the software, the
hardware, and the interfaces between different components.

You are not required to understand the architecture to be able to use
the Gizmo platform.  This will not be on the test.

## Overall System Architecture

The overall architecture of the Gizmo system composes a control point,
the Gizmo itself, and anything hooked up to it.  This can be broadly
visualized with the diagram below:

```d2
direction: right
Operator -> Controller : Human controls Gamepad
Controller -> Gizmo : UDP Control Stream
Gizmo -> Peripherals : Gizmo Controls Peripherals
```

## Gizmo Hardware Architecture

The hardware architecture is comprised of 2 processors, an I2C link
between them.  Here's a block diagram of how it all works.

```d2
direction: right

Gizmo {
    System Processor {
        tooltip: Runs Gizmo System Software (GSS)
    }

    User Processor {
        tooltip: Runs User Code
    }

    User Processor -> System Processor : I2C
}

Serial Device {
    tooltip: Sensor or IR emitter
}

PWM Motor Controller {
    tooltip: R/C compatible PWM controller
}

Servo {
    tooltip: Hobby Servo
}

Limit Switch {
    tooltip: Normally open or normally closed switch
}

Neopixels {
    tooltip: Programmable RGB LEDs
}

Gizmo.User Processor -> Serial Device : Asynchronous Serial
Gizmo.User Processor -> PWM Motor Controller : Hardware PWM
Gizmo.User Processor -> Servo : Hardware PWM
Gizmo.User Processor -> Limit Switch : GPIO
Gizmo.User Processor -> Neopixels : High-speed serial
```

The system processor is implemented as a peripheral device that the
user processor controls.  This allows the user code to be the root
node on the I2C bus which dramatically simplies the implementation.
The System Processor also supervises a number of different devices on
the board including voltage supply rails, battery input voltage, and
when the last time the User Processor asked for data was.


## Gizmo Software Architecture

The command line `gizmo` tool is a "multi-call binary" meaning that it
can do different things depending on the options you start it up with.
You can think of it as a bunch of different programs all contained in
the same file which makes it easier to distribute.  The same exact
`gizmo` tool is what runs on the Driver's Stations and the FMS.  While
we make every effort to make compatible updates, in general you'll
want to run the same version of the Gizmo utilities everywhere.  We
note explicitly when backwards compatibility cannot be maintained, so
check the patch notes.

### Direct Connect

Speaking in terms of time connected, most Gizmos will spend most of
their time in what is called "Direct Connect" mode, which forms a
direct point-to-point link between the Driver's Station and the Gizmo
System Processor.  This looks like this visually:

```d2

ds : Driver's Station {
  gamepad : Gamepad
  sw : Gizmo Software

  net : DS Network {
    enet : Ethernet
    radio : Integrated Radio
  }

  gamepad -> sw : USB
  sw -> net.radio : Host Network Stack
}

gizmo : Gizmo {
  system : System Processor
  user : User Processor

  system -> user : I2C
}

ds.net.radio -> gizmo.system : Point to Point Link
```

The control is passed from the gamepad to the Driver's Station via
USB, then via wireless link to the Gizmo System Processor, then via
I2C to the user processor where these values are consumed by user
programs to control a robot.

### Field Radio

During a competition, it is imperative that a solid link be maintained
between the Gizmo and the Driver's Station.  The weak part of this
chain is the wireless conection, so to ensure a solid connection, we
replace the Driver Station's internal radio with a substantially more
powerful external radio wth external antennas.  This also ensures that
each team is equally affected by any interference, since the central
radio talks to all robots on a given field.  One field radio per field
is required.

The FMS does not process or interact with the control data, it only
provides a different means of transfering this control stream from the
DS to the Gizmo.  Visually, this is the path:

```d2

ds : Driver's Station {
  net : DS Network {
    enet : Ethernet
    radio : Integrated Radio
  }
}

fms : Field Management System {
  field : Field Box {
    style.multiple: true
  }
  score : Scoring Box
  pi : FMS Workstation

  score <-> field : Ethernet
  score <-> pi : Ethernet
}

gizmo : Gizmo {
  system : System Processor
  user : User Processor

  system -> user : I2C
}

ds.net.enet -> fms.field : Ethernet
fms.field -> gizmo.system : Wireless
```

The FMS maintains an isolated data-path for each Gizmo/DS pair, and
ensures that no robot can interfere with another's control stream.
This is why the FMS needs to know where each team is located, so that
it can provision the correct access onto the specific Field Box port
going to that team.

## Observability


Gizmo boards pass statistics information back to the driver's station,
which in turn shares them with the FMS.  The Gizmo board also reports
some metadata information related to things that can prevent the
system from working directly to both the Driver's Station and the FMS.

You can use this information to get a better understanding of what's
going on with your field and Gizmos.  Here are some example questions
you can answer using this data:

  * Are all Gizmos connected?
  * Is there persistent interference?
  * Did any team experience a disconnect?
  * What is the battery voltage on all connected Gizmos?

This information is ingested by the metrics module and made available
over HTTP as an OpenMetrics compatible metrics stream.  Example
Prometheus and Grafana configurations are available in the [Gizmo
repository](https://github.com/gizmo-platform/gizmo).
