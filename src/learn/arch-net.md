# Network

> [!WARNING]
>
> This learn page is SIGNIFICANTLY more technical than any of the
> others.  You are not expected to understand this, and it represents
> an extremely deep level of knowledge in systems and network
> engineering.  If this kind of stuff interests you, consider a career
> in network engineering!

The Gizmo platform relies on a lot of IP communications to move data
from one place to another.  This data comprises location information
for robot assignments, control data for various systems, and status
information reported back to centralized logging and metrics
collection systems.  In order to maintain a uniform approach, all of
these systems communicate over one or more IP networks.

During field initializations, a special local subnet is used to
communicate between the FMS and the Scoring Box which provides core IP
services to the network.  This is done since during setup the layer 2
network topology will be changed, and the normal IP addresses the
system would use are not yet available.  Once the core FMS network is
up, field boxes, the FMS server, and scoring boxes receive IPs in the
`100.64.0.0/24` subnet.  While this subnet is not intended for local
assignment, instead being part of the CG-NAT allocation
([RFC6598](https://datatracker.ietf.org/doc/html/rfc6598)).  We select
this range and the adjacent subnet for bootstrapping (`100.64.0.1/24`)
due to the extremely small chance in a collission between this range
and the range in a host venue, which would render normal NAT services
impossible to deliver.  There is no risk to sitting this range behind
an external CGN as the intermediate NAT will prevent each side from
seeing the conflicting range.

Gizmo communications happen on networks which include the team number.
The team numbers all reside within the
[RFC1918](https://datatracker.ietf.org/doc/html/rfc1918) space in the
`10/8` Class A block.  This space was chosen to allow for up to 9999
teams to be identified in a single network without additional
considerations.  Future work would include migrating the Gizmo network
to IPv6 space in order to permit an even higher limit to number
allocations.

Team subnets are allocated by taking the team number, padding to 4
digits, then splitting the first and second 2-digit groups into the
second and third octets of the address.  For example, team number 4
would be represented as 0004, then assigned IP space in `10.0.4.0/24`.
Note that duplicate zeros are collapsed prior to allocation.  While
technically not necessary, this normalization makes addresses easier
to parse and reason about.  Lets look at another example.  Team number
1729 is already 4 digits in length, so we partition the number and
allocate `10.17.29.0/24` for this team's use.

These networks are managed by the FMS's Scoring Box which resides at
the first address in each block and provides DNS as well as DHCP
services.  In the case that the FMS is unavailable, the Driver's
Station runs a reduced functionality DHCP server that can assign
addresses in its local subnet only.

## Wireless Networks

The Gizmo is designed to function as part of a mobile robotics
platform, and as such needs to support wireless communications.  While
a variety of wireless standards exist, we use 802.11n wireless
networking in the 2.4Ghz channel space.  This ensures a good balance
between range, overhead, and interworking with other wireless users.

By operating according to the 802.11n standard we support the most
permissive interworking profile, and can down-clock to support any
lower mode, ensuring that the Gizmo poses no risk to any other nearby
fixed or mobile networks following North American standards.
Interworking profiles are how this is achieved, and the Gizmo supports
the highest profiles currently available for the 2.4Ghz bands.

While the flexibility of the 802.11n standard allows the use of any
SSID and security combination, for consistency the Gizmo makes use of
specific patterns.  Since no human needs to join the point-to-point
control network the SSID is formatted as a 64 character hexadecimal
sequence.  This sequence is randomly generated and is the maximum
length that SSIDs are permitted to be.  Similarly, WPA2-PSK
authentication with TKIP encryption is used to secure the channel
against unauthorized users.  The `gizmo` utilities generate 64
character random hexadecimal strings to use for these keys.

## FMS Network

Based on the information above, we can now fully detail the FMS
network.  The network is comprised of 2 broad components.  First, the
internal network on which the FMS workstation, the scoring box, and
all field boxes reside is formed.  This network resides in the
`100.64.0.0/24` space, and is the only network from which certain
control actions can be taken.  It operates on VLAN ID 1 using standard
802.1q tagging for ports facing internal devices.

Team networks are assigned to VLANs with numerically increasing IDs
starting at 500.  While this technically limits the number of teams
that could compete in a single event to just 3,596 it is considered an
acceptable tradeoff given that no venue exists which can comfortably
host that many teams.  Prior to each match, the team location mapping
is consulted to determine which field and quadrant each team will be
assigned to, and the relevant networks are made available on those
radios and access ports.  This sophisticated automation is managed by
the FMS workstation, and is completely passive once changes are
synced.
