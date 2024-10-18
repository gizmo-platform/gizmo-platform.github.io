# Setup

MicroBlocks program are run on a firmware interpreter. This firmware needs
to be installed on your Gizmo's student processor before you can use
MicroBlocks to program it.

## Installing the MicroBlocks Firmware

1. Download the firmware .uf2 file from the MicroBlocks downloads page:
[https://microblocks.fun/downloads/latest/vm/vm_gizmo_mechatronics.uf2](https://microblocks.fun/downloads/latest/vm/vm_gizmo_mechatronics.uf2)
1. Hold your Gizmo's student processor BOOTSEL ("Boot Select") button down
while connecting the USB programming cable to your computer. Once you've
connected the cable, release the BOOTSEL button. You should now see an
external drive on your computer called "RPI-RP2".
1. Copy the .uf2 file you downloaded to the "RPI-RP2" drive. Your Gizmo's
student processor will automatically reset when the installation is done.

## Connecting the MicroBlocks Editor to Your Gizmo

The MicroBlocks editor can be accessed online at
[https://microblocks.fun/run/](https://microblocks.fun/run/).
This editor will allow you to create programs without a Gizmo connected.
You can save and load programs to and from your computer.

To send programs to your Gizmo, you'll need to connect the editor to your
Gizmo. Plug your Gizmo into your computer with a USB cable connected to
the student processor's USB port. (Do NOT hold the Boot Select button.)

In the editor, click the "Connect" button, which looks like a USB plug at
the top of the editor screen. In the Connect menu, select "connect" (not
"connect (BLE)"). This will open a menu from your browser to pick the
serial port for your Gizmo (COM ports on Windows).

If the connection is successful, the Connect button will have a green
circle behind it. At this point, when you press the green run button
in the upper right, your Gizmo will run your program. whenever your Gizmo
is powered on without being connected to the MicroBlocks editor, it will
start running the program saved to it automatically.

## Troubleshooting Locked Up Gizmos

Though unlikely, it is possible to save a program to your Gizmo that is
broken and prevents it from connecting to the editor. In these cases, you
may see the connection process fail or freeze your browser. To recover
from this, you can use the
[flash_nuke.uf2](https://cdn-learn.adafruit.com/assets/assets/000/099/419/original/flash_nuke.uf2?1613329170)
firmware from Adafruit to erase the program from the Gizmo's processor.
After installing this firmware, you will need to reinstall the MicroBlocks
firmware.
