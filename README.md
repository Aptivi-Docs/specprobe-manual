---
description: Welcome to SpecProbe!
icon: hand-wave
---

# Welcome!

SpecProbe is not only a reboot of Inxi.NET that probes all hardware information from your computer, such as your graphics card, your CPU, and your hard drives, but also provides you with some platform-specific tools that are useful, such as loading native libraries.

***

## <mark style="color:$primary;">Benchmarking</mark>

The benchmark shows the comparison of performance of SpecProbe and Inxi.NET in their default settings with a dry job running the hardware probe only once.

* SpecProbe: CPU, GPU, RAM, and HDD (3.2.1)
* Inxi.NET: CPU, GPU, RAM, and HDD (2022.5.0.4)

```
BenchmarkDotNet v0.14.0, Windows 11 (10.0.22631.4317/23H2/2023Update/SunValley3)
Intel Core i7-8700 CPU 3.20GHz (Coffee Lake), 1 CPU, 12 logical and 6 physical cores
.NET SDK 8.0.403
  [Host] : .NET 8.0.10 (8.0.1024.46610), X64 RyuJIT AVX2
  Dry    : .NET 8.0.10 (8.0.1024.46610), X64 RyuJIT AVX2

Job=Dry  IterationCount=1  LaunchCount=1
RunStrategy=ColdStart  UnrollFactor=1  WarmupCount=1

| Method                   | Mean       | Error |
|------------------------- |-----------:|------:|
| ProbeEverythingInxiNet   | 2,356.4 ms |    NA |
| ProbeEverythingSpecProbe |   434.1 ms |    NA |
```

Inxi.NET took `2,356.4` milliseconds, or `2.3` seconds, to probe all the supported hardware types, while SpecProbe only took `434.1` milliseconds for all the supported types mentioned above.

{% hint style="info" %}
This results in SpecProbe being faster than Inxi.NET.
{% endhint %}

***

## <mark style="color:$primary;">Release history</mark>

Below is the release history of the library:

{% updates format="full" %}
{% update date="2026-04-19" %}
## <mark style="color:$primary;">v3.9.0</mark>

<mark style="color:green;">Added removable hard drive support</mark>

<mark style="color:green;">Added error messages for loading libraries</mark>

<mark style="color:green;">Added L1, L2, and L3 support for FreeBSD and macOS (x64)</mark>

<mark style="color:green;">Added new ARM implementers</mark>

<mark style="color:green;">Added more PCI IDs</mark>

<mark style="color:yellow;">Merged SpecProbe.Native and SpecProbe.Loader to one library</mark>

<mark style="color:yellow;">Restructured parts of code</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2026-04-13" %}
## <mark style="color:$primary;">v3.8.1</mark>

<mark style="color:green;">Added initial support for FreeBSD systems</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2026-02-15" %}
## <mark style="color:$primary;">v3.8.0.1</mark>

<mark style="color:yellow;">Fixed crash when trying to parse drives on Android</mark>
{% endupdate %}

{% update date="2026-01-11" %}
## <mark style="color:$primary;">v3.8</mark>

<mark style="color:green;">Added more path tools</mark>

<mark style="color:yellow;">Updated PCI IDs for new devices</mark>
{% endupdate %}

{% update date="2025-12-19" %}
## <mark style="color:$primary;">v3.7</mark>

<mark style="color:yellow;">Updated PCI and USB IDs for new devices</mark>

<mark style="color:yellow;">Re-built libspecprober for consistency</mark>
{% endupdate %}

{% update date="2025-10-27" %}
## <mark style="color:$primary;">v3.6.2.1</mark>

<mark style="color:yellow;">General improvements</mark>
{% endupdate %}

{% update date="2025-10-27" %}
## <mark style="color:$primary;">v3.6.2</mark>

<mark style="color:yellow;">General improvements</mark>
{% endupdate %}

{% update date="2025-10-25" %}
## <mark style="color:$primary;">v3.6.1</mark>

<mark style="color:yellow;">General improvements</mark>
{% endupdate %}

{% update date="2025-07-03" %}
## <mark style="color:$primary;">v3.6</mark>

<mark style="color:green;">Added localized strings</mark>
{% endupdate %}

{% update date="2025-05-13" %}
## <mark style="color:$primary;">v3.5.1</mark>

<mark style="color:yellow;">General improvements</mark>
{% endupdate %}

{% update date="2025-02-26" %}
## <mark style="color:$primary;">v3.5</mark>

<mark style="color:green;">Added specifying multiple paths per library file class</mark>
{% endupdate %}

{% update date="2024-12-19" %}
## <mark style="color:$primary;">v3.4</mark>

<mark style="color:green;">Added the xdg-open wrapper</mark>
{% endupdate %}

{% update date="2024-11-30" %}
## <mark style="color:$primary;">v3.3</mark>

<mark style="color:green;">Added PATH tools</mark>

<mark style="color:green;">Added GUI detection helpers</mark>
{% endupdate %}

