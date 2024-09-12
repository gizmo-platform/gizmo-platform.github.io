# Gizmo

The Gizmo itself is relatively straightforward, but does occasionally
encounter trouble.  Before going deeper into any of these steps, try
turning off the Gizmo and turning it on again.

## What do the lights mean?

There are 2 main groups of indicators on the board.  There are static
indicators which show which systems on the Gizmo have power, and there
are color changing indicators that show specific status.

### Power Status Indicators

There are 7 power indicator lamps which are distinct colors.  From top
to bottom these lights indicate:

  * Main Power
  * Pico Power
  * GPIO Power
  * Servo Power
  * Motor Bank A
  * Motor Bank B
  * Student NeoPixel Power

### Dynamic Status Indicators

Below the power indicator lamps, there are 3 dynamic indicator lamps
that change colors.  These indicate from top to bottom:

  * Network
  * Field
  * Battery

Here's a handy graphic that helps explain what the different
combinations mean:

![Gizmo Lights](/img/gizmo-lights.png)

## Serial Event Log

If you can't determine what the Gizmo is doing from the lights, you
can connect a USB cable to the system processor.  It will present
itself as a serial port which will stream an event log at
`9600,8,n,1`.  The messages in this log provide useful debugging
information and you may be asked to provide this information when
opening a problem report.

If you do not already have a serial terminal available, Google Chrome
can serve this function with the aid of [this
site](https://www.serialterminal.com/).
