# Driver Station

The driver station has a handful of failure modes and things that can
break it.  Here's some common failure modes and issues.

### Imaging Issues

During imaging you may encounter issues where Balena Etcher does not
write the full image.  This can usually be resolved by restarting the
imaging process, disconnecting and reconnecting the micro SD card from
the reader, or using a downloaded copy of the `.img` file rather than
the URL.

### Boot Issues

If you encounter issues with booting the Gizmo DS make sure that you
have supplied a `gsscfg.json` file as described in the [setup
guide](/startup/ds.md).

Under rare circumstances the micro SD card can become corrupted.  If
this occurs you must re-image the micro SD card.

### Binding Issues

The binding process relies on passing information back and forth
between the Gizmo and the driver station.  After pressing the system
processor `BOOTSEL` button it may take up to 1 minute to complete
binding.  If the Gizmo has not bound within this interval, reboot both
the driver's station and the Gizmo to try again.

