---
description: Random Access Memory
icon: memory
---

# Memory (RAM)

SpecProbe can probe RAM information by calling the `HardwareProber.GetMemory()` function.

<details>

<summary>Properties in the RAM information</summary>

This populates the following values in accordance to the available information:

<table><thead><tr><th width="199.9947509765625">Value</th><th>Notes</th></tr></thead><tbody><tr><td><code>TotalMemory</code></td><td></td></tr><tr><td><code>TotalPhysicalMemory</code></td><td>Unavailable for unrooted Android devices</td></tr><tr><td><code>SystemReservedMemory</code></td><td>Unavailable for unrooted Android devices</td></tr></tbody></table>

</details>

***

## <mark style="color:$primary;">How parsing works</mark>

This section describes how parsing works for the below systems:

<details>

<summary>Windows</summary>

For Windows systems, it calls the native Windows API function [`GlobalMemoryStatusEx()`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-globalmemorystatusex) to get the total usable installed memory. Then, it calls the [`GetPhysicallyInstalledSystemMemory()`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getphysicallyinstalledsystemmemory) function to get the physically installed system memory in kilobytes. SpecProbe then converts the value to bytes by multiplying it by `1024`.

</details>

<details>

<summary>Linux</summary>

RAM information is obtained by using the `meminfo` file from the `/proc` interface usually accessible by normal users on Linux systems. The following entries are searched for:

* `MemTotal`

For total usable memory (excluding the memory reserved by the system), it attempts to parse the `MemTotal` entry to get the size in `kB` (`KiB`) to convert it to bytes by multiplying the value by `1024`.

However, for total physically installed memory, SpecProbe attempts to fetch the block size for each memory node in bytes by opening the `/sys/devices/system/memory/block_size_bytes` file as a first step to get accurate information about total memory.

Then, SpecProbe tries to get all the memory blocks and verify that the block is online before trying to add the block size to the total physical memory value in bytes.

{% hint style="info" %}
To get this information on Android phones and tablets, your device needs to be rooted and SpecProbe needs to be updated to version 1.1.0. Unrooted Android devices only show the total installed memory.
{% endhint %}

</details>

<details>

<summary>macOS</summary>

For Macintosh systems, it executes `sysctl` with the following arguments:

| Argument            | Description                 |
| ------------------- | --------------------------- |
| `hw.memsize_usable` | Usable memory size in bytes |
| `hw.memsize`        | Total memory size in bytes  |

Then, SpecProbe processes their values as appropriate.

</details>
