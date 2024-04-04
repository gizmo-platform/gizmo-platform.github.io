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
