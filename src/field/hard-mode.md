# Hard Mode!

> [!DANGER] Danger, Will Robinson!
>
> Absolutely nothing in this section is supported.  This is provided
> for your entertainment and information, and is supposed to help you
> understand what exactly is inside of the FMS image file that's
> provided for the Raspberry Pi.
>
> You do not need to read, engage with, or even acknowledge this
> section to use the FMS or operate the Gizmo system.

Life not interesting enough?  Nothing to do on a rainy afternoon?  One
last day of summer, one more day before school has begun?  Lets have
some serious fun and build the entire FMS, by hand, from nothing.  And
for added bonus points, we'll do it on a conventional `amd64` system
to really prove we know how it works and what it does.

In case it wasn't clear, this section of the manual deals with very
advanced concepts that are just for fun, and let you know what the
magic is inside the system.  Broadly, we'll build the FMS using a few
steps:

  * Install and Configure Void Linux
  * Install the FMS System Software
  * Flash Network Devices
  * Bootstrap the Network (by hand)

Lets begin.

## Install and Configure Void Linux

The FMS expects to run on top of Void Linux.  While the Gizmo tools
are statically compiled Go, changing the underlying Linux distribution
is beyond the scope of even this document.  If you're really
interested in changing event that, its possible and the Gizmo team
wishes you Godspeed and good luck.

We'll use Void Linux for x86_64, and we'll use the `musl` flavor.  The
`musl` flavor substitutes the conventional `glibc` flavor for the much
more compact `musl` library developed by Rich Felker.  This library
has the bare minimum in it to satisfy the requirements of the C
library interface.

