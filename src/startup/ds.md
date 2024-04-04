# Driver Station

While it possible to use the Gizmo software and hardware with any
computer, the Gizmo Team has developed software to provide an
appliance-like experience using an embedded Linux system called a
driver's station.

The station code is regularly tested using a [Raspberry Pi Zero 2
W](https://www.pishop.us/product/raspberry-pi-zero-2-w/) with a
[Waveshare Ethernet and USB
hub](https://www.waveshare.com/product/eth-usb-hub-box.htm).  Though
we don't actively qualify against the hardware, you should also be
able to use our driver station system images on a Raspberry Pi 4B.

## Physical Assembly

The driver's station comprises the Raspberry Pi Zero 2 W and the
Waveshare board, which you will need to assemble.  This need only be
done once and once assembled there is no need to disassemble.

Consult the following graphic from the Waveshare documentation for
assembly instructions, paying special attention to the order of the
brass spacers that are used to assemble the PCB stack:

![Waveshare Guide](/img/ETH-USB-HUB-BOX-Assembly.jpg)

When installing the white cover plate, use the one without the cutout
in it.  Both are provided, but only the one without the cutout is
required.

## Software Installation

Once physical assembly is complete, you must install the software
image that runs the driver's station.  Ensure you have a means of
writing a micro SD card for this step, using adapters if necessary.

Obtain the latest system image from the [GitHub releases
page](https://github.com/gizmo-platform/gizmo/releases/).  For the
driver's station, use the `driver-station.zip` file.  After unzipping
the file, you'll have a file called `driver-station.img`.

To write the image to the SD card, you have a few choices.  If you're
on a mac or a Linux machine, you can use `dd`.  On Windows you can use
[Win32DiskImager](https://win32diskimager.org/).  If you've never
written disk images before, the Gizmo team recommends you use [Balena
Etcher](https://etcher.balena.io/) which is available for all
platforms, and guides you through the process.

> [!TIP]
>
> Balena Etcher Portable edition can be used without needing to
> install.  This can be convenient to not clutter your machine with
> extraneous programs, or to write files from a machine where you do
> not have administrative permissions to install new programs.

Writing the Gizmo driver station image with Balena Etcher looks like
this:

When you open the main application screen, it will look like this:

![Balena Etcher First Open](/img/balena_opened.png)

Either download and extract the driver station file as mentioned
above, or paste the URL directly into the box on the "Use Image URL"
screen:

![Balena Etcher Use URL](/img/balena_urlconf.png)

Click the "Select target" button and connect your micro SD card.  The
disk will appear as a target that you can use.

![Balena Etcher Target Selection](/img/balena_target.png)

> [!WARNING]
>
> Balena will automatically hide any volumes that it detects the
> system may be booted from, so generally you can't hurt your computer
> with this process.  Be extremely careful though to select the right
> disk if you show hidden devices.  A good way to make sure you've got
> the right disk is to confirm the size of the device, which should
> match closely to the size of your micro SD card.  The exact size may
> differ slightly due to the way in which device manufacturers market
> capacity.

Once configured, the Etcher window will look similar to this:

![Balena Etcher Ready to Go](/img/balena_ready.png)

The flashing process will take several minutes.  On average with a
modern computer, the Gizmo team observes this process taking about 3
minutes.

> [!NOTE]
>
> You may be prompted to authorize the command prompt prior to the
> flash process commencing, this is a normal prompt as Balena Etcher
> needs to access low-level system utilities to write the data to the
> micro SD card.

Once complete, the Etcher window will look like this:

![Balena Etcher Finished](/img/balena_finished.png)

You may now close Etcher and remove the micro SD card.


## Software Configuration

Once you have successfully written the driver station image to the
micro SD card, it must be configured.  The driver station consumes the
same configuration file as the Gizmo System Software.  Locate the
`gsscfg.json` file that was generated as part of the
[configuration](config.md) process, and copy it to the micro SD card.

> [!NOTE]
>
> On some computers, you may need to disconnect and reconnect the
> micro SD card for it to show up as a removable disk after using
> Etcher.
>
> Only one volume should attach in most cases, but in the event your
> computer has additional device drivers typically only found on
> developer workstations, you may see two drives when you plug in the
> micro SD card.  In the event you see two drives, copy `gsscfg.json`
> to the drive that contains `bootcode.bin`.

Once you copy the file, eject/safely remove the drive and install it
in the micro SD card slot on the driver's station.  The card should be
inserted with the contacts facing downwards, aligned against the flat
edge of the card.  Should you need to remove it, you may find tweezers
useful; alternatively with the lid removed from the driver's station,
it is possible to use a paperclip to push the card back out.
