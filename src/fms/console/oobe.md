# Initial Tasks

Upon logging in you must connect the FMS workstation to wifi in order
to download some utilities and files that are not distributable.

Connect to wifi by right clicking anywhere on the blank space of the
desktop, or use the menu in the lower left hand corner, and select
"iwgtk" from the network menu:

> Settings > HardwareSettings > iwgtk

Select your wifi network and click connect, entering the password if
your network requires one.

Right click anywhere on the blank space of the desktop, or use the
menu in the lower left hand corner, and select the XFCE4 terminal
from:

> System > TerminalEmulator > Xfce Terminal

> [!NOTE]
>
> You are also strongly encouraged to change the FMS password with the
> following command:
>
> ```
> $ passwd
> ```
>
> This will prompt you for your current, new, and confirmation of new
> passowrds.  The default password is `gizmo`.


With the terminal open, execute the following commands:

```
$ sudo tzupdate
```

This will set the timezone based on your location.  It may print
errors depending on how far off the clock actually is.

Now you must retrieve some restricted packages that are used under
license from Mikrotik.  The following commands will retrieve these
files from the internet.

```
$ sudo -u _gizmo gizmo fms setup fetch-tools
$ sudo -u _gizmo gizmo fms setup fetch-packages
```

> [!NOTE]
>
> Note that the `sudo` command is changing the active user to
> `_gizmo`.  All components of the Gizmo FMS run as this special user,
> and store state in `/var/lib/gizmo`.  Be careful to only ever
> interact with this directory as this user.


## Updating the Gizmo Binary

> [!NOTE]
>
> You only need to update when a new version is released.  If you just
> installed, you're already on the latest version.

From time to time the Gizmo application receives updates.  To update
the software on the Gizmo FMS workstation, execute the following
commands in a terminal:

```
$ sudo xbps-install -Su
$ sudo reboot
```

The update command will tell you how much space is required and how
much data will need to be downloaded from the internet.
