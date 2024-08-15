# FMS

The FMS is made up of multiple systems.  These can each encounter
unique failure modes, this page lists some of the common ones and
steps to recover from each.

### Flash Device Failure

During device flashing, you may get a key refused message, or other
error from the flash process.  If this occurs, re-run the flash-device
command without rebooting the device you are flashing.

### Bootstrap Failure

Most bootstrap issues arise from not waiting long enough for the
network devices to boot.  This is a process that requires patience.
If you find that you have started the bootstrap too early and the
process has crashed, you can clear out the state and restart it:

```sh
$ rm -r ~/.netstate
```

Note that doing this after the network configuration is actually done
will result in an unrecoverable situation where you need to re-flash
your equipment and re-bootstrap.

### Bootstrap Terraform Initialization Fault

If the bootstrap process fails with a message related to terraform
provider initialization, check that the FMS Workstation is still
connected to the internet via WiFi.  This connection must be
maintained until the FMS network is fully configured and connected to
wired internet.

### Field Remap Aborted

If you try to remap the location bindings for teams while another
remap is in progress the system may abort both.  If this occurs, retry
the binding and the new attempt will succeed.
