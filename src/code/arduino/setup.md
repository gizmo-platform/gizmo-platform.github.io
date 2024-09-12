# Setup

Programming with the Arduino environment will require the Arduino IDE
to be installed and configured.  This setup will involve installing
the Arduino IDE itself, installing the Board Support Package (BSP) and
installing the Gizmo-specific libraries.  This process takes about 15
minutes on a modern computer with typical broadband internet.

> [!TIP]
>
> If you're performing this process on Windows, the Gizmo CLI can
> actually install everything fully automatically.
>
> Just invoke `gizmo.exe arduino install` followed by `gizmo.exe
> arduino setup` and the IDE, support packages, and Gizmo libraries
> will be installed for you.  This is the recommended path for
> institutional admins to deploy as well since it can be automated
> with a relatively simple GPO.

## Installing the IDE

Start by installing the latest Arduino IDE for your operating system.
You can find the latest installers on [this
page](https://www.arduino.cc/en/software).  In the blue sidebar of the
first card, select the right package for your computer.

Once you have installed the IDE, open it and proceed to install the
Raspberry Pi Pico plugin.

## Installing the Raspberry Pi Pico Support Package

The Raspberry Pi Pico requires an additional package that contains
compilers, libraries, and other supporting components.  These packages
are usually referred to as Board Support Packages (BSPs).  Detailed
installation instructions are available from the [upstream
documentation](https://arduino-pico.readthedocs.io/en/latest/install.html#installing-via-arduino-boards-manager).
Here is an abridged version:

  * Open the Arduino IDE
  * Open File > Preferences
  * In the settings tab, add the following URL in the "Additional
    Boards Manager URLs":

    > https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json

  * Hit "OK" to save the URL.
  * Open the boards manager from Tools > Boards > Board Manager
  * Search for "Pico" and install the entry by Earle F. Philhower,
    III.

## Install the Gizmo Libraries

The Gizmo makes use of a custom library to provide aliases for all the
pin names and to provide a means of retrieving information from the
system processor.

To install the library, follow these steps:

  * Open the Arduino IDE
  * Open Tools > Manage Libraries...
  * Search for "Gizmo"
  * Install the entry by M. Aldridge.  Unless you have received other
    instructions, install the most recent version.
