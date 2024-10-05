---
description: Hard drives, Solid state drives, NVMe, etc.
icon: hard-drive
---

# Hard Disk (HDD)

SpecProbe can probe HDD information by calling the `HardwareProber.HardDisk` property. This populates the following values in accordance to the available information:

| Value                | Notes |
| -------------------- | ----- |
| `HardDiskSize`       |       |
| `PartitionCount`     |       |
| `Partitions`         |       |
| `PartitionTableType` |       |

Each partition in the `Partitions` list contains the following properties:

| Value               | Notes                                                             |
| ------------------- | ----------------------------------------------------------------- |
| `PartitionNumber`   |                                                                   |
| `PartitionSize`     |                                                                   |
| `PartitionType`     |                                                                   |
| `PartitionBootable` | Always `false` on macOS, and only `true` on Linux if it's an ESP. |
| `PartitionOffset`   |                                                                   |

## How parsing works

This section describes how parsing works for the below systems:

### Linux

SpecProbe on Linux systems tries to get all the block devices defined in the `/sys/block` folder. Once the list of block devices are populated (loop devices are not supported), it first checks the contents of the `removable` file.

If the `removable` state is zero, which means that it's a fixed drive, SpecProbe attempts to get the total size of the drive by probing the block size of each drive from the `size` file for each drive. Then, it translates that size to the actual size in bytes by multiplying it by `512`. It then tries to figure out the partition table type.

For the partitions, SpecProbe gets the device name and attempts to check the block device name to see if it ends with a number (`nvme0n1` for NVMe drives, `mmbclk0` for eMMC drives, ...). After that, it assigns the drive number as appropriate. SpecProbe attempts to formulate the correct partition block device name in accordance to the drive number existence (`nvme0n1` vs. `sda`) so that it can check to see if that partition exists.

Then, SpecProbe tries to get the size of a single partition by parsing the contents of the `size` file from the partition path and multiplying the block size reported by that file by `512` to get the total number of bytes. SpecProbe then uses `lsblk` to query the partition type and the partition table type, assuming that your system has `udev` enabled.

{% hint style="info" %}
To get this information on Android phones and tablets, your device needs to be rooted and SpecProbe needs to be updated to version 1.1.0. Unrooted Android devices only show little to no information.
{% endhint %}

### Windows

For Windows systems, it tries to get the list of all the recognized physical Windows drives, including the unmounted partitions, such as your system reserved partition. Once the drive is ready, SpecProbe formulates the Windows kernel partition path for the next step.

After that, it calls the [`CreateFile()`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilea) Windows API function to the formulated partition path to add the resulting drive handle to the list of handles.

Then, SpecProbe attempts to access the [`DeviceIoControl()`](https://learn.microsoft.com/en-us/windows/win32/api/ioapiset/nf-ioapiset-deviceiocontrol) function for two things:

* Drive geometry to get access to the Cylinder, Heads, and Sectors (CHS) info
* Device number that gets assigned by the Windows operating system

Then, the hard disk size is calculated by multiplying the four values:

* Cylinders
* Tracks per cylinder (heads)
* Sectors per track (or per head)
* Bytes per sector

Next, SpecProbe tries to get information about the partition table for each disk and type for each partition.

{% hint style="info" %}
Because of the usage of `CreateFile()` and the `DeviceIoControl()` Windows API functions on your drives, you need to run every application that uses SpecProbe to probe your hard drives as an elevated administrator. The console app shipped with SpecProbe sets the requirement of the administrator privileges.

To do this in your application, create an `app.manifest` file and add this inside the `requestedPrivileges` list:

```xml
<requestedExecutionLevel level="requireAdministrator" uiAccess="false" />
```
{% endhint %}

### macOS

SpecProbe on macOS systems tries to get all the block devices defined in the `/dev` folder that their names start with `disk`. `diskutil` then gets executed with the full path to the block device, such as `/dev/disk0` for the first disk and `/dev/disk0s1` for the first partition.

If the `Removable Media` state is `Fixed`, which means that it's a fixed drive, SpecProbe attempts to get the total size of the drive by getting the total number of bytes from the `Disk Size` for physical disks or the `Volume Used Space` for APFS virtual partitions.

For the partitions, SpecProbe gets the disk ID and the partition number, and checks to see if the disk is one of the known virtual disks. Then, SpecProbe adds the disk or partition to the list. Next, SpecProbe tries to figure out how to parse the partition table and the partition type.