Installing Void is a generic process, and you are encouraged to
consult the [upstream
documentation](https://docs.voidlinux.org/installation/index.html).
The only constraint you need to keep in mind is that the Gizmo tools
presently hard-code the user that you're expected to run as, so its
important to make sure that your user account has a login name of
`admin`.

### Disable "predictable" Interface Names

Recent versions of most Linux distributions use a so-called
"predictable" interface naming function.  If you have a magic decoder
ring and know the precise hardware layout, data-plane architecture, and
chip interface types its possible to predict the interfaces.  Since
the computer used in this guide has one wireless interface and one
wired interface its much easier to turn off this "predictability" and
know that the wireless interface will be `wlan0` and the wired
interface will be `eth0`.

Do so by changing the value of `GRUB_CMDLINE_LINUX_DEFAULT` in
`/etc/default/grub`:

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=4 net.ifnames=0"
```

One you make this change, ensure GRUB has the updated configuration by
running `sudo update-grub` and reboot.  After restarting, your
interfaces should look similar to below:

```
[admin@gizmo ~]$ ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f4:4d:30:6f:70:c9 brd ff:ff:ff:ff:ff:ff
3: wlan0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 3c:f8:62:12:32:66 brd ff:ff:ff:ff:ff:ff
```

> [!SUCCESS] Checkpoint!
>
> At this point we've completed the steps outlined in [Workstation
> Setup - Install the System
> Image](setup.md#install-the-system-image).  Some of this setup is
> normally handled by CI when Void prepares images.  For example, Void
> pre-disables "predictable" interface naming on Raspberry Pi images.

## Install the Gizmo Software

You'll need to connect to the internet for this, and its recommended
to do so using wired Ethernet.  You'll need to install some files and
run scripts that expect to have access to the internet.

> [!TIP]
>
> Throughout this section you'll see the term "binary" used to refer
> to the Gizmo software.  This is a common shorthand used by
> programmers to refer to the compiled form of a program.  Humans
> work with ASCII text whereas computers deal with binary information,
> and so when we want to interact with the computer, we do so using
> compiled programs containing machine code.  Machine code is a long
> sequence of 1s and 0s, referred to as a binary file.  Since "binary
> file" becomes cumbersome, its common to refer to these artifacts as
> binaries.

Head over to the [GitHub
Releases](https://github.com/gizmo-platform/gizmo/releases) page.
Locate the most recent released version that is not a pre-release and
obtain the correct file for the architecture you've installed.  Since
the machine used in this demo is an Intel NUC, the correct file will
be `gizmo_Linux_x86_64.tar.gz`.  You can download it directly using
`xbps-uhelper fetch`:

```
$ xbps-uhelper fetch https://github.com/gizmo-platform/gizmo/releases/download/v0.1.0/gizmo_Linux_x86_64.tar.gz
```

Expand the archive and copy the `gizmo` binary into `/usr/local/bin/`:

```
$ tar -xvzf gizmo_Linux_x86_64.tar.gz
LICENSE
README.md
gizmo
$ sudo mv -v gizmo /usr/local/bin/
renamed 'gizmo' -> '/usr/local/bin/gizmo'
```

Now we can let the `gizmo` tool install a bunch of packages and expand
some files onto the disk.  This may seem like cheating, but there's
really no way around it without actually expanding parts of the binary
using external tools.  The hidden command `gizmo fms system-install`
will drive the package manager to install a graphical environment, and
a number of support packages that make the Gizmo system work.  You can
find the list of packages it installs
[here](https://github.com/gizmo-platform/gizmo/blob/master/pkg/fms/system_setup.go).

```
$ sudo gizmo fms system-install
```

This command will generate a lot of output, but it will eventually
finish fetching packages and installing them.  The last thing that we
need to do before rebooting is to make sure that the groups membership
of the `admin` user is correct.  We'll do that with the following
`usermod` invocation:

```
$ sudo usermod -c "FMS Admin" -G wheel,storage,dialout,docker admin
```

> [!SUCCESS] Checkpoint!
>
> If you've made it this far, we've just done all the steps that get
> done normally by the Continuous Integration (CI) system that builds
> the Gizmo FMS images on GitHub.
>
> We normally provide these system images because as you can tell,
> this is a tedious process that's easy to miss steps in, so running
> this entire process using a script is more reliable.
>
> You can view the exact steps that the CI system performs by
> examining the `.release/build-fms.sh` script in
> [gizmo-platform/gizmo](https://github.com/gizmo-platform/gizmo).
> The script includes a handful of extra steps that have to do with
> running an arm64 software stack on top of an amd64 installation
> environment, but performs an otherwise identical set of steps to
> what we've done above.

On reboot, `/etc/runit/core-services/06-gizmo.sh` will be run which
enforces some special permissions on the `gizmo` file itself and asks
the `gizmo` binary to perform some configuration steps.  These steps
run on every boot and make the system somewhat self-repairing.  Its
not as self-repairing as more advanced implementations such as
Chrome OS's dual-root architecture, but this strikes a good balance for
usability.

These setup steps do broadly the following tasks:

  * Configure the system hostname
  * Configure "sysctls" which are system tuning values
  * Configure the IceWM session for the Admin user
  * Configure auto-login via SDDM for the Admin user
  * Configure `dhcpcd`, `iwd`, `pipewire`, and `sudo`
  * Configure `grafana` which provides dashboards
  * Configure `prometheus` which stores metrics
  * Enforces a set of services that are expected to be enabled

## Flash Network Devices

Now that the FMS dashboard is available and the system is behaving
like a workstation, we'll briefly rejoin the main guide to fetch some
non-redistributable packages and assets from Mikrotik.  The network
components that make up the field are made by Mikrotik and leverage
the extreme programability of RouterOS.  While the licensing terms of
RouterOS are extremely generous, they do not permit the Gizmo team to
redistribute the binaries packaged with our software, so we provide
tools to download the files instead.

Since this is hard mode and we are doing things by hand, we won't be
using those tools.

First we'll download and install `netinstall-cli` which as the name
implies is a command line interface for performing network
installations.  This tool is what allows us to reset the hEX and hAP
devices to a known state and then set them up from there.  Starting
from a known state is extremely important so that the automated setup
is starting from a fixed base configuration.

Download the CLI from Mikrotik's website and copy the file to
`/usr/local/bin/`:

```
$ xbps-uhelper fetch https://download.mikrotik.com/routeros/7.15.3/netinstall-7.15.3.tar.gz
$ tar -xvzf netinstall-7.15.3.tar.gz
$ sudo mv netinstall-cli /usr/local/bin/
```

We want to be able to use the `netinstall-cli` as the `admin` user
later without using `sudo` to elevate permissions.  On Linux since we
know what the specific permissions are that the `netinstall-cli` needs
we can assign those statically to the file:

```
$ sudo setcap cap_net_bind_service+ep /usr/local/bin/netinstall-cli
```

> [!WARNING]
>
> This has some pretty serious security implications because the
> permissions are "sticky".  This means that any user on the system
> can now use the `netinstall-cli` tool without needing to have access
> to `sudo` since we've permanently conferred the permission to bind
> services to this file.
>
> We can make this choice here because the FMS Workstation is what's
> usually referred to as an Appliance.  Appliances are special purpose
> computers that have known configurations and specific trade-offs to
> accomplish a given goal.  Changes like we're making here should
> usually not be made on general purpose systems.

Now that we have the `netinstall-cli` tool installed, we can fetch the
firmware packages from Mikrotik's website and store them in the
expected locations:

```
$ sudo mkdir -p /usr/share/routeros/
$ cd /usr/share/routeros/
$ sudo xbps-uhelper fetch https://cdn.mikrotik.com/routeros/7.15.3/routeros-7.15.3-mipsbe.npk
$ sudo xbps-uhelper fetch https://cdn.mikrotik.com/routeros/7.15.3/wireless-7.15.3-mipsbe.npk
```

Note the versions and that one of the files is `routeros` and one is
`wireless`.  The `routeros` file is the base operating system for the
network devices, and the `wireless` package contains additional
drivers and programs to operate the radios on the hAP devices.

> [!SUCCESS] Checkpoint!
>
> At this point, we've run the equivalent of `gizmo fms setup
> fetch-tools` and `gizmo fms setup fetch-packages`.

At this point you'll need to actually configure the FMS.  This is no
different than the normal install and involves running `gizmo fms setup
wizard`.  You can get more information in [this page](config.md).

We're now ready to use `netinstall-cli` to install the software on the
devices.  The `gizmo` tool provides significant automation to setup
the network devices and be then invoke `netinstall-cli` with all the
right options.  We'll do this by hand only invoking the `flash-device`
command to get the templated installation script from it which is
necessary to preset the credentials during installation on the network
devices.

Start by configuring the network to support the installation process.
Disconnect from the wired network if still connected, and add the
provisioning IP address to `eth0`.

```
$ sudo ip address add 192.168.88.2/24 dev eth0
```

Next, run the flash command to get the `rsc` files that contain the
bootstrap configuration.

```
$ gizmo fms setup flash-device --template-to scoring.rsc
$ gizmo fms setup flash-device --template-to field.rsc
```

Make sure you select the right option when making each file!

Now we can do the flash device procedure.  This involves setting up
some IPs for the FMS workstation so that it can remotely boot the
various Mikrotik devices.  Using `iproute2` the FMS workstation can
have `192.168.88.2/24`, then we start the netinstall process for the
scoring box:

```
$ sudo ip address add 192.168.88.2/24 dev eth0
$ sudo netinstall-cli -s scoring.rsc -r -a 192.168.88.1 /usr/share/routeros/routeros*.npk
```

This will wait for the RouterOS device to attempt to netboot.  Plug
into port 1 and hold down the reset button while connecting the power.
This causes RouterOS to boot from the fail-safe boot-loader, and then
wait for the user LED to be solid, then be blinking, then turn off.
Once the user LED turns off the device will netboot.

Install the software on the field boxes one at a time using a similar
command, but this time including the `wireless` package:

```
$ sudo netinstall-cli -s field.rsc -r -a 192.168.88.1 /usr/share/routeros/*.npk
```

> [!NOTE]
>
> The glob expression here will actually install _all_ RouterOS
> packages, but the only 2 in the directory are the 2 that were
> downloaded earlier.

Once you've installed all of your field boxes, make sure to remove the
provisioning address from `eth0`.  It won't technically break
anything, but it does leave spare entries in the routing table which
can have unintended side effects depending on how you get internet.

```
$ sudo ip address delete 192.168.88.2/24 dev eth0
```

> [!SUCCESS] Checkpoint!
>
> At this point we've completed all device flashing, and are ready to
> bootstrap the network.  This is equivalent to the steps from the
> [Hardware Preparation](hw-prep.md) section.

## Bootstrap the Network

The final step in setting up the FMS the hard way is to actually
bootstrap the network.  This is done by a careful set of changes
applied by a tool called Terraform.


> [!TIP]
>
> Terraform is an infrastructure automation and configuration tool
> that we use to leverage existing code that knows how to manage
> RouterOS devices.  Terraform is an industry standard tool that you
> can learn more about [here](https://terraform.io).

We need to use the Gizmo tool to actually extract the `.tf` files onto
the disk, but after that we can use the `terraform` CLI to actually
apply them.  The `gizmo` tool actually calls out to `terraform` in the
background, so this is just skipping a layer of abstraction.  Because
this is early provisioning, the normal network connectivity isn't
available.  Create a bootstrapping VLAN adapter to communicate with
the scoring box during provisioning:

```
$ sudo ip link add bootstrap0 link eth0 type vlan id 2
$ sudo ip address add 100.64.1.2/24 dev bootstrap0
$ sudo ip link set bootstrap0 up
```

Now extract the terraform source files to disk:

```
$ gizmo fms net bootstrap --skip-apply
```

This will put a terraform workspace at `~/.netstate` which is where
we'll run the next several commands from.  At this point you can
connect an Ethernet cable between the FMS workstation and port 2 on
the scoring box, power it on, and wait for it to boot.

> [!NOTE]
>
> During normal operation `~/.netstate` still exists.  This special
> directory holds all of the stateful data that is related to the FMS,
> and is the directory that would have to be synchronized to a second
> machine to make the FMS highly available.

After the hEX is booted, we're ready to apply configuration.  We could
wait several minutes to see if it boots, or we could use the same
check that the bootstrap command uses:

```
$ sudo xbps-install -y curl
$ export GIZMO_AUTO_PASS=$(gizmo fms net credential auto)
$ curl -k --user "gizmo-fms:$GIZMO_AUTO_PASS" https://100.64.1.1/rest/system/identity
```

Once that command returns the valid JSON and does not return a
connection error, its time to proceed to configuring the terraform.
Change directories to the netstate directory, then initialize
terraform.

```
$ cd ~/.netstate
$ terraform init
```

Initializing terraform downloads provider plugins and other data that
terraform needs to fetch from the internet to work.  Finally, because
we're not running the automated bootstrap, we need to create the
`tlm.json` file that the Gizmo FMS normally writes out.  This can be
an empty file, but it does have to be an empty _JSON_
file.

```
$ echo '{}' > tlm.json
```

Finally, invoke terraform with the target filter set to the
router module.  We have to configure the route components first
because those have to be set prior to booting the field boxes so that
the field boxes wind up at known addresses.  This is why during normal
setup you're prompted for the MAC addresses and told not to power on
the field boxes until later.

```
$ terraform apply -target module.router
```

This will take a few minutes to complete, but once it does the fields
can be connected and provisioned in a similar way.  Since this demo
only has one field, we'll just connect one and then use the wait
command from above to wait for it to boot:

```
$ curl -k --user "gizmo-fms:$GIZMO_AUTO_PASS" https://100.64.0.10/rest/system/identity
```

> [!NOTE]
>
> Note that the address changed in the above command.  The core
> network is up now which means that the FMS received an address via
> DHCP and we're no longer using the bootstrap interface or VLAN.  The
> bootstrap command retains the interface until the very end, but if
> you want to tear down the interface now, you can.

Configure the fields.  They're all named `module.fieldN` where `N` is
the index of the field:

```
$ terraform apply -target module.field1
```

Now that all the fields are configured, we can request the terraform
files be replaced with the non-bootstrap values set, and then run a
terraform apply without a target filter to make sure everything is
set:

```
$ cd ~
$ gizmo fms net reconcile --skip-apply
$ cd ~/.netstate
$ terraform init
$ terraform apply
```

> [!SUCCESS] Checkpoint!
>
> The fields are now bootstrapped and the steps from [Bootstrap
> Network](net-bootstrap.md) are now complete.

## Wrapping Up

If you decided to try and follow this guide, congratulations, you now
have a completely unsupported but functional FMS and should have a
deeper understanding of what the automated steps are actually doing.

This is as far as you can go with Hard Mode, and from here using the
Gizmo platform re-joins the guide at [Connecting
Gizmos](connect-gizmo.md).  The Gizmo FMS process runs some access
control lists on the MQTT broker that are difficult to reproduce on a
non-programmable broker, so while its technically possible to use any
broker instead of the Gizmo provided one, doing so would be very
insecure.

If this page has been interesting and you like learning about things
at this level, consider becoming a contributor to the greater Gizmo
ecosystem.  Join us on [GitHub](https://github.com/gizmo-platform/)
and pick up an interesting bug or issue to get started.
