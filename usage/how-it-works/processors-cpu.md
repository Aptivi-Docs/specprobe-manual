---
icon: microchip
description: Central Processing Unit
---

# Processors (CPU)

SpecProbe can probe CPU information by calling the `HardwareProber.Processors` property. This populates the following values in accordance to the available information:

| Value                   | Notes                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `ProcessorCores`        |                                                                                                                    |
| `CoresForEachCore`      | Always 1 on ARM systems                                                                                            |
| `TotalCores`            |                                                                                                                    |
| `L1CacheSize`           | Unavailable on ARM systems                                                                                         |
| `L2CacheSize`           | Unavailable on ARM systems                                                                                         |
| `L3CacheSize`           | Unavailable on ARM systems                                                                                         |
|                         | Unavailable on macOS                                                                                               |
| `Name`                  | A list of part names on ARM systems                                                                                |
| `CpuidVendor`           | Can sometimes be empty on some systems.                                                                            |
|                         | Unavailable on ARM systems                                                                                         |
| `Vendor`                | A list of implementers on ARM systems                                                                              |
| `Speed`                 | Unavailable on ARM systems                                                                                         |
| `Hypervisor`            | True if running on a hypervisor; false otherwise. If you've enabled WSL or Hyper-V on Windows, it may return true. |
|                         | Unavailable on ARM systems                                                                                         |
| `CpuidHypervisorVendor` | Unavailable on ARM systems                                                                                         |
| `HypervisorVendor`      | Unavailable on ARM systems                                                                                         |
| `OnHypervisor`          | True if a hypervisor is running and a hypervisor is known.                                                         |
|                         | Unavailable on ARM syst                                                                                            |

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
* `flags`
* `Processor` (ARM)
* `CPU implementer` (ARM)
* `CPU part` (ARM)

If the system is running either 32-bit or 64-bit, SpecProbe attempts to get the CPU name and the vendor string directly from the system processor by calling the `cpuid` assembly instruction on your processor using the native wrapper library.

For cache sizes and other properties that don't have their entry in the `/proc/cpuinfo` file, it attempts to use the `/sys/devices/system/cpu` folder to get the appropriate files and use them to calculate these values.

Whole processor's cache size can be calculated using only the primary processor's information, which is located in the `cache` folder of `/sys/devices/system/cpu/cpu0`. SpecProbe attempts to use the following folders for all the cache levels:

* L1 cache: `{cacheFolder}/index1`
* L2 cache: `{cacheFolder}/index2`
* L3 cache: `{cacheFolder}/index3`

The size file in the abovementioned folders is represented in kilobytes, so we need to convert the values to bytes by multiplying these values by `1024`. Finally, if SpecProbe is not run from ARM processor, it queries the feature list using the CPUID instruction to get all features by parsing the integer as a sequence of bits by reading bit by bit.

### Windows

