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
Controller -> Gizmo : MQTT Control Stream
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
the same file which makes it easier to distribute.

The diagram below shows the most important parts of the Gizmo as they
are arrayed out for the operation of a field.  When operating in
practice mode, the same components are in play, but are all
initialized with default values that remove the need to configure
everything.

This diagram shows the components in play when running `gizmo field
serve` which provides field control services.

```d2

tlm : Team Location Mapper {
    tooltip: Provides mapping of team to field location
}

mqttserver : MQTT Server {
    tooltip: High performance message broker
}

mqttpusher: MQTT Pusher {
    tooltip: Relay between gamepads and MQTT
}

gamepad : Gamepad Driver {
    tooltip: Provides an abstraction over different joystick APIs
}

http : HTTP Server {
    tooltip: Provides an administrative interface
}

metrics : Prometheus Metrics Registry {
    tooltip: Ingests and collates statistics
}

http -> metrics: Fronts metrics registry
http -> tlm : Updates mappings

tlm -> mqttserver : Pushes location information
mqttpusher -> mqttserver : Pushes control information
mqttpusher <- mqttserver : Queries location information
mqttpusher -> gamepad: Polls gamepad state
```

The primary adminstrative interface for the `gizmo fms run` is an HTTP
API that consumes JSON formatted data.  This API can update the data
stored in the Team Location Mapper which maintains a mapping of team
number to field location.  This mapping is published via routine MQTT
messages as well as made available via the HTTP API (read-only).

MQTT is a publish/subscribe bus mechanism that allows various
consumers to express interest in topics of information such as
location for a particular team or the entire system, and for
publishers to simultaneously notify all consumers of new information.
MQTT is a network-transparent system which means that each of the
components described above can even be run on different computers.

Gizmo boards connect to the `gizmo fms run` process using MQTT.  All
messages that get passed over MQTT are JSON formatted.

The gamepad components abstract over the various differences in
operating system APIs between macOS, Microsoft Windows, and the Linux
kernel.  This allows the `gizmo` tool to be cross platform and read
the gamepad API in a neutral way on a selection of platforms.

Gizmo boards pass statistics information back to the server via MQTT
which is ingested by the metrics module and made available over HTTP
as an OpenMetrics compatible metrics stream.  Example Prometheus and
Grafana configurations are available in the [Gizmo
repository](https://github.com/gizmo-platform/gizmo).
