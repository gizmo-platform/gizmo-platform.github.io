# Physical Setup

Having now configured your FMS, imported teams, and performed other
setup tasks, its time to physically setup your Field Management
System.

## Field Components

Install your field radio somewhere central on the field.  The field
radio has a good range, but you should try to have your entire field
within approximately 50' of the radio.  The device itself can be
mounted inconspicuously somewhere on the field itself, but should be
out of the way of errant robots.

> [!NOTE]
>
> The radio becomes warm after operating, if using an adhesive tape to
> mount it, select a tape that is not released with heat or your radio
> will detatch itself from the mounting point.

On the field boxes, port 1 should be connected back to a field port on
the scoring box.  Ports 2-5 go to the colored field positions in the
following order:

  * Port 2 - Red
  * Port 3 - Blue
  * Port 4 - Green
  * Port 5 - Yellow

The Gizmo team recommends using flat Ethernet cables to reach the
driving positions from the field box.  You can find these [on
Amazon](https://www.amazon.com/dp/B07PKQ6QC7) for reasonably low cost,
but feel free to use any Ethernet cables you want.  The cable that
connects the field box to the scoring box will need to carry some
amount of power when using the PoE equipped scoring box, so avoid
using any "ultra thin" cables for this purpose.

> [!TIP]
>
> To protect the tabs on your long cables, consider using a coupler to
> connect them to short "sacrificial" cables that can be more easily
> replaced if damaged.
>
>   * [Couplers](https://www.amazon.com/dp/B07WR1N7FX)
>   * [Short Cables](https://www.amazon.com/dp/B0CH646LSF)
>
> This way if a cable becomes damaged, you can simply replace the
> short jumper at the end, rather than having to undo and replace the
> potentially much longer cable that may be routed under carpet or
> otherwise inaccessible.

## Scoring Components

Connect your scoring box to power, and connect the field boxes to
ports 3-5.  The FMS workstation should be connected to port 2, and the
internet/uplink network should be connected to port 1.

The scoring box will warm up as it operates which is normal.  Do not
cover it or otherwise restrict airflow.

If you need to connect more devices to the FMS trusted network, such
as a laptop running a scoring application or addtional workstations
you wish to control your event from, you may use an unmanaged/dumb
switch connected to port 2 of the scoring box with the FMS workstation
connected to this switch.  [Here is an example
switch](https://www.amazon.com/dp/B0034CL3MA/).

## Labeling the Network Components

If you have access to a 3d printer labels exist [on
Printables](https://www.printables.com/model/1026634-gizmo-network-box-labels)
that can be snapped on to your network devices to label the ports.

If you do not have access to a 3d printer and want these labels, reach
out to the Gizmo team, we may be able to put you in touch with someone
who has a printer and spare label plates.