For Windows systems, it calls the native Windows API [`GetLogicalProcessorInformation()`](https://learn.microsoft.com/en-gb/windows/win32/api/sysinfoapi/nf-sysinfoapi-getlogicalprocessorinformation) function twice (the first time to get number of cores, and the second time to populate processor information), which contains some of the necessary information needed to, such as the processor core count and the cache sizes.

* For core count, the `RelationProcessorCore` relationship type specifies the number of real processor cores (not multiplied by 2x or more for hyper-threaded systems).
* For cores per physical core, the `RelationProcessorPackage` relationship type specifies how many processor cores are there per each physical core.
* For caches, the `RelationCache` relationship type specifies the cache size in bytes (`cacheInfo.Size`) for each cache level as specified in the `cacheInfo.Level` property.

If your Windows system is running on 32-bit or 64-bit, SpecProbe attempts to call its helper native library which attempts to call the `cpuid` machine instruction to get the CPU name and the CPU vendor.

For clock speed, it queries the `CallNtPowerInformation()` function result to get the maximum clock speed in MHz. Finally, if SpecProbe is not run from ARM processor, it queries the feature list using the CPUID instruction to get all features by parsing the integer as a sequence of bits by reading bit by bit.

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

Then, SpecProbe probes these values and processes them as appropriate. Finally, if SpecProbe is not run from ARM processor, it queries the feature list using the CPUID instruction to get all features by parsing the integer as a sequence of bits by reading bit by bit.

## CPU flags

The following CPU flags are parsed and queried by SpecProbe:

* `fpu`
* `vme`
* `de`
* `pse`
* `tsc`
* `msr`
* `pae`
* `mce`
* `cx8`
* `apic`
* `mtrr_reserved`
* `sep`
* `mtrr`
* `pge`
* `mca`
* `cmov`
* `pat`
* `pse-36`
* `psn`
* `clfsh`
* `nx`
* `ds`
* `acpi`
* `mmx`
* `fxsr`
* `sse`
* `sse2`
* `ss`
* `htt`
* `tm`
* `ia64`
* `pbe`
* `sse3`
* `pclmulqdq`
* `dtes64`
* `monitor`
* `ds-cpl`
* `vmx`
* `smx`
* `est`
* `tm2`
* `ssse3`
* `cnxt-id`
* `sdbg`
* `fma`
* `cx16`
* `xtpr`
* `pdcm`
* `pchnl`
* `pcid`
* `dca`
* `sse4.1`
* `sse4.2`
* `x2apic`
* `movbe`
* `popcnt`
* `tsc-deadline`
* `aes-ni`
* `xsave`
* `osxsave`
* `avx`
* `f16c`
* `rdrnd`
* `hypervisor`
* `fpu`
* `vme`
* `de`
* `pse`
* `tsc`
* `msr`
* `pae`
* `mce`
* `cx8`
* `apic`
* `syscall_k6`
* `syscall`
* `mtrr`
* `pge`
* `mca`
* `cmov`
* `pat`
* `pse-36`
* `ecc`
* `nx`
* `mmxext`
* `mmx`
* `fxsr`
* `fxsr_opt`
* `pdpe1gb`
* `rdtscp`
* `lm`
* `3dnowext`
* `3dnow`
* `lahf_lm`
* `cmp_legacy`
* `svm`
* `extapic`
* `cr8_legacy`
* `abm/lzcnt`
* `sse4a`
* `misalignsse`
* `3dnowprefetch`
* `osvw`
* `ibs`
* `xop`
* `skinit`
* `wdt`
* `lwp`
* `fma4`
* `tce`
* `nodeid_msr`
* `tbm`
* `topoext`
* `perfctr_core`
* `perfctr_nb`
* `streamperfmon`
* `dbx`
* `perftsc`
* `pcx_l2i`
* `monitorx`
* `addr_mask_ext`
* `fsgsbase`
* `ia32_tsc_adjust_msr`
* `sgx`
* `bmi1`
* `hle`
* `avx2`
* `fdp-excptn-only`
* `smep`
* `bmi2`
* `erms`
* `invpcid`
* `rtm`
* `rdt-m/pqm`
* `fpucsds`
* `mpx`
* `rdt-a/pqe`
* `avx512-f`
* `avx512-dq`
* `rdseed`
* `adx`
* `smap`
* `avx512-ifma`
* `pcommit`
* `clflushopt`
* `clwb`
* `pt`
* `avx512-pf`
* `avx512-er`
* `avx512-cd`
* `sha`
* `avx512-bw`
* `avx512-vl`
* `prefetchwt1`
* `avx512-vbmi`
* `umip`
* `pku`
* `ospke`
* `waitpkg`
* `avx512-vbmi2`
* `cet_ss/shstk`
* `gfni`
* `vaes`
* `vpclmulqdq`
* `avx512-vnni`
* `avx512-bitalg`
* `tme_en`
* `avx512-vpopcntdq`
* `fzm`
* `la57`
* `mawau`
* `rdpid`
* `kl`
* `bus-lock-detect`
* `cldemote`
* `mprr`
* `movdiri`
* `movdir64b`
* `enqcmd`
* `sgx-lc`
* `pks`
* `sgx-tem`
* `sgx-keys`
* `avx512-4vnniw`
* `avx512-4fmaps`
* `fsrm`
* `uintr`
* `avx512-vp2intersect`
* `srbds-ctrl`
* `md-clear`
* `rtm-always-abort`
* `rtm-force-abort`
* `serialize`
* `hybrid`
* `tsxldtrk`
* `pconfig`
* `lbr`
* `cet-ibt`
* `amx-bf16`
* `avx512-fp16`
* `amx-tile`
* `amx-int8`
* `ibrs/spec_ctrl`
* `stibp`
* `l1d_flush`
* `ia32_arch_capabilities_msr`
* `is32_core_capabilities_msr`
* `ssbd`
* `sha512`
* `sm3`
* `sm4`
* `rao-int`
* `avx-vnni`
* `avx512-bf16`
* `lass`
* `cmpccxadd`
* `archperfmonext`
* `dedup`
* `fzrm`
* `fsrs`
* `rsrcs`
* `fred`
* `lkgs`
* `wrmsrns`
* `nmi_src`
* `amx-fp16`
* `hreset`
* `avx-ifma`
* `lam`
* `msrlist`
* `invd_disable_­post_bios_done`
* `pbn`
* `pbndkb`
* `legacy_reduced_isa`
* `sipi64`
* `avx-vnni-int8`
* `avx-ne-convert`
* `amx-complex`
* `avx-vnni-int16`
* `utmr`
* `prefetchi`
* `user_msr`
* `uiret-uif-from-rflags`
* `cet-sss`
* `avx10`
* `apx_f`
* `mwait`
* `psfd`
* `ipred_ctrl`
* `rrsba_ctrl`
* `ddpd_u`
* `bhi_ctrl`
* `mcdt_no`
* `uc_lock_no`
* `monitor_mitg_no`
