# Fully Repartitioning Disks

Under certain circumstances, it may be necessary to completely
reformat disks.  This should not ever be necessary for the FMS as it
is a fully self-contained system image, however the driver station
disks have very strict format requirements which can be corrupted by
certain automated Windows processes.  This page includes instructions
for Windows.  If you'd like to contribute a guide for macOS, please
feel free to contribute!

## Microsoft Windows

Fully repartitioning a disk on Windows is performed using an elevated
Command Prompt or PowerShell.  In the start menu, type "PowerShell"
and then click the option to "Run as Administrator".  Authenticate
when prompted, and you will be greeted with an administrative command
prompt.

> [!DANGER]
>
> Take care when working with an administrative command prompt, and
> doubly so when working with disks.  It is possible to erase system
> disks or corrupt other data while following this procedure if you do
> not pay attention and select the disk you think you're selecting.

Ensure the micro SD card you wish to partition is plugged in and
launch `diskpart` in the PowerShell window.

Obtain a listing of disks by typing `list disk`.  The output will look
similar to this:

```
DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          232 GB  1024 KB        *
  Disk 1    Online           28 GB      0 B        *
```

Note the `*` mark in the GPT column on Disk 1.  Disk 1 is the micro SD
card in this example, and has been identified as a GPT disk.  GPT
disks are a more advanced format for disk partitions that do not work
with the Driver's Station ROM, so this disk must be repartitioned.
Disk 1 was identified as the micro SD card because its size matches
the size of the expected disk, which was a 32GB card.

> [!NOTE]
>
> The disk shown above says it has a size of 28GB but the label
> physically on the disk identifies it as 32GB.  This is normal as
> most drive manufacturers market capacity using powers of 10, whereas
> drive capacity is actually measured in powers of 2.  This results in
> a disparity between the labelled capacity and the actual capacity.
> The actual capacity will always be slightly less than the labelled
> capacity.

Select disk 1 and remove all data from the disk.  This action is
immediate and cannot practically be undone, so be sure you are
selecting the right disk!

```
DISKPART> select disk 1

Disk 1 is now the selected disk.

DISKPART> clean

DiskPart succeeded in cleaning the disk.
```

Create a new partition table using MBR format, create a new partition
using the entire drive, and finally mark it as active.

```
DISKPART> convert mbr

DiskPart successfully converted the selected disk to MBR format.

DISKPART> create partition primary

DiskPart succeeded in creating the specified partition.

DISKPART> active

DiskPart marked the current partition as active.
```

Finally, format the new partition as a FAT32 volume.  If formatting
for a driver's station, make sure you set the volume label to include
the team number.  In this example, this particular Gizmo Driver's
Station is being prepared for team #228.

```
DISKPART> format fs=FAT32 label=GIZMO228 quick

  100 percent completed

DiskPart successfully formatted the volume.

DISKPART> exit

Leaving DiskPart...
```
