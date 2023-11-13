---
description: Random Access Memory
---

# 💻 Memory (RAM)

SpecProbe can probe RAM information by calling the `HardwareProber.Memory` property. This populates the following values in accordance to the available information:

| Value                  | Notes                                    |
| ---------------------- | ---------------------------------------- |
| `TotalMemory`          |                                          |
| `TotalPhysicalMemory`  | Unavailable for unrooted Android devices |
| `SystemReservedMemory` | Unavailable for unrooted Android devices |

## How parsing works

This section describes how parsing works for the below systems:

### Linux

RAM information is obtained by using the `meminfo` file from the `/proc` interface usually accessible by normal users on Linux systems. The following entries are searched for:

* `MemTotal`

For total usable memory (excluding the memory reserved by the system), it attempts to parse the `MemTotal` entry to get the size in `kB` (`KiB`) to convert it to bytes by multiplying the value by `1024`.

However, for total physically installed memory, SpecProbe attempts to fetch the block size for each memory node in bytes by opening the `/sys/devices/system/memory/block_size_bytes` file as a first step to get accurate information about total memory.

Then, SpecProbe tries to get all the memory blocks and verify that the block is online before trying to add the block size to the total physical memory value in bytes.

{% hint style="info" %}
To get this information on Android phones and tablets, your device needs to be rooted and SpecProbe needs to be updated to version 1.1.0. Unrooted Android devices only show the total installed memory.
{% endhint %}

### Windows

For Windows systems, it calls the native Windows API function [`GlobalMemoryStatusEx()`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-globalmemorystatusex) to get the total usable installed memory. Then, it calls the [`GetPhysicallyInstalledSystemMemory()`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getphysicallyinstalledsystemmemory) function to get the physically installed system memory in kilobytes. SpecProbe then converts the value to bytes by multiplying it by `1024`.

### macOS

For Macintosh systems, it executes `sysctl` with the following arguments:

* `hw.memsize_usable`: Usable memory size in bytes
* `hw.memsize`: Total memory size in bytes

Then, SpecProbe processes their values as appropriate.
