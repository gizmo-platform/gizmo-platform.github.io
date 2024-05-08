# Field Management System (FMS)

The Field Management System is, as the name would imply, a system that
provides management services for one or more fields.  These services
include scheduling, orchestration of the field networks, collection of
logs and metrics, and interfaces to consume and configure all of these
features.

## Why is there an FMS?

You might wonder why there's an FMS at all.  After all, the Driver's
Station provides a fine way of controlling a single Gizmo, so you
should be able to just have a bunch of driver's stations all
controlling a bunch of Gizmos, right?

Technically this works, but it comes with some major caveats.

First off, without a central coordinator its difficult to ensure that
every user gets a fair slice of airtime to send and receive control
information.  If one user is consuming all the available time then
that shows up as other Gizmos resetting, stuttering, or acting
erratically due to an inconsistent control stream.

Second, the Driver's Station doesn't have a very good radio in it.
Its perfectly fine for short range practice or limited use, but its
not as powerful as a fixed radio with good antennas.  Ideally, we'd
want to have one of these more powerful radios per field to allow the
machines on those fields to have the shortest possible connection path
back to a powerful radio that fairly divides its transmit time amongst
Gizmos that need it.  Which Gizmos need it?  To ensure the most fair
scenarios in a competition, only machines that are associated with a
currently active match should be operating wirelessly since this
ensures that bandwidth isn't being spent on data that's not part of
match.

To solve this coordination problem, we need a central component that
manages this complexity.  This component is the FMS and is comprised
of hardware and software that work together to manage one or more
fields.

## What is the FMS?

The FMS is made up of 3 key components.  Each of these components has
a specific function to fill and works in concert with the others.
These components are referred to as the FSM Workstation, the Scoring
Box, and Field Boxes.

### FMS Workstation

The FMS Workstation is a Linux computer that actually runs all the
software.  This software includes an MQTT Broker which forms the
center point of the various data streams in the Gizmo ecosystem, a
suite of utilities that perform configuration and management
utilities, and an observability stack that provides centralized
management of logs and metrics.

The FMS Workstation can techncially be any Linux computer, but the
Gizmo team peforms detailed testing and releases software images that
target the Raspberry Pi 400, which is a convenient self contained
computer with a dual monitor capability.

### Scoring Box

The Scoring Box is a network device that resides at the Scoring Table
with the FMS Workstation.  The Scoring Box is just commodity network
hardware, a Mikrotik hEX Lite.  The Gizmo team recommends using the
hEX Lite PoE version, since this allows the Scoring box to power all
the Field Boxes remotely.  The Scoring box hosts all the DHCP servers,
DNS server, NAT services, and a host of other advanced network
components to support interoperability between the Gizmo network and
other networks which may be present at events.

### Field Boxes

Field Boxes are Mikrotik hAP AC Lite devices that are associated with
fields.  Each field has exactly one field box, which means most users
will only have one in total.  The Field Box contains the dual-chain
radios, as well as provides 4 Ethernet ports for driver's stations to
plug into.  The field boxes are powered ether locally or using passive
PoE from the scoring box.  In addition to the SSIDs used by the Gizmo
devices, the Field Boxes also have 5Ghz facilities to broadcast a
network suitable for scoring control, information transfer, or just
general access for game related hardware that is independent of the
wired network.