{% update date="2024-11-14" %}
## <mark style="color:$primary;">v3.2.2</mark>

<mark style="color:yellow;">Added system error codes to exception messages</mark>
{% endupdate %}

{% update date="2024-10-18" %}
## <mark style="color:$primary;">v3.2.1</mark>

<mark style="color:green;">Added cache invalidation</mark>
{% endupdate %}

{% update date="2024-10-17" %}
## <mark style="color:$primary;">v3.2</mark>

<mark style="color:green;">Added CPU flag support</mark>

<mark style="color:green;">Added USB ID list</mark>

<mark style="color:green;">Added environment variable management</mark>

<mark style="color:yellow;">Updated pci.ids</mark>

<mark style="color:yellow;">Improved support for ARM processors (names)</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2024-10-13" %}
## <mark style="color:$primary;">v3.1.1</mark>

<mark style="color:yellow;">Fixed a security issue</mark>
{% endupdate %}

{% update date="2024-09-04" %}
## <mark style="color:$primary;">v3.1</mark>

<mark style="color:green;">Added nullability support</mark>
{% endupdate %}

{% update date="2024-08-18" %}
## <mark style="color:$primary;">v3.0</mark>

<mark style="color:green;">Added PCI ID parser</mark>

<mark style="color:green;">Added more partition info, including partition types</mark>

<mark style="color:green;">Added hypervisor check</mark>

<mark style="color:yellow;">Updated the RID graph</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2024-08-11" %}
## <mark style="color:$primary;">v2.0.1</mark>

<mark style="color:yellow;">Added ARM64 support (DirectX)</mark>

<mark style="color:red;">Removed 32-bit support</mark>

{% hint style="warning" %}
Starting from SpecProbe 2.0.1, we no longer support 32-bit architectures. In case you need to support 32-bit platforms, you can still use 2.0.0, but you'll miss out in both improvements and in additions. We recommend you to ditch 32-bit support as soon as you can.
{% endhint %}
{% endupdate %}

{% update date="2024-08-01" %}
## <mark style="color:$primary;">v2.0</mark>

<mark style="color:green;">Native library loader added</mark>

<mark style="color:yellow;">Improved GPU parsing for Windows</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2024-07-27" %}
## <mark style="color:$primary;">v1.6.0.1</mark>

<mark style="color:yellow;">Updated NativeLand</mark>
{% endupdate %}

{% update date="2024-07-27" %}
## <mark style="color:$primary;">v1.6</mark>

<mark style="color:yellow;">Widened availability of the library with support for more frameworks</mark>
{% endupdate %}

{% update date="2024-07-10" %}
## <mark style="color:$primary;">v1.5</mark>

<mark style="color:green;">Added generic RID getter</mark>

<mark style="color:yellow;">Fixed a security bug,</mark> [<mark style="color:yellow;">CVE-2024-30105</mark>](https://github.com/advisories/GHSA-hh2w-p6rv-4g7w)

<mark style="color:yellow;">Fixed empty CPUID vendor in some PCs</mark>

<mark style="color:yellow;">Fixed hard drive parser crash on macOS</mark>
{% endupdate %}

{% update date="2024-02-06" %}
## <mark style="color:$primary;">v1.4.2</mark>

<mark style="color:yellow;">Changed symbol nupkg to snupkg</mark>
{% endupdate %}

{% update date="2024-02-05" %}
## <mark style="color:$primary;">v1.4.1</mark>

<mark style="color:green;">Added environment parsers</mark>

<mark style="color:green;">Added WSL parsers</mark>
{% endupdate %}

{% update date="2024-02-01" %}
## <mark style="color:$primary;">v1.4</mark>

<mark style="color:green;">Added RID graph feature</mark>
{% endupdate %}

{% update date="2024-01-18" %}
## <mark style="color:$primary;">v1.3</mark>

<mark style="color:green;">Added support for .NET Framework 4.8 apps</mark>

<mark style="color:yellow;">Updated dependencies</mark>
{% endupdate %}

{% update date="2023-12-21" %}
## <mark style="color:$primary;">v1.2.0.2</mark>

<mark style="color:yellow;">Facilitated debugging by using Source Link</mark>
{% endupdate %}

{% update date="2023-12-02" %}
## <mark style="color:$primary;">v1.2.0.1</mark>

<mark style="color:yellow;">SpecProbe.Software is now compatible with the majority of .NET versions</mark>
{% endupdate %}

{% update date="2023-11-29" %}
## <mark style="color:$primary;">v1.2</mark>

<mark style="color:yellow;">Re-written the Windows HDD parser</mark>

<mark style="color:yellow;">General improvements</mark>
{% endupdate %}

{% update date="2023-11-15" %}
## <mark style="color:$primary;">v1.1</mark>

<mark style="color:green;">Added support for macOS</mark>
{% endupdate %}

{% update date="2023-10-15" %}
## <mark style="color:$primary;">v1.0</mark>

The initial release of the library is now live!
{% endupdate %}
{% endupdates %}
