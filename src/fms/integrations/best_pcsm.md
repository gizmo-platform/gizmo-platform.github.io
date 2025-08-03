# BEST Robotics PCSM

To enable automatic operation of the FMS by PCSM, you will need to set
a system level environment variable that defines the location that the
PCSM application will use to contact the FMS.  In an administrative
PowerShell window (Run as Administrator), key the following command:


```shell
[System.Environment]::SetEnvironmentVariable('FieldManagementUrl', 'http://100.64.0.2:8080', 'Machine')
```

After setting this variable, you must reboot.  After rebooting, you
may confirm the variable has been persisted by running the following
in a non-administrative PowerShell:

```shell
[System.Environment]::GetEnvironmentVariable('FieldManagementUrl', 'Machine')
```

### BEST Robotics PiSM

The PiSM is similar to PCSM, but runs as a headless process.  When
running the process on Windows, use the method above to set the
environment variable.  In all other cases, export the variable
`FieldManagementUrl` in the shell where PiSM is running.
