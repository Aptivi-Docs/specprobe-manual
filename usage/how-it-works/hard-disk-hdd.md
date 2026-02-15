---
description: Hard drives, Solid state drives, NVMe, etc.
icon: hard-drive
---

# Hard Disk (HDD)

SpecProbe can probe HDD information by calling the `HardwareProber.GetHardDisks()` property.

<details>

<summary>Properties in the HDD information</summary>

This populates the following values in accordance to the available information:

<table><thead><tr><th width="189.9947509765625">Value</th><th>Notes</th></tr></thead><tbody><tr><td><code>HardDiskSize</code></td><td></td></tr><tr><td><code>PartitionCount</code></td><td></td></tr><tr><td><code>Partitions</code></td><td></td></tr><tr><td><code>PartitionTableType</code></td><td></td></tr></tbody></table>

Each partition in the `Partitions` list contains the following properties:

<table><thead><tr><th width="179.9947509765625">Value</th><th>Notes</th></tr></thead><tbody><tr><td><code>PartitionNumber</code></td><td></td></tr><tr><td><code>PartitionSize</code></td><td></td></tr><tr><td><code>PartitionType</code></td><td></td></tr><tr><td><code>PartitionBootable</code></td><td>Always <code>false</code> on macOS, and only <code>true</code> on Linux if it's an ESP.</td></tr><tr><td><code>PartitionOffset</code></td><td></td></tr></tbody></table>

</details>

***

## <mark style="color:$primary;">How parsing works</mark>

This section describes how parsing works for the below systems:

<details>

<summary>Windows</summary>

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

</details>

<details>

<summary>Linux</summary>

SpecProbe on Linux performs the following steps:

1. It tries to get all the block devices defined in the `/sys/block` folder.
2. Once the list of block devices are populated (loop devices are not supported), it first checks the contents of the `removable` file.
3. If the `removable` state is zero, which means that it's a fixed drive, SpecProbe attempts to get the total size of the drive by probing the block size of each drive from the `size` file for each drive.
4. Then, it translates that size to the actual size in bytes by multiplying it by `512`. It then tries to figure out the partition table type.
5. For the partitions, SpecProbe gets the device name and attempts to check the block device name to see if it ends with a number (`nvme0n1` for NVMe drives, `mmbclk0` for eMMC drives, ...).
6. After that, it assigns the drive number as appropriate.
7. SpecProbe attempts to formulate the correct partition block device name in accordance to the drive number existence (`nvme0n1` vs. `sda`) so that it can check to see if that partition exists.
8. Then, SpecProbe tries to get the size of a single partition by parsing the contents of the `size` file from the partition path and multiplying the block size reported by that file by `512` to get the total number of bytes.
9. SpecProbe then uses `lsblk` to query the partition type and the partition table type, assuming that your system has `udev` enabled.

{% hint style="info" %}
To get this information on Android phones and tablets, your device needs to be rooted and SpecProbe needs to be updated to version 1.1.0. Unrooted Android devices only show little to no information.
{% endhint %}

</details>

<details>

<summary>macOS</summary>

SpecProbe on macOS performs the following steps:

1. SpecProbe on macOS systems tries to get all the block devices defined in the `/dev` folder that their names start with `disk`.
2. `diskutil` then gets executed with the full path to the block device, such as `/dev/disk0` for the first disk and `/dev/disk0s1` for the first partition.
3. If the `Removable Media` state is `Fixed`, which means that it's a fixed drive, SpecProbe attempts to get the total size of the drive by getting the total number of bytes from the `Disk Size` for physical disks or the `Volume Used Space` for APFS virtual partitions.
4. For the partitions, SpecProbe gets the disk ID and the partition number, and checks to see if the disk is one of the known virtual disks.
5. Then, SpecProbe adds the disk or partition to the list.
6. Finally, SpecProbe tries to figure out how to parse the partition table and the partition type.

</details>
