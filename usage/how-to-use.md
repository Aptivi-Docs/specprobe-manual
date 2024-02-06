---
description: How do you use this library?
---

# 🖥 How to use

{% hint style="info" %}
You can now use this library in .NET Framework 4.8 applications!
{% endhint %}

This library is simple to use compared to Inxi.NET. You can selectively parse hardware and get information for each part that is currently supported by SpecProbe below:

<table><thead><tr><th width="267">Type</th><th>Property</th></tr></thead><tbody><tr><td>Processor (CPU)</td><td><code>HardwareProber.Processors</code></td></tr><tr><td>Graphics Card (GPU)</td><td><code>HardwareProber.Video</code></td></tr><tr><td>System Memory (RAM)</td><td><code>HardwareProber.Memory</code></td></tr><tr><td>Storage Devices (HDD, SSD, NVMe, eMMC, ...)</td><td><code>HardwareProber.HardDisk</code></td></tr></tbody></table>

Once you call these properties, the parser relevant to the part that you need to get information will try to fetch info from the hardware in native ways.

## Benchmark Results

The benchmark shows the comparison of performance of SpecProbe and Inxi.NET in their default settings.

* SpecProbe: CPU, GPU, RAM, and HDD (1.0.0)
* Inxi.NET: All supported hardware types (2022.5.0.4)

<pre><code>BenchmarkDotNet v0.13.9+228a464e8be6c580ad9408e98f18813f6407fb5a, Windows 10 (10.0.19045.3570/22H2/2022Update)
Intel Core i7-8700 CPU 3.20GHz (Coffee Lake), 1 CPU, 12 logical and 6 physical cores
.NET SDK 7.0.402
  [Host]     : .NET 6.0.23 (6.0.2323.48002), X64 RyuJIT AVX2
  DefaultJob : .NET 6.0.23 (6.0.2323.48002), X64 RyuJIT AVX2


| Method                   | Mean                 | Error             | StdDev            |
|------------------------- |---------------------:|------------------:|------------------:|
<strong>| ProbeEverythingInxiNet   | 10,220,355,442.86 ns | 20,018,911.754 ns | 17,746,243.838 ns |
</strong><strong>| ProbeEverythingSpecProbe |             14.25 ns |          0.079 ns |          0.074 ns |
</strong></code></pre>

Inxi.NET took `10,220,355,442.86` nanoseconds, or `10.2` seconds, to probe all the supported hardware types, while SpecProbe only took `14.25` nanoseconds for all the supported types mentioned above. This makes SpecProbe thousands of times faster than Inxi.NET.

## Parts

For individual parts, check their individual pages in the page below:

{% content-ref url="how-it-works/" %}
[how-it-works](how-it-works/)
{% endcontent-ref %}

## Software

In addition to the hardware parser that SpecProbe provides, this library also provides a separate NuGet package that allows you to get software information, including your kernel version.

### Kernel version

Just use the `UnameManager` class that contains:

* `GetUname(UnameTypes)`

This function queries information about your kernel and its basic information, like your system architecture and your kernel version, based on the passed `uname` flags. Currently, it supports all the portable flags in `UnameTypes`:︎

* Kernel name (`UnameTypes.KernelName`)
* Kernel release (`UnameTypes.KernelRelease`)
* Kernel version (`UnameTypes.KernelVersion`)
* Network host name (`UnameTypes.NetworkNode`)
* Machine architecture (`UnameTypes.Machine`)
* Operating system (`UnameTypes.OperatingSystem`)

### Platform

You can also query the platform of your choice using functions defined in the `PlatformHelper` class. It allows you to check to see if the host is running Windows or Linux, and more. It also allows you to get the terminal emulator and the type.

You can also use the RID graph reader by using `GetGraphFromRid()` found in the `RidGraphReader` class to get all the RIDs that can be used to resolve them to basically the base RID.

You can refer to the `ridgraph.json` file found in the source code for the whole RID map.
