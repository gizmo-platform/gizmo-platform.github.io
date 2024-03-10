# Prerequisites

Before you can start writing your own programs, its necessary to get
your environment setup with some tools that support the Gizmo itself.
You can find detailed guides for various programming languages and
their respective tools in the table of contents.  This section will
focus on the initial setup to get the Gizmo ready to run.

## Install the Gizmo CLI Tool

The `gizmo` CLI tool is a Command Line Interface (cli) tool.  This
means that it does not make use of the mouse, instead taking input
entirely in text form and providing feedback entirely as text.  The
`gizmo` tool can help setup specific programming languages, but it
also can help you get firmware installed, drive your robot, or even
run a complete competition field system.

The `gizmo` tool is available on [this
page](https://github.com/gizmo-platform/gizmo/releases).  From that
page, find the most recent release that says "Latest" on it.  You may
need to scroll down.

Once you have located the latest release, download the appropriate
files for your operating system.  For most users, this will be
'gizmo_Windows_arm64.zip'.  Download this file expand the zip archive.
You can run the `gizmo.exe` file from anywhere, so save it somewhere
you'll remember.

> [!TIP]
> If you're in IT and would like to deploy the Gizmo tools from a
> central location, reach out to the team and we can provide you with
> MSI packages suitable for silent installation via Group Policy.

Once you have the `gizmo` tool downloaded, you can open a PowerShell
window and navigate to the directory where you saved the `gizmo.exe`
file.

> [!CAUTION]
> If you find that the `gizmo.exe` file is missing.  You may need to
> add an exception to the Windows Real Time Protection system for the
> folder you want to save the Gizmo tools into.  Windows Real Time
> Protection incorrectly fingerprints our tools based on the embedded
> example code they contain as malicious.  To add an exception to the
> folder, follow [this
> guide](https://support.microsoft.com/en-us/windows/add-an-exclusion-to-windows-security-811816c0-4dfd-af4a-47e4-c301afe13b26)
> from Microsoft.
>
> Always consult your IT or Information Security team prior to
> disabling or modifying your computer's security policy.

With PowerShell open, you can now type `.\gizmo.exe` and receive the
following help output:

```text
The Gizmo Platform provides servers for field control, configuration for your joysticks, and tools to program the system processor on your robot control board.

Usage:
  gizmo [command]

Available Commands:
  arduino     Configure Arduino tools for use with Gizmo
  completion  Generate the autocompletion script for the specified shell
  field       field cmdlets operate or configure a field
  firmware    firmware cmdlets manage the GSS firmware image
  help        Help about any command

Flags:
  -h, --help   help for gizmo

Use "gizmo [command] --help" for more information about a command.
```

Congratulations, you now have the Gizmo tools installed and are ready
to proceed to the next steps!

## Installing the Arduino IDE

Even if you are not planning to use Arduino to program your user code,
the Arduino Suite is still required to compile the firmware for the
system processor.  Do not worry, this is an automated process.

> [!TIP]
> If you don't intend to program your user code using the Arduino
> environment, you can install only the Arduino CLI instead of the
> full IDE.  This will save disk space.
>
> You can find the Arduino CLI installers on [this
> page](https://arduino.github.io/arduino-cli/latest/installation/#latest-release).
>
> If installing the Arduino CLI, you don't need to complete this section!

If using Microsoft Windows, the Gizmo tool can perform all of the
Arduino setup for you.  In a PowerShell prompt, run the following:

```
$ .\gizmo.exe arduino install
```

This command will download the latest installer for the Arduino IDE
and launch the installer in a non-interactive mode.  A window will
appear with a progress bar while the Arduino IDE is installed for your
user.  Administrator privileges are not required.  Once the installer
finishes, you should see a shortcut on the desktop to the Arduino IDE.
Feel free to delete this shortcut if you do not need it.

After the Arduino IDE or Arduino CLI have been installed, they must be
configured to support running the Gizmo software.  The Gizmo tool can
do this for you with the following command:

```
$ .\gizmo.exe arduino setup
```

The full command will look similar to this and will take several
minutes to run, but your version numbers and paths may be different:

```
$ .\gizmo.exe arduino setup
Using arduino-cli at C:\Users\best\AppData\Local\programs\arduino-ide\resources\app\lib\backend\resources\arduino-cli.exe
Attempting to install Pico Core
Downloading index: library_index.tar.bz2 downloaded MiB    0.00%
Downloading index: package_index.tar.bz2 downloaded2 KiB    0.00%
Downloading index: package_rp2040_index.json downloaded49 KiB    0.00%
Downloading missing tool builtin:ctags@5.8-arduino11...
builtin:ctags@5.8-arduino11 downloaded73 KiB    0.00%
Installing builtin:ctags@5.8-arduino11...
Skipping tool configuration....
builtin:ctags@5.8-arduino11 installed
Downloading missing tool builtin:serial-discovery@1.4.0...
builtin:serial-discovery@1.4.0 downloaded MiB    0.00%
Installing builtin:serial-discovery@1.4.0...
Skipping tool configuration....
builtin:serial-discovery@1.4.0 installed
Downloading missing tool builtin:mdns-discovery@1.0.9...
builtin:mdns-discovery@1.0.9 downloaded MiB    0.00%
Installing builtin:mdns-discovery@1.0.9...
Skipping tool configuration....
builtin:mdns-discovery@1.0.9 installed
Downloading missing tool builtin:serial-monitor@0.14.1...
builtin:serial-monitor@0.14.1 downloaded MiB    0.00%
Installing builtin:serial-monitor@0.14.1...
Skipping tool configuration....
builtin:serial-monitor@0.14.1 installed
Downloading missing tool builtin:dfu-discovery@0.1.2...
builtin:dfu-discovery@0.1.2 downloaded MiB    0.00%
Installing builtin:dfu-discovery@0.1.2...
Skipping tool configuration....
builtin:dfu-discovery@0.1.2 installed
Downloading packages...
rp2040:pqt-gcc@2.2.0-d04e724 downloaded/ 101.55 MiB   85.28%
rp2040:pqt-mklittlefs@2.2.0-d04e724 downloaded25 KiB    0.00%
rp2040:pqt-elf2uf2@2.2.0-d04e724 downloaded7 KiB    0.00%
rp2040:pqt-pioasm@2.2.0-d04e724 downloaded92 KiB    0.00%
rp2040:pqt-python3@1.0.1-base-3a57aed downloaded MiB    0.00%
rp2040:pqt-openocd@2.2.0-d04e724 downloaded MiB    0.00%
rp2040:pqt-picotool@2.2.0-d04e724 downloaded99 KiB    0.00%
rp2040:rp2040@3.7.2 downloaded/ 82.32 MiB   88.38%
Installing rp2040:pqt-gcc@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-gcc@2.2.0-d04e724 installed
Installing rp2040:pqt-mklittlefs@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-mklittlefs@2.2.0-d04e724 installed
Installing rp2040:pqt-elf2uf2@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-elf2uf2@2.2.0-d04e724 installed
Installing rp2040:pqt-pioasm@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-pioasm@2.2.0-d04e724 installed
Installing rp2040:pqt-python3@1.0.1-base-3a57aed...
Skipping tool configuration....
rp2040:pqt-python3@1.0.1-base-3a57aed installed
Installing rp2040:pqt-openocd@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-openocd@2.2.0-d04e724 installed
Installing rp2040:pqt-picotool@2.2.0-d04e724...
Skipping tool configuration....
rp2040:pqt-picotool@2.2.0-d04e724 installed
Installing platform rp2040:rp2040@3.7.2...
Skipping platform configuration....
Platform rp2040:rp2040@3.7.2 installed

Attempting to refresh library index
Downloading index: library_index.tar.bz2 downloaded MiB    0.00%

Attempting to install Gizmo Libraries
Downloading Gizmo@0.0.3...
Gizmo@0.0.3 downloaded KiB    0.00%
Installing Gizmo@0.0.3...
Installed Gizmo@0.0.3
```

Any errors encountered will be clearly printed as the last text from
the command.  No errors were printed in the above example, so we're
good to go now!
