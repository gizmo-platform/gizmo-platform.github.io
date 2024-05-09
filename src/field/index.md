# Field Services

The `gizmo` tool provides certain services related to a robotics
competition field.  These services include field location as well as
control signal transmission.  Metrics, logging, and statistical
information is also provided by the FMS.

The FMS contains several components, depicted in the following diagram:


```d2
wan : {
  Label: Internet
  Icon: https://icons.terrastruct.com/essentials%2F140-internet.svg
}

fms_workstation : {
  Label: FMS Workstation
  Icon: https://icons.terrastruct.com/tech%2Fdesktop.svg
}

score_box : {
  Label: Scoring Table Box
  Icon: https://icons.terrastruct.com/infra%2F014-network.svg
}

field_box1 : {
  Label: Field Box
  Icon: https://icons.terrastruct.com/tech%2Frouter.svg
}

field_box2 : {
  Label: Field Box
  Icon: https://icons.terrastruct.com/tech%2Frouter.svg
}

gizmo1 : {
  Label: Team 123 Gizmo
  Icon: https://icons.terrastruct.com/aws%2FRobotics%2FRobotics.svg
}

ds1 : {
  Label: Team 123 Driver's Station
  Icon: https://icons.terrastruct.com/tech%2F048-gamepad.svg
}

gizmo2 : {
  Label: Team 42 Gizmo
  Icon: https://icons.terrastruct.com/aws%2FRobotics%2FRobotics.svg
}

ds2 : {
  Label: Team 42 Driver's Station
  Icon: https://icons.terrastruct.com/tech%2F048-gamepad.svg
}

score_box <-> wan : Ethernet
score_box <-> fms_workstation : Ethernet
score_box <-> field_box1 : Ethernet
score_box <-> field_box2 : Ethernet

field_box1 <-> ds1 : Ethernet
field_box1 <-> gizmo1 : WiFi

field_box1 <-> ds2 : Ethernet
field_box1 <-> gizmo2 : WiFi
```

At the center of the network is the Scoring Table box, which connects
to the FMS Workstation, any Field Boxes, and Internet if available.
The FMS Workstation is a Linux PC running the administrative software
that powers the FMS, and the Field Boxes provide a means of access to
these services via wired interfaces for Driver's Stations and Wireless
interfaces for Gizmo devices.
