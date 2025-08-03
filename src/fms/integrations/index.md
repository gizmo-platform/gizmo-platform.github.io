# Integrations

Automatic operation is available for some external software on an
as-requested basis.  If you have software you'd like to integrate in,
please reach out to the Gizmo team.

For any automation to interact with the FMS it must originate traffic
from either the FMS workstation itself, which is implicitly trusted,
or from a machine connected to the trusted field network (having an
address in `100.64.0.0/24`).  You can access this network by either
using an unmanaged workgroup switch connected to the FMS port, or
connecting to any unused field port on the scoring box.

> [!WARNING]
>
> Be careful what you plug into the FMS network.  The system ensures
> that traffic is coming from only expected locations and cannot hop
> networks, but you must still observe basic security measures such as
> not connecting untrusted devices to your scoring network.  Rule of
> thumb: if it doesn't have a good reason to be on the network it
> shouldn't be!
