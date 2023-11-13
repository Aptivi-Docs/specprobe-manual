---
description: Central Processing Unit
---

# 🖥 Processors (CPU)

SpecProbe can probe CPU information by calling the `HardwareProber.Processors` property. This populates the following values in accordance to the available information:

| Value              | Notes                                       |
| ------------------ | ------------------------------------------- |
| `ProcessorCores`   |                                             |
| `CoresForEachCore` | Always 1 on ARM systems                     |
| `TotalCores`       |                                             |
| `L1CacheSize`      | Unavailable on ARM systems                  |
| `L2CacheSize`      | Unavailable on ARM systems                  |
| `L3CacheSize`      | Unavailable on ARM systems                  |
|                    | Unavailable on macOS                        |
| `Name`             |                                             |
| `CpuidVendor`      | Can sometimes be empty on some systems.     |
|                    | Unavailable on ARM systems                  |
| `Vendor`           | Usually ARM or ARM Processor on ARM systems |
| `Speed`            | Unavailable on ARM systems                  |

## How parsing works

This section describes how parsing works for the below systems:

### Linux

CPU information is obtained by using the `cpuinfo` file from the `/proc` interface usually accessible by normal users on Linux systems. The following entries are searched for:

* `physical id`
* `cpu cores`
* `cpu MHz`
* `vendor_id`
* `model name`
* `processor`
* `Processor` (ARM)
* `CPU implementer` (ARM)

If the system is running either 32-bit or 64-bit, SpecProbe attempts to get the CPU name and the vendor string directly from the system processor by calling the `cpuid` assembly instruction on your processor using the native wrapper library.

For cache sizes and other properties that don't have their entry in the `/proc/cpuinfo` file, it attempts to use the `/sys/devices/system/cpu` folder to get the appropriate files and use them to calculate these values.

Whole processor's cache size can be calculated using only the primary processor's information, which is located in the `cache` folder of `/sys/devices/system/cpu/cpu0`. SpecProbe attempts to use the following folders for all the cache levels:

* L1 cache: `{cacheFolder}/index1`
* L2 cache: `{cacheFolder}/index2`
* L3 cache: `{cacheFolder}/index3`

The size file in the abovementioned folders is represented in kilobytes, so we need to convert the values to bytes by multiplying these values by `1024`.

### Windows

For Windows systems, it calls the native Windows API [`GetLogicalProcessorInformation()`](https://learn.microsoft.com/en-gb/windows/win32/api/sysinfoapi/nf-sysinfoapi-getlogicalprocessorinformation) function twice (the first time to get number of cores, and the second time to populate processor information), which contains some of the necessary information needed to, such as the processor core count and the cache sizes.

* For core count, the `RelationProcessorCore` relationship type specifies the number of real processor cores (not multiplied by 2x or more for hyper-threaded systems).
* For cores per physical core, the `RelationProcessorPackage` relationship type specifies how many processor cores are there per each physical core.
* For caches, the `RelationCache` relationship type specifies the cache size in bytes (`cacheInfo.Size`) for each cache level as specified in the `cacheInfo.Level` property.

If your Windows system is running on 32-bit or 64-bit, SpecProbe attempts to call its helper native library which attempts to call the `cpuid` machine instruction to get the CPU name and the CPU vendor.

Finally, for clock speed, it queries the `CallNtPowerInformation()` function result to get the maximum clock speed in MHz.

{% hint style="info" %}
Note that you can't get processor name if you're running Windows ARM, because there is no acceptable way to get the processor name and vendor without either slowing down (WMI) or being forged (registry).
{% endhint %}

### macOS

The name is obtained using the helper library, SpecProber, and optionally falls back to the `sysctl` method if it failed to get the name.

SpecProbe executes the `sysctl` command with the following arguments:

* `machdep.cpu.core_count`: CPU core count
* `machdep.cpu.cores_per_package`: CPU cores per processor package
* `hw.cpufrequency`: The frequency of the CPU in hertz (Hz)
* `machdep.cpu.vendor`: The processor vendor (GenuineIntel, AuthenticAMD, etc.)
* `machdep.cpu.brand_string`: The processor brand string
* `hw.l1icachesize`: L1 cache size in bytes
* `hw.l2cachesize`: L2 cache size in bytes

Then, SpecProbe probes these values and processes them as appropriate.
